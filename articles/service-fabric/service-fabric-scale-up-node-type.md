---
title: Hochskalieren eines Azure Service Fabric-Knotentyps
description: In diesem Artikel erfahren Sie, wie ein Service Fabric-Cluster durch Hinzufügen einer VM-Skalierungsgruppe skaliert wird.
ms.topic: article
ms.date: 02/13/2019
ms.openlocfilehash: a8a8a432d4a11427f2c4f545a0907540af3112bc
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056481"
---
# <a name="scale-up-a-service-fabric-cluster-primary-node-type-by-adding-a-virtual-machine-scale-set"></a>Hochskalieren eines primären Knotentyps eines Service Fabric-Clusters durch Hinzufügen einer VM-Skalierungsgruppe
Dieser Artikel beschreibt, wie Sie den primären Knotentyp eines Service Fabric-Clusters durch Erhöhen der Ressourcen des virtuellen Computers hochskalieren können. Ein Service Fabric-Cluster enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden. Ein physischer oder virtueller Computer, der Teil eines Clusters ist, wird als Knoten bezeichnet. VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Sammlung von virtuellen Computern als Gruppe bereitstellen und verwalten können. Jeder Knotentyp, der in einem Azure-Cluster definiert ist, wird [als separate Skalierungsgruppe eingerichtet](service-fabric-cluster-nodetypes.md). Jeder Knotentyp kann dann separat verwaltet werden. Nach dem Erstellen eines Service Fabric-Clusters können Sie einen Clusterknotentyp vertikal skalieren (die Ressourcen der Knoten ändern) oder das Betriebssystem der Knotentyp-VMs aktualisieren.  Sie können die Skalierung für den Cluster jederzeit durchführen – auch bei Ausführung von Workloads im Cluster.  Wenn der Cluster skaliert wird, werden Ihre Anwendungen ebenfalls automatisch skaliert.

> [!WARNING]
> Versuchen Sie nicht, eine Hochskalierungsprozedur für primäre Knoten auszuführen, wenn der Cluster einen fehlerhaften Status aufweist. Dies würde die Stabilität des Clusters nur weiter gefährden.
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="process-to-upgrade-the-size-and-operating-system-of-the-primary-node-type-vms"></a>Prozess zum Aktualisieren der Größe und des Betriebssystems der VMs des primären Knotentyps
Im Folgenden wird der Vorgang zum Aktualisieren der VM-Größe und des Betriebssystems der VMs des primären Knotentyps beschrieben.  Nach dem Upgrade weisen die VMs des primären Knotentyps die Größe „Standard D4_V2“ auf und werden unter Windows Server 2016 Datacenter mit Containern ausgeführt.

> [!WARNING]
> Bevor Sie versuchen, diesen Vorgang für einen Produktionscluster auszuführen, sollten Sie sich die Beispielvorlagen ansehen und den Vorgang mithilfe eines Testclusters überprüfen. Beachten Sie zudem, dass der Cluster zeitweilig nicht verfügbar ist. Sie können KEINE Änderungen an mehreren VMSS vornehmen, die parallel als dasselbe NodeType-Element deklariert sind. Sie müssen separate Bereitstellungsvorgänge durchführen, um Änderungen auf jede NodeType-VMSS einzeln anzuwenden.

1. Stellen Sie den Anfangscluster mit zwei Knotentypen und zwei Skalierungsgruppen (eine Skalierungsgruppe pro Knotentyp) mithilfe dieser [Beispielvorlage](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.json) und diesen [Parameterdateien](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.parameters.json) bereit.  Beide Skalierungsgruppen weisen die Größe „Standard D2_V2“ auf und werden unter Windows Server 2012 R2 Datacenter ausgeführt.  Warten Sie, bis das Baselineupgrade des Clusters abgeschlossen ist.   
2. Optional: Stellen Sie ein zustandsbehaftetes Beispiel für den Cluster bereit.
3. Nachdem Sie sich für das Upgrade der VMs des primären Knotentyps entschieden haben, können Sie dem primären Knotentyp mithilfe dieser [Beispielvorlage](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) und diesen [Parameterdateien](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) eine neue Skalierungsgruppe hinzufügen, damit der primäre Knotentyp über zwei Skalierungsgruppen verfügt.  Systemdienste und Benutzeranwendungen können zwischen VMs in den zwei unterschiedlichen Skalierungsgruppen migriert werden.  Die neuen Skalierungsgruppen-VMs weisen die Größe „Standard D4_V2“ auf und werden unter Windows Server 2016 Datacenter mit Containern ausgeführt.  Mit der neuen Skalierungsgruppe werden auch ein neuer Lastenausgleich und eine öffentliche IP-Adresse hinzugefügt.  
    Um die neue Skalierungsgruppe in der Vorlage zu finden, suchen Sie nach der Ressource „Microsoft.Compute/virtualMachineScaleSets“, die durch den Parameter *vmNodeType2Name* benannt wird.  Die neue Skalierungsgruppe wird dem primären Knotentyp mithilfe der Einstellung „properties->virtualMachineProfile->extensionProfile->extensions->properties->settings->nodeTypeRef“ hinzugefügt.
