---
title: Hochskalieren eines primären Azure Service Fabric-Knotentyps
description: Skalieren Sie Ihren Service Fabric-Cluster vertikal, indem Sie einen neuen Knotentyp hinzufügen und den vorherigen entfernen.
ms.date: 12/11/2020
ms.author: pepogors
ms.topic: how-to
ms.openlocfilehash: 325ece761481077171a670c52e9d98071237601a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98251176"
---
# <a name="scale-up-a-service-fabric-cluster-primary-node-type"></a>Hochskalieren des primären Knotentyps eines Service Fabric-Clusters

Dieser Artikel beschreibt, wie Sie den primären Knotentyp eines Service Fabric-Clusters mit minimalen Ausfallzeiten hochskalieren können. Beim Upgraden eines Service Fabric-Clusterknotentyps gilt die folgende allgemeine Vorgehensweise:

1. Fügen Sie Ihrem Service Fabric-Cluster einen neuen Knotentyp hinzu, der durch die SKU und die Konfiguration der aktualisierten (oder geänderten) VM-Skalierungsgruppe unterstützt wird. Dieser Schritt umfasst auch das Einrichten eines neuen Load Balancers, Subnetzes und einer öffentlichen IP-Adresse für die Skalierungsgruppe.

1. Sobald die ursprüngliche und die upgegradete Skalierungsgruppe parallel ausgeführt werden, müssen Sie die ursprünglichen Knoteninstanzen nacheinander deaktivieren, damit die Systemdienste (oder die Replikate der zustandsbehafteten Dienste) zur neuen Skalierungsgruppe migriert werden.

1. Überprüfen Sie, ob der Cluster und die neuen Knoten fehlerfrei sind, und entfernen Sie dann die ursprüngliche Skalierungsgruppe (sowie in Beziehung stehende Ressourcen) und den Knotenzustand für die gelöschten Knoten.

Im folgenden wird Schritt für Schritt erläutert, wie Sie die VM-Größe und das Betriebssystem von VMs des primären Knotentyps eines Beispielclusters mit [Dauerhaftigkeit „Silber“](service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster) aktualisieren, die von einer einzelnen Skalierungsgruppe mit fünf Knoten unterstützt werden. Sie aktualisieren den primären Knotentyp:

- Aus der VM-Größe *Standard_D2_V2* in *Standard D4_V2*.
- Aus VM-Betriebssystem *Windows Server 2016 Datacenter with Containers* in *Windows Server 2019 Datacenter with Containers*.

> [!WARNING]
> Bevor Sie versuchen, diesen Vorgang für einen Produktionscluster auszuführen, sollten Sie sich die Beispielvorlagen ansehen und den Vorgang mithilfe eines Testclusters überprüfen. Der Cluster ist möglicherweise auch für kurze Zeit nicht verfügbar.
>
> Versuchen Sie nicht, eine Hochskalierungsprozedur für primäre Knoten auszuführen, wenn der Cluster einen fehlerhaften Status aufweist. Dies würde die Stabilität des Clusters nur weiter gefährden.

Im Folgenden finden Sie die schrittweisen Azure-Bereitstellungsvorlagen, die wir für dieses Upgradeszenario verwenden: https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade

## <a name="set-up-the-test-cluster"></a>Einrichten des Testclusters

Richten Sie zunächst Ihren Service Fabric-Testcluster ein. [Laden Sie dazu die Azure Resource Manager-Beispielvorlagen herunter](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade), die Sie für dieses Szenario benötigen.

Melden Sie sich anschließend bei Ihrem Azure-Konto an.

```powershell
# Sign in to your Azure account
Login-AzAccount -SubscriptionId "<subscription ID>"
```

Öffnen Sie dann die Datei [*parameters.json*](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade/parameters.json), und aktualisieren Sie den Wert für `clusterName` in einen eindeutigen Wert (in Azure).

