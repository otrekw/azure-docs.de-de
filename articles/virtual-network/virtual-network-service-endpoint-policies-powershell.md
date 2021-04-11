---
title: 'Einschränken der Datenexfiltration in Azure Storage: Azure PowerShell'
description: In diesem Artikel erfahren Sie, wie Sie mithilfe von Azure PowerShell die Exfiltration von Daten virtueller Netzwerke in Azure-Speicherressourcen mit VNET-Dienstendpunktrichtlinien einschränken können.
services: virtual-network
documentationcenter: virtual-network
author: RDhillon
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: rdhillon
ms.custom: ''
ms.openlocfilehash: 3ff0ee79413640c09132b8399a3b0c07dd1783cc
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106063694"
---
# <a name="manage-data-exfiltration-to-azure-storage-accounts-with-virtual-network-service-endpoint-policies-using-azure-powershell"></a>Verwalten der Datenexfiltration in Azure Storage-Konten mit VNET-Dienstendpunktrichtlinien mithilfe von Azure PowerShell

Mithilfe von VNET-Dienstendpunktrichtlinien können Sie Zugriffssteuerung auf Azure Storage-Konten innerhalb eines virtuellen Netzwerks über Dienstendpunkte anwenden. Dies ist ein wichtiger Aspekt zum Sichern von Workloads und zum Verwalten der zulässigen Speicherkonten sowie der Situationen mit zulässiger Datenexfiltration.
In diesem Artikel werden folgende Vorgehensweisen behandelt:

