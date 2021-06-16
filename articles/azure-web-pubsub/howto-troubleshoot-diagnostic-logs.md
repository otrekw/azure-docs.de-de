---
title: Problembehandlung mithilfe der Diagnoseprotokolle des Azure Web PubSub-Diensts
description: Erfahren Sie, wie Sie Diagnoseprotokolle abrufen und damit Probleme beheben können.
author: yjin81
ms.author: yajin1
ms.service: azure-web-pubsub
ms.topic: how-to
ms.date: 03/22/2021
ms.openlocfilehash: 119591c4a337cf622b95fa4ed70a7508d8acb99e
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111963090"
---
# <a name="how-to-troubleshoot-with-diagnostic-logs"></a>Problembehandlung mit Diagnoseprotokollen

In dieser Schrittanleitung werden die Optionen zum Abrufen der Diagnoseprotokolle und wie Sie damit Probleme behandeln können, veranschaulicht.

## <a name="whats-the-diagnostic-logs"></a>Was sind Diagnoseprotokolle?

Die Diagnoseprotokolle bieten eine umfassendere Ansicht von Konnektivitäts- und Messaginginformationen für die Azure Web PubSub-Dienstinstanz. Sie können zur Identifizierung von Problemen, zur Verbindungsverfolgung, Nachrichtenablaufverfolgung und Analyse verwendet werden.

Es gibt zwei Arten von Protokollen: das Konnektivitätsprotokoll und das Messagingprotokoll.

### <a name="connectivity-logs"></a>Konnektivitätsprotokolle

Konnektivitätsprotokolle bieten ausführliche Informationen zu Azure Web PubSub-Hubverbindungen. Dies sind z. B. grundlegende Informationen (Benutzer-ID, Verbindungs-ID usw.) und Ereignisinformationen (Verbinden-, Trennen-, Abbrechen-Ereignis usw.). Aus diesem Grund ist das Konnektivitätsprotokoll hilfreich, um verbindungsbezogene Probleme zu beheben. 

### <a name="messaging-logs"></a>Messagingprotokolle

Messagingprotokolle bieten Ablaufverfolgungsinformationen für die Azure Web PubSub-Hubnachrichten, die über den Azure Web PubSub-Dienst empfangen und gesendet werden. Zum Beispiel die Ablaufverfolgungs-ID und der Nachrichtentyp der Nachricht. In der Regel wird die Nachricht aufgezeichnet, wenn sie beim Dienst eintrifft oder ihn verlässt. Daher sind Messagingprotokolle hilfreich für die Behandlung von Problemen im Zusammenhang mit Nachrichten. 

## <a name="capture-diagnostic-logs-with-azure-web-pubsub-service-live-trace-tool"></a>Erfassen von Diagnoseprotokollen mit dem Liveablaufverfolgungs-Tool des Azure Web PubSub-Diensts 

Das Live-Ablaufverfolgungstool des Azure Web PubSub-Diensts kann Diagnoseprotokolle in Echtzeit erfassen und ist hilfreich bei der Ablaufverfolgung mit der Entwicklungsumgebung des Kunden. Das Liveablaufverfolgungs-Tool könnte sowohl Konnektivitätsprotokolle als auch Messagingprotokolle erfassen.

> [!NOTE]
> Die vom Liveablaufverfolgungs-Tool erfassten Echtzeitdiagnoseprotokolle werden als Nachrichten (ausgehender Datenverkehr) abgerechnet.

> [!NOTE]
> Für die Azure Web PubSub-Dienstinstanz, die im kostenlosen Tarif (Free) erstellt wurde, unterliegt dem täglichen Limit für Nachrichten (ausgehender Datenverkehr).

### <a name="launch-the-live-trace-tool"></a>Starten des Liveablaufverfolgungs-Tools

