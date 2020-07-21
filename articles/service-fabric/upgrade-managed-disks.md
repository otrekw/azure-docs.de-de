---
title: Upgraden von Clusterknoten für die Verwendung verwalteter Azure-Datenträger
description: Im diesem Artikel wird erläutert, wie Sie einen vorhandenen Service Fabric-Cluster mit geringer oder gar keiner Downtime so upgraden, dass dieser verwaltete Azure-Datenträger verwendet.
ms.topic: how-to
ms.date: 4/07/2020
ms.openlocfilehash: cff0f99412f189f38f1b14d15c7285166a048c87
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86255896"
---
# <a name="upgrade-cluster-nodes-to-use-azure-managed-disks"></a>Upgraden von Clusterknoten für die Verwendung verwalteter Azure-Datenträger

[Verwaltete Azure-Datenträger](../virtual-machines/windows/managed-disks-overview.md) sind die für die dauerhafte Datenspeicherung empfohlene Datenträger-Speicherlösung für Azure-VMs. Sie können die Resilienz Ihrer Service Fabric-Workloads verbessern, indem Sie die VM-Skalierungsgruppen upgraden, die Ihren Knotentypen zugrunde liegen, sodass diese verwaltete Datenträger verwenden. In diesem Artikel erfahren Sie, wie Sie einen vorhandenen Service Fabric-Cluster mit geringer oder gar keiner Downtime so upgraden, dass dieser verwalteten Azure-Datenträger verwendet.

Die allgemeine Vorgehensweise beim Upgraden eines Service Fabric-Clusterknotens für die Verwendung verwalteter Datenträger lautet wie folgt:

