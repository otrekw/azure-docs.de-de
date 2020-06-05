---
title: Kundeneigene Speicherkonten für die Protokollerfassung
description: Verwenden Sie Ihr eigenes Speicherkonto für die Erfassung von Protokolldaten in einem Log Analytics-Arbeitsbereich in Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/20/2020
ms.openlocfilehash: 7213cb10936fc1c2117b2c5c3fc32a6bfea02d30
ms.sourcegitcommit: fc0431755effdc4da9a716f908298e34530b1238
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/24/2020
ms.locfileid: "83816585"
---
# <a name="customer-owned-storage-accounts-for-log-ingestion-in-azure-monitor"></a>Kundeneigene Speicherkonten für die Protokollerfassung in Azure Monitor

Azure Monitor verwendet Speicherkonten für den Erfassungsprozess einiger Datentypen, beispielsweise bei [benutzerdefinierten Protokollen](data-sources-custom-logs.md) und einigen [Azure-Protokollen](azure-storage-iis-table.md). Während des Erfassungsprozesses werden die Protokolle zuerst an ein Speicherkonto gesendet und später in Log Analytics oder Application Insights erfasst. Wenn Sie während der Erfassung Kontrolle über Ihre Daten benötigen, können Sie Ihre eigenen Speicherkonten anstatt des über den Dienst verwalteten Speichers verwenden. Mit einem eigenen Speicherkonto haben Sie die Kontrolle über Zugriff, Inhalte, Verschlüsselung und Aufbewahrung der Protokolle während der Erfassung. Dies wird als „Bring Your Own Storage“ bzw. BYOS bezeichnet. 

Ein Szenario, das BYOS erfordert, ist die Netzwerkisolation über Private Link-Instanzen. Bei der Verwendung von virtuellen Netzwerken ist die Netzwerkisolation häufig eine Anforderung, und der Zugriff auf das öffentliche Internet ist beschränkt. In solchen Fällen wird der Zugriff auf den Azure Monitor-Dienstspeicher zur Protokollerfassung entweder vollständig blockiert oder als nicht empfohlene Vorgehensweise eingestuft. Stattdessen sollten Protokolle mit einem kundeneigenen Speicherkonto erfasst werden, das sich innerhalb des virtuellen Netzwerks befindet oder auf das problemlos aus diesem Netzwerk zugegriffen werden kann.

Ein weiteres Szenario ist die Verschlüsselung von Protokollen mit kundenseitig verwalteten Schlüsseln (Customer-Managed Keys, CMKs). Kunden können protokollierte Daten mithilfe eines CMK in den Clustern verschlüsseln, in denen die Protokolle gespeichert sind. Derselbe Schlüssel kann auch zum Verschlüsseln von Protokollen während des Erfassungsprozesses verwendet werden.

## <a name="data-types-supported"></a>Unterstützte Datentypen

Folgende Datentypen können aus einem Speicherkonto erfasst werden. Weitere Informationen zum Erfassen dieser Typen finden Sie unter [Sammeln von Daten aus der Azure-Diagnoseerweiterung in Azure Monitor-Protokollen](azure-storage-iis-table.md).

| type | Tabelleninformationen |
|:-----|:------------------|
| IIS-Protokolle | Blob: wad-iis-logfiles|
|Windows-Ereignisprotokolle | Tabelle: WADWindowsEventLogsTable |
| syslog | Tabelle: LinuxsyslogVer2v0 |
| Windows-ETW-Protokolle | Tabelle: WADETWEventTable|
| Service Fabric | Tabelle: WADServiceFabricSystemEventTable <br/> WADServiceFabricReliableActorEventTable<br/> WADServiceFabricReliableServicEventTable |
| Benutzerdefinierte Protokolle | – |
| Watson-Dumpdateien in Azure Security Center | –|  

## <a name="storage-account-requirements"></a>Anforderungen an das Speicherkonto 
Das Speicherkonto muss die folgenden Anforderungen erfüllen:

- Ressourcen in Ihrem virtuellen Netzwerk, die Protokolle in den Speicher schreiben, müssen darauf zugreifen können.
- Es muss sich in derselben Region befinden wie der Arbeitsbereich, mit dem es verknüpft ist.
- Durch Auswahl von *Vertrauenswürdigen MS-Diensten den Zugriff auf dieses Speicherkonto erlauben* muss Log Analytics explizit erlaubt werden, Protokolle aus dem Speicherkonto zu lesen.

## <a name="process-to-configure-customer-owned-storage"></a>Prozess zum Konfigurieren eines kundeneigenen Speichers
Wenn Sie Ihr eigenes Speicherkonto für die Erfassung verwenden möchten, verläuft der grundlegende Prozess folgendermaßen:

1. Erstellen Sie ein neues Speicherkonto, oder wählen Sie ein vorhandenes aus.
2. Verknüpfen Sie das Speicherkonto mit einem Log Analytics-Arbeitsbereich.
3. Stellen Sie sicher, dass der Speicher erwartungsgemäß funktioniert, indem Sie die Einstellungen für Auslastung und Aufbewahrung überprüfen.

Links können ausschließlich über die REST-API erstellt und entfernt werden. In den folgenden Abschnitten finden Sie Details zu den einzelnen API-Anforderungen für jeden Prozess.

## <a name="api-request-values"></a>Werte für API-Anforderungen

#### <a name="datasourcetype"></a>dataSourceType 

- AzureWatson: Verwenden Sie diesen Wert für Azure Watson-Dumpdateien in Azure Security Center.
- CustomLogs: Verwenden Sie diesen Wert für die folgenden Datentypen:
  - Benutzerdefinierte Protokolle
  - IIS-Protokolle
  - Ereignisse (Windows)
  - Syslog (Linux)
  - ETW-Protokolle
  - Service Fabric-Ereignisse
  - Bewertungsdaten  

#### <a name="storage_account_resource_id"></a>storage_account_resource_id
Dieser Wert verwendet die folgende Struktur:

```
subscriptions/{subscriptionId}/resourcesGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName1}
```



## <a name="get-current-links"></a>Abrufen aktueller Verknüpfungen

### <a name="get-linked-storage-accounts-for-a-specific-data-source-type"></a>Abrufen verknüpfter Speicherkonten für einen bestimmten Datenquellentyp

#### <a name="api-request"></a>API-Anforderung

```
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/linkedStorageAccounts/{dataSourceType}?api-version=2019-08-01-preview  
```

#### <a name="response"></a>Antwort 

```json
{
    "properties":
    {
        "dataSourceType": "CustomLogs",
        "storageAccountIds  ": 
        [  
            "<storage_account_resource_id_1>",
            "<storage_account_resource_id_2>"
        ],
    },
    "id":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/microsoft. operationalinsights/workspaces/{resourceName}/linkedStorageAccounts/CustomLogs",
    "name": "CustomLogs",
    "type": "Microsoft.OperationalInsights/workspaces/linkedStorageAccounts"
}
```

### <a name="get-all-linked-storage-accounts"></a>Abrufen sämtlicher verknüpfter Speicherkonten

#### <a name="api-request"></a>API-Anforderung

```
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/linkedStorageAccounts?api-version=2019-08-01-preview  
```

#### <a name="response"></a>Antwort

```json
{
    [
        {
            "properties":
            {
                "dataSourceType": "CustomLogs",
                "storageAccountIds  ": 
                [  
                    "<storage_account_resource_id_1>",
                    "<storage_account_resource_id_2>"
                ],
            },
            "id":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/microsoft. operationalinsights/workspaces/{resourceName}/linkedStorageAccounts/CustomLogs",
            "name": "CustomLogs",
            "type": "Microsoft.OperationalInsights/workspaces/linkedStorageAccounts"
        },
        {
            "properties":
            {
                "dataSourceType": " AzureWatson "
                "storageAccountIds  ": 
                [  
                    "<storage_account_resource_id_3>",
                    "<storage_account_resource_id_4>"
                ],
            },
            "id":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/microsoft. operationalinsights/workspaces/{resourceName}/linkedStorageAccounts/AzureWatson",
            "name": "AzureWatson",
            "type": "Microsoft.OperationalInsights/workspaces/linkedStorageAccounts"
        }
    ]
}
```

## <a name="create-or-modify-a-link"></a>Erstellen oder Ändern einer Verknüpfung

Sobald Sie ein Speicherkonto mit einem Arbeitsbereich verknüpfen, verwendet Log Analytics dieses anstelle des Speicherkontos, das im Besitz des Diensts ist. Sie können eine Liste mit mehreren Speicherkonten gleichzeitig registrieren, und Sie können dasselbe Speicherkonto für mehrere Arbeitsbereiche verwenden.

Wenn Ihr Arbeitsbereich Ressourcen sowohl innerhalb als auch außerhalb eines virtuellen Netzwerks verarbeitet, müssen Sie sicherstellen, dass Datenverkehr aus dem Internet nicht abgelehnt wird. Der Speicher sollte dieselben Einstellungen aufweisen wie der Arbeitsbereich und Ressourcen außerhalb des virtuellen Netzwerks verfügbar gemacht werden. 

### <a name="api-request"></a>API-Anforderung

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/linkedStorageAccounts/{dataSourceType}?api-version=2019-08-01-preview  
```

### <a name="payload"></a>Nutzlast

```json
{
    "properties":
    {
        "storageAccountIds  " : 
        [  
            "<storage_account_resource_id_1>",
            "<storage_account_resource_id_2>"
        ],
    }
}
```

### <a name="response"></a>Antwort

```json
{
    "properties":
    {
        "dataSourceType": "CustomLogs"
        "storageAccountIds  ": 
        [  
            "<storage_account_resource_id_1>",
            "<storage_account_resource_id_2>"
        ],
    },
"id":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/microsoft. operationalinsights/workspaces/{resourceName}/linkedStorageAccounts/CustomLogs",
"name": "CustomLogs",
"type": "Microsoft.OperationalInsights/workspaces/linkedStorageAccounts"
}
```


## <a name="unlink-a-storage-account"></a>Aufheben der Verknüpfung eines Speicherkontos
Wenn Sie ein Speicherkonto nicht mehr für die Erfassung verwenden oder den verwendeten Arbeitsbereich ersetzen möchten, sollten Sie die Verknüpfung des Speichers mit dem Arbeitsbereich aufheben.

Das Aufheben der Verknüpfungen aller Speicherkonten mit einem Arbeitsbereich bedeutet, dass bei der Erfassung versucht wird, dienstseitig verwaltete Speicherkonten zu verwenden. Wenn Ihre Agents in einem virtuellen Netzwerk mit eingeschränktem Zugriff auf das Internet ausgeführt werden, tritt bei der Erfassung erwartungsgemäß ein Fehler auf. Der Arbeitsbereich muss über ein verknüpftes Speicherkonto verfügen, das für Ihre überwachten Ressourcen erreichbar ist.

Vor dem Löschen eines Speicherkontos müssen Sie sicherstellen, dass alle in diesem Konto enthaltenen Daten im Arbeitsbereich erfasst wurden. Als Vorsichtsmaßnahme sollten Sie die Verfügbarkeit Ihres Speicherkontos noch eine Weile beibehalten, nachdem Sie einen anderen Speicher verknüpft haben. Löschen Sie das Konto erst, wenn sämtliche Inhalte erfasst wurden und Sie sicher sind, dass neue Daten in das neu verknüpfte Speicherkonto geschrieben werden.


### <a name="api-request"></a>API-Anforderung
```
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/linkedStorageAccounts/{dataSourceType}?api-version=2019-08-01-preview  
```

## <a name="replace-a-storage-account"></a>Ersetzen eines Speicherkontos

Um ein für die Erfassung verwendetes Speicherkonto zu ersetzen, erstellen Sie zunächst eine Verknüpfung für ein neues Speicherkonto. Die Protokollierungs-Agents rufen die aktualisierte Konfiguration ab und beginnen damit, Daten auch an den neuen Speicher zu senden.

Im nächsten Schritt heben Sie die Verknüpfung mit dem alten Speicherkonto auf, sodass die Agents keine Daten mehr in das zu entfernende Konto schreiben. Der Erfassungsprozess liest weiterhin Daten aus diesem Konto, bis sämtliche Daten erfasst wurden. Löschen Sie das alte Speicherkonto erst, wenn Sie sicher sind, dass alle Protokolle erfasst wurden.

Die Agent-Konfiguration wird nach einigen Minuten aktualisiert, und die Agents wechseln zum neuen Speicher.

## <a name="manage-storage-account"></a>Verwalten eines Speicherkontos

### <a name="load"></a>Laden

Speicherkonten können eine bestimmte Last an Lese- und Schreibanforderungen verarbeiten, bevor sie beginnen, Anforderungen zu drosseln. Die Drosselung wirkt sich auf die für die Erfassung von Protokollen benötigte Zeit aus und kann zu Datenverlust führen. Wenn der Speicher überlastet ist, registrieren Sie zusätzliche Speicherkonten und verteilen die Last auf diese. 

### <a name="related-charges"></a>Anfallende Gebühren

Die Gebühren für Speicherkonten werden anhand der Menge der gespeicherten Daten, der Speichertypen und des Redundanztyps berechnet. Details dazu finden Sie unter [Preise für Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs/) und [Azure Table Storage – Preise](https://azure.microsoft.com/pricing/details/storage/tables/).

Wenn das registrierte Speicherkonto Ihres Arbeitsbereichs sich in einer anderen Region befindet, wird der ausgehende Datenverkehr gemäß der [Preisübersicht Bandbreite](https://azure.microsoft.com/pricing/details/bandwidth/) abgerechnet.



## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Einrichten einer Private Link-Instanz finden Sie unter [Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Monitor}(private-link-security.md)