1. Öffnen Sie das Azure-Portal. 
1. Wählen Sie auf der Seite **Diagnoseeinstellungen** Ihrer Azure Web PubSub-Dienstinstanz die Option **Liveablaufverfolgungs-Tool öffnen** aus. 

    :::image type="content" source="./media/howto-troubleshoot-diagnostic-logs/diagnostic-logs-with-live-trace-tool.png" alt-text="Starten des Liveablaufverfolgungs-Tools.":::

### <a name="capture-the-diagnostic-logs"></a>Erfassen der Diagnoseprotokolle

Das Liveablaufverfolgungs-Tool bietet einige grundlegende Funktionen, mit denen Sie die Diagnoseprotokolle für die Problembehandlung erfassen können.

* **Erfassen**: Beginnen Sie mit der Erfassung der Echtzeitdiagnoseprotokolle von der Azure Web PubSub-Instanz mit dem Liveablaufverfolgungs-Tool.
* **Löschen**: Löschen Sie die erfassten Echtzeitdiagnoseprotokolle.
* **Protokollfilter**: Mit dem Liveablaufverfolgungs-Tool können Sie die erfassten Echtzeitdiagnoseprotokolle mit einem bestimmten Schlüsselwort filtern. Das allgemeine Trennzeichen (z. B. Leerzeichen, Komma, Semikolon usw.) wird als Teil des Schlüsselworts behandelt. 
* **Status**: Der Status zeigt an, ob das Liveablaufverfolgungs-Tool mit der jeweiligen Instanz verbunden oder von dieser getrennt ist.

:::image type="content" source="./media/howto-troubleshoot-diagnostic-logs/live-trace-tool-capture.png" alt-text="Erfassen von Diagnoseprotokollen mit dem Liveablaufverfolgungs-Tool.":::

Die vom Liveablaufverfolgungs-Tool erfassten Echtzeitdiagnoseprotokolle enthalten ausführliche Informationen für die Problembehandlung. 

| Name | BESCHREIBUNG |
| ------------ |  ------------------------ | 
| Time | Protokollereigniszeit |
| Protokollebene | Protokollereignisebene (Ablaufverfolgung/Debuggen/Information/Warnung/Fehler) |
| Veranstaltungsname | Vorgangsname des Ereignisses. |
| `Message` | Ausführliche Meldung des Protokollereignisses. |
| Ausnahme | Die Laufzeitausnahme des Azure Web PubSub-Diensts |
| Hub | Vom Benutzer definierter Hubname |
| Verbindungs-ID | Identität der Verbindung. |
| Benutzerkennung | Identität des Benutzers. |
| IP | Die IP-Adresse des Clients | 

Sobald der Azure Web PubSub-Dienst allgemein verfügbar ist, unterstützt das Liveablaufverfolgungs-Tool auch das Exportieren der Protokolle in einem bestimmten Format und hilft Ihnen dann bei deren Freigabe für andere Personen zur Problembehandlung. 

## <a name="capture-diagnostic-logs-with-azure-monitor"></a>Erfassen von Diagnoseprotokollen mit Azure Monitor