1. Stellen Sie eine mit diesem Knotentyp identische VM-Skalierungsgruppe bereit. Diese darf sich nur dadurch vom Knotentyp unterscheiden, dass das [managedDisk](/azure/templates/microsoft.compute/2019-07-01/virtualmachinescalesets/virtualmachines#ManagedDiskParameters)-Objekt im Abschnitt `osDisk` der Bereitstellungsvorlage hinzugefügt wurde. Die neue Skalierungsgruppe sollte an denselben Lastenausgleich/dieselbe IP-Adresse wie das Original gebunden werden, damit es bei Ihren Kunden während der Migration zu keinem Dienstausfall kommt.

2. Sobald die ursprüngliche und die upgegradete Skalierungsgruppe parallel ausgeführt werden, müssen Sie die ursprünglichen Knoteninstanzen nacheinander deaktivieren, damit die Systemdienste (oder die Replikate der zustandsbehafteten Dienste) zur neuen Skalierungsgruppe migriert werden.

3. Überprüfen Sie, ob der Cluster und die neuen Knoten fehlerfrei sind, und entfernen Sie dann die ursprüngliche Skalierungsgruppe und den Knotenzustand für die gelöschten Knoten.

In diesem Artikel wird beschrieben, mit welchen Schritten Sie den primären Knotentyp eines Beispielclusters upgraden, sodass dieser verwaltete Datenträger verwendet, und dabei jegliche Clusterdowntime vermeiden (siehe Hinweis unten). Der anfängliche Zustand des Beispieltestclusters besteht aus einem Knotentyp der [Dauerhaftigkeitsstufe „Silber“](service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster), der eine einzelne Skalierungsgruppe mit fünf Knoten enthält.

> [!CAUTION]
> Bei diesem Verfahren treten nur dann Ausfälle auf, wenn im Cluster-DNS Abhängigkeiten bestehen (z. B. beim Zugriff auf [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)). Zu den [bewährten Architekturmethoden für Front-End-Dienste](/azure/architecture/microservices/design/gateway) zählt das Implementieren eines [Lastenausgleichs](/azure/architecture/guide/technology-choices/load-balancing-overview), der Ihren Knotentypen vorgeschaltet ist und den Knotenaustausch ohne Dienstausfälle ermöglicht.

Unter folgendem Link finden Sie die [Vorlagen und Cmdlets](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade-no-outage) für Azure Resource Manager, mit denen Sie das Upgradeszenario abschließen. Die Vorlagenänderungen werden weiter unten im Abschnitt [Bereitstellen einer upgegradeten Skalierungsgruppe für den primären Knotentyp](#deploy-an-upgraded-scale-set-for-the-primary-node-type) erläutert.

## <a name="set-up-the-test-cluster"></a>Einrichten des Testclusters

Richten Sie zunächst Ihren Service Fabric-Testcluster ein. [Laden Sie dazu die Azure Resource Manager-Beispielvorlagen herunter](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade-no-outage), die Sie für dieses Szenario benötigen.

Melden Sie sich anschließend bei Ihrem Azure-Konto an.

```powershell
# Sign in to your Azure account
Login-AzAccount -SubscriptionId "<subscription ID>"
```

Mithilfe der folgenden Befehle können Sie ein neues selbstsigniertes Zertifikat erstellen und das Testcluster bereitstellen. Wenn Sie bereits über ein Zertifikat verfügen, das Sie verwenden möchten, können Sie direkt mit dem Abschnitt [Verwenden eines bestehenden Zertifikats für die Clusterbereitstellung](#use-an-existing-certificate-to-deploy-the-cluster) fortfahren.

### <a name="generate-a-self-signed-certificate-and-deploy-the-cluster"></a>Generieren eines selbstsignierten Zertifikats und Bereitstellen des Clusters

Weisen Sie zunächst die Variablen zu, die Sie für die Bereitstellung des Service Fabric-Clusters benötigen. Passen Sie die Werte für `resourceGroupName`, `certSubjectName`, `parameterFilePath` und `templateFilePath` für ihr spezifisches Konto und Ihre Umgebung an:

```powershell
# Assign deployment variables
$resourceGroupName = "sftestupgradegroup"
$certOutputFolder = "c:\certificates"
$certPassword = "Password!1" | ConvertTo-SecureString -AsPlainText -Force
$certSubjectName = "sftestupgrade.southcentralus.cloudapp.azure.com"
$templateFilePath = "C:\Initial-1NodeType-UnmanagedDisks.json"
$parameterFilePath = "C:\Initial-1NodeType-UnmanagedDisks.parameters.json"
```

> [!NOTE]
> Stellen Sie sicher, dass der Speicherort `certOutputFolder` auf Ihrem lokalen Computer vorhanden ist, bevor Sie den Befehl zum Bereitstellen eines neuen Service Fabric-Clusters ausführen.

Öffnen Sie anschließend die Datei [*Initial-1NodeType-UnmanagedDisks.parameters.json*](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Initial-1NodeType-UnmanagedDisks.parameters.json), und passen Sie die Werte für `clusterName` und `dnsName` an, sodass diese den dynamischen Werten entsprechen, die Sie in PowerShell festlegen. Speichern Sie anschließend Ihre Änderungen.

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

Bei diesem Vorgang wird der Zertifikatfingerabdruck zurückgegeben, mit dem Sie später [eine Verbindung zum neuen Cluster herstellen](#connect-to-the-new-cluster-and-check-health-status) und dessen Integritätsstatus überprüfen. (Sie können den folgenden Abschnitt überspringen, denn darin wird ein alternativer Ansatz für die Clusterbereitstellung beschrieben.)

### <a name="use-an-existing-certificate-to-deploy-the-cluster"></a>Verwenden eines bestehenden Zertifikats für die Clusterbereitstellung

Das Testcluster lässt sich auch mithilfe eines vorhandenen Azure Key Vault-Zertifikats bereitstellen. Dazu müssen Sie die [Verweise auf Ihren Key Vault-Tresor](#obtain-your-key-vault-references) und den Zertifikatfingerabdruck abrufen.

```powershell
# Key Vault variables
$certUrlValue = "https://sftestupgradegroup.vault.azure.net/secrets/sftestupgradegroup20200309235308/dac0e7b7f9d4414984ccaa72bfb2ea39"
$sourceVaultValue = "/subscriptions/########-####-####-####-############/resourceGroups/sftestupgradegroup/providers/Microsoft.KeyVault/vaults/sftestupgradegroup"
$thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"
```

Öffnen Sie die Datei [*Initial-1NodeType-UnmanagedDisks.parameters.json*](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Initial-1NodeType-UnmanagedDisks.parameters.json), und ändern Sie die Werte von `clusterName` und `dnsName` so, dass diese eindeutig sind.

Weisen Sie dem Cluster zuletzt einen Ressourcengruppennamen zu, und legen Sie die Werte `templateFilePath` und `parameterFilePath` Ihrer *Initial-1NodeType-UnmanagedDisks*-Datei fest:

> [!NOTE]
> Die bereitgestellte Ressourcengruppe muss bereits vorhanden sein und sich in derselben Region wie Ihr Key Vault-Tresor befinden.

```powershell
# Deploy the new scale set (upgraded to use managed disks) into the primary node type.
$resourceGroupName = "sftestupgradegroup"
$templateFilePath = "C:\Upgrade-1NodeType-2ScaleSets-ManagedDisks.json"
$parameterFilePath = "C:\Upgrade-1NodeType-2ScaleSets-ManagedDisks.parameters.json"
```

Führen Sie abschließend den folgenden Befehl aus, um den anfangs erstellten Testcluster bereitzustellen:

```powershell
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

Stellen Sie eine Verbindung zu dem Cluster her, und stellen Sie sicher, dass alle fünf dazugehörigen Knoten fehlerfrei sind (ersetzen Sie dazu die Variablen `clusterName` und `thumb` für Ihren Cluster):

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

## <a name="deploy-an-upgraded-scale-set-for-the-primary-node-type"></a>Bereitstellen einer upgegradeten Skalierungsgruppe für den primären Knotentyp

Für das Upgrade bzw. das *vertikale Skalieren* eines Knotentyps müssen Sie eine Kopie der VM-Skalierungsgruppe dieses Knotentyps bereitstellen. Diese VM-Skalierungsgruppe muss bis auf die Tatsache, dass das gewünschte Upgrade bzw. die gewünschten Änderungen sowie das entsprechende separate Subnetz und der eingehender NAT-Adresspool enthalten sind, mit der ursprünglichen Skalierungsgruppe identisch sein (einschließlich der Verweise auf dieselben `nodeTypeRef`-, `subnet`- und `loadBalancerBackendAddressPools`-Elemente). Da Sie einen primären Knotentyp upgraden, wird auch die neue Skalierungsgruppe als primär gekennzeichnet (`isPrimary: true`), genauso wie die ursprüngliche Skalierungsgruppe. (Lassen Sie diesen Schritt bei Upgrades von nicht primären Knotentypen einfach aus.)

Der Einfachheit halber wurden die Änderungen in der [Vorlagen-](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Upgrade-1NodeType-2ScaleSets-ManagedDisks.json) und der [Parameterdatei](https://github.com/erikadoyle/service-fabric-scripts-and-templates/blob/managed-disks/templates/nodetype-upgrade-no-outage/Upgrade-1NodeType-2ScaleSets-ManagedDisks.parameters.json) von *Upgrade-1NodeType-2ScaleSets-ManagedDisks* bereits vorgenommen.

In den folgenden Abschnitten werden die Vorlagenänderungen ausführlicher erläutert. Wenn Sie möchten, können Sie diese Erklärung überspringen und mit [dem nächsten Upgradeschritt](#obtain-your-key-vault-references) fortfahren.

### <a name="update-the-cluster-template-with-the-upgraded-scale-set"></a>Aktualisieren der Clustervorlage mit der upgegradeten Skalierungsgruppe

Nachfolgend finden Sie abschnittsweise die Änderungen an der ursprünglichen Bereitstellungsvorlage für das Cluster, mit der eine aktualisierte Skalierungsgruppe für den primären Knotentyp hinzugefügt wird.

#### <a name="parameters"></a>Parameter

Fügen Sie neue Parameter für den Instanznamen, die Instanzanzahl und die Größe der neuen Skalierungsgruppe hinzu. Beachten Sie, dass `vmNodeType1Name` für die neue Skalierungsgruppe spezifisch ist, während die Anzahl und die Größe mit der ursprünglichen Skalierungsgruppe identisch sind.

**Vorlagendatei**

```json
"vmNodeType1Name": {
    "type": "string",
    "defaultValue": "NTvm2",
    "maxLength": 9
},
"nt1InstanceCount": {
    "type": "int",
    "defaultValue": 5,
    "metadata": {
        "description": "Instance count for node type"
    }
},
"vmNodeType1Size": {
    "type": "string",
    "defaultValue": "Standard_D2_v2"
},
```

**Parameterdatei**

```json
"vmNodeType1Name": {
    "value": "NTvm2"
},
"nt1InstanceCount": {
    "value": 5
},
"vmNodeType1Size": {
    "value": "Standard_D2_v2"
}
```

### <a name="variables"></a>Variables

Fügen Sie im Abschnitt `variables` der Bereitstellungsvorlage einen Eintrag für den eingehenden NAT-Adresspool der neuen Skalierungsgruppe hinzu.

**Vorlagendatei**

```json
"lbNatPoolID1": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool1')]", 
```

### <a name="resources"></a>Ressourcen

Fügen Sie im Abschnitt *resources* der Bereitstellungsvorlage die neue VM-Skalierungsgruppe hinzu, und beachten Sie dabei Folgendes:

* Die neue Skalierungsgruppe verweist auf denselben Knotentyp wie die ursprüngliche:

    ```json
    "nodeTypeRef": "[parameters('vmNodeType0Name')]",
    ```

* Die neue Skalierungsgruppe verweist auf dieselbe Back-End-Adresse und dasselbe Subnetz des Lastenausgleichs (verwendet jedoch einen anderen eingehenden NAT-Pool für den Lastenausgleich):

   ```json
    "loadBalancerBackendAddressPools": [
        {
            "id": "[variables('lbPoolID0')]"
        }
    ],
    "loadBalancerInboundNatPools": [
        {
            "id": "[variables('lbNatPoolID1')]"
        }
    ],
    "subnet": {
        "id": "[variables('subnet0Ref')]"
    }
   ```

* So wie die ursprüngliche Skalierungsgruppe ist auch die neue Skalierungsgruppe als primärer Knotentyp gekennzeichnet. (Lassen Sie diese Änderung beim Upgrade eines nicht primären Knotentyps aus.)

    ```json
    "isPrimary": true,
    ```

* Im Gegensatz zur ursprünglichen Skalierungsgruppe wird die neue Skalierungsgruppe upgegradet, sodass sie verwaltete Datenträger verwendet.

    ```json
    "managedDisk": {
        "storageAccountType": "[parameters('storageAccountType')]"
    }
    ```

Wenn Sie alle Änderungen in der Vorlagen- und der Parameterdatei implementiert haben, fahren Sie mit dem nächsten Abschnitt fort, um Ihre Key Vault-Verweise abzurufen und die Updates in Ihrem Cluster bereitzustellen.

### <a name="obtain-your-key-vault-references"></a>Abrufen Ihrer Key Vault-Verweise

Zum Bereitstellen der aktualisierten Konfiguration müssen Sie zunächst mehrere Verweise auf Ihr Clusterzertifikat abrufen, das in Ihrem Key Vault-Tresor gespeichert ist. Dies lässt sich am einfachsten im Azure-Portal durchführen. Sie benötigen Folgendes:

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

Passen Sie die Werte von `parameterFilePath` und `templateFilePath` nach Bedarf an, und führen Sie den folgenden Befehl aus:

```powershell
# Deploy the new scale set (upgraded to use managed disks) into the primary node type.
$templateFilePath = "C:\Upgrade-1NodeType-2ScaleSets-ManagedDisks.json"
$parameterFilePath = "C:\Upgrade-1NodeType-2ScaleSets-ManagedDisks.parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

Überprüfen Sie nach Abschluss der Bereitstellung noch einmal die Clusterintegrität, und stellen Sie sicher, dass alle zehn Knoten (fünf in der ursprünglichen und fünf in der neuen Skalierungsgruppe) fehlerfrei sind.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="migrate-seed-nodes-to-the-new-scale-set"></a>Migrieren der Startknoten zur neuen Skalierungsgruppe

Sie werden nun mit der Deaktivierung der Knoten der ursprünglichen Skalierungsgruppe beginnen. Infolge dessen werden die Knoten der Systemdienste und die Startknoten zu den VMs der neuen Skalierungsgruppe migriert, da diese ebenfalls als primärer Knotentyp gekennzeichnet ist.

```powershell
# Disable the nodes in the original scale set.
$nodeNames = @("_NTvm1_0","_NTvm1_1","_NTvm1_2","_NTvm1_3","_NTvm1_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames){
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Verwenden Sie Service Fabric Explorer, um die Migration der Startknoten zur neuen Skalierungsgruppe sowie den Statuswechsel der Knoten in der ursprünglichen Skalierungsgruppe von *Wird deaktiviert* zu *Deaktiviert* zu überwachen.

![Service Fabric Explorer: Anzeige des Status der deaktivierten Knoten](./media/upgrade-managed-disks/service-fabric-explorer-node-status.png)

> [!NOTE]
> Die Deaktivierung aller Knoten in der ursprünglichen Skalierungsgruppe kann einige Zeit in Anspruch nehmen. Damit die Datenkonsistenz sichergestellt ist, können Startknoten nur nacheinander deaktiviert werden. Jede Deaktivierung eines Startknotens erfordert ein Clusterupdate. Das Ersetzen eines Startknotens erfordert daher zwei Clusterupgrades (eines für das Hinzufügen und eines für das Entfernen von Knoten). Das Upgraden der fünf Startknoten in diesem Beispielszenario ergibt zehn Clusterupgrades.

## <a name="remove-the-original-scale-set"></a>Entfernen der ursprünglichen Skalierungsgruppe

Nachdem die Deaktivierung abgeschlossen ist, müssen Sie die Skalierungsgruppe entfernen.

```powershell
# Remove the original scale set
$scaleSetName = "NTvm1"

Remove-AzVmss `
    -ResourceGroupName $resourceGroupName `
    -VMScaleSetName $scaleSetName `
    -Force

Write-Host "Removed scale set $scaleSetName"
```

In Service Fabric Explorer werden die entfernten Knoten (und damit auch der *Integritätszustand des Clusters*) nun mit dem Status *Fehler* angezeigt.

![Service Fabric Explorer: Anzeige der deaktivierten Knoten mit einem Fehlerstatus](./media/upgrade-managed-disks/service-fabric-explorer-disabled-nodes-error-state.png)

Entfernen Sie die veralteten Knoten aus dem Service Fabric-Cluster, um den Clusterintegritätszustand *OK* wiederherzustellen.

```powershell
# Remove node states for the deleted scale set
foreach($name in $nodeNames){
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}
```

![Service Fabric Explorer: Deaktivierte Knoten mit Fehlerstatus entfernt](./media/upgrade-managed-disks/service-fabric-explorer-healthy-cluster.png)

## <a name="next-steps"></a>Nächste Schritte

In dieser exemplarischen Vorgehensweise haben Sie gelernt, wie Sie die VM-Skalierungsgruppen eines Service Fabric-Clusters so upgraden, dass diese verwaltete Datenträger verwenden, und gleichzeitig Dienstausfälle vermieden. Weitere Informationen zu ähnlichen Themen finden Sie in den folgenden Ressourcen.

In diesem Artikel werden folgende Themen erläutert:

* [Hochskalieren des primären Knotentyps eines Service Fabric-Clusters](service-fabric-scale-up-node-type.md)

* [Konvertieren einer Skalierungsgruppenvorlage in eine Skalierungsgruppenvorlage für verwaltete Datenträger](../virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md.md)

* [Entfernen eines Knotentyps in Service Fabric](service-fabric-how-to-remove-node-type.md)

Weitere Informationen:

* [Beispiel: Upgraden von Clusterknoten für die Verwendung verwalteter Azure-Datenträger](https://github.com/microsoft/service-fabric-scripts-and-templates/tree/master/templates/nodetype-upgrade-no-outage)

* [Überlegungen zur vertikalen Skalierung](service-fabric-best-practices-capacity-scaling.md#vertical-scaling-considerations)
