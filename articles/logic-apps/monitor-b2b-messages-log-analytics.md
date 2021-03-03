---
title: Überwachen von B2B-Nachrichten mit Azure Monitor
description: Behandeln von Problemen mit AS2-, X12- und EDIFACT-Nachrichten durch Einrichten von Azure Monitor-Protokollen und Sammeln von Diagnosedaten für Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: divswa, logicappspm
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 765c15897bd5d435503d3bef07e76a93b148971c
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100596858"
---
# <a name="set-up-azure-monitor-logs-and-collect-diagnostics-data-for-b2b-messages-in-azure-logic-apps"></a>Einrichten von Azure Monitor-Protokollen und Sammeln von Diagnosedaten für B2B-Nachrichten in Azure Logic Apps

Nachdem Sie die B2B-Kommunikation zwischen Handelspartnern in Ihrem Integrationskonto eingerichtet haben, können diese Partner untereinander Nachrichten mit Protokollen wie AS2, X12 und EDIFACT austauschen. Um zu überprüfen, ob diese Kommunikation erwartungsgemäß funktioniert, können Sie [Azure Monitor-Protokolle](../azure-monitor/logs/data-platform-logs.md) für Ihr Integrationskonto einrichten. [Azure Monitor](../azure-monitor/overview.md) hilft Ihnen bei der Überwachung Ihrer Cloud- und lokalen Umgebungen, sodass Sie deren Verfügbarkeit und Leistung leichter sicherstellen können. Unter Verwendung von Azure Monitor-Protokollen können Sie Informationen zu Laufzeitdaten und -ereignissen aufzeichnen und speichern, z. B. Triggerereignisse, Ausführungsereignisse und Aktionsereignisse in einem [Log Analytics-Arbeitsbereich](../azure-monitor/essentials/resource-logs.md#send-to-log-analytics-workspace). Bei Nachrichten sammelt die Protokollierung außerdem Informationen wie:

* Anzahl und Status von Nachrichten
* Bestätigungsstatus
* Korrelationen zwischen Nachrichten und Bestätigungen
* Ausführliche Fehlerbeschreibung für Fehler

Mit Azure Monitor-Protokollen können Sie [Protokollabfragen](../azure-monitor/logs/log-query-overview.md) erstellen, die Ihnen helfen, diese Informationen zu finden und zu überprüfen. Sie können [diese Diagnosedaten auch mit anderen Azure-Diensten verwenden](../logic-apps/monitor-logic-apps-log-analytics.md#extend-data), z. B. Azure Storage und Azure Event Hubs.

Um die Protokollierung für Ihr Integrationskonto einzurichten, [installieren Sie die B2B-Logik-App-Verwaltungslösung](#install-b2b-solution) im Azure-Portal. Diese Lösung stellt aggregierte Informationen für B2B-Nachrichtenereignisse bereit. Um dann die Protokollierung zu aktivieren und Abfragen für diese Informationen zu erstellen, [richten Sie Azure Monitor-Protokolle ein](#set-up-resource-logs).

In diesem Artikel wird gezeigt, wie Sie die Azure Monitor-Protokollierung für Ihr Integrationskonto aktivieren.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Einen Log Analytics-Arbeitsbereich Falls Sie keinen Log Analytics-Arbeitsbereich besitzen, lesen Sie die Informationen zum [Erstellen eines Log Analytics-Arbeitsbereichs](../azure-monitor/logs/quick-create-workspace.md).

* Eine Logik-App, die mit Azure Monitor-Protokollierung eingerichtet ist und diese Informationen an einen Log Analytics-Arbeitsbereich sendet. Erfahren Sie, [wie Sie Azure Monitor-Protokolle für Ihre Logik-App einrichten](../logic-apps/monitor-logic-apps.md).

* Ein mit Ihrer Logik-App verknüpftes Integrationskonto. Erfahren Sie, [wie Sie Ihr Integrationskonto mit Ihrer Logik-App verknüpfen](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

<a name="install-b2b-solution"></a>

## <a name="install-logic-apps-b2b-solution"></a>Installieren der Lösung B2B-Logik-App-Verwaltung

Bevor Azure Monitor-Protokolle die B2B-Nachrichten für Ihre Logik-App nachverfolgen können, fügen Sie Ihrem Log Analytics-Arbeitsbereich die Lösung **B2B-Logik-App-Verwaltung** hinzu.

1. Geben Sie in das Suchfeld des [Azure-Portals](https://portal.azure.com) `log analytics workspaces` ein, und wählen Sie dann **Log Analytics-Arbeitsbereiche** aus.

   ![Auswählen von „Log Analytics-Arbeitsbereiche“](./media/monitor-b2b-messages-log-analytics/find-select-log-analytics-workspaces.png)

1. Wählen Sie unter **Log Analytics-Arbeitsbereiche** Ihren Arbeitsbereich aus.

   ![Auswählen Ihres Log Analytics-Arbeitsbereichs](./media/monitor-b2b-messages-log-analytics/select-log-analytics-workspace.png)

1. Wählen Sie auf der Übersichtsseite unter **Erste Schritte mit Log Analytics** > **Überwachungslösungen konfigurieren** die Option **Lösungen anzeigen** aus.

   ![Auswählen von „Lösungen anzeigen“ im Bereich „Übersicht“](./media/monitor-b2b-messages-log-analytics/log-analytics-workspace.png)

1. Wählen Sie im Bereich „Übersicht“ die Option **Hinzufügen** aus.

   ![Hinzufügen einer neuen Lösung im Bereich „Übersicht“](./media/monitor-b2b-messages-log-analytics/add-logic-apps-management-solution.png)

1. Geben Sie, nachdem der **Marketplace** geöffnet wurde, in das Suchfeld `logic apps b2b` ein, und wählen Sie **B2B-Logik-App-Verwaltung** aus.

   ![Im Marketplace „Logic Apps-Verwaltung“ auswählen](./media/monitor-b2b-messages-log-analytics/select-logic-apps-b2b-solution.png)

1. Wählen Sie im Bereich mit der Lösungsbeschreibung **Erstellen** aus.

   ![„Erstellen“ auswählen, um die Lösung „B2B-Logik-App-Verwaltung“ hinzuzufügen](./media/monitor-b2b-messages-log-analytics/create-logic-apps-b2b-solution.png)

1. Überprüfen und bestätigen Sie den Log Analytics-Arbeitsbereich, in dem Sie die Lösung installieren möchten, und wählen Sie erneut **Erstellen** aus.

   ![Auswählen von „Erstellen“ für „B2B-Logik-App-Verwaltung“](./media/monitor-b2b-messages-log-analytics/confirm-log-analytics-workspace.png)

   Nachdem Azure die Lösung in der Azure-Ressourcengruppe bereitgestellt hat, die den Log Analytics Arbeitsbereich enthält, wird die Lösung im Zusammenfassungsbereich Ihres Arbeitsbereichs angezeigt. Beim Verarbeiten von B2B-Nachrichten wird die Nachrichtenanzahl in diesem Bereich aktualisiert.

   ![Zusammenfassungsbereich des Arbeitsbereichs](./media/monitor-b2b-messages-log-analytics/b2b-overview-messages-summary.png)

<a name="set-up-resource-logs"></a>

## <a name="set-up-azure-monitor-logs"></a>Einrichten von Azure Monitor-Protokollen

Sie können die Azure Monitor-Protokollierung direkt in Ihrem Integrationskonto aktivieren.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrem Integrationskonto, und wählen Sie es aus.

   ![Suchen und Auswählen Ihres Integrationskontos](./media/monitor-b2b-messages-log-analytics/find-integration-account.png)

1. Wählen Sie im Menü Ihres Integrationskontos unter **Überwachung** die Option **Diagnoseeinstellungen** aus. Klicken Sie auf **Diagnoseeinstellung hinzufügen**.

   ![Auswählen von „Diagnoseeinstellungen“ unter „Überwachung“](./media/monitor-b2b-messages-log-analytics/monitor-diagnostics-settings.png)

1. Führen Sie die folgenden Schritte aus, um die Einstellung zu erstellen:

   1. Geben Sie einen Namen für die Einstellung an.

   1. Wählen Sie **An Log Analytics senden** aus.

   1. Wählen Sie für **Abonnement** das Azure-Abonnement aus, das Ihrem Log Analytics-Arbeitsbereich zugeordnet ist.

   1. Wählen Sie für **Log Analytics-Arbeitsbereich** den Arbeitsbereich aus, den Sie verwenden möchten.

   1. Wählen Sie unter **Protokoll** die Kategorie **IntegrationAccountTrackingEvents** aus, die die Ereigniskategorie angibt, die Sie aufzeichnen möchten.

   1. Klicken Sie auf **Speichern**, wenn Sie fertig sind.

   Beispiel: 

   ![Einrichten von Azure Monitor-Protokollen zum Sammeln von Diagnosedaten](./media/monitor-b2b-messages-log-analytics/send-diagnostics-data-log-analytics-workspace.png)

<a name="view-message-status"></a>

## <a name="view-message-status"></a>Anzeigen des Nachrichtenstatus

Im Anschluss an die Ausführung Ihrer Logik-App können Sie den Status und die Daten zu diesen Nachrichten in Ihrem Log Analytics-Arbeitsbereich anzeigen.

1. Suchen Sie im Suchfeld des [Azure-Portals](https://portal.azure.com) Ihren Log Analytics-Arbeitsbereich, und öffnen Sie ihn.

1. Wählen Sie im Menü Ihres Arbeitsbereichs **Arbeitsbereichszusammenfassung** > **B2B-Logik-App-Verwaltung** aus.

   ![Zusammenfassungsbereich des Arbeitsbereichs](./media/monitor-b2b-messages-log-analytics/b2b-overview-messages-summary.png)

   > [!NOTE]
   > Wenn auf der Kachel „B2B-Logik-App-Verwaltung“ nicht sofort nach einer Ausführung Ergebnisse angezeigt werden, wählen Sie **Aktualisieren** aus, oder warten Sie einen kurzen Moment, bevor Sie es noch mal versuchen.

   Standardmäßig werden auf der Kachel **B2B-Logik-App-Verwaltung** Daten basierend auf einem einzelnen Tag angezeigt. Wählen Sie oben auf der Seite das Steuerelement für die Bereichsauswahl aus, um für den Datenbereich ein anderes Intervall festzulegen:

   ![Ändern des Intervalls](./media/monitor-b2b-messages-log-analytics/change-summary-interval.png)

1. Nachdem das Dashboard mit dem Nachrichtenstatus angezeigt wird, können Sie weitere Details für einen bestimmten Nachrichtentyp anzeigen, für den Daten basierend auf einem einzelnen Tag angezeigt werden. Wählen Sie die Kachel für **AS2**, **X12** oder **EDIFACT** aus.

   ![Anzeigen des Status für Nachrichten](./media/monitor-b2b-messages-log-analytics/workspace-summary-b2b-messages.png)

   Für die von Ihnen gewählte Kachel wird eine Liste mit Nachrichten angezeigt. Eine AS2-Nachrichtenliste kann beispielsweise wie folgt aussehen:

   ![Status und Details für AS2-Nachrichten](./media/monitor-b2b-messages-log-analytics/as2-message-results-list.png)

   Weitere Informationen zu den Eigenschaften der einzelnen Nachrichtentypen finden Sie unter diesen Beschreibungen von Nachrichteneigenschaften:

   * [AS2-Nachrichteneigenschaften](#as2-message-properties)
   * [X12-Nachrichteneigenschaften](#x12-message-properties)
   * [EDIFACT-Nachrichteneigenschaften](#EDIFACT-message-properties)

<!--
1. To view or export the inputs and outputs for specific messages, select those messages, and select **Download**. When you're prompted, save the .zip file to your local computer, and then extract that file.

   The extracted folder includes a folder for each selected message. If you set up acknowledgements, the message folder also includes files with acknowledgement details. Each message folder has at least these files:
   
   * Human-readable files with the input payload and output payload details
   * Encoded files with the inputs and outputs

   For each message type, you can find the folder and file name formats here:

   * [AS2 folder and file name formats](#as2-folder-file-names)
   * [X12 folder and file name formats](#x12-folder-file-names)
   * [EDIFACT folder and file name formats](#edifact-folder-file-names)

   ![Download message files](./media/monitor-b2b-messages-log-analytics/download-messages.png)

1. To view all actions that have the same run ID, on the **Log Search** page, select a message from the message list.

   You can sort these actions by column, or search for specific results.

   * To search results with prebuilt queries, select **Favorites**.

   * Learn [how to build queries by adding filters](../logic-apps/create-monitoring-tracking-queries.md). Or learn more about [how to find data with log searches in Azure Monitor logs](../azure-monitor/logs/log-query-overview.md).

   * To change query in the search box, update the query with the columns and values that you want to use as filters.
-->

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Eigenschaftsbeschreibungen und Namensformate für AS2-, X12- und EDIFACT-Nachrichten

Für jeden Nachrichtentyp sind hier die Eigenschaftsbeschreibungen und Namensformate für heruntergeladene Nachrichtendateien aufgeführt.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>Eigenschaftsbeschreibungen für AS2-Nachrichten

Hier sind die Eigenschaftsbeschreibungen für die einzelnen AS2-Nachrichten aufgeführt.

| Eigenschaft | BESCHREIBUNG |
|----------|-------------|
| **Sender** | Der unter **Empfangseinstellungen** angegebene Gastpartner oder der unter **Sendeeinstellungen** angegebene Hostpartner für eine AS2-Vereinbarung |
| **Empfänger** | Der unter **Empfangseinstellungen** angegebene Hostpartner oder der unter **Sendeeinstellungen** angegebene Gastpartner für eine AS2-Vereinbarung |
| **Logik-App** | Die Logik-App, in der die AS2-Aktionen eingerichtet wurden |
| **Status** | AS2-Nachrichtenstatus <br>Erfolg = Eine gültige AS2-Nachricht wurde empfangen oder gesendet. Es wird keine MDN eingerichtet. <br>Erfolg = Eine gültige AS2-Nachricht wurde empfangen oder gesendet. Eine MDN wird eingerichtet und empfangen, oder eine MDN wird gesendet. <br>Fehler = Eine ungültige AS2-Nachricht wurde empfangen. Es wird keine MDN eingerichtet. <br>Ausstehend = Eine gültige AS2-Nachricht wurde empfangen oder gesendet. Eine MDN wird eingerichtet, und eine MDN wird erwartet. |
| **ACK** | MDN-Nachrichtenstatus <br>Akzeptiert = Positive MDN wurde empfangen oder gesendet. <br>Ausstehend = Es wird auf das Empfangen oder Senden einer MDN gewartet. <br>Abgelehnt = Eine negative MDN wurde empfangen oder gesendet. <br>Nicht erforderlich = Eine MDN wurde in der Vereinbarung nicht eingerichtet. |
| **Richtung** | Richtung der AS2-Nachricht |
| **Nachverfolgungs-ID** | ID, die alle Trigger und Aktionen in einer Logik-App korreliert |
| **Nachrichten-ID** | AS2-Nachrichten-ID aus den AS2-Nachrichtenheadern |
| **Timestamp** | Der Zeitpunkt, zu dem die AS2-Aktion die Nachricht verarbeitet hat |
|||

<!--
<a name="as2-folder-file-names"></a>

### AS2 name formats for downloaded message files

Here are the name formats for each downloaded AS2 message folder and files.

| Folder or file | Name format |
|----------------|-------------|
| Message folder | [sender]\_[receiver]\_AS2\_[correlation-ID]\_[message-ID]\_[timestamp] |
| Input, output, and if set up, acknowledgement files | **Input payload**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_input_payload.txt </p>**Output payload**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_output\_payload.txt </p></p>**Inputs**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_inputs.txt </p></p>**Outputs**: [sender]\_[receiver]\_AS2\_[correlation-ID]\_outputs.txt |
|||
-->

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>Beschreibungen der Eigenschaften von X12-Nachrichten

Hier sind die Eigenschaftsbeschreibungen für die einzelnen X12-Nachrichten aufgeführt.

| Eigenschaft | BESCHREIBUNG |
|----------|-------------|
| **Sender** | Der unter **Empfangseinstellungen** angegebene Gastpartner oder der unter **Sendeeinstellungen** angegebene Hostpartner für eine X12-Vereinbarung |
| **Empfänger** | Der unter **Empfangseinstellungen** angegebene Hostpartner oder der unter **Sendeeinstellungen** angegebene Gastpartner für eine X12-Vereinbarung |
| **Logik-App** | Die Logik-App, in der die X12-Aktionen eingerichtet wurden |
| **Status** | X12-Nachrichtenstatus <br>Erfolg = Eine gültige X12-Nachricht wurde empfangen oder gesendet. Keine Funktionsbestätigung wird eingerichtet. <br>Erfolg = Eine gültige X12-Nachricht wurde empfangen oder gesendet. Eine Funktionsbestätigung wird eingerichtet und empfangen, oder eine Funktionsbestätigung wird gesendet. <br>Fehler = Eine ungültige X12-Nachricht wurde empfangen oder gesendet. <br>Ausstehend = Eine gültige X12-Nachricht wurde empfangen oder gesendet. Eine Funktionsbestätigung wird eingerichtet, und eine Funktionsbestätigung wird erwartet. |
| **ACK** | Status der Funktionsbestätigung (997) <br>Akzeptiert = Positive Funktionsbestätigung empfangen oder gesendet. <br>Abgelehnt = Negative Funktionsbestätigung empfangen oder gesendet. <br>Ausstehend = Funktionsbestätigung wird erwartet, wurde aber noch nicht empfangen. <br>Ausstehend = Funktionsbestätigung wurde generiert, konnte aber nicht an Partner gesendet werden. <br>Nicht erforderlich = Funktionsbestätigung wurde nicht eingerichtet. |
| **Richtung** | Richtung der X12-Nachricht |
| **Nachverfolgungs-ID** | ID, die alle Trigger und Aktionen in einer Logik-App korreliert |
| **Msg Type** | EDI-X12-Nachrichtentyp |
| **ICN** | Austauschkontrollnummer für die X12-Nachricht |
| **TSCN** | Transaktionssatz-Kontrollnummer für die X12-Nachricht |
| **Timestamp** | Der Zeitpunkt, zu dem die X12-Aktion die Nachricht verarbeitet hat |
|||

<!--
<a name="x12-folder-file-names"></a>

### X12 name formats for downloaded message files

Here are the name formats for each downloaded X12 message folder and files.

| Folder or file | Name format |
|----------------|-------------|
| Message folder | [sender]\_[receiver]\_X12\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Input, output, and if set up, acknowledgement files | **Input payload**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_input_payload.txt </p>**Output payload**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_output\_payload.txt </p></p>**Inputs**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_inputs.txt </p></p>**Outputs**: [sender]\_[receiver]\_X12\_[interchange-control-number]\_outputs.txt |
|||
-->

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>Eigenschaftsbeschreibungen für EDIFACT-Nachrichten

Hier sind die Eigenschaftsbeschreibungen für die einzelnen EDIFACT-Nachrichten aufgeführt.

| Eigenschaft | BESCHREIBUNG |
|----------|-------------|
| **Sender** | Der unter **Empfangseinstellungen** angegebene Gastpartner oder der unter **Sendeeinstellungen** angegebene Hostpartner für eine EDIFACT-Vereinbarung |
| **Empfänger** | Der unter **Empfangseinstellungen** angegebene Hostpartner oder der unter **Sendeeinstellungen** angegebene Gastpartner für eine EDIFACT-Vereinbarung |
| **Logik-App** | Die Logik-App, in der die EDIFACT-Aktionen eingerichtet wurden |
| **Status** | EDIFACT-Nachrichtenstatus <br>Erfolg = Eine gültige EDIFACT-Nachricht wurde empfangen oder gesendet. Keine Funktionsbestätigung wird eingerichtet. <br>Erfolg = Eine gültige EDIFACT-Nachricht wurde empfangen oder gesendet. Eine Funktionsbestätigung wird eingerichtet und empfangen, oder eine Funktionsbestätigung wird gesendet. <br>Fehler = Eine ungültige EDIFACT-Nachricht wurde empfangen oder gesendet. <br>Ausstehend = Eine gültige EDIFACT-Nachricht wurde empfangen oder gesendet. Eine Funktionsbestätigung wird eingerichtet, und eine Funktionsbestätigung wird erwartet. |
| **ACK** | (CONTRL) Status der Funktionsbestätigung <br>Akzeptiert = Positive Funktionsbestätigung empfangen oder gesendet. <br>Abgelehnt = Negative Funktionsbestätigung empfangen oder gesendet. <br>Ausstehend = Funktionsbestätigung wird erwartet, wurde aber noch nicht empfangen. <br>Ausstehend = Funktionsbestätigung wurde generiert, konnte aber nicht an Partner gesendet werden. <br>Nicht erforderlich = Funktionsbestätigung wurde nicht eingerichtet. |
| **Richtung** | Richtung der EDIFACT-Nachricht |
| **Nachverfolgungs-ID** | ID, die alle Trigger und Aktionen in einer Logik-App korreliert |
| **Msg Type** | EDIFACT-Nachrichtentyp |
| **ICN** | Austauschkontrollnummer für die EDIFACT-Nachricht |
| **TSCN** | Transaktionssatz-Kontrollnummer für die EDIFACT-Nachricht |
| **Timestamp** | Der Zeitpunkt, zu dem die EDIFACT-Aktion die Nachricht verarbeitet hat |
|||

<!--
<a name="edifact-folder-file-names"></a>

### EDIFACT name formats for downloaded message files

Here are the name formats for each downloaded EDIFACT message folder and files.

| Folder or file | Name format |
|----------------|-------------|
| Message folder | [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Input, output, and if set up, acknowledgement files | **Input payload**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_input_payload.txt </p>**Output payload**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_output\_payload.txt </p></p>**Inputs**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_inputs.txt </p></p>**Outputs**: [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_outputs.txt |
|||
-->

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen von Überwachungs- und Nachverfolgungsabfragen](../logic-apps/create-monitoring-tracking-queries.md)
