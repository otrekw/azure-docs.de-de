---
title: Hochskalieren eines primären Azure Service Fabric-Knotentyps
description: In diesem Artikel erfahren Sie, wie ein Service Fabric-Cluster durch Hinzufügen eines Knotentyps skaliert wird.
ms.topic: article
ms.date: 08/06/2020
ms.author: pepogors
ms.openlocfilehash: b34f3f77dab6c4dcd8b7653f552c32a669d257c9
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88854625"
---
# <a name="scale-up-a-service-fabric-cluster-primary-node-type-by-adding-a-node-type"></a>Hochskalieren eines primären Knotentyps eines Service Fabric-Clusters durch Hinzufügen eines Knotentyps
Dieser Artikel beschreibt, wie Sie den primären Knotentyp eines Service Fabric-Clusters durch Hinzugefügt eine zusätzlicher Knotentyps zum Cluster hochskalieren können. Ein Service Fabric-Cluster enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden. Ein physischer oder virtueller Computer, der Teil eines Clusters ist, wird als Knoten bezeichnet. VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Sammlung von virtuellen Computern als Gruppe bereitstellen und verwalten können. Jeder Knotentyp, der in einem Azure-Cluster definiert ist, wird [als separate Skalierungsgruppe eingerichtet](service-fabric-cluster-nodetypes.md). Jeder Knotentyp kann dann separat verwaltet werden.

Die Beispielvorlagen des folgenden Tutorials finden Sie hier: [Beispiel für die Skalierung mit einem primären Service Fabric-Knotentyp](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/Primary-NodeType-Scaling-Sample)

> [!WARNING]
> Versuchen Sie nicht, eine Hochskalierungsprozedur für primäre Knoten auszuführen, wenn der Cluster einen fehlerhaften Status aufweist. Dies würde die Stabilität des Clusters nur weiter gefährden.
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="process-to-upgrade-the-size-and-operating-system-of-the-primary-node-type"></a>Prozess für ein Upgrade der Größe und des Betriebssystems des primären Knotentyps
Im Folgenden wird der Vorgang zum Aktualisieren der VM-Größe und des Betriebssystems von VMs des primären Knotentyps beschrieben.  Nach dem Upgrade weisen die VMs des primären Knotentyps die Größe „Standard D4_V2“ auf und werden unter Windows Server 2019 Datacenter mit Containern ausgeführt.

> [!WARNING]
> Bevor Sie versuchen, diesen Vorgang für einen Produktionscluster auszuführen, sollten Sie sich die Beispielvorlagen ansehen und den Vorgang mithilfe eines Testclusters überprüfen. Der Cluster ist möglicherweise auch für kurze Zeit nicht verfügbar. 

### <a name="deploy-the-initial-service-fabric-cluster"></a>Bereitstellen des anfänglichen Service Fabric-Clusters 
Wenn Sie das Beispiel parallel absolvieren möchten, stellen Sie den anfänglichen Cluster mit einem einzelnen primären Knotentyp und einer einzelnen Skalierungsgruppe [Service Fabric – anfänglicher Cluster](https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/Primary-NodeType-Scaling-Sample/AzureDeploy-1.json) bereit. Sie können diesen Schritt überspringen, wenn Sie bereits einen vorhandenen Service Fabric-Cluster bereitgestellt haben. 

1. Melden Sie sich bei Ihrem Azure-Konto an. 
```powershell
# sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId "<your subscription ID>"
```
2. Erstellen Sie eine neue Ressourcengruppe. 
```powershell
# create a resource group for your cluster deployment
$resourceGroupName = "myResourceGroup"
$location = "WestUS"

New-AzResourceGroup `
    -Name $resourceGroupName `
    -Location $location
```
3. Tragen Sie die Parameterwerte in die Vorlagendateien ein. 
4. Stellen Sie den Cluster in der in Schritt 2 erstellten Ressourcengruppe bereit. 
```powershell
# deploy the template files to the resource group created above
$templateFilePath = "C:\AzureDeploy-1.json"
$parameterFilePath = "C:\AzureDeploy.Parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath
```

### <a name="add-a-new-primary-node-type-to-the-cluster"></a>Hinzufügen eines neuen primären Knotentyps zum Cluster
> [!Note]
> Die in den folgenden Schritten erstellten Ressourcen werden zu dem neuen primären Knotentyp in Ihrem Cluster, sobald der Skalierungsvorgang beendet ist. Stellen Sie sicher, dass Sie Namen verwenden, die für das anfängliche Subnetz, die öffentliche IP-Adresse, den Load Balancer, die VM-Skalierungsgruppe und den Knotentyp eindeutig sind. 

Eine Vorlage mit allen folgenden Schritten finden Sie hier: [Service Fabric – Neuer Knotentypcluster](https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/Primary-NodeType-Scaling-Sample/AzureDeploy-2.json). Die folgenden Schritte enthalten partielle Ressourcenausschnitte, die die Änderungen in den neuen Ressourcen hervorheben.  