Die Integration in [Azure Monitor](https://azure.microsoft.com/services/monitor/), [Azure Storage](../azure-monitor/essentials/resource-logs.md#send-to-azure-storage) und [Log Analytics](../azure-monitor/essentials/resource-logs.md#send-to-log-analytics-workspace) zum Erfassen der Diagnoseprotokolle wird für die Public Preview nicht unterstützt.

## <a name="troubleshoot-with-the-diagnostic-logs"></a>Problembehandlung mit den Diagnoseprotokollen

Wenn die Auslastung der Verbindung unerwartet zunimmt oder sie unterbrochen wird, können Sie die Diagnoseprotokolle für die Problembehandlung nutzen. Typische Probleme sind häufig unerwartete Auslastungsveränderungen bei Verbindungen, das Erreichen von Verbindungslimits bei Verbindungen sowie Autorisierungsfehler.

### <a name="unexpected-connection-number-changes"></a>Unerwartete Änderungen der Verbindungsanzahl

#### <a name="unexpected-connection-dropping"></a>Unerwartete Verbindungsunterbrechungen

Wird eine Verbindung getrennt, zeichnet das Diagnoseprotokoll dieses Trennungsereignis mit `ConnectionAborted` oder `ConnectionEnded` in `operationName` auf.

Der Unterschied zwischen `ConnectionAborted` und `ConnectionEnded` besteht darin, dass `ConnectionEnded` eine erwartete Verbindungstrennung ist, die vom Client oder Server ausgelöst wird. Während es sich bei `ConnectionAborted` in der Regel um ein unerwartetes Verbindungsunterbrechungsereignis handelt, und der Grund für den Abbruch wird in `message` bereitgestellt.

Die Gründe für Abbrüche sind in der folgenden Tabelle aufgeführt:

| `Reason` | BESCHREIBUNG |
| ------- | ------- |
| Anzahl der Verbindungen erreicht das Limit. | Die Anzahl der Verbindungen erreicht das Limit Ihres aktuellen Tarifs. Erwägen Sie, die Diensteinheit hochzuskalieren.
| Erneutes Laden des Diensts, Wiederherstellen der Verbindung. | Azure Web PubSub-Dienst wird neu geladen. Sie müssen Ihren eigenen Mechanismus für das Wiederverbinden implementieren oder manuell eine neue Verbindung mit dem Azure Web PubSub-Dienst herstellen. |
| Vorübergehender interner Serverfehler. | Ein vorübergehender Fehler tritt im Azure Web PubSub-Dienst auf; sollte automatisch wiederhergestellt werden.

#### <a name="unexpected-connection-growing"></a>Unerwarteter Anstieg der Verbindungsauslastung.

Um unerwartete Anstiege der Verbindungsauslastung zu behandeln, müssen Sie zuerst die zusätzlichen Verbindungen ausfiltern. Sie können Ihrer Testclientverbindung eine eindeutige Testbenutzer-ID hinzufügen. Verifizieren Sie diese dann in den Diagnoseprotokollen. Wenn Sie dort mehr als eine Clientverbindung mit derselben Testbenutzer-ID oder IP-Adresse vorfinden, ist es wahrscheinlich, dass der Client mehr Verbindungen erstellt und unterhält als erwartet. Überprüfen Sie die Clientseite.

### <a name="authorization-failure"></a>Autorisierungsfehler

Wenn Sie bei Clientanforderungen die Rückgabe „401 – Nicht autorisiert“ erhalten, überprüfen Sie Ihre Diagnoseprotokolle. Wenn `Failed to validate audience. Expected Audiences: <valid audience>. Actual Audiences: <actual audience>` auftritt, bedeutet dies, dass alle Zielgruppen in Ihrem Zugriffstoken ungültig sind. Versuchen Sie, die im Protokoll vorgeschlagenen gültigen Zielgruppen zu verwenden.

### <a name="throttling"></a>Drosselung

Wenn Sie feststellen, dass Sie keine Clientverbindungen mit dem Azure Web PubSub-Dienst herstellen können, überprüfen Sie Ihre Diagnoseprotokolle. Wenn im Diagnoseprotokoll `Connection count reaches limit` vorkommt, stellen Sie zu viele Verbindungen mit dem Azure Web PubSub-Dienst her, die das Limit für die Verbindungsanzahl erreichen. Erwägen Sie die Hochskalierung Ihrer Azure Web PubSub-Dienstinstanz. Wenn im Diagnoseprotokoll `Message count reaches limit` vorkommt, bedeutet dies, dass Sie den kostenlosen Tarif (Free) verwenden und im Begriff sind, das Kontingent für Nachrichten aufzubrauchen. Wenn Sie weitere Nachrichten senden möchten, sollten Sie für die Azure Web PubSub-Dienstinstanz einen Wechsel in den Standardtarif erwägen, um zusätzliche Nachrichten zu senden. Weitere Informationen finden Sie unter [Azure Web PubSub-Dienst – Preise](https://azure.microsoft.com/pricing/details/web-pubsub/).