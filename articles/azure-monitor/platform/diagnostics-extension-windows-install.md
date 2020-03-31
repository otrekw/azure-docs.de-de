---
title: Installieren und Konfigurieren der Microsoft Azure-Diagnoseerweiterung (WAD)
description: Erfahren Sie, wie Sie Azure-Diagnosedaten in einem Azure Storage-Konto erfassen, damit Sie sie mit einem der verschiedenen verfügbaren Tools anzeigen können.
services: azure-monitor
author: bwren
ms.subservice: diagnostic-extension
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: bwren
ms.openlocfilehash: 929ab4109eb8d0e90b6c561a2135c0b7dd4205bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672258"
---
# <a name="install-and-configure-windows-azure-diagnostics-extension-wad"></a>Installieren und Konfigurieren der Microsoft Azure-Diagnoseerweiterung (WAD)
Die Azure-Diagnoseerweiterung ist ein Agent in Azure Monitor, der Überwachungsdaten vom Gastbetriebssystem und Workloads von virtuellen Azure-Computern und anderen Computeressourcen sammelt. Dieser Artikel enthält ausführliche Informationen zum Installieren und Konfigurieren der Windows-Diagnoseerweiterung sowie eine Beschreibung der Speicherung von Daten in einem Azure Storage-Konto.

Die Diagnoseerweiterung wird als [VM-Erweiterung](../../virtual-machines/extensions/overview.md) in Azure implementiert, sodass die gleichen Installationsoptionen mit Resource Manager-Vorlagen, PowerShell und der Befehlszeilenschnittstelle unterstützt werden. Weitere Informationen finden Sie unter [Erweiterungen und Features für virtuelle Computer für Windows](../../virtual-machines/extensions/features-windows.md).

## <a name="install-with-azure-portal"></a>Installieren über das Azure-Portal
Sie können die Diagnoseerweiterung auf einem einzelnen virtuellen Computer im Azure-Portal installieren und konfigurieren. Bei dieser Methode können Sie eine Oberfläche verwenden, anstatt direkt mit der Konfiguration zu arbeiten. Wenn Sie die Diagnoseerweiterung aktivieren, wird automatisch eine Standardkonfiguration mit den gängigsten Leistungsindikatoren und Ereignissen verwendet. Sie können diese Standardkonfiguration entsprechend Ihren spezifischen Anforderungen ändern.

> [!NOTE]
> Es gibt Einstellungen für Diagnoseerweiterungen, die Sie nicht mithilfe des Azure-Portals konfigurieren können, darunter das Senden von Daten an Azure Event Hubs. Für diese Einstellungen müssen Sie eine der anderen Konfigurationsmethoden verwenden.

1. Öffnen Sie das Menü für einen virtuellen Computer im Azure-Portal.
2. Klicken Sie im Abschnitt **Überwachung** des VM-Menüs auf **Diagnoseeinstellungen**.
3. Klicken Sie auf **Überwachung auf Gastebene aktivieren**, wenn die Diagnoseerweiterung noch nicht aktiviert wurde.
4. Für den virtuellen Computer wird ein neues Azure Storage-Konto erstellt, dessen Name auf dem Namen der Ressourcengruppe für die VM basiert. Sie können den virtuellen Computer an ein anderes Speicherkonto anfügen, indem Sie die Registerkarte **Agent** auswählen.

![Diagnoseeinstellungen](media/diagnostics-extension-windows-install/diagnostic-settings.png)


Nachdem die Diagnoseerweiterung aktiviert wurde, können Sie die Standardkonfiguration ändern. In der folgenden Tabelle werden die Optionen beschrieben, die Sie auf den einzelnen Registerkarten ändern können. Einige Optionen verfügen über einen **benutzerdefinierten** Befehl, über den Sie eine detailliertere Konfiguration angeben können. Informationen zu den verschiedenen Einstellungen finden Sie unter [Schema der Windows-Diagnoseerweiterung](diagnostics-extension-schema-windows.md).

| Registerkarte | BESCHREIBUNG |
|:---|:---|
| Übersicht | Zeigt die aktuelle Konfiguration mit Links zu den anderen Registerkarten an. |
| Leistungsindikatoren | Wählen Sie die zu sammelnden Leistungsindikatoren und die zugehörigen Abtastraten aus.  |
| Protokolle | Wählen Sie die zu sammelnden Protokolldaten aus. Hierzu gehören Windows-Ereignisprotokolle, IIS-Protokolle, .NET-Anwendungsprotokolle und ETW-Ereignisse.  |
| Absturzabbilder | Aktivieren Sie das Absturzabbild für verschiedene Prozesse. |
| Senken | Aktivieren Sie Datensenken, um Daten auch an andere Ziele als Azure Storage zu senden.<br>Azure Monitor: sendet Leistungsdaten an Azure Monitor-Metriken.<br>Application Insights: sendet Daten an die Application Insights-Anwendung. |
| Agent | Ändern Sie die folgende Konfiguration für den Agent:<br>- Ändern Sie das Speicherkonto.<br>- Geben Sie die maximalen lokalen Datenträger für den Agent an.<br>- Konfigurieren Sie Protokolle für die Integrität des Agents selbst.|


> [!NOTE]
> Obwohl die Konfiguration für die Diagnoseerweiterung in JSON oder XML formatiert werden kann, wird jede im Azure-Portal ausgeführte Konfiguration immer als JSON gespeichert. Wenn Sie bei einer anderen Konfigurationsmethode XML verwenden und dann die Konfiguration über das Azure-Portal ändern, werden die Einstellungen in JSON geändert.

## <a name="resource-manager-template"></a>Resource Manager-Vorlage
Informationen zum Bereitstellen der Diagnoseerweiterung mit Azure Resource Manager-Vorlagen finden Sie unter [Verwenden von Überwachung und Diagnose bei einer Windows-VM und Azure Resource Manager-Vorlagen](../../virtual-machines/extensions/diagnostics-template.md). 

## <a name="azure-cli-deployment"></a>Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle
Sie können die Azure-Diagnoseerweiterung mithilfe der Azure-Befehlszeilenschnittstelle über [az vm extension set](https://docs.microsoft.com/cli/azure/vm/extension?view=azure-cli-latest#az-vm-extension-set) auf einem vorhandenen virtuellen Computer bereitstellen, wie im folgenden Beispiel erläutert. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --protected-settings protected-settings.json \
  --settings public-settings.json 
```

Die geschützten Einstellungen werden im [PrivateConfig-Element](diagnostics-extension-schema-windows.md#privateconfig-element) des Konfigurationsschemas definiert. Im Folgenden finden Sie ein minimales Beispiel für eine geschützte Einstellungsdatei, die das Speicherkonto definiert. Ausführliche Informationen zu den privaten Einstellungen finden Sie unter [Beispielkonfiguration](diagnostics-extension-schema-windows.md#privateconfig-element).

```JSON
{
    "storageAccountName": "mystorageaccount",
    "storageAccountKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "storageAccountEndPoint": "https://mystorageaccount.blob.core.windows.net"
}
```
Die öffentlichen Einstellungen werden im [Public-Element](diagnostics-extension-schema-windows.md#publicconfig-element) des Konfigurationsschemas definiert. Im Folgenden finden Sie ein minimales Beispiel für eine öffentliche Einstellungsdatei, die die Sammlung von Diagnoseinfrastruktur-Protokollen, eines einzelnen Leistungsindikators und eines einzelnen Ereignisprotokolls ermöglicht. Ausführliche Informationen zu den öffentlichen Einstellungen finden Sie unter [Beispielkonfiguration](diagnostics-extension-schema-windows.md#publicconfig-element).

```JSON
{
  "StorageAccount": "mystorageaccount",
  "WadCfg": {
    "DiagnosticMonitorConfiguration": {
      "overallQuotaInMB": 5120,
      "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": [
          {
            "counterSpecifier": "\\Processor Information(_Total)\\% Processor Time",
            "unit": "Percent",
            "sampleRate": "PT60S"
          }
        ]
      },
      "WindowsEventLog": {
        "scheduledTransferPeriod": "PT1M",
        "DataSource": [
          {
            "name": "Application!*[System[(Level=1 or Level=2 or Level=3)]]"
          }
        ]
      }
    }
  }
}
```



## <a name="powershell-deployment"></a>PowerShell-Bereitstellung
Sie können die Azure-Diagnoseerweiterung mithilfe von PowerShell über [Set-AzVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmdiagnosticsextension) auf einem vorhandenen virtuellen Computer bereitstellen, wie im folgenden Beispiel erläutert. 

```powershell
Set-AzVMDiagnosticsExtension -ResourceGroupName "myvmresourcegroup" `
  -VMName "myvm" `
  -DiagnosticsConfigurationPath "DiagnosticsConfiguration.json"
```

Die privaten Einstellungen werden im [PrivateConfig-Element](diagnostics-extension-schema-windows.md#privateconfig-element) des Konfigurationsschemas definiert, während die öffentlichen Einstellungen im [Public-Element](diagnostics-extension-schema-windows.md#publicconfig-element) definiert sind. Sie können die Details des Speicherkontos auch als Parameter des Cmdlets Set-AzVMDiagnosticsExtension angeben, anstatt sie in die privaten Einstellungen einzubeziehen.

Im Folgenden finden Sie ein minimales Beispiel für eine Konfigurationsdatei, die die Sammlung von Diagnoseinfrastruktur-Protokollen, eines einzelnen Leistungsindikators und eines einzelnen Ereignisprotokolls ermöglicht. Ausführliche Informationen zu den privaten und öffentlichen Einstellungen finden Sie unter [Beispielkonfiguration](diagnostics-extension-schema-windows.md#publicconfig-element). 

```JSON
{
    "PublicConfig": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
                "overallQuotaInMB": 10000
            },
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT3M",
                        "unit": "percent"
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT1M",
                    "DataSource": [
                    {
                        "name": "Application!*[System[(Level=1 or Level=2 or Level=3)]]"
                    }
                ]
            }
        },
        "StorageAccount": "mystorageaccount",
        "StorageType": "TableAndBlob"
    },
    "PrivateConfig": {
        "storageAccountName": "mystorageaccount",
        "storageAccountKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "storageAccountEndPoint": "https://mystorageaccount.blob.core.windows.net"
    }
}
```

Informationen finden Sie außerdem unter [Aktivieren der Azure-Diagnose auf einer virtuellen Azure-Maschine unter Windows mithilfe von PowerShell](../../virtual-machines/extensions/diagnostics-windows.md).

## <a name="data-storage"></a>Datenspeicher
In der nachstehenden Tabelle sind die unterschiedlichen Arten von Daten aufgelistet, die von der Diagnoseerweiterung gesammelt werden, sowie ob sie als Tabelle oder Blob gespeichert werden. Die in Tabellen gespeicherten Daten können auch in Blob Storage gespeichert werden, abhängig von der [StorageType-Einstellung](diagnostics-extension-schema-windows.md#publicconfig-element) in Ihrer öffentlichen Konfiguration.


| Daten | Speichertyp | BESCHREIBUNG |
|:---|:---|:---|
| WADDiagnosticInfrastructureLogsTable | Tabelle | Diagnosemonitor und Konfigurationsänderungen |
| WADDirectoriesTable | Tabelle | Vom Diagnosemonitor überwachte Verzeichnisse.  Dies umfasst IIS-Protokolle, Protokolle zu IIS-Anforderungsfehlern und benutzerdefinierte Verzeichnisse.  Der Speicherort der Blob-Protokolldatei ist im Feld „Container“ festgelegt, ihr Name im Feld „RelativePath“.  Das Feld „AbsolutePath“ gibt den Speicherort und Namen der Datei wie auf dem virtuellen Azure-Computer vorhanden an. |
| WadLogsTable | Tabelle | Mit dem Ablaufverfolgungslistener in Code erstellte Protokolle. |
| WADPerformanceCountersTable | Tabelle | Leistungsindikatoren. |
| WADWindowsEventLogsTable | Tabelle | Windows-Ereignisprotokolle. |
| wad-iis-failedreqlogfiles | Blob | Enthält Informationen aus den Protokollen zu IIS-Anforderungsfehlern. |
| wad-iis-logfiles | Blob | Enthält Informationen zu IIS-Protokollen. |
| "custom" | Blob | Ein benutzerdefinierter Container basierend auf Konfigurationsverzeichnissen, die vom Diagnosemonitor überwacht werden.  Der Name dieses Blob-Containers wird in WADDirectoriesTable festgelegt. |

## <a name="tools-to-view-diagnostic-data"></a>Tools zum Anzeigen von Diagnosedaten
Mehrere Tools stehen zum Anzeigen der Daten nach der Übertragung an den Speicher zur Verfügung. Beispiel:

* Server-Explorer in Visual Studio – wenn Sie die Azure-Tools für Microsoft Visual Studio installiert haben, können Sie den Azure-Speicherknoten im Server-Explorer zum Anzeigen schreibgeschützter Blob- und Tabellendaten aus Ihren Azure-Speicherkonten verwenden. Sie können Daten aus dem lokalen Speicheremulatorkonto und auch aus den für Azure erstellten Speicherkonten anzeigen. Weitere Informationen finden Sie unter [Durchsuchen und Verwalten von Speicherressourcen mit dem Server-Explorer](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage).
* Beim [Microsoft Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) handelt es sich um eine eigenständige App, über die Sie ganz einfach mit Azure Storage-Daten arbeiten können – unter Windows, OSX und Linux.
* [Azure Management Studio](https://www.cerebrata.com/products/azure-management-studio/introduction) enthält Azure Diagnostics Manager, mit dem Sie die Diagnosedaten anzeigen, herunterladen und verwalten können, die von den auf Azure ausgeführten Anwendungen gesammelt werden.

## <a name="next-steps"></a>Nächste Schritte
- Ausführliche Informationen zum Weiterleiten von Überwachungsdaten an Azure Event Hubs finden Sie unter [Senden von Daten von der Microsoft Azure-Diagnoseerweiterung an Event Hubs](diagnostics-extension-stream-event-hubs.md).