Mithilfe der folgenden Befehle können Sie ein neues selbstsigniertes Zertifikat erstellen und das Testcluster bereitstellen. Wenn Sie bereits über ein Zertifikat verfügen, das Sie verwenden möchten, können Sie direkt mit dem Abschnitt [Verwenden eines bestehenden Zertifikats für die Clusterbereitstellung](#use-an-existing-certificate-to-deploy-the-cluster) fortfahren.

### <a name="generate-a-self-signed-certificate-and-deploy-the-cluster"></a>Generieren eines selbstsignierten Zertifikats und Bereitstellen des Clusters

Weisen Sie zunächst die Variablen zu, die Sie für die Bereitstellung des Service Fabric-Clusters benötigen. Passen Sie die Werte für `resourceGroupName`, `certSubjectName`, `parameterFilePath` und `templateFilePath` für ihr spezifisches Konto und Ihre Umgebung an:

```powershell
# Assign deployment variables
$resourceGroupName = "sftestupgradegroup"
$certOutputFolder = "c:\certificates"
$certPassword = "Password!1" | ConvertTo-SecureString -AsPlainText -Force
$certSubjectName = "sftestupgrade.southcentralus.cloudapp.azure.com"
$parameterFilePath = "C:\parameters.json"
$templateFilePath = "C:\Initial-TestClusterSetup.json"
```

> [!NOTE]
> Stellen Sie sicher, dass der Speicherort `certOutputFolder` auf Ihrem lokalen Computer vorhanden ist, bevor Sie den Befehl zum Bereitstellen eines neuen Service Fabric-Clusters ausführen.

Stellen Sie nun den Service Fabric-Testcluster bereit:

```powershell
# Deploy the initial test cluster
New-AzServiceFabricCluster `
    -ResourceGroupName $resourceGroupName `
    -CertificateOutputFolder $certOutputFolder `
    -CertificatePassword $certPassword `
    -CertificateSubjectName $certSubjectName `
    -TemplateFile $templateFilePath `
    -ParameterFile $parameterFilePath
```

Nachdem die Bereitstellung abgeschlossen ist, suchen Sie die *.pfx*-Datei (`$certPfx`) auf Ihrem lokalen Computer, und importieren Sie sie in Ihren Zertifikatspeicher:

```powershell
cd c:\certificates
$certPfx = ".\sftestupgradegroup20200312121003.pfx"

Import-PfxCertificate `
     -FilePath $certPfx `
     -CertStoreLocation Cert:\CurrentUser\My `
     -Password (ConvertTo-SecureString Password!1 -AsPlainText -Force)
```

Bei diesem Vorgang wird der Zertifikatfingerabdruck zurückgegeben, mit dem Sie nun eine [Verbindung mit dem neuen Cluster herstellen](#connect-to-the-new-cluster-and-check-health-status) und dessen Integritätsstatus überprüfen können. (Sie können den folgenden Abschnitt überspringen, denn darin wird ein alternativer Ansatz für die Clusterbereitstellung beschrieben.)

### <a name="use-an-existing-certificate-to-deploy-the-cluster"></a>Verwenden eines bestehenden Zertifikats für die Clusterbereitstellung

Der Testcluster lässt sich alternativ mithilfe eines vorhandenen Azure Key Vault-Zertifikats bereitstellen. Dazu müssen Sie die [Verweise auf Ihren Key Vault-Tresor](#obtain-your-key-vault-references) und den Zertifikatfingerabdruck abrufen.

```powershell
# Key Vault variables
$certUrlValue = "https://sftestupgradegroup.vault.azure.net/secrets/sftestupgradegroup20200309235308/dac0e7b7f9d4414984ccaa72bfb2ea39"
$sourceVaultValue = "/subscriptions/########-####-####-####-############/resourceGroups/sftestupgradegroup/providers/Microsoft.KeyVault/vaults/sftestupgradegroup"
$thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"
```

Legen Sie nun einen Ressourcengruppennamen für den Cluster fest, und legen Sie die Speicherorte `templateFilePath` und `parameterFilePath` fest:

> [!NOTE]
> Die bereitgestellte Ressourcengruppe muss bereits vorhanden sein und sich in derselben Region wie Ihr Key Vault-Tresor befinden.

```powershell
$resourceGroupName = "sftestupgradegroup"
$templateFilePath = "C:\Initial-TestClusterSetup.json"
$parameterFilePath = "C:\parameters.json"
```

Führen Sie abschließend den folgenden Befehl aus, um den anfangs erstellten Testcluster bereitzustellen:

```powershell
# Deploy the initial test cluster
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

### <a name="connect-to-the-new-cluster-and-check-health-status"></a>Herstellen einer Verbindung zu dem neuen Cluster und Überprüfen des Integritätsstatus

Stellen Sie eine Verbindung mit dem Cluster her, und stellen Sie sicher, dass alle fünf Knoten fehlerfrei sind (ersetzen Sie die Variablen `clusterName` und `thumb` durch Ihre eigenen Werte):

```powershell
# Connect to the cluster
$clusterName = "sftestupgrade.southcentralus.cloudapp.azure.com:19000"
$thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"

Connect-ServiceFabricCluster `
    -ConnectionEndpoint $clusterName `
    -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My

# Check cluster health
Get-ServiceFabricClusterHealth
```

Damit sind Sie bereit dazu, das Upgrade zu beginnen.

## <a name="deploy-a-new-primary-node-type-with-upgraded-scale-set"></a>Bereitstellen eines neuen primären Knotentyps mit der aktualisierten Skalierungsgruppe

Um einen Knotentyp zu aktualisieren (vertikal zu skalieren), müssen Sie zunächst einen neuen Knotentyp bereitstellen, der durch eine neue Skalierungsgruppe und unterstützende Ressourcen unterstützt wird. Die neue Skalierungsgruppe wird als primär gekennzeichnet (`isPrimary: true`), genauso wie die ursprüngliche Skalierungsgruppe (wenn Sie kein Upgrade eines nicht primären Knotentyps ausführen). Die im folgenden Abschnitt erstellten Ressourcen werden schließlich zum neuen primären Knotentyp in Ihrem Cluster, und die ursprünglichen primären Knotentypressourcen werden gelöscht.

### <a name="update-the-cluster-template-with-the-upgraded-scale-set"></a>Aktualisieren der Clustervorlage mit der upgegradeten Skalierungsgruppe

Im Folgenden finden Sie die abschnittsweisen Änderungen der ursprünglichen Vorlage für die Bereitstellung von Clustern zum Hinzufügen eines neuen primären Knotentyps und unterstützender Ressourcen.

Die erforderlichen Änderungen für diesen Schritt wurden bereits in der Vorlagendatei [*Step1-AddPrimaryNodeType.json*](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade/Step1-AddPrimaryNodeType.json) vorgenommen. Im Folgenden werden diese Änderungen ausführlich erläutert. Wenn Sie möchten, können Sie die Erklärung überspringen und weiterhin [Ihre Key Vault Verweise abrufen](#obtain-your-key-vault-references) und [die aktualisierte Vorlage](#deploy-the-updated-template) bereitstellen, mit der dem Cluster ein neuer primärer Knotentyp hinzugefügt wird.

> [!Note]
> Stellen Sie sicher, dass Sie Namen verwenden, die sich eindeutig vom ursprünglichen Knotentyp, der Skalierungsgruppe, dem Load Balancer, der öffentlichen IP-Adresse und dem Subnetz des ursprünglichen primären Knotentyps unterscheiden, da diese Ressourcen in einem späteren Schritt des Prozesses gelöscht werden.

#### <a name="create-a-new-subnet-in-the-existing-virtual-network"></a>Erstellen eines neuen Subnetzes im vorhandenen virtuellen Netzwerk

```json
{
    "name": "[variables('subnet1Name')]",
    "properties": {
        "addressPrefix": "[variables('subnet1Prefix')]"
    }
}
```

#### <a name="create-a-new-public-ip-with-a-unique-domainnamelabel"></a>Erstellen einer neuen öffentlichen IP-Adresse mit einem eindeutigen domainNameLabel-Element

```json
{
    "apiVersion": "[variables('publicIPApiVersion')]",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat(variables('lbIPName'),'-',variables('vmNodeType1Name'))]",
    "location": "[variables('computeLocation')]",
    "properties": {
    "dnsSettings": {
        "domainNameLabel": "[concat(variables('dnsName'),'-','nt1')]"
    },
    "publicIPAllocationMethod": "Dynamic"
    },
    "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
    }
}
```

#### <a name="create-a-new-load-balancer-for-the-public-ip"></a>Erstellen eines neuen Load Balancers für die öffentliche IP-Adresse

```json
"dependsOn": [
    "[concat('Microsoft.Network/publicIPAddresses/',concat(variables('lbIPName'),'-',variables('vmNodeType1Name')))]"
]
```

#### <a name="create-a-new-virtual-machine-scale-set-with-upgraded-vm-and-os-skus"></a>Erstellen einer neuen VM-Skalierungsgruppe (mit aktualisierter VM und aktualisierten Betriebssystem-SKUs)

Knotentypverweis

```json
"nodeTypeRef": "[variables('vmNodeType1Name')]"
```

VM-SKU

```json
"sku": {
    "name": "[parameters('vmNodeType1Size')]",
    "capacity": "[parameters('nt1InstanceCount')]",
    "tier": "Standard"
}
```

Betriebssystem-SKU

```json
"imageReference": {
    "publisher": "[parameters('vmImagePublisher1')]",
    "offer": "[parameters('vmImageOffer1')]",
    "sku": "[parameters('vmImageSku1')]",
    "version": "[parameters('vmImageVersion1')]"
}
```

Stellen Sie sicher, dass alle zusätzlichen Erweiterungen einbezogen werden, die für Ihre Workload erforderlich sind.

#### <a name="add-a-new-primary-node-type-to-the-cluster"></a>Hinzufügen eines neuen primären Knotentyps zum Cluster

Nachdem der neue Knotentyp (vmNodeType1Name) einen eigenen Namen, ein Subnetz, eine IP-Adresse, einen Load Balancer und eine Skalierungsgruppe besitzt, kann er alle anderen Variablen aus dem ursprünglichen Knotentyp (z. B. `nt0applicationEndPort`, `nt0applicationStartPort` und `nt0fabricTcpGatewayPort`) wiederverwenden:

```json
"name": "[variables('vmNodeType1Name')]",
"applicationPorts": {
    "endPort": "[variables('nt0applicationEndPort')]",
    "startPort": "[variables('nt0applicationStartPort')]"
},
"clientConnectionEndpointPort": "[variables('nt0fabricTcpGatewayPort')]",
"durabilityLevel": "Bronze",
"ephemeralPorts": {
    "endPort": "[variables('nt0ephemeralEndPort')]",
    "startPort": "[variables('nt0ephemeralStartPort')]"
},
"httpGatewayEndpointPort": "[variables('nt0fabricHttpGatewayPort')]",
"isPrimary": true,
"reverseProxyEndpointPort": "[variables('nt0reverseProxyEndpointPort')]",
"vmInstanceCount": "[parameters('nt1InstanceCount')]"
```

Wenn Sie alle Änderungen in der Vorlagen- und der Parameterdatei implementiert haben, fahren Sie mit dem nächsten Abschnitt fort, um Ihre Key Vault-Verweise abzurufen und die Updates in Ihrem Cluster bereitzustellen.

### <a name="obtain-your-key-vault-references"></a>Abrufen Ihrer Key Vault-Verweise

Zum Bereitstellen der aktualisierten Konfiguration benötigen Sie mehrere Verweise auf das Clusterzertifikat, das in Ihrem Key Vault gespeichert ist. Dies lässt sich am einfachsten im Azure-Portal durchführen. Sie benötigen Folgendes:

* **Die Key Vault-URL Ihres Clusterzertifikats:** Klicken Sie im Azure-Portal in Ihrem Key Vault-Tresor auf **Zertifikate** > *Ihr gewünschtes Zertifikat* > **Geheimnis-ID**:

    ```powershell
    $certUrlValue="https://sftestupgradegroup.vault.azure.net/secrets/sftestupgradegroup20200309235308/dac0e7b7f9d4414984ccaa72bfb2ea39"
    ```

* **Den Fingerabdruck Ihres Clusterzertifikats:** (Wenn Sie eine [Verbindung zum ursprünglichen Cluster hergestellt haben](#connect-to-the-new-cluster-and-check-health-status), um dessen Integritätsstatus zu überprüfen, verfügen Sie wahrscheinlich bereits über den Fingerabdruck.) Kopieren Sie im Azure-Portal auf demselben Zertifikatblatt (**Zertifikate** > *Ihr gewünschtes Zertifikat*) den Wert unter **X.509 SHA-1 Thumbprint (in hex)** (X.509-SHA-1-Fingerabdruck [hexadezimal]):

    ```powershell
    $thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"
    ```

* **Die Ressourcen-ID Ihres Key Vault-Tresors:** Klicken Sie im Azure-Portal in Ihrem Key Vault-Tresor auf **Eigenschaften** > **Ressourcen-ID**:

    ```powershell
    $sourceVaultValue = "/subscriptions/########-####-####-####-############/resourceGroups/sftestupgradegroup/providers/Microsoft.KeyVault/vaults/sftestupgradegroup"
    ```

### <a name="deploy-the-updated-template"></a>Bereitstellen der aktualisierten Vorlage

Passen Sie `templateFilePath` nach Bedarf an, und führen Sie den folgenden Befehl aus:

```powershell
# Deploy the new node type and its resources
$templateFilePath = "C:\Step1-AddPrimaryNodeType.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

Überprüfen Sie nach Abschluss der Bereitstellung noch einmal die Clusterintegrität, und stellen Sie sicher, dass alle Knoten für beide Knotentypen fehlerfrei sind.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="migrate-seed-nodes-to-the-new-node-type"></a>Migrieren der Startknoten zum neuen Knotentyp

Wir sind nun bereit, den ursprünglichen Knotentyp als nicht primär zu aktualisieren und mit der Deaktivierung seiner Knoten zu beginnen. Wenn die Knoten deaktiviert werden, werden die Systemdienste und Startknoten des Clusters zur neuen Skalierungsgruppe migriert.

### <a name="unmark-the-original-node-type-as-primary"></a>Aufheben der Markierung des ursprünglichen Knotentyps als primär

Entfernen Sie zunächst die Bezeichnung `isPrimary` in der Vorlage aus dem ursprünglichen Knotentyp.

```json
{
    "isPrimary": false,
}
```

Stellen Sie die Vorlage dann mit dem Update bereit. Dadurch wird die Migration der Startknoten zur neuen Skalierungsgruppe initiiert.

```powershell
$templateFilePath = "C:\Step2-UnmarkOriginalPrimaryNodeType.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

> [!Note]
> Es dauert einige Zeit, bis die Migration des Startknotens zur neuen Skalierungsgruppe durchgeführt wird. Damit die Datenkonsistenz sichergestellt ist, können Startknoten nur nacheinander deaktiviert werden. Jede Deaktivierung eines Startknotens erfordert ein Clusterupdate. Das Ersetzen eines Startknotens erfordert daher zwei Clusterupgrades (eines für das Hinzufügen und eines für das Entfernen von Knoten). Das Upgraden der fünf Startknoten in diesem Beispielszenario ergibt zehn Clusterupgrades.

Verwenden Sie Service Fabric Explorer, um die Migration von Startknoten zur neuen Skalierungsgruppe zu überwachen. Die Knoten des ursprünglichen Knotentyps (nt0vm) sollten in der Spalte **Ist Startknoten** alle *false* sein, und die Knoten des neuen Knotentyps (nt1vm) sind *true*.

### <a name="disable-the-nodes-in-the-original-node-type-scale-set"></a>Deaktivieren der Knoten in der ursprünglichen Knotentyp-Skalierungsgruppe

Nachdem alle Startknoten zur neuen Skalierungsgruppe migriert wurden, können Sie die Knoten der ursprünglichen Skalierungsgruppe deaktivieren.

```powershell
# Disable the nodes in the original scale set.
$nodeType = "nt0vm"
$nodes = Get-ServiceFabricNode

Write-Host "Disabling nodes..."
foreach($node in $nodes)
{
  if ($node.NodeType -eq $nodeType)
  {
    $node.NodeName

    Disable-ServiceFabricNode -Intent RemoveNode -NodeName $node.NodeName -Force
  }
}
```

Verwenden Sie Service Fabric Explorer, um den Statuswechsel der Knoten in der ursprünglichen Skalierungsgruppe von *Wird deaktiviert* zu *Deaktiviert* zu überwachen.

:::image type="content" source="./media/scale-up-primary-node-type/service-fabric-explorer-node-status.png" alt-text="Service Fabric Explorer: Anzeige des Status der deaktivierten Knoten":::

Bei der Dauerhaftigkeit Silber und Gold werden einige Knoten in den Zustand „Deaktiviert“ versetzt, während andere in einem Zustand *Wird deaktiviert* verbleiben. Überprüfen Sie in Service Fabric Explorer die Registerkarte **Details** der Knoten im Zustand „Wird deaktiviert“. Wenn sie eine *Ausstehende Sicherheitsüberprüfung* der Art *EnsurePartitionQuorum* (Sicherstellen des Quorums für Infrastrukturdienstpartitionen) anzeigt, kann der Vorgang ohne Bedenken fortgesetzt werden.

:::image type="content" source="./media/scale-up-primary-node-type/service-fabric-explorer-node-status-disabling.png" alt-text="Sie können fortfahren, indem Sie Daten beenden und Knoten entfernen, die im Status „Wird deaktiviert“ verbleiben, wenn sie eine ausstehende Sicherheitsüberprüfung der Art „EnsurePartitionQuorum“ anzeigen.":::

Warten Sie für die Dauerhaftigkeit Bronze, bis alle Knoten den Zustand *Deaktiviert* erreicht haben.

### <a name="stop-data-on-the-disabled-nodes"></a>Beenden von Daten auf den deaktivierten Knoten

Nun können Sie Daten auf den deaktivierten Knoten beenden.

```powershell
# Stop data on the disabled nodes.
foreach($node in $nodes)
{
  if ($node.NodeType -eq $nodeType)
  {
    $node.NodeName

    Start-ServiceFabricNodeTransition -Stop -OperationId (New-Guid) -NodeInstanceId $node.NodeInstanceId -NodeName $node.NodeName -StopDurationInSeconds 10000
  }
}
```

## <a name="remove-the-original-node-type-and-cleanup-its-resources"></a>Entfernen des ursprünglichen Knotentyps und Bereinigen seiner Ressourcen

Sie sind bereit, den ursprünglichen Knotentyp und die zugehörigen Ressourcen zu entfernen, um die vertikale Skalierung abzuschließen.

### <a name="remove-the-original-scale-set"></a>Entfernen der ursprünglichen Skalierungsgruppe

Entfernen Sie zuerst die sichernde Skalierungsgruppe des Knotentyps.

```powershell
$scaleSetName = "nt0vm"
$scaleSetResourceType = "Microsoft.Compute/virtualMachineScaleSets"

Remove-AzResource -ResourceName $scaleSetName -ResourceType $scaleSetResourceType -ResourceGroupName $resourceGroupName -Force
```

### <a name="delete-the-original-ip-and-load-balancer-resources"></a>Löschen der ursprünglichen IP-Adresse und der Load Balancer-Ressourcen

Sie können nun die ursprüngliche IP-Adresse und die Load Balancer-Ressourcen löschen. In diesem Schritt aktualisieren Sie auch den DNS-Namen.

> [!Note]
> Dieser Schritt ist optional, wenn Sie bereits eine öffentliche IP-Adresse einer *Standard-SKU* und einen Load Balancer Standard verwenden. In diesem Fall können mehrere VM-Skalierungsgruppen/Knotentypen unter demselben Load Balancer vorhanden sein.

Führen Sie die folgenden Befehle aus, und ändern Sie den Wert `$lbname` nach Bedarf.

```powershell
# Delete the original IP and load balancer resources
$lbName = "LB-sftestupgrade-nt0vm"
$lbResourceType = "Microsoft.Network/loadBalancers"
$ipResourceType = "Microsoft.Network/publicIPAddresses"
$oldPublicIpName = "PublicIP-LB-FE-nt0vm"
$newPublicIpName = "PublicIP-LB-FE-nt1vm"

$oldPrimaryPublicIP = Get-AzPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $resourceGroupName
$primaryDNSName = $oldPrimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldPrimaryPublicIP.DnsSettings.Fqdn

Remove-AzResource -ResourceName $lbName -ResourceType $lbResourceType -ResourceGroupName $resourceGroupName -Force
Remove-AzResource -ResourceName $oldPublicIpName -ResourceType $ipResourceType -ResourceGroupName $resourceGroupName -Force

$PublicIP = Get-AzPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $resourceGroupName
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzPublicIpAddress -PublicIpAddress $PublicIP
```

### <a name="remove-node-state-from-the-original-node-type"></a>Entfernen des Knotenstatus aus dem ursprünglichen Knotentyp

Die Knoten des ursprünglichen Knotentyps zeigen nun *Fehler* als ihren **Integritätsstatus** an. Entfernen Sie ihren Knotenstatus aus dem Cluster.

```powershell
# Remove state of the obsolete nodes from the cluster
$nodeType = "nt0vm"
$nodes = Get-ServiceFabricNode

Write-Host "Removing node state..."
foreach($node in $nodes)
{
  if ($node.NodeType -eq $nodeType)
  {
    $node.NodeName

    Remove-ServiceFabricNodeState -NodeName $node.NodeName -Force
  }
}
```

Service Fabric Explorer sollte nun nur die fünf Knoten des neuen Knotentyps (nt1vm) mit Integritätsstatuswerten *OK* für alle Knoten anzeigen. Der Integritätsstatus des Clusters wird weiterhin als *Fehler* angezeigt. Sie beheben dieses Problem nun, indem Sie die Vorlage so aktualisieren, dass sie die neuesten Änderungen berücksichtigt. Stellen Sie die Vorlage dann erneut bereit.

### <a name="update-the-deployment-template-to-reflect-the-newly-scaled-up-primary-node-type"></a>Aktualisieren der Bereitstellungsvorlage, damit sie den neu skalierten primären Knotentyp berücksichtigt

Die erforderlichen Änderungen für diesen Schritt wurden bereits in der Vorlagendatei [*Step3-CleanupOriginalPrimaryNodeType.json*](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade/Step3-CleanupOriginalPrimaryNodeType.json) vorgenommen. In den folgenden Abschnitten werden diese Vorlagenänderungen ausführlich erläutert. Wenn Sie möchten, können Sie die Erklärung überspringen und mit dem [Bereitstellen der aktualisierten Vorlage](#deploy-the-finalized-template) fortfahren und das Tutorial abschließen.

#### <a name="update-the-cluster-management-endpoint"></a>Aktualisieren des Verwaltungsendpunkts des Clusters

Aktualisieren Sie den Cluster `managementEndpoint` in der Bereitstellungsvorlage, um auf die neue IP-Adresse zu verweisen (durch Aktualisieren von *vmNodeType0Name* in *vmNodeType1Name*).

```json
  "managementEndpoint": "[concat('https://',reference(concat(variables('lbIPName'),'-',variables('vmNodeType1Name'))).dnsSettings.fqdn,':',variables('nt0fabricHttpGatewayPort'))]",
```

#### <a name="remove-the-original-node-type-reference"></a>Entfernen des ursprünglichen Knotentypverweises

Entfernen Sie den ursprünglichen Knotentypverweis aus der Service Fabric-Ressource in der Bereitstellungsvorlage:

```json
"name": "[variables('vmNodeType0Name')]",
"applicationPorts": {
    "endPort": "[variables('nt0applicationEndPort')]",
    "startPort": "[variables('nt0applicationStartPort')]"
},
"clientConnectionEndpointPort": "[variables('nt0fabricTcpGatewayPort')]",
"durabilityLevel": "Bronze",
"ephemeralPorts": {
    "endPort": "[variables('nt0ephemeralEndPort')]",
    "startPort": "[variables('nt0ephemeralStartPort')]"
},
"httpGatewayEndpointPort": "[variables('nt0fabricHttpGatewayPort')]",
"isPrimary": true,
"reverseProxyEndpointPort": "[variables('nt0reverseProxyEndpointPort')]",
"vmInstanceCount": "[parameters('nt0InstanceCount')]"
```

#### <a name="configure-health-policies-to-ignore-existing-errors"></a>Konfigurieren von Integritätsrichtlinien zum Ignorieren vorhandener Fehler

Nur für Cluster mit der Dauerhaftigkeit „Silber“ und höher aktualisieren Sie die Clusterressource in der Vorlage und konfigurieren die Integritätsrichtlinien so, dass der Zustand von `fabric:/System` ignoriert wird, indem Sie *applicationDeltaHealthPolicies* unter den Clusterressourceneigenschaften wie unten angegeben hinzufügen. Die folgende Richtlinie ignoriert vorhandene Fehler, lässt aber keine neuen Integritätsfehler zu.

```json
"upgradeDescription":  
{ 
 "forceRestart": false, 
 "upgradeReplicaSetCheckTimeout": "10675199.02:48:05.4775807", 
 "healthCheckWaitDuration": "00:05:00", 
 "healthCheckStableDuration": "00:05:00", 
 "healthCheckRetryTimeout": "00:45:00", 
 "upgradeTimeout": "12:00:00", 
 "upgradeDomainTimeout": "02:00:00", 
 "healthPolicy": { 
   "maxPercentUnhealthyNodes": 100, 
   "maxPercentUnhealthyApplications": 100 
 }, 
 "deltaHealthPolicy":  
 { 
   "maxPercentDeltaUnhealthyNodes": 0, 
   "maxPercentUpgradeDomainDeltaUnhealthyNodes": 0, 
   "maxPercentDeltaUnhealthyApplications": 0, 
   "applicationDeltaHealthPolicies":  
   { 
       "fabric:/System":  
       { 
           "defaultServiceTypeDeltaHealthPolicy":  
           { 
                   "maxPercentDeltaUnhealthyServices": 0 
           } 
       } 
   } 
 } 
}
```

#### <a name="remove-supporting-resources-for-the-original-node-type"></a>Entfernen unterstützender Ressourcen für den ursprünglichen Knotentyp

Entfernen Sie alle anderen Ressourcen im Zusammenhang mit dem ursprünglichen Knotentyp aus der ARM-Vorlage und der Parameterdatei. Löschen Sie Folgendes:

```json
    "vmImagePublisher": {
      "value": "MicrosoftWindowsServer"
    },
    "vmImageOffer": {
      "value": "WindowsServer"
    },
    "vmImageSku": {
      "value": "2016-Datacenter-with-Containers"
    },
    "vmImageVersion": {
      "value": "latest"
    },
```

#### <a name="deploy-the-finalized-template"></a>Bereitstellen der fertigen Vorlage

Stellen Sie schließlich die geänderte Azure Resource Manager-Vorlage bereit.

```powershell
# Deploy the updated template file
$templateFilePath = "C:\Step3-CleanupOriginalPrimaryNodeType"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

> [!NOTE]
> Dieser Schritt dauert eine Weile (in der Regel bis zu zwei Stunden).

Durch dieses Upgrade ändern sich die Einstellungen für *InfrastructureService*. Daher ist ein Neustart des Knotens erforderlich. In diesem Fall wird *forceRestart* ignoriert. Der Parameter `upgradeReplicaSetCheckTimeout` gibt die maximale Zeit an, die Service Fabric wartet, bis sich eine Partition in einem sicheren Zustand befindet, sofern dies noch nicht der Fall ist. Sobald die Sicherheitsprüfungen für alle Partitionen eines Knotens absolviert sind, fährt Service Fabric mit dem Upgrade auf diesem Knoten fort. Der Wert für den Parameter `upgradeTimeout` kann auf 6 Stunden verkürzt werden, aber für maximale Sicherheit sollten 12 Stunden gewählt werden.

Nachdem die Bereitstellung abgeschlossen wurde, überprüfen Sie im Azure-Portal, ob der Status der Service Fabric-Ressource *Bereit* ist. Vergewissern Sie sich, dass Sie den neuen Service Fabric Explorer-Endpunkt erreichen können, dass der **Integritätsstatus des Clusters** den Wert *OK* aufweist und dass alle bereitgestellten Anwendungen ordnungsgemäß funktionieren.

Sie haben den primären Knotentyp eines Clusters vertikal skaliert!

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie [einem Cluster ein Knotentyp hinzugefügt wird](virtual-machine-scale-set-scale-node-type-scale-out.md).
* Machen Sie sich mit der [Skalierbarkeit von Anwendungen](service-fabric-concepts-scalability.md) vertraut.
* [Skalieren eines Service Fabric-Clusters](service-fabric-tutorial-scale-cluster.md) (horizontal hoch oder herunter)
* [Programmgesteuertes Skalieren eines Service Fabric-Clusters](service-fabric-cluster-programmatic-scaling.md) (per Azure Fluent-Compute-SDK)
* [Horizontales Herunter- oder Hochskalieren eines eigenständigen Clusters](service-fabric-cluster-windows-server-add-remove-nodes.md)
