---
title: Anzeigen und Erstellen von Abfragen für Logik-Apps in Azure Monitor-Protokolle
description: Anzeigen und Erstellen von Abfragen in Azure Monitor-Protokolle für Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: divswa, logicappspm
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: cb1af437fc663fcb95c768ec295862209a26064e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87090398"
---
# <a name="view-and-create-queries-for-monitoring-and-tracking-in-azure-monitor-logs-for-azure-logic-apps"></a>Anzeigen und Erstellen von Abfragen zur Überwachung und Nachverfolgung in Azure Monitor-Protokolle für Azure Logic Apps

Sie können die zugrunde liegenden Abfragen anzeigen, die die Ergebnisse aus [Azure Monitor Protokollen](../azure-monitor/log-query/log-query-overview.md) erzeugen, und Abfragen erstellen, die die Ergebnisse auf Grundlage Ihrer spezifischen Kriterien filtern. So können Sie beispielsweise anhand einer bestimmten Austauschkontrollnummer nach Nachrichten suchen. Abfragen verwenden die [Kusto-Abfragesprache](https://aka.ms/LogAnalyticsLanguageReference), die Sie bearbeiten können, wenn Sie andere Ergebnisse anzeigen möchten. Weitere Informationen finden Sie unter [Azure Monitor-Protokolle für Abfragen](../azure-monitor/log-query/query-language.md).

## <a name="prerequisites"></a>Voraussetzungen

* Einen Log Analytics-Arbeitsbereich Falls Sie keinen Log Analytics-Arbeitsbereich besitzen, lesen Sie die Informationen zum [Erstellen eines Log Analytics-Arbeitsbereichs](../azure-monitor/learn/quick-create-workspace.md).

* Eine Logik-App, die mit Azure Monitor-Protokollierung eingerichtet ist und diese Informationen an einen Log Analytics-Arbeitsbereich sendet. Erfahren Sie, [wie Sie Azure Monitor-Protokolle für Ihre Logik-App einrichten](../logic-apps/monitor-logic-apps.md).

* Wenn Sie ein Integrationskonto verwenden, stellen Sie sicher, dass Sie das Konto mit Azure Monitor-Protokollierung eingerichtet haben, um diese Informationen an einen Log Analytics-Arbeitsbereich zu senden. Erfahren Sie, wie Sie [die Azure Monitor-Protokollierung für Ihr Integrationskonto einrichten](../logic-apps/monitor-b2b-messages-log-analytics.md).

## <a name="view-queries-behind-results"></a>Anzeigen von Abfragen hinter Ergebnissen

Gehen Sie folgendermaßen vor, um die Abfrage anzuzeigen oder zu bearbeiten, die die Ergebnisse in Ihrer Arbeitsbereichszusammenfassung erzeugt:

1. Wählen Sie auf einer beliebigen Ergebnisseite im unteren Bereich **Alle anzeigen** aus.

   ![Anzeigen aller Ergebnisse](./media/create-monitoring-tracking-queries/logic-app-see-all.png)

   Die Seite „Protokolle“ wird geöffnet und zeigt die Abfrage hinter der vorherigen Ergebnisseite an.

   ![Seite „Protokolle“ – Abfrageansicht](./media/create-monitoring-tracking-queries/view-query-behind-results.png)

1. Auf der Seite **Protokolle** können Sie diese Optionen auswählen:

   * Um die Abfrageergebnisse als Tabelle anzuzeigen, wählen Sie **Tabelle** aus.

   * Aktualisieren Sie zum Ändern der Abfrage die Abfragezeichenfolge, und wählen Sie **Ausführen** aus, um die Ergebnisse in der Tabelle anzuzeigen.

## <a name="create-your-own-query"></a>Erstellen Ihrer eigenen Abfrage

Um Ergebnisse auf Grundlage bestimmter Eigenschaften oder Werte zu suchen oder zu filtern, können Sie eine eigene Abfrage erstellen, indem Sie mit einer leeren Abfrage anfangen oder eine vorhandene Abfrage verwenden. Weitere Informationen finden Sie unter [Erste Schritte mit Protokollabfragen in Azure Monitor](../azure-monitor/log-query/get-started-queries.md).

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) Ihren Log Analytics-Arbeitsbereich, und wählen Sie ihn aus.

1. Wählen Sie im Menü Ihres Arbeitsbereichs unter **Allgemein** die Option **Protokolle** aus.

1. Beginnen Sie mit einer leeren Abfrage oder einer verfügbaren vorhandenen Abfrage.

   * Um zu überprüfen, ob vorhandene Abfragen verfügbar sind, wählen Sie auf der Abfragesymbolleiste entweder **Beispielabfragen** > **Verlauf** aus, womit Abfragen aus vorherigen Abfrageausführungen angezeigt werden, oder wählen Sie **Abfrage-Explorer** aus, in dem vordefinierte Abfragen angezeigt werden.

     Die B2B-Logik-App-Verwaltungslösung stellt beispielsweise diese vordefinierten Abfragen bereit:

     ![Mit vordefinierten Abfragen der Lösung „B2B-Logik-App-Verwaltung“ beginnen](./media/create-monitoring-tracking-queries/b2b-prebuilt-queries.png)

   * Um mit einer leeren Abfrage zu beginnen, geben Sie im Abfrage-Editor die [Kusto-Abfragesprache](../azure-monitor/log-query/query-language.md) für Ihre Abfrage ein.

     ![Beginnen mit einer leeren Abfrage](./media/create-monitoring-tracking-queries/create-query-from-blank.png)

## <a name="next-steps"></a>Nächste Schritte

* [AS2-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Benutzerdefinierte Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)