4. Überprüfen Sie die Clusterintegrität und ob alle Knoten fehlerfrei sind.
5. Deaktivieren Sie die Knoten in der alten Skalierungsgruppe des primären Knotentyps mit der Absicht, die Knoten zu entfernen. Sie können alle gleichzeitig deaktivieren. Die Vorgänge werden dann in eine Warteschlange eingereiht. Warten Sie, bis alle Knoten deaktiviert sind. Dies kann einige Zeit dauern.  Nachdem die älteren Knoten im Knotentyp deaktiviert sind, werden die Systemdienste und Seed-Knoten zu den VMs der neuen Skalierungsgruppe im primären Knotentyp migriert.
6. Entfernen Sie die ältere Skalierungsgruppe aus dem primären Knotentyp. (Nachdem die Knoten wie in Schritt 5 deaktiviert wurden, heben Sie im Blatt für die VM-Skalierungsgruppe im Azure-Portal nacheinander die Zuordnung der Knoten für den alten Knotentyp auf.)
7. Entfernen Sie den der alten Skalierungsgruppe zugeordneten Lastenausgleich. Während die neue öffentliche IP-Adresse und der Lastenausgleich für die neue Skalierungsgruppe konfiguriert werden, ist der Cluster nicht verfügbar.  
8. Speichern Sie die DNS-Einstellungen der öffentlichen IP-Adresse, die der alten Skalierungsgruppe des primären Knotentyps zugeordnet ist, in einer Variablen, und entfernen Sie diese öffentliche IP-Adresse.
9. Ersetzen Sie die DNS-Einstellungen der öffentlichen IP-Adresse, die der neuen Skalierungsgruppe des primären Knotentyps zugeordnet ist, durch die DNS-Einstellungen der gelöschten öffentlichen IP-Adresse.  Der Cluster ist jetzt wieder verfügbar.
10. Entfernen Sie den Knotenstatus der Knoten aus dem Cluster.  Wenn die alte Skalierungsgruppe die Dauerhaftigkeitsstufe „Silber“ oder „Gold“ aufwies, wird dieser Schritt automatisch vom System ausgeführt.
11. Wenn Sie die zustandsbehaftete Anwendung in einem vorherigen Schritt bereitgestellt haben, stellen Sie sicher, dass die Anwendung funktionsfähig ist.

## <a name="set-up-the-test-cluster"></a>Einrichten des Testclusters

Beginnen Sie mit dem Herunterladen der beiden Dateisätze, die für dieses Tutorial erforderlich sind: [Vorlage]() und [Parameter]() vor dem Vorgang und [Vorlage]() und [Parameter]() nach dem Vorgang.

Melden Sie sich anschließend bei Ihrem Azure-Konto an.

```powershell
# sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId "<your subscription ID>"
```

