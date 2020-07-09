---
title: Diagnoseprotokolle für Hybrid Connections
description: Dieser Artikel enthält eine Übersicht über alle verfügbaren Aktivitäts- und Diagnoseprotokolle für Azure Relay.
ms.topic: how-to
ms.date: 06/23/2020
ms.openlocfilehash: f79e4aa21fcb9e592f431e252981dc0e7c02817f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85314905"
---
# <a name="enable-diagnostics-logs-for-azure-relay-hybrid-connections"></a>Aktivieren von Diagnoseprotokollen für Azure Relay Hybrid Connections
Wenn Sie Azure Relay Hybrid Connections verwenden, möchten Sie möglicherweise überwachen, wie und wann Ihre Listener und Absender geöffnet und geschlossen werden und wie Hybrid Connections erstellt und Nachrichten gesendet werden. Dieser Artikel enthält eine Übersicht über die verfügbaren Aktivitäts- und Diagnoseprotokolle, die vom Azure Relay-Dienst bereitgestellt werden. 

Sie können zwei Typen von Protokollen für Azure Relay anzeigen:

- [Aktivitätsprotokolle](../azure-monitor/platform/platform-logs-overview.md): Diese Protokolle enthalten Informationen zu Vorgängen, die für Ihren Namespace im Azure-Portal oder über die Azure Resource Manager-Vorlage ausgeführt werden. Diese Protokolle sind immer aktiviert. Beispiel: „Namespace erstellen oder aktualisieren“, „Hybridverbindung erstellen oder aktualisieren“. 
- [Diagnoseprotokolle](../azure-monitor/platform/platform-logs-overview.md): Sie können Diagnoseprotokolle konfigurieren, um einen umfassenderen Überblick über alle Vorgänge zu erhalten, die bei Vorgängen und Aktionen auftreten, die für Ihren Namespace mithilfe der-API oder über das Sprach-SDK durchgeführt werden.

## <a name="view-activity-logs"></a>Anzeigen von Aktivitätsprotokollen
Navigieren Sie zum Anzeigen von Aktivitätsprotokollen für Ihren Azure Relay-Namespace zur Seite **Aktivitätsprotokoll** im Azure-Portal.

![Azure Relay: Aktivitätsprotokoll](./media/diagnostic-logs/activity-log.png)

## <a name="enable-diagnostic-logs"></a>Aktivieren von Diagnoseprotokollen

> [!NOTE]
> Diagnoseprotokolle sind nur für Hybrid Connections und nicht für das WCF-Relay (Windows Communication Foundation) verfügbar.

Führen Sie folgende Schritte aus, um Diagnoseprotokolle zu aktivieren:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Azure Relay-Namespace, und klicken Sie dann unter **Überwachung** auf **Diagnoseeinstellungen**.
1. Wählen Sie auf der Seite **Diagnoseeinstellungen** die Option **Diagnoseeinstellung hinzufügen** aus.  

   ![Der Link „Diagnoseeinstellung hinzufügen“](./media/diagnostic-logs/add-diagnostic-setting.png)

1. Konfigurieren Sie die Diagnoseeinstellungen, indem Sie die folgenden Schritte ausführen:
    1. Geben Sie im Feld **Name** einen Name für die Diagnoseeinstellungen ein.  
    2. Wählen Sie **HybridConnectionsEvent** als Protokolltyp aus. 
    3. Wählen Sie eines der drei folgenden **Ziele** für Ihre Diagnoseprotokolle aus:  
        1. Wenn Sie **In einem Speicherkonto archivieren** auswählen, konfigurieren Sie das Speicherkonto, in dem die Diagnosprotokolle gespeichert werden.  
        2. Wenn Sie **An einen Event Hub streamen** auswählen, konfigurieren Sie den Event Hub, in den die Diagnoseprotokolle gestreamt werden sollen.
        3. Wenn Sie **An Log Analytics senden** auswählen, geben Sie an, an welche Instanz von Log Analytics die Diagnoseprotokolle gesendet werden.  

        ![Beispiel für Diagnoseeinstellungen](./media/diagnostic-logs/sample-diagnostic-settings.png)
1. Wählen Sie auf der Symbolleiste **Speichern** aus, um die Einstellungen zu speichern.

Die neuen Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel im Bereich **Diagnoseprotokolle** angezeigt. Weitere Informationen zum Konfigurieren der Diagnoseeinstellungen finden Sie in der [Übersicht über Protokolle der Azure-Plattform](../azure-monitor/platform/diagnostic-logs-overview.md).


