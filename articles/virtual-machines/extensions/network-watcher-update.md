---
title: Aktualisieren der Network Watcher-Erweiterung auf die neueste Version
description: In diesem Artikel erfahren Sie, wie Sie die Azure Network Watcher-Erweiterung auf die neueste Version aktualisieren.
services: virtual-machines-windows
documentationcenter: ''
author: damendo
manager: balar
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.workload: infrastructure-services
ms.date: 09/23/2020
ms.author: damendo
ms.openlocfilehash: 640b148dc22aa87592a6adcfca99c8ed35731934
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92220586"
---
# <a name="update-the-network-watcher-extension-to-the-latest-version"></a>Aktualisieren der Network Watcher-Erweiterung auf die neueste Version

## <a name="overview"></a>Übersicht

[Azure Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md) ist ein Dienst zur Überwachung, Diagnose und Analyse der Netzwerkleistung für Azure-Netzwerke. Die VM-Erweiterung des Network Watcher-Agents ist eine Voraussetzung für die bedarfsgesteuerte Erfassung von Netzwerkdatenverkehr und die Verwendung anderer erweiterter Funktionen auf virtuellen Azure-Computern. Die Network Watcher-Erweiterung wird von Features wie dem Verbindungsmonitor, der Vorschauversion des Verbindungsmonitors, der Verbindungsproblembehandlung und der Paketerfassung verwendet.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass die Network Watcher-Erweiterung auf Ihrem virtuellen Computer installiert ist.

## <a name="latest-version"></a>Aktuelle Version

Bei der aktuellen Version der Network Watcher-Erweiterung handelt es sich derzeit um `1.4.1654.1`.

## <a name="update-your-extension"></a>Aktualisieren der Erweiterung

Um die Erweiterung aktualisieren zu können, müssen Sie Ihre Erweiterungsversion kennen.

### <a name="check-your-extension-version"></a>Überprüfen der Erweiterungsversion

Die Erweiterungsversion kann über das Azure-Portal, mithilfe der Azure-Befehlszeilenschnittstelle oder per PowerShell überprüft werden.

#### <a name="usetheazureportal"></a>Verwenden des Azure-Portals

1. Navigieren Sie im Azure-Portal zum Bereich **Erweiterungen** für Ihren virtuellen Computer.
1. Wählen Sie die Erweiterung **AzureNetworkWatcher** aus, um den Detailbereich anzuzeigen.
1. Die Versionsnummer wird im Feld **Version** angezeigt.  

#### <a name="use-the-azure-cli"></a>Verwenden der Azure-CLI

Führen Sie an einer Azure CLI-Eingabeaufforderung den folgenden Befehl aus:

```azurecli
az vm get-instance-view --resource-group  "SampleRG" --name "Sample-VM"
```
Suchen Sie in der Ausgabe nach **"AzureNetworkWatcherExtension"** . Die Versionsnummer befindet sich im Feld *"typeHandlerVersion"* der Ausgabe.  Hinweis: Die JSON-Ausgabe enthält mehrmals Informationen zur Erweiterung. Die vollständige Versionsnummer der Erweiterung ist im Block „extensions“ angegeben. 

Das sieht in etwa wie folgt aus: ![Screenshot: Azure-Befehlszeilenschnittstelle](./media/network-watcher/azure-cli-screenshot.png)

#### <a name="usepowershell"></a>Verwenden von PowerShell

Führen Sie an einer PowerShell-Eingabeaufforderung die folgenden Befehle aus:

```powershell
Get-AzVM -ResourceGroupName "SampleRG" -Name "Sample-VM" -Status
```
Suchen Sie in der Ausgabe nach der Azure Network Watcher-Erweiterung. Die Versionsnummer befindet sich im Feld *TypeHandlerVersion* der Ausgabe.   

Das sieht in etwa wie folgt aus: ![Screenshot: PowerShell](./media/network-watcher/powershell-screenshot.png)

### <a name="update-your-extension"></a>Aktualisieren der Erweiterung

Ist Ihre Version niedriger als `1.4.1654.1` (die derzeit aktuelle Version), aktualisieren Sie Ihre Erweiterung mithilfe einer der folgenden Optionen:

#### <a name="option-1-use-powershell"></a>Option 1: Verwenden von PowerShell

Führen Sie die folgenden Befehle aus:

```powershell
#Linux command
Set-AzVMExtension `  -ResourceGroupName "myResourceGroup1" `  -Location "WestUS" `  -VMName "myVM1" `  -Name "AzureNetworkWatcherExtension" `  -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentLinux"   

#Windows command
Set-AzVMExtension `  -ResourceGroupName "myResourceGroup1" `  -Location "WestUS" `  -VMName "myVM1" `  -Name "AzureNetworkWatcherExtension" `  -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentWindows"   
```

Falls dies nicht funktioniert, entfernen Sie die Erweiterung, und installieren Sie sie noch einmal mithilfe der folgenden Schritte. Hierdurch wird die aktuelle Version automatisch hinzugefügt.

Entfernen der Erweiterung 

```powershell
#Same command for Linux and Windows
Remove-AzVMExtension -ResourceGroupName "SampleRG" -VMName "Sample-VM" -Name "AzureNetworkWatcherExtension"
``` 

Neuinstallieren der Erweiterung

```powershell
#Linux command
Set-AzVMExtension -ResourceGroupName "SampleRG" -Location "centralus" -VMName "Sample-VM" -Name "AzureNetworkWatcherExtension" -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentLinux" -typeHandlerVersion "1.4"

#Windows command
Set-AzVMExtension -ResourceGroupName "SampleRG" -Location "centralus" -VMName "Sample-VM" -Name "AzureNetworkWatcherExtension" -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentWindows" -typeHandlerVersion "1.4"
```

#### <a name="option-2-use-the-azure-cli"></a>Option 2: Verwenden der Azure-CLI

Erzwingen Sie ein Upgrade:

```azurecli
#Linux command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher" --force-update

#Windows command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" --force-update
```

Sollte das nicht funktionieren, entfernen Sie die Erweiterung, installieren Sie sie erneut, und führen Sie die folgenden Schritte aus, um automatisch die neueste Version hinzuzufügen:

Entfernen Sie die Erweiterung:

```azurecli
#Same for Linux and Windows
az vm extension delete --resource-group "myResourceGroup1" --vm-name "myVM1" -n "AzureNetworkWatcherExtension"

```

Installieren Sie die Erweiterung erneut:

```azurecli
#Linux command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher"  

#Windows command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" 

```

#### <a name="option-3-reboot-your-vms"></a>Option 3: Neustarten Ihrer virtuellen Computer

Wenn Sie für die Network Watcher-Erweiterung das automatische Upgrade aktiviert haben, starten Sie Ihre VM-Installation neu, um die aktuelle Erweiterung zu erhalten.

## <a name="support"></a>Support

Sollten Sie im Rahmen dieses Artikels weitere Hilfe benötigen, lesen Sie die Dokumentation zur Network Watcher-Erweiterung für [Linux](./network-watcher-linux.md) oder [Windows](./network-watcher-windows.md). Über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) können Sie sich auch mit den Azure-Experten in Verbindung setzen. Alternativ dazu können Sie eine Azure-Supportanfrage erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie die Option **Support erhalten** . Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).