Dieses Tutorial führt Sie schrittweise durch das Szenario der Erstellung eines selbstsignierten Zertifikats. Wenn Sie ein vorhandenes Zertifikat aus Azure Key Vault verwenden möchten, überspringen Sie den folgenden Schritt und führen stattdessen die Schritte unter [Verwenden eines vorhandenen Zertifikats zum Bereitstellen des Clusters](./upgrade-managed-disks.md#use-an-existing-certificate-to-deploy-the-cluster) aus.

### <a name="generate-a-self-signed-certificate-and-deploy-the-cluster"></a>Generieren eines selbstsignierten Zertifikats und Bereitstellen des Clusters

Weisen Sie zunächst die Variablen zu, die Sie für die Bereitstellung des Service Fabric-Clusters benötigen. Passen Sie die Werte für `resourceGroupName`, `certSubjectName`, `parameterFilePath` und `templateFilePath` für ihr spezifisches Konto und Ihre Umgebung an:

```powershell
# Assign deployment variables
$resourceGroupName = "sftestupgradegroup"
$certOutputFolder = "c:\certificates"
$certPassword = "Password!1" | ConvertTo-SecureString -AsPlainText -Force
$certSubjectName = "sftestupgrade.southcentralus.cloudapp.azure.com"
$templateFilePath = "C:\Deploy-2NodeTypes-2ScaleSets.json"
$parameterFilePath = "C:\Deploy-2NodeTypes-2ScaleSets.parameters.json"
```

> [!NOTE]
> Stellen Sie sicher, dass der Speicherort `certOutputFolder` auf Ihrem lokalen Computer vorhanden ist, bevor Sie den Befehl zum Bereitstellen eines neuen Service Fabric-Clusters ausführen.

Öffnen Sie anschließend die Datei *Deploy-2NodeTypes-2ScaleSets.parameters.json*, und passen Sie die Werte für `clusterName` und `dnsName` an, sodass diese den dynamischen Werten entsprechen, die Sie in PowerShell festlegen. Speichern Sie anschließend Ihre Änderungen.

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

Bei diesem Vorgang wird der Zertifikatfingerabdruck zurückgegeben, mit dem Sie später eine Verbindung mit dem neuen Cluster herstellen und dessen Integritätsstatus überprüfen.

### <a name="connect-to-the-new-cluster-and-check-health-status"></a>Herstellen einer Verbindung zu dem neuen Cluster und Überprüfen des Integritätsstatus

Stellen Sie eine Verbindung mit dem Cluster her, und stellen Sie sicher, dass alle seine Knoten fehlerfrei sind (ersetzen Sie dazu die Variablen `clusterName` und `thumb` für Ihren Cluster):

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

Wir können nun mit dem Upgradevorgang beginnen.

## <a name="upgrade-the-primary-node-type-vms"></a>Upgraden der VMs des primären Knotentyps

Nachdem Sie sich für das Upgrade der VMs des primären Knotentyps entschieden haben, fügen Sie dem primären Knotentyp so eine neue Skalierungsgruppe hinzu, dass der primäre Knotentyp nun über zwei Skalierungsgruppen verfügt. Beispieldateien für die [Vorlage](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) und [Parameter](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) wurden bereitgestellt, um die erforderlichen Änderungen anzuzeigen. Die neuen VMs der Skalierungsgruppe weisen die Größe „Standard D4_V2“ auf und werden unter Windows Server 2016 Datacenter mit Containern ausgeführt. Mit der neuen Skalierungsgruppe werden auch ein neuer Lastenausgleich und eine öffentliche IP-Adresse hinzugefügt. 

Um die neue Skalierungsgruppe in der Vorlage zu finden, suchen Sie nach der Ressource „Microsoft.Compute/virtualMachineScaleSets“, die durch den Parameter vmNodeType2Name benannt wird. Die neue Skalierungsgruppe wird dem primären Knotentyp mithilfe der Einstellung „properties->virtualMachineProfile->extensionProfile->extensions->properties->settings->nodeTypeRef“ hinzugefügt.

### <a name="deploy-the-updated-template"></a>Bereitstellen der aktualisierten Vorlage

Passen Sie die Werte von `parameterFilePath` und `templateFilePath` nach Bedarf an, und führen Sie den folgenden Befehl aus:

```powershell
# Deploy the new scale set into the primary node type along with a new load balancer and public IP
$templateFilePath = "C:\Deploy-2NodeTypes-3ScaleSets.json"
$parameterFilePath = "C:\Deploy-2NodeTypes-3ScaleSets.parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

Überprüfen Sie nach Abschluss der Bereitstellung noch einmal die Clusterintegrität, und stellen Sie sicher, dass alle Knoten (in der ursprünglichen und in der neuen Skalierungsgruppe) fehlerfrei sind.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="migrate-nodes-to-the-new-scale-set"></a>Migrieren von Knoten zur neuen Skalierungsgruppe

Sie werden nun mit der Deaktivierung der Knoten der ursprünglichen Skalierungsgruppe beginnen. Infolge dessen werden die Knoten der Systemdienste und die Startknoten zu den VMs der neuen Skalierungsgruppe migriert, da diese ebenfalls als primärer Knotentyp gekennzeichnet ist.

Für das Hochskalieren nicht primärer Knotentypen würden Sie in diesem Schritt die Dienstplatzierungseinschränkungen so ändern, dass die neue VM-Skalierungsgruppe bzw. der neue Knotentyp enthalten ist, und dann die Instanzenanzahl der alten VM-Skalierungsgruppe auf null reduzieren (nacheinander für alle Knoten, um sicherzustellen, dass das Entfernen der Knoten die Zuverlässigkeit des Clusters nicht beeinträchtigt).

```powershell
# Disable the nodes in the original scale set.
$nodeNames = @("_NTvm1_0","_NTvm1_1","_NTvm1_2","_NTvm1_3","_NTvm1_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames){
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Verwenden Sie Service Fabric Explorer, um die Migration der Startknoten zur neuen Skalierungsgruppe sowie den Statuswechsel der Knoten in der ursprünglichen Skalierungsgruppe von *Wird deaktiviert* zu *Deaktiviert* zu überwachen.

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

## <a name="remove-the-old-load-balancer-and-update-dns-settings"></a>Entfernen des alten Load Balancers und Aktualisieren der DNS-Einstellungen

Nun können wir die Ressourcen, die mit dem alten primären Knotentyp verknüpft sind, beginnend mit dem Load Balancer und der alten öffentlichen IP-Adresse entfernen. 

```powershell
$lbname="LB-sfupgradetest-NTvm1"
$oldPublicIpName="PublicIP-LB-FE-0"
$newPublicIpName="PublicIP-LB-FE-2"

# Store DNS settings of public IP address related to old Primary NodeType into variable 
$oldprimaryPublicIP = Get-AzPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname

$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel

$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

# Remove Load Balancer related to old Primary NodeType. This will cause a brief period of downtime for the cluster
Remove-AzLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force

# Remove the old public IP
Remove-AzPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force
```

Dann aktualisieren wir die DNS-Einstellungen der neuen öffentlichen IP-Adresse, um die Einstellungen der öffentlichen IP-Adresse des alten primären Knotentyps zu spiegeln.

```powershell
# Replace DNS settings of Public IP address related to new Primary Node Type with DNS settings of Public IP address related to old Primary Node Type
$PublicIP = Get-AzPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzPublicIpAddress -PublicIpAddress $PublicIP
```

Überprüfen Sie die Integrität des Clusters erneut.

```powershell
# Check the cluster health
Get-ServiceFabricClusterHealth
```

Entfernen Sie schließlich den Knotenstatus für jeden der zugehörigen Knoten. Wenn die alte Skalierungsgruppe die Dauerhaftigkeitsstufe „Silber“ oder „Gold“ aufwies, geschieht dies automatisch.

```powershell
# Remove node state for the deleted nodes.
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}
```

Der primäre Knotentyp des Clusters wurde jetzt aktualisiert. Stellen Sie sicher, dass alle bereitgestellten Anwendungen ordnungsgemäß funktionieren und die Clusterintegrität fehlerfrei ist.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie [einem Cluster ein Knotentyp hinzugefügt wird](virtual-machine-scale-set-scale-node-type-scale-out.md).
* Machen Sie sich mit der [Skalierbarkeit von Anwendungen](service-fabric-concepts-scalability.md) vertraut.
* [Skalieren eines Service Fabric-Clusters](service-fabric-tutorial-scale-cluster.md) (horizontal hoch oder herunter)
* [Programmgesteuertes Skalieren eines Service Fabric-Clusters](service-fabric-cluster-programmatic-scaling.md) (per Azure Fluent-Compute-SDK)
* [Horizontales Herunter- oder Hochskalieren eines eigenständigen Clusters](service-fabric-cluster-windows-server-add-remove-nodes.md)