* Erstellen Sie ein virtuelles Netzwerk.
* Fügen Sie ein Subnetz hinzu, und aktivieren Sie einen Dienstendpunkt für Azure Storage.
* Erstellen Sie zwei Azure Storage-Konten, und lassen Sie Netzwerkzugriff auf diese Konten aus dem oben erstellten Subnetz zu.
* Erstellen einer Dienstendpunkt-Richtlinie zum Zulassen des Zugriffs nur auf eines der Speicherkonten
* Bereitstellen eines virtuellen Computers im Subnetz
* Bestätigen des Zugriffs auf das zulässige Speicherkonto aus dem Subnetz
* Bestätigen der Verweigerung des Zugriffs auf das unzulässige Speicherkonto aus dem Subnetz

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 1.0.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Vor der Erstellung eines virtuellen Netzwerks müssen Sie eine Ressourcengruppe für das virtuelle Netzwerk und alle anderen in diesem Artikel erstellten Ressourcen erstellen. Erstellen Sie mit [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Ressourcengruppe. Im folgenden Beispiel wird die Ressourcengruppe *myResourceGroup* erstellt: 

```azurepowershell-interactive
New-AzResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS
```

Erstellen Sie mit [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Netzwerk namens *myVirtualNetwork* mit dem Adresspräfix *10.0.0.0/16* erstellt.

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

## <a name="enable-a-service-endpoint"></a>Aktivieren eines Dienstendpunkts

Erstellen Sie ein Subnetz im virtuellen Netzwerk. In diesem Beispiel wird das Subnetz *Private* mit einem Dienstendpunkt für *Microsoft.Storage* erstellt: 

```azurepowershell-interactive
$subnetConfigPrivate = Add-AzVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork `
  -ServiceEndpoint Microsoft.Storage

$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="restrict-network-access-for-the-subnet"></a>Einschränken des Netzwerkzugriffs für das Subnetz

Erstellen Sie mit [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) Netzwerksicherheitsgruppen-Sicherheitsregeln. Die folgende Regel erlaubt den ausgehenden Zugriff auf die öffentlichen IP-Adressen, die dem Azure Storage-Dienst zugewiesen sind: 

```azurepowershell-interactive
$rule1 = New-AzNetworkSecurityRuleConfig `
  -Name Allow-Storage-All `
  -Access Allow `
  -DestinationAddressPrefix Storage `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 100 -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

Die folgende Regel verweigert den Zugriff auf alle öffentlichen IP-Adressen. Die vorherige Regel überschreibt diese Regel. Dies ist auf die höhere Priorität zurückzuführen, die den Zugriff auf die öffentlichen IP-Adressen von Azure Storage zulässt.

```azurepowershell-interactive
$rule2 = New-AzNetworkSecurityRuleConfig `
  -Name Deny-Internet-All `
  -Access Deny `
  -DestinationAddressPrefix Internet `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 110 -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

Die folgende Regel erlaubt eingehenden RDP-Datenverkehr (Remote Desktop Protocol) in das Subnetz von überall aus. Remotedesktopverbindungen sind im Subnetz zulässig, sodass Sie den Netzwerkzugriff auf eine Ressource in einem späteren Schritt bestätigen können.

```azurepowershell-interactive
$rule3 = New-AzNetworkSecurityRuleConfig `
  -Name Allow-RDP-All `
  -Access Allow `
  -DestinationAddressPrefix VirtualNetwork `
  -DestinationPortRange 3389 `
  -Direction Inbound `
  -Priority 120 `
  -Protocol * `
  -SourceAddressPrefix * `
  -SourcePortRange *
```

Erstellen Sie mit [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) eine Netzwerksicherheitsgruppe. Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens *myNsgPrivate* erstellt.

```azurepowershell-interactive
$nsg = New-AzNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myNsgPrivate `
  -SecurityRules $rule1,$rule2,$rule3
```

Ordnen Sie die Netzwerksicherheitsgruppe mit [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) dem Subnetz *Private* hinzu, und schreiben Sie dann die Subnetzkonfiguration in das virtuelle Netzwerk. Im folgenden Beispiel wird die Netzwerksicherheitsgruppe *myNsgPrivate* dem Subnetz *Private* hinzugefügt:

```azurepowershell-interactive
Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.0.0/24 `
  -ServiceEndpoint Microsoft.Storage `
  -NetworkSecurityGroup $nsg

$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="restrict-network-access-to-azure-storage-accounts"></a>Einschränken des Netzwerkzugriffs auf Azure Storage-Konten

Die Schritte, die erforderlich sind, um den Netzwerkzugriff auf Ressourcen einzuschränken, die durch Azure-Dienste erstellt und für Dienstendpunkte aktiviert wurden, sind je nach Dienst unterschiedlich. Informationen zu den Schritten für einzelne Dienste finden Sie in der Dokumentation des jeweiligen Diensts. Der Rest dieses Artikels enthält als Beispiel Schritte zum Einschränken des Netzwerkzugriffs für ein Azure Storage-Konto.

### <a name="create-two-storage-accounts"></a>Erstellen von zwei Speicherkonten

Erstellen Sie mit [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) ein Azure Storage-Konto.

```azurepowershell-interactive
$storageAcctName1 = 'allowedaccount'

New-AzStorageAccount `
  -Location EastUS `
  -Name $storageAcctName1 `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Rufen Sie nach dem Erstellen des Speicherkontos mit [Get-AzStorageAccountKey](/powershell/module/az.storage/get-azstorageaccountkey) den Schlüssel für das Speicherkonto in eine Variable ab:

```azurepowershell-interactive
$storageAcctKey1 = (Get-AzStorageAccountKey -ResourceGroupName myResourceGroup -AccountName $storageAcctName1).Value[0]
```

Mit dem Schlüssel wird in einem späteren Schritt eine Dateifreigabe erstellt. Geben Sie `$storageAcctKey` ein, und notieren Sie den Wert. Diesen müssen Sie in einem späteren Schritt auch manuell eingeben, wenn Sie die Dateifreigabe einem Laufwerk in einem virtuellen Computer zuordnen.

Wiederholen Sie nun die oben aufgeführten Schritte, um ein zweites Speicherkonto zu erstellen.

```azurepowershell-interactive
$storageAcctName2 = 'notallowedaccount'

New-AzStorageAccount `
  -Location EastUS `
  -Name $storageAcctName2 `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Rufen Sie auch den Speicherkontoschlüssel aus diesem Konto ab, um ihn später zum Erstellen einer Dateifreigabe zu verwenden.

```azurepowershell-interactive
$storageAcctKey2 = (Get-AzStorageAccountKey -ResourceGroupName myResourceGroup -AccountName $storageAcctName2).Value[0]
```

### <a name="create-a-file-share-in-each-of-the-storage-account"></a>Erstellen einer Dateifreigabe in beiden Speicherkonten

Erstellen Sie mit [New-AzStorageContext](/powershell/module/az.storage/new-AzStoragecontext) einen Kontext für das Speicherkonto und den Speicherschlüssel. Der Kontext kapselt den Speicherkontonamen und den Kontoschlüssel:

```azurepowershell-interactive
$storageContext1 = New-AzStorageContext $storageAcctName1 $storageAcctKey1

$storageContext2 = New-AzStorageContext $storageAcctName2 $storageAcctKey2
```

Erstellen Sie mit [New-AzStorageShare](/powershell/module/az.storage/new-azstorageshare) eine Dateifreigabe:

```azurepowershell-interactive
$share1 = New-AzStorageShare my-file-share -Context $storageContext1

$share2 = New-AzStorageShare my-file-share -Context $storageContext2
```

### <a name="deny-all-network-access-to-a-storage-accounts"></a>Verweigern des gesamten Netzwerkzugriffs auf ein Speicherkonto

Standardmäßig akzeptieren Speicherkonten Netzwerkverbindungen von Clients in allen Netzwerken. Um den Zugriff auf ausgewählte Netzwerke einzuschränken, ändern Sie die Standardaktion mit [Update-AzStorageAccountNetworkRuleSet](/powershell/module/az.storage/update-azstorageaccountnetworkruleset) in *Deny* (Verweigern). Nachdem der Netzwerkzugriff verweigert wurde, kann auf das Speicherkonto aus keinem Netzwerk mehr zugegriffen werden.

```azurepowershell-interactive
Update-AzStorageAccountNetworkRuleSet `
  -ResourceGroupName myresourcegroup `
  -Name $storageAcctName1 `
  -DefaultAction Deny

Update-AzStorageAccountNetworkRuleSet  `
  -ResourceGroupName myresourcegroup `
  -Name $storageAcctName2 `
  -DefaultAction Deny
```

### <a name="enable-network-access-only-from-the-vnet-subnet"></a>Aktivieren des Netzwerkzugriffs ausschließlich aus dem VNET-Subnetz

Rufen Sie mit [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) das erstellte virtuelle Netzwerk und dann mit [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) das Subnetzobjekt „Private“ in eine Variable ab:

```azurepowershell-interactive
$privateSubnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Name myVirtualNetwork `
  | Get-AzVirtualNetworkSubnetConfig -Name Private
```

Erlauben Sie den Netzwerkzugriff auf das Speicherkonto aus dem Subnetz *Private* mit [Add-AzStorageAccountNetworkRule](/powershell/module/az.network/add-aznetworksecurityruleconfig).

```azurepowershell-interactive
Add-AzStorageAccountNetworkRule `
  -ResourceGroupName myresourcegroup `
  -Name $storageAcctName1 `
  -VirtualNetworkResourceId $privateSubnet.Id

Add-AzStorageAccountNetworkRule `
  -ResourceGroupName myresourcegroup `
  -Name $storageAcctName2 `
  -VirtualNetworkResourceId $privateSubnet.Id
```

## <a name="apply-policy-to-allow-access-to-valid-storage-account"></a>Anwenden der Richtlinie, um den Zugriff auf ein gültiges Speicherkonto zuzulassen

Um sicherzustellen, dass die Benutzer im virtuellen Netzwerk nur auf sichere und zulässige Azure Storage-Konten zugreifen können, können Sie eine Dienstendpunktrichtlinie mit der Liste der zulässigen Speicherkonten in der Definition erstellen. Diese Richtlinie wird dann auf das VNET-Subnetz angewendet, das über Dienstendpunkte mit dem Speicher verbunden ist.

### <a name="create-a-service-endpoint-policy"></a>Erstellen einer Dienstendpunktrichtlinie

In diesem Abschnitt wird die Richtliniendefinition mit der Liste der zulässigen Ressourcen für Zugriff über den Dienstendpunkt erstellt.

Abrufen der Ressourcen-ID für das erste (zulässige) Speicherkonto 

```azurepowershell-interactive
$resourceId = (Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name $storageAcctName1).id
```

Erstellen der Richtliniendefinition zum Zulassen der oben genannten Ressource

```azurepowershell-interactive
$policyDefinition = New-AzServiceEndpointPolicyDefinition -Name mypolicydefinition `
  -Description "Service Endpoint Policy Definition" `
  -Service "Microsoft.Storage" `
  -ServiceResource $resourceId
```

Erstellen der Dienstendpunktrichtlinie mithilfe der oben erstellten Richtliniendefinition

```azurepowershell-interactive
$sepolicy = New-AzServiceEndpointPolicy -ResourceGroupName myresourcegroup `
  -Name mysepolicy -Location EastUS
  -ServiceEndpointPolicyDefinition $policyDefinition
```

### <a name="associate-the-service-endpoint-policy-to-the-virtual-network-subnet"></a>Zuordnen der Dienstendpunktrichtlinie zum Subnetz des virtuellen Netzwerks

Nachdem Sie die Dienstendpunktrichtlinie erstellt haben, verknüpfen Sie sie mit dem Zielsubnetz mit der Dienstendpunktkonfiguration für Azure Storage.

```azurepowershell-interactive
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.0.0/24 `
  -NetworkSecurityGroup $nsg `
  -ServiceEndpoint Microsoft.Storage `
  -ServiceEndpointPolicy $sepolicy

$virtualNetwork | Set-AzVirtualNetwork
```
## <a name="validate-access-restriction-to-azure-storage-accounts"></a>Überprüfen der Zugriffsbeschränkung auf Azure Storage-Konten

### <a name="deploy-the-virtual-machine"></a>Bereitstellen des virtuellen Computers

Zum Testen des Netzwerkzugriffs auf ein Speicherkonto stellen Sie einen virtuellen Computer im Subnetz bereit.

Erstellen Sie mit *New-AzVM* einen virtuellen Computer im Subnetz [Privat](/powershell/module/az.compute/new-azvm). Wenn Sie den folgenden Befehl ausführen, werden Sie aufgefordert, Anmeldeinformationen einzugeben. Die eingegebenen Werte werden als Benutzername und Kennwort für den virtuellen Computer konfiguriert. Mit der Option `-AsJob` wird der virtuelle Computer im Hintergrund erstellt, sodass Sie mit dem nächsten Schritt fortfahren können.

```azurepowershell-interactive
New-AzVm -ResourceGroupName myresourcegroup `
  -Location "East US" `
  -VirtualNetworkName myVirtualNetwork `
  -SubnetName Private `
  -Name "myVMPrivate" -AsJob
```

Die zurückgegebene Ausgabe ähnelt der folgenden Beispielausgabe:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM
```

### <a name="confirm-access-to-the-allowed-storage-account"></a>Bestätigen des Zugriffs auf das *zulässige* Speicherkonto

Geben Sie mit [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) die öffentliche IP-Adresse eines virtuellen Computers zurück. Das folgende Beispiel gibt die öffentliche IP-Adresse der *myVmPrivate*-VM zurück:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Ersetzen Sie im folgenden Befehl `<publicIpAddress>` durch die vom vorherigen Befehl zurückgegebene öffentliche IP-Adresse, und geben Sie dann den folgenden Befehl ein:

```powershell
mstsc /v:<publicIpAddress>
```

Eine RDP-Datei (Remotedesktopprotokoll) wird erstellt und auf Ihren Computer heruntergeladen. Öffnen Sie die heruntergeladene RDP-Datei. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben. Unter Umständen müssen Sie auf **Weitere Optionen** und anschließend auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben. Klicken Sie auf **OK**. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn eine Warnung angezeigt wird, klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.

Ordnen Sie auf dem virtuellen Computer *myVmPrivate* mithilfe von PowerShell die Azure-Dateifreigabe aus dem zulässigen Speicherkonto dem Laufwerk „Z:“ zu. 

```powershell
$acctKey = ConvertTo-SecureString -String $storageAcctKey1 -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList ("Azure\allowedaccount"), $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\allowedaccount.file.core.windows.net\my-file-share" -Credential $credential
```

PowerShell gibt eine Ausgabe ähnlich der folgenden Beispielausgabe zurück:

```powershell
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
Z                                      FileSystem    \\allowedaccount.file.core.windows.net\my-f...
```

Die Azure-Dateifreigabe wurde dem Laufwerk Z erfolgreich zugeordnet.

Schließen Sie die Remotedesktopsitzung mit der VM *myVmPrivate*.

### <a name="confirm-access-is-denied-to-non-allowed-storage-account"></a>Bestätigen, dass der Zugriff auf das *unzulässige* Speicherkonto verweigert wird

Versuchen Sie, auf dem gleichen virtuellen Computer *myVmPrivate* mithilfe von PowerShell die Azure-Dateifreigabe dem Laufwerk „X:“ zuzuordnen. 

```powershell
$acctKey = ConvertTo-SecureString -String $storageAcctKey1 -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\notallowedaccount", $acctKey
New-PSDrive -Name X -PSProvider FileSystem -Root "\\notallowedaccount.file.core.windows.net\my-file-share" -Credential $credential
```

Der Zugriff auf die Freigabe wird verweigert, und die Fehlermeldung `New-PSDrive : Access is denied` wird angezeigt. Der Zugriff wird verweigert, weil das Speicherkonto *notallowedaccount* nicht in der Liste der zulässigen Ressourcen in der Dienstendpunktrichtlinie enthalten ist. 

Schließen Sie die Remotedesktopsitzung für den virtuellen Computer *myVmPublic*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) entfernen:

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine Dienstendpunkt-Richtlinie über den Dienstendpunkt eines virtuellen Azure-Netzwerks auf Azure Storage angewandt. Sie haben Azure Storage-Konten erstellt und den Netzwerkzugriff auf bestimmte Speicherkonten im Subnetz eines virtuellen Netzwerks eingeschränkt (und somit für andere Konten verweigert). Weitere Informationen zu Dienstendpunkt-Richtlinien finden Sie unter [Dienstendpunkt-Richtlinien: Übersicht](virtual-network-service-endpoint-policies-overview.md).