1. Erstellen Sie ein neues Subnetz in Ihrem vorhandenen virtuellen Netzwerk.
```json
{
    "name": "[variables('subnet1Name')]",
    "properties": {
        "addressPrefix": "[variables('subnet1Prefix')]"
    }
}
```
2. Erstellen Sie eine neue öffentliche IP-Ressource mit einem eindeutigen domainNameLabel. 
```json
{
    "apiVersion": "[variables('publicIPApiVersion')]",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat(variables('lbIPName'),'-',variables('vmNodeType1Name'))]",
    "location": "[variables('computeLocation')]",
    "properties": {
    "dnsSettings": {
        "domainNameLabel": "[concat(variables('dnsName'),'-','nt2')]"
    },
    "publicIPAllocationMethod": "Dynamic"
    },
    "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
    }
}
```
3. Erstellen Sie eine neue Load Balancer-Ressource, die von der oben erstellten öffentlichen IP-Adresse abhängt. 
```json
"dependsOn": [
    "[concat('Microsoft.Network/publicIPAddresses/',concat(variables('lbIPName'),'-',variables('vmNodeType1Name')))]"
]
```
4. Erstellen Sie eine neue VM-Skalierungsgruppe, die die neue VM-SKU und Betriebssystem-SKU verwendet, auf die Sie hochskalieren möchten. 

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
5. Fügen Sie dem Cluster einen neuen Knotentyp hinzu, der auf die oben erstellte VM-Skalierungsgruppe verweist. Die **isPrimary**-Eigenschaft für diesen Knotentyp sollte auf „true“ festgelegt werden. 
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
6. Stellen Sie die aktualisierte ARM-Vorlage bereit. 
```powershell
# deploy the updated template files to the existing resource group
$templateFilePath = "C:\AzureDeploy-2.json"
$parameterFilePath = "C:\AzureDeploy.Parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
```

Der Service Fabric-Cluster verfügt jetzt über zwei Knotentypen, wenn die Bereitstellung abgeschlossen ist. 

### <a name="remove-the-existing-node-type"></a>Entfernen des vorhandenen Knotentyps 
Nachdem die Ressourcen die Bereitstellung abgeschlossen haben, können Sie beginnen, die Knoten im ursprünglichen primären Knotentyp zu deaktivieren. Wenn die Knoten deaktiviert sind, migrieren die Systemdienste zum neuen primären Knotentyp, der im obigen Schritt bereitgestellt wurde.

1. Legen Sie primäre Knotentypeigenschaft in der Service Fabric-Clusterressource auf „false“ fest. 
```json
{
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
    "isPrimary": false,
    "reverseProxyEndpointPort": "[variables('nt0reverseProxyEndpointPort')]",
    "vmInstanceCount": "[parameters('nt0InstanceCount')]"
}
```
2. Stellen Sie die Vorlage mit der aktualisierten IsPrimary-Eigenschaft für den ursprünglichen Knotentyp bereit. Sie finden eine Vorlage, bei der das primäre Flag für den ursprünglichen Knotentyp auf „false“ festgelegt ist hier: [Service Fabric – Primärer Knotentyp „False“](https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/Primary-NodeType-Scaling-Sample/AzureDeploy-3.json).

```powershell
# deploy the updated template files to the existing resource group
$templateFilePath = "C:\AzureDeploy-3.json"
$parameterFilePath = "C:\AzureDeploy.Parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
```

3. Deaktivieren Sie die Knoten im Knotentyp 0. 
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterConnectionEndpoint `
    -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Write-Host "Connected to cluster"


$nodeType = "nt1vm" # specify the name of node type
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
* Warten Sie für die Dauerhaftigkeit „Bronze“, bis alle Knoten im Zustand „Deaktiviert“ sind.
* Für die Dauerhaftigkeit „Silber“ und „Gold“ wechseln einige Knoten in den Zustand „Deaktiviert“. Der Rest wechselt in den Zustand „Wird deaktiviert“. Überprüfen Sie die Registerkarte „Details“ der Knoten im deaktivierten Zustand. Wenn alle Knoten bei der Sicherstellung des Quorums für die Infrastrukturdienstpartitionen hängen bleiben, können Sie sicher fortfahren.

> [!Note]
> Dieser Schritt kann eine Weile dauern. 

4. Beenden Sie Daten im Knotentyp 0. 
```powershell
foreach($node in $nodes)
{
  if ($node.NodeType -eq $nodeType)
  {
    $node.NodeName

    Start-ServiceFabricNodeTransition -Stop -OperationId (New-Guid) -NodeInstanceId $node.NodeInstanceId -NodeName $node.NodeName -StopDurationInSeconds 10000
  }
}
```
5. Aufheben der Zuordnung von Knoten in der ursprünglichen VM-Skalierungsgruppe 
```powershell
$scaleSetName="nt1vm"
$scaleSetResourceType="Microsoft.Compute/virtualMachineScaleSets"

