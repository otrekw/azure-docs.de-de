---
title: Aktualisieren der Network Watcher-Erweiterung auf die neueste Version
description: In diesem Artikel erfahren Sie, wie Sie die Network Watcher-Erweiterung auf die neueste Version aktualisieren.
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
ms.openlocfilehash: fd3fff2d438bbf804e35f04db0cfae15eea5e782
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91973338"
---
# <a name="how-to-update-the-network-watcher-extension-to-the-latest-the-version"></a>Vorgehensweise: Aktualisieren der Network Watcher-Erweiterung auf die neueste Version 

## <a name="overview"></a>Übersicht

[Azure Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md) ist ein Dienst zur Überwachung, Diagnose und Analyse der Netzwerkleistung, der die Überwachung von Azure-Netzwerken ermöglicht. Die VM-Erweiterung des Network Watcher-Agents ist eine Voraussetzung für die bedarfsgesteuerte Erfassung von Netzwerkdatenverkehr und andere erweiterte Funktionalität auf virtuellen Azure-Computern. Die Network Watcher-Erweiterung wird von Features wie dem Verbindungsmonitor, der Vorschauversion des Verbindungsmonitors, der Verbindungsproblembehandlung und der Paketerfassung verwendet.   

## <a name="prerequisites"></a>Voraussetzungen
In diesem Dokument wird davon ausgegangen, dass Sie die Network Watcher-Erweiterung auf Ihrem virtuellen Computer installiert haben, und eine Anleitung für das Aktualisieren der Erweiterung auf die neueste Version geboten. 

## <a name="latest-version"></a>Aktuelle Version
Bei der aktuellen Version der Network Watcher-Erweiterung handelt es sich derzeit um `1.4.1654.1`.

## <a name="updating-your-extension"></a>Aktualisieren der Erweiterung 

### <a name="check-your-extension-version"></a>Überprüfen der Erweiterungsversion  

**Über das Azure-Portal**

1. Wechseln Sie im Azure-Portal für Ihre VM zum Blatt „Erweiterungen“.   
2. Klicken Sie auf die Erweiterung „AzureNetworkWatcher“, um den Detailbereich anzuzeigen.  
3. Suchen Sie die Versionsnummer im Feld „Version“.  

**Über die Azure CLI** Führen Sie den folgenden Befehl in einer Azure CLI-Eingabeaufforderung aus.   

```azurecli
az vm extension list --resource-group  <ResourceGroupName> --vm-name <VMName>
```

Suchen Sie in der Ausgabe nach der Erweiterung „AzureNetworkWatcher“, und ermitteln Sie die Versionsnummer im Ausgabefeld „TypeHandlerVersion“.  


**Über PowerShell** Führen Sie die folgenden Befehle in einer PowerShell-Eingabeaufforderung aus:   

```powershell
Get-AzVMExtension -ResourceGroupName <ResourceGroupName> -VMName <VMName>  
```

Suchen Sie die Erweiterung „AzureNetworkWatcher“ in der Ausgabe, und ermitteln Sie die Versionsnummer im Ausgabefeld „TypeHandlerVersion“.   


### <a name="update-your-extension"></a>Aktualisieren der Erweiterung

Ist Ihre Version niedriger als `1.4.1654.1` (die derzeit aktuelle Version), sollten Sie Ihre Erweiterung mithilfe einer der folgenden Methoden aktualisieren. 

**Option 1: Verwenden von PowerShell**

```powershell
#Linux command
Set-AzVMExtension `  -ResourceGroupName "myResourceGroup1" `  -Location "WestUS" `  -VMName "myVM1" `  -Name "AzureNetworkWatcherExtension" `  -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentLinux"   

#Windows command
Set-AzVMExtension `  -ResourceGroupName "myResourceGroup1" `  -Location "WestUS" `  -VMName "myVM1" `  -Name "AzureNetworkWatcherExtension" `  -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentWindows"   
```


**Option 2: Verwenden der Azure CLI**  

Erzwingen eines Upgrades 

```azurecli
#Linux command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher" --force-update

#Windows command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" --force-update
```

Falls dies nicht funktioniert, entfernen Sie die Erweiterung, und installieren Sie sie noch einmal mithilfe der folgenden Schritte. Hierdurch wird die aktuelle Version automatisch hinzugefügt. 

Entfernen der Erweiterung 

```azurecli
#Same for Linux and Windows
az vm extension delete --resource-group "myResourceGroup1" --vm-name "myVM1" -n "AzureNetworkWatcherExtension"

```

Neuinstallieren der Erweiterung

```azurecli
#Linux command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher"  

#Windows command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" 

```

**Option 3: Neu starten Ihrer VMs**

Wenn Sie das automatische Upgrade für die Erweiterung „NetworkWatcher“ auf TRUE festgelegt haben, starten Sie Ihre VM-Installation mit der aktuellen Erweiterungsversion neu.


## <a name="support"></a>Support

Wenn Sie Fragen zu diesem Artikel haben, können Sie in der Dokumentation zur Network Watcher-Erweiterung nachlesen ([Linux](./network-watcher-linux.md), [Windows](./network-watcher-windows.md)) oder sich an die Azure-Experten in den [MSDN Azure- und Stack Overflow-Foren](https://azure.microsoft.com/support/forums/) wenden. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).