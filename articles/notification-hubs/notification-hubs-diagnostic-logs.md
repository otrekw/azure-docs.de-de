---
title: Azure Notification Hubs-Diagnoseprotokolle | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über alle verfügbaren Betriebs- und Diagnoseprotokolle für Azure Notification Hubs.
author: brannon
ms.author: brjones
ms.service: notification-hubs
ms.topic: article
ms.date: 01/29/2021
ms.openlocfilehash: b532dca6ceba44a32132bf64b322e1b4764fd5fa
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100416696"
---
# <a name="enable-diagnostics-logs-for-notification-hubs"></a>Aktivieren von Diagnoseprotokollen für Notification Hubs

Wenn Sie mit der Verwendung des Azure Notification Hubs-Namespace beginnen, sollten Sie kontrollieren, wie und wann Ihr Namespace erstellt, gelöscht oder aufgerufen wird. Dieser Artikel enthält eine Übersicht über alle verfügbaren Betriebs-/Diagnoseprotokolle.

Azure Notification Hubs unterstützt derzeit Aktivitäts- und Betriebsprotokolle, die *Verwaltungsvorgänge* erfassen, die für den Azure Notification Hubs-Namespace ausgeführt werden.

## <a name="diagnostic-logs-schema"></a>Schema „Diagnoseprotokolle“

Alle Protokolle werden im JSON-Format (JavaScript Object Notation) an den folgenden beiden Speicherorten gespeichert:

- **AzureActivity:** Zeigt Protokolle aus Vorgängen und von Aktionen an, die für Ihren Namespace im Azure-Portal oder über Azure Resource Manager-Vorlagenbereitstellungen ausgeführt werden.
- **AzureDiagnostics:** Zeigt Protokolle aus Vorgängen von Aktionen an, die für Ihren Namespace mithilfe der API oder über Verwaltungsclients im Sprach-SDK ausgeführt werden.

JSON-Zeichenfolgen im Diagnoseprotokoll enthalten Elemente, die in der folgenden Tabelle aufgeführt sind:

| Name | BESCHREIBUNG |
| ------- | ------- |
| time | UTC-Zeitstempel des Protokolls |
| resourceId | Relativer Pfad zur Azure-Ressource |
| operationName | Name des Verwaltungsvorgangs |
| category | Protokollkategorie. Gültige Werte: `OperationalLogs` |
| callerIdentity | Identität des Aufrufers, der den Verwaltungsvorgang initiiert hat |
| resultType | Status des Verwaltungsvorgangs. Gültige Werte: `Succeeded` oder `Failed` |
| resultDescription | Beschreibung des Verwaltungsvorgangs |
| correlationId | Korrelations-ID des Verwaltungsvorgangs (wenn angegeben) |
| callerIpAddress | Die IP-Adresse des Aufrufers. Leer bei Aufrufen, die aus dem Azure-Portal stammen |

Hier ein Beispiel für eine JSON-Zeichenfolge im Betriebsprotokoll:

```json
{
    "operationName": "Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action",
    "resourceId": "/SUBSCRIPTIONS/2CAC2A14-BA6B-46A6-BCE8-2D9781A41BA2/RESOURCEGROUPS/SAMPLES/PROVIDERS/MICROSOFT.NOTIFICATIONHUBS/NAMESPACES/SAMPLE-NS",
    "time": "1/1/2021 5:16:32 AM +00:00",
    "category": "OperationalLogs",
    "resultType": "Succeeded",
    "resultDescription": "Gets Hub Authorization Rules",
    "correlationId": "00000000-0000-0000-0000-000000000000",
    "callerIdentity": "{ \"identityType\": \"Portal\", \"identity\": \"\" }"
}
```

Das Feld `callerIdentity` kann leer oder eine JSON-Zeichenfolge in einem der folgenden Formate sein.

Bei Aufrufen, die aus dem Azure-Portal stammen, ist das Feld `identity` leer. Das Protokoll kann mit Aktivitätsprotokollen korreliert werden, um den angemeldeten Benutzer zu bestimmen.
```json
{
    "identityType": "Portal",
    "identity": ""
}
```

Bei Aufrufen, die über Azure Resource Manager durchgeführt werden, enthält das Feld `identity` den Benutzernamen des angemeldeten Benutzers.
```json
{
   "identityType": "Username",
   "identity": "test@foo.com"
}
```

Bei Aufrufen der Notification Hubs-REST-API enthält das Feld `identity` den Namen der Zugriffsrichtlinie, die zum Generieren des SharedAccessSignature-Tokens verwendet wird.
```json
{
   "identityType": "KeyName",
   "identity": "SharedAccessRootKey2"
}
```

## <a name="events-and-operations-captured-in-operational-logs"></a>In Betriebsprotokollen erfasste Ereignisse und Vorgänge

Betriebsprotokolle erfassen alle Verwaltungsvorgänge, die für den Azure Notification Hubs-Namespace ausgeführt werden. Datenvorgänge werden aufgrund der hohen Menge an Datenvorgängen, die für Azure Notification Hubs durchgeführt werden, nicht erfasst.

Die folgenden Verwaltungsvorgänge werden in Betriebsprotokollen erfasst: 

| `Scope` | Vorgangsname | Beschreibung des Vorgangs |
| :-- | :-- | :-- |
| Namespace | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | Auflisten von Autorisierungsregeln |
| Namespace | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Löschen von Autorisierungsregeln |
| Namespace | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | Dient zum Auflisten von Schlüsseln. |
| Namespace | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | Abrufen von Autorisierungsregeln |
| Namespace | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | Erneutes Generieren von Schlüsseln |
| Namespace | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Erstellen oder Aktualisieren von Autorisierungsregeln |
| Namespace | Microsoft.NotificationHubs/Namespaces/delete | Löschen des Namespace |
| Namespace | Microsoft.NotificationHubs/Namespaces/read | Abrufen des Namespace |
| Namespace | Microsoft.NotificationHubs/Namespaces/write | Erstellen oder Aktualisieren des Namespace |
| Notification Hub | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | Auflisten von Autorisierungsregeln |
| Notification Hub | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Löschen von Autorisierungsregeln |
| Notification Hub | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | Dient zum Auflisten von Schlüsseln. |
| Notification Hub | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | Lesen von Autorisierungsregeln |
| Notification Hub | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | Erneutes Generieren von Schlüsseln |
| Notification Hub | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Erstellen oder Aktualisieren von Autorisierungsregeln |
| Notification Hub | Microsoft.NotificationHubs/Namespaces/NotificationHubs/delete | Löschen des Notification Hubs |
| Notification Hub | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Erstellen, Aktualisieren oder Abrufen von PNS-Anmeldeinformationen |
| Notification Hub | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | Abrufen des Notification Hubs |
| Notification Hub | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Erstellen oder Aktualisieren des Notification Hubs |

## <a name="enable-operational-logs"></a>Aktivieren von Betriebsprotokollen

Betriebsprotokolle sind standardmäßig deaktiviert. Gehen Sie folgendermaßen vor, um Protokolle zu aktivieren:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Azure Notification Hub-Namespace, und klicken Sie unter **Überwachung** auf **Diagnoseeinstellungen**.

   ![Der Link „Diagnoseeinstellungen“](./media/notification-hubs-diagnostic-logs/image-1.png)

1. Wählen Sie im Bereich **Diagnoseeinstellungen** den Eintrag **Diagnoseeinstellung hinzufügen** aus.  

   ![Der Link „Diagnoseeinstellung hinzufügen“](./media/notification-hubs-diagnostic-logs/image-2.png)

1. Konfigurieren Sie die Diagnoseeinstellungen, indem Sie die folgenden Schritte ausführen:

   a. Geben Sie im Feld **Name** einen Name für die Diagnoseeinstellungen ein.  

   b. Wählen Sie eines der drei folgenden Ziele für Ihre Diagnoseprotokolle aus:  
   - Wenn Sie **An Log Analytics-Arbeitsbereich senden** auswählen, müssen Sie angeben, an welche Instanz von Log Analytics die Diagnoseprotokolle gesendet werden.  
   - Wenn Sie **In einem Speicherkonto archivieren** auswählen, müssen Sie das Speicherkonto konfigurieren, in dem die Diagnosprotokolle gespeichert werden.  
   - Wenn Sie **An einen Event Hub streamen** auswählen, müssen Sie den Event Hub konfigurieren, in den die Diagnoseprotokolle gestreamt werden sollen.

   c. Klicken Sie auf das Kästchen neben **OperationalLogs**.

    ![Der Bereich „Diagnoseeinstellungen“](./media/notification-hubs-diagnostic-logs/image-3.png)

1. Wählen Sie **Speichern** aus.

Die neuen Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel im Bereich **Diagnoseprotokolle** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Konfigurieren von Diagnosespeicher finden Sie hier:
* [Übersicht über Azure-Diagnoseprotokolle](../azure-monitor/platform/platform-logs-overview.md).

Weitere Informationen zu Azure Notification Hubs finden Sie hier:
* [Was ist Azure Notification Hubs?](notification-hubs-push-notification-overview.md)