## <a name="schema-for-hybrid-connections-events"></a>Schema für Hybridverbindungsereignisse
JSON-Zeichenfolgen im Hybridverbindungs-Ereignisprotokoll enthalten Elemente, die in der folgenden Tabelle aufgeführt sind:

| Name | BESCHREIBUNG |
| ------- | ------- |
| resourceId | Azure Resource Manager-Ressourcen-ID |
| ActivityId | Interne ID, die zum Identifizieren des angegebenen Vorgangs verwendet wird. Kann auch als „TrackingId“ bezeichnet werden. |
| Endpunkt | Die Adresse der Relay-Ressource. |
| Vorgangsname | Der Typ des Hybrid Connections-Vorgangs, der protokolliert wird. |
| EventTimeString | Der UTC-Zeitstempel des Protokollsatzes. |
| `Message` | Die ausführliche Meldung des Ereignisses. |
| Category | Kategorie des Ereignisses. Aktuell ist nur `HybridConnectionsEvents` verfügbar. 


## <a name="sample-hybrid-connections-event"></a>Beispiel für ein Hybridverbindungsereignis
Im Folgenden finden Sie ein Beispiel für ein Hybridverbindungsereignis im JSON-Format. 

```json
{
    "resourceId": "/SUBSCRIPTIONS/0000000000-0000-0000-0000-0000000000000/RESOURCEGROUPS/MyResourceGroup/PROVIDERS/MICROSOFT.RELAY/NAMESPACES/MyRelayNamespace",
    "ActivityId": "7006a0db-27eb-445c-939b-ce86133014cc",
    "endpoint": "sb://myrelaynamespace.servicebus.windows.net/myhybridconnection/7006a0db-27eb-445c-939b-ce86133014cc_G5",
    "operationName": "Microsoft.Relay/HybridConnections/NewSenderRegistering",
    "EventTimeString": "2020-04-27T20:27:57.3842810Z",
    "message": "A new sender is registering.",
    "category": "HybridConnectionsEvent"
}
```

## <a name="events-and-operations-captured-in-diagnostic-logs"></a>In Diagnoseprotokollen erfasste Ereignisse und Vorgänge

| Vorgang | BESCHREIBUNG | 
| --------- | ----------- | 
| AuthorizationFailed | Fehler bei der Autorisierung.|
| InvalidSasToken | Ungültiges SAS-Token. | 
| ListenerAcceptingConnection | Der Listener akzeptiert die Verbindung. |
| ListenerAcceptingConnectionTimeout | Timeout des Listeners, der die Verbindung annimmt. |
| ListenerAcceptingHttpRequestFailed | Fehler beim Listener, der die HTTP-Anforderung akzeptiert, aufgrund einer Ausnahme. |
| ListenerAcceptingRequestTimeout | Timeout des Listeners, der die Anforderung annimmt. |  
| ListenerClosingFromExpiredToken | Der Listener wird geschlossen, da das Sicherheitstoken abgelaufen ist. | 
| ListenerRejectedConnection | Der Listener hat die Verbindung abgelehnt. |
| ListenerReturningHttpResponse | Der Listener gibt eine HTTP-Antwort zurück. |  
| ListenerReturningHttpResponseFailed | Der Listener gibt eine HTTP-Antwort mit einem Fehlercode zurück. | 
 ListenerSentHttpResponse | Der Relaydienst hat eine HTTP-Antwort vom Listener empfangen. | 
| ListenerUnregistered | Die Registrierung des Listeners wird aufgehoben. | 
| ListenerUnresponsive | Der Listener reagiert beim Zurückgeben einer Antwort nicht. | 
| MessageSendingToListener | Nachricht wird aktuell an den Listener gesendet. |
| MessageSentToListener | Nachricht wird an den Listener gesendet. | 
| NewListenerRegistered | Neuer Listener wurde registriert. |
| NewSenderRegistering | Neuer Absender wird aktuell registriert. | 
| ProcessingRequestFailed | Die Verarbeitung eines Hybrid Verbindungs-Vorgangs ist fehlgeschlagen. | 
| SenderConnectionClosed | Die Absenderverbindung wurde geschlossen. |
| SenderListenerConnectionEstablished | Der Absender und der Listener haben die Verbindung erfolgreich hergestellt. |
| SenderSentHttpRequest | Der Absender hat eine HTTP-Anforderung gesendet. | 


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Relay finden Sie unter:

* [Einführung in Azure Relay](relay-what-is-it.md)
* [Erste Schritte mit Relay Hybrid Connections](relay-hybrid-connections-dotnet-get-started.md)