Remove-AzResource -ResourceName $scaleSetName -ResourceType $scaleSetResourceType -ResourceGroupName $resourceGroupName -Force
```
> [!Note]
> Die Schritte 6 und 7 sind optional, wenn Sie bereits eine öffentliche IP-Adresse einer Standard-SKU und einen Standard-SKU Load Balancer verwenden. In diesem Fall könnten Sie mehrere VM-Skalierungsgruppen/Knotentypen unter demselben Lastenausgleich haben. 

6. Sie können jetzt die ursprüngliche IP-Adresse und die Load Balancer-Ressourcen löschen. In diesem Schritt aktualisieren Sie auch den DNS-Namen. 

```powershell
$lbname="LB-cluster-name-nt1vm"
$lbResourceType="Microsoft.Network/loadBalancers"
$ipResourceType="Microsoft.Network/publicIPAddresses"
$oldPublicIpName="PublicIP-LB-FE-nt1vm"
$newPublicIpName="PublicIP-LB-FE-nt2vm"

$oldprimaryPublicIP = Get-AzPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $resourceGroupName
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

Remove-AzResource -ResourceName $lbname -ResourceType $lbResourceType -ResourceGroupName $resourceGroupName -Force
Remove-AzResource -ResourceName $oldPublicIpName -ResourceType $ipResourceType -ResourceGroupName $resourceGroupName -Force

$PublicIP = Get-AzPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $resourceGroupName
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzPublicIpAddress -PublicIpAddress $PublicIP
``` 

7. Aktualisieren Sie den Verwaltungsendpunkt im Cluster, sodass er auf die neue IP-Adresse verweist. 
```json
  "managementEndpoint": "[concat('https://',reference(concat(variables('lbIPName'),'-',variables('vmNodeType1Name'))).dnsSettings.fqdn,':',variables('nt0fabricHttpGatewayPort'))]",
```
8. Entfernen Sie den Knotenstatus aus dem Knotentyp 0.
```powershell
foreach($node in $nodes)
{
  if ($node.NodeType -eq $nodeType)
  {
    $node.NodeName

    Remove-ServiceFabricNodeState -NodeName $node.NodeName -Force
  }
}
```
9. Entfernen Sie den ursprünglichen Knotentypverweis aus der Service Fabric-Ressource in der ARM-Vorlage. 
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
Nur für Cluster mit der Dauerhaftigkeit „Silber“ und höher aktualisieren Sie die Clusterressource in der Vorlage und konfigurieren die Integritätsrichtlinien so, dass der Zustand von „fabric:/Systemanwendungen“ ignoriert wird, indem Sie „applicationDeltaHealthPolicies“ unter den Clusterressourceneigenschaften wie unten angegeben hinzufügen. Die folgende Richtlinie sollte bestehende Fehler ignorieren, aber keine neuen Integritätsfehler zulassen.
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
10. Entfernen Sie alle anderen Ressourcen im Zusammenhang mit dem ursprünglichen Knotentyp aus der ARM-Vorlage. Eine Vorlage, aus der alle diese ursprünglichen Ressourcen entfernt sind, finden Sie unter [Service Fabric – Neuer Knotentypcluster](https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/Primary-NodeType-Scaling-Sample/AzureDeploy-4.json).

11. Stellen Sie die geänderte Azure Resource Manager-Vorlage bereit. ** Dieser Schritt dauert eine Weile, in der Regel bis zu zwei Stunden. Durch dieses Upgrade ändern sich die Einstellungen für InfrastructureService. Daher ist ein Neustart des Knotens erforderlich. In diesem Fall wird „forceRestart“ ignoriert. Der Parameter „upgradeReplicaSetCheckTimeout“ gibt die maximale Zeit an, die Service Fabric wartet, bis sich eine Partition in einem sicheren Zustand befindet, sofern dies noch nicht der Fall ist. Sobald die Sicherheitsprüfungen für alle Partitionen eines Knotens absolviert sind, fährt Service Fabric mit dem Upgrade auf diesem Knoten fort. Der Wert für den Parameter „upgradeTimeout“ kann auf 6 Stunden verkürzt werden, aber für maximale Sicherheit sollten 12 Stunden gewählt werden.
Überprüfen Sie dann, ob die Service Fabric-Ressource im Portal als „bereit“ angezeigt wird. 

```powershell
# deploy the updated template files to the existing resource group
$templateFilePath = "C:\AzureDeploy-4.json"
$parameterFilePath = "C:\AzureDeploy.Parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
```

Der primäre Knotentyp des Clusters wurde jetzt aktualisiert. Stellen Sie sicher, dass alle bereitgestellten Anwendungen ordnungsgemäß funktionieren und die Clusterintegrität fehlerfrei ist.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie [einem Cluster ein Knotentyp hinzugefügt wird](virtual-machine-scale-set-scale-node-type-scale-out.md).
* Machen Sie sich mit der [Skalierbarkeit von Anwendungen](service-fabric-concepts-scalability.md) vertraut.
* [Skalieren eines Service Fabric-Clusters](service-fabric-tutorial-scale-cluster.md) (horizontal hoch oder herunter)
* [Programmgesteuertes Skalieren eines Service Fabric-Clusters](service-fabric-cluster-programmatic-scaling.md) (per Azure Fluent-Compute-SDK)
* [Horizontales Herunter- oder Hochskalieren eines eigenständigen Clusters](service-fabric-cluster-windows-server-add-remove-nodes.md)
