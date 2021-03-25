---
title: Azure Service Bus-Diagnoseprotokolle | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über alle verfügbaren Betriebs- und Diagnoseprotokolle für Azure Service Bus.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: f2672d61285025eadb8f742ca2518297107595ce
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100585275"
---
# <a name="enable-diagnostics-logs-for-service-bus"></a>Aktivieren von Diagnoseprotokollen für Service Bus

Wenn Sie mit der Verwendung des Azure Service Bus-Namespace beginnen, sollten Sie kontrollieren, wie und wann Ihr Namespace erstellt, gelöscht oder aufgerufen wird. Dieser Artikel enthält eine Übersicht über alle verfügbaren Betriebs-/Diagnoseprotokolle.

Azure Service Bus unterstützt derzeit Aktivitäts- und Betriebsprotokolle, die *Verwaltungsvorgänge* erfassen, die für den Azure Service Bus-Namespace ausgeführt werden. Diese Protokolle erfassen insbesondere den Vorgangstyp, darunter das Erstellen von Warteschlangen, verwendete Ressourcen und den Status des Vorgangs.

## <a name="operational-logs-schema"></a>Schema „Betriebsprotokolle“

Alle Protokolle werden im JSON-Format (JavaScript Object Notation) an den folgenden beiden Speicherorten gespeichert:

- **AzureActivity:** Zeigt Protokolle aus Vorgängen und von Aktionen an, die für Ihren Namespace im Azure-Portal oder über Azure Resource Manager-Vorlagenbereitstellungen ausgeführt werden.
- **AzureDiagnostics:** Zeigt Protokolle aus Vorgängen von Aktionen an, die für Ihren Namespace mithilfe der API oder über Verwaltungsclients im Sprach-SDK ausgeführt werden.

JSON-Zeichenfolgen im Betriebsprotokoll enthalten Elemente, die in der folgenden Tabelle aufgeführt sind:

| Name | BESCHREIBUNG |
| ------- | ------- |
| ActivityId | Interne ID, die zum Identifizieren der angegebenen Aktivität verwendet wird |
| EventName | Vorgangsname |
| resourceId | Azure Resource Manager-Ressourcen-ID |
| SubscriptionId | Abonnement-ID |
| EventTimeString | Vorgangsdauer |
| EventProperties | Vorgangseigenschaften |
| Status | Vorgangsstatus |
| Caller | Aufrufer des Vorgangs (Azure-Portal oder Verwaltungsclient) |
| Category | OperationalLogs |

Hier ein Beispiel für eine JSON-Zeichenfolge im Betriebsprotokoll:

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="events-and-operations-captured-in-operational-logs"></a>In Betriebsprotokollen erfasste Ereignisse und Vorgänge

Betriebsprotokolle erfassen alle Verwaltungsvorgänge, die für den Azure Service Bus-Namespace ausgeführt werden. Datenvorgänge werden aufgrund der hohen Menge an Datenvorgängen, die für Azure Service Bus durchgeführt werden, nicht erfasst.

> [!NOTE]
> Damit Sie Datenvorgänge besser nachverfolgen können, empfehlen wir Ihnen die clientseitige Nachverfolgung.

Die folgenden Verwaltungsvorgänge werden in Betriebsprotokollen erfasst: 

| `Scope` | Vorgang|
|-------| -------- |
| Namespace | <ul> <li> Erstellen des Namespaces</li> <li> Aktualisieren des Namespace </li> <li> Löschen des Namespace </li> <li> Aktualisieren des Namespace: SharedAccess-Richtlinie </li> </ul> | 
| Warteschlange | <ul> <li> Erstellen einer Warteschlange</li> <li> Aktualisieren einer Warteschlange</li> <li> Löschen einer Warteschlange </li> <li> Automatisches Löschen: Löschen einer Warteschlange </li> </ul> | 
| Thema | <ul> <li> Erstellen eines Themas </li> <li> Aktualisieren eines Themas </li> <li> Löschen eines Themas </li> <li> Automatisches Löschen: Löschen eines Themas </li> </ul> |
| Subscription | <ul> <li> Erstellen von Abonnements </li> <li> Aktualisieren des Abonnements </li> <li> Löschen des Abonnements </li> <li> Automatisches Löschen: Löschen eines Abonnements </li> </ul> |

> [!NOTE]
> *Lesevorgänge* werden in Betriebsprotokollen derzeit nicht nachverfolgt.

## <a name="enable-operational-logs"></a>Aktivieren von Betriebsprotokollen

Betriebsprotokolle sind standardmäßig deaktiviert. Führen Sie folgende Schritte aus, um die Diagnoseprotokolle zu aktivieren:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Azure Service Bus-Namespace, und klicken Sie unter **Überwachung** auf **Diagnoseeinstellungen**.

   ![Der Link „Diagnoseeinstellungen“](./media/service-bus-diagnostic-logs/image1.png)

1. Wählen Sie im Bereich **Diagnoseeinstellungen** den Eintrag **Diagnoseeinstellung hinzufügen** aus.  

   ![Der Link „Diagnoseeinstellung hinzufügen“](./media/service-bus-diagnostic-logs/image2.png)

1. Konfigurieren Sie die Diagnoseeinstellungen, indem Sie die folgenden Schritte ausführen:

   a. Geben Sie im Feld **Name** einen Name für die Diagnoseeinstellungen ein.  

   b. Wählen Sie eines der drei folgenden Ziele für Ihre Diagnoseprotokolle aus:  
   - Wenn Sie **In einem Speicherkonto archivieren** auswählen, müssen Sie das Speicherkonto konfigurieren, in dem die Diagnosprotokolle gespeichert werden.  
   - Wenn Sie **An einen Event Hub streamen** auswählen, müssen Sie den Event Hub konfigurieren, in den die Diagnoseprotokolle gestreamt werden sollen.
   - Wenn Sie **An Log Analytics senden** auswählen, müssen Sie angeben, an welche Instanz von Log Analytics die Diagnoseprotokolle gesendet werden.  

   c. Klicken Sie auf das Kästchen neben **OperationalLogs**.

    ![Der Bereich „Diagnoseeinstellungen“](./media/service-bus-diagnostic-logs/image3.png)

1. Wählen Sie **Speichern** aus.

Die neuen Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel im Bereich **Diagnoseprotokolle** angezeigt.

Weitere Informationen zum Konfigurieren der Diagnoseeinstellungen finden Sie in der [Übersicht über Protokolle der Azure-Plattform](../azure-monitor/essentials/platform-logs-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Service Bus:

* [Einführung in Servicebus](service-bus-messaging-overview.md)
* [Erste Schritte mit Service Bus](service-bus-dotnet-get-started-with-queues.md)