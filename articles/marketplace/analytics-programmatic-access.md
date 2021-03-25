---
title: Paradigma für den programmgesteuerten Zugriff
description: Machen Sie sich mit den Grundlagen des API-Aufrufmusters für programmgesteuerte Analysen vertraut. Die APIs für den Zugriff auf Analyseberichte im kommerziellen Microsoft-Marketplace werden ebenfalls behandelt.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 8e0b94a46e96dd8ba16040e16b421520eb67de19
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102583610"
---
# <a name="programmatic-access-paradigm"></a>Paradigma für den programmgesteuerten Zugriff

Dieses Diagramm zeigt das API-Aufrufmuster zum Erstellen neuer Berichtsvorlagen, Planen von benutzerdefinierten Berichten und Abrufen von Fehlerdaten.

[![Veranschaulicht das API-Aufrufmuster zum Erstellen neuer Berichtsvorlagen, Planen von benutzerdefinierten Berichten und Abrufen von Fehlerdaten](./media/analytics-programmatic-access/analytics-high-level-flow.png)](./media/analytics-programmatic-access/analytics-high-level-flow.png#lightbox)
***Abbildung 1: Allgemeiner Aufbau des API-Aufrufmusters***

Weitere Details zu Abbildung 1:

1. Die Clientanwendung kann das benutzerdefinierte Berichtsschema bzw. die benutzerdefinierte Vorlage definieren, indem sie die [API zum Erstellen von Berichtsabfragen](#create-report-query-api) aufruft. Alternativ können Sie auch eine Berichtsvorlage (`QueryId`) aus der [Liste der Systemabfragen](analytics-system-queries.md) verwenden.
1. Wenn der Vorgang erfolgreich ist, gibt die API zum Erstellen von Berichtsvorlagen `QueryId` zurück.
1. Die Clientanwendung ruft dann die [API zum Erstellen von Berichten](#create-report-api) mithilfe von `QueryID` zusammen mit dem Startdatum des Berichts, dem Wiederholungsintervall, der Serie und einem optionalen Rückruf-URI auf.
1. Wenn der Vorgang erfolgreich ist, gibt die [API zum Erstellen von Berichten](#create-report-api) `ReportID` zurück.
1. Die Clientanwendung wird über den Rückruf-URI benachrichtigt, sobald die Berichtsdaten zum Download bereit sind.
1. Die Clientanwendung verwendet dann die [API zum Abrufen von Berichtsausführungen](#get-report-executions-api), um den Status des Berichts mit der `Report ID` und dem Datumsbereich abzufragen.
1. Bei erfolgreicher Ausführung wird der Link zum Herunterladen des Berichts zurückgegeben, und die Anwendung kann den Download der Daten initiieren.

## <a name="report-query-language-specification"></a>Spezifikation der Berichtsabfragesprache

Wir stellen Ihnen zwar [Systemabfragen](analytics-system-queries.md) zur Verfügung, die Sie zum Erstellen von Berichten verwenden können, Sie können aber auch eigene Abfragen basierend auf den Anforderungen Ihres Unternehmens erstellen. Weitere Informationen zu benutzerdefinierten Abfragen finden Sie unter [Spezifikation für benutzerdefinierte Abfragen](analytics-custom-query-specification.md).

## <a name="create-report-query-api"></a>Erstellen einer Berichtsabfrage-API

Mithilfe dieser API können Sie benutzerdefinierte Abfragen erstellen, mit denen das Dataset definiert wird, aus dem Spalten und Metriken exportiert werden müssen. Die API bietet die Flexibilität, die für das Erstellen einer neuen Berichtsvorlage notwendig ist, die auf den Anforderungen Ihres Unternehmens basiert.

Sie können ebenfalls die von uns bereitgestellten [Systemabfragen](analytics-system-queries.md) verwenden. Wenn keine benutzerdefinierte Berichtsvorlagen benötigt werden, können Sie die [API zum Erstellen von Berichten](#create-report-api) direkt mithilfe der [QueryIDs](analytics-system-queries.md) der von uns bereitgestellten Systemabfragen verwenden.

Im folgenden Beispiel wird gezeigt, wie Sie eine benutzerdefinierte Abfrage erstellen können, um _einen normalisierten Verbrauch und Kostenschätzungen für kostenpflichtige SKUs_ aus dem Dataset [ISVUsage](analytics-make-your-first-api-call.md#programmatic-api-call) für den letzten Monat zu erhalten.

*Anforderungssyntax*

| Methode | Anforderungs-URI |
| ------------ | ------------- |
| POST | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledQueries` |
|||

*Anforderungsheader*

| Header | type | BESCHREIBUNG |
| ------------- | ------------- | ------------- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken (Azure Active Directory). Das Format ist `Bearer <token>`. |
| Content-Type | `string` | `application/JSON` |
||||

*Pfadparameter*

Keine

*Query parameter (Abfrageparameter)*

Keine

*Beispiel für Anforderungsnutzdaten*

```json
{
    "Name": "ISVUsageQuery",
    "Description": "Normalized Usage and Estimated Financial Charges for PAID SKUs",
    "Query": "SELECT UsageDate, NormalizedUsage, EstimatedExtendedChargePC FROM ISVUsage WHERE SKUBillingType = 'Paid' ORDER BY UsageDate DESC TIMESPAN LAST_MONTH"
}
```

*Glossar*

Diese Tabelle enthält die Schlüsseldefinitionen der Elemente in den Anforderungsnutzdaten.

| Parameter | Erforderlich | BESCHREIBUNG | Zulässige Werte |
| ------------ | ------------- | ------------- | ------------- |
| `Name` | Ja | Anzeigename der Abfrage | Zeichenfolge |
| `Description` | Nein | Beschreibung der Rückgabe der Abfrage | Zeichenfolge |
| `Query` | Ja | Zeichenfolge der Berichtsabfrage | Datentyp: String<br>[Benutzerdefinierte Abfrage](analytics-sample-queries.md), die auf den Unternehmensanforderungen basiert |
|||||

> [!NOTE]
> Beispiele für benutzerdefinierte Abfragen finden Sie unter [diesem Link](analytics-sample-queries.md).

*Beispiel für eine Antwort*

Die Antwortnutzdaten sind wie folgt strukturiert:

Antwortcodes: 200, 400, 401, 403, 500

Beispiel einer Antwortnutzlast:

```json
{
  "value": [
        {
            "queryId": "78be43f2-e35f-491a-8cd5-78fe14194f9c",
            "name": " ISVUsageQuery",
            "description": "Normalized Usage and Estimated Financial Charges for PAID SKUs”,
            "query": " SELECT UsageDate, NormalizedUsage, EstimatedExtendedChargePC FROM ISVUsage WHERE SKUBillingType = 'Paid' ORDER BY UsageDate DESC TIMESPAN LAST_MONTH",
            "type": "userDefined",
            "user": "142344300",
            "createdTime": "2021-01-06T05:38:34Z"
        }
    ],
    "totalCount": 1,
    "message": "Query created successfully",
    "statusCode": 200
}
```

*Glossar*

Diese Tabelle enthält die Schlüsseldefinitionen der Elemente in der Antwort.

| Parameter | Beschreibung |
| ------------ | ------------- |
| `QueryId` | Universally Unique Identifier (UUID) der von Ihnen erstellten Abfrage |
| `Name` | Anzeigename für die Abfrage in den Anforderungsnutzdaten |
| `Description` | Die Beschreibung, die der Abfrage bei ihrer Erstellung hinzugefügt wurde |
| `Query` | Bei der Erstellung der Abfrage als Eingabe übergebene Berichtsabfrage |
| `Type` | Legen Sie den Wert `userDefined` |
| `User` | Benutzer-ID, die zum Erstellen der Abfrage verwendet wird |
| `CreatedTime` | Die UTC-Zeit, zu der die Abfrage erstellt wurde, im folgenden Format: yyyy-MM-ddTHH:mm:ssZ |
| `TotalCount` | Anzahl von Datasets im Wertarray |
| `StatusCode` | Ergebniscode<br>Die möglichen Werte sind 200, 400, 401, 403, 500. |
| `message` | Statusmeldung der API-Ausführung |
|||

## <a name="create-report-api"></a>Erstellen einer Berichts-API

Wenn Sie erfolgreich eine benutzerdefinierte Berichtsvorlage erstellt und die `QueryID` als Teil der Antwort der [API zum Erstellen von Berichten](#create-report-query-api) erhalten haben, kann diese API aufgerufen werden, um eine Abfrage zu planen, die in regelmäßigen Abständen ausgeführt werden soll. Sie können eine Häufigkeit und einen Zeitplan für die Übermittlung des Berichts festlegen. Für die von uns bereitgestellten Systemabfragen kann die API zum Erstellen von Berichten ebenfalls mit der [QueryID](analytics-sample-queries.md) abgerufen werden.

*Anforderungssyntax*

| Methode | Anforderungs-URI |
| ------------ | ------------- |
| POST | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledReport` |
|||

*Anforderungsheader*

| Header | type | BESCHREIBUNG |
| ------ | ---- | ----------- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken (Azure Active Directory). Das Format ist `Bearer <token>`. |
| Inhaltstyp | Zeichenfolge | `application/JSON` |
||||

*Pfadparameter*

Keine

*Query parameter (Abfrageparameter)*

Keine

*Beispiel für Anforderungsnutzdaten*

```json
{
  "ReportName": "ISVUsageReport",
  "Description": "Normalized Usage and Estimated Financial Charges for PAID SKUs",
  "QueryId": "78be43f2-e35f-491a-8cd5-78fe14194f9c ",
  "StartTime": "2021-01-06T19:00:00Z ",
  "RecurrenceInterval": 48,
  "RecurrenceCount": 20,
  "Format": "csv",
  "CallbackUrl": "https://<SampleCallbackUrl>"
}
```

*Glossar*

Diese Tabelle enthält die Schlüsseldefinitionen der Elemente in den Anforderungsnutzdaten.

| Parameter | Erforderlich | BESCHREIBUNG | Zulässige Werte |
| ------------ | ------------- | ------------- | ------------- |
| `ReportName` | Ja | Name, der dem Bericht zugewiesen ist | Zeichenfolge |
| `Description` | Nein | Beschreibung des erstellten Berichts | Zeichenfolge |
| `QueryId` | Ja | Abfrage-ID für den Bericht | Zeichenfolge |
| `StartTime` | Ja | Der UTC-Zeitstempel für den Start der Berichtsgenerierung.<br>Das Format sollte wie folgt lauten: yyyy-MM-ddTHH:mm:ssZ. | Zeichenfolge |
| `RecurrenceInterval` | Ja | Die Häufigkeit (in Stunden), mit der der Bericht generiert werden soll.<br>Der Mindestwert ist 4 und der Höchstwert 90. | integer |
| `RecurrenceCount` | Nein | Anzahl der zu generierenden Berichte | integer |
| `Format` | Nein | Dateiformat für die exportierte Datei.<br>Das Standardformat ist CSV. | CSV/TSV |
| `CallbackUrl` | Nein | Öffentlich erreichbare URL, die optional als Rückrufziel konfiguriert werden kann | Zeichenfolge (HTTP-URL) |
| `ExecuteNow` | Nein | Dieser Parameter sollte verwendet werden, um einen Bericht zu erstellen, der nur einmal ausgeführt wird. `StartTime`, `RecurrenceInterval` und `RecurrenceCount` werden ignoriert, wenn dieser Parameter auf `true` festgelegt ist. Der Bericht wird sofort und asynchron ausgeführt. | TRUE/FALSE |
| `QueryStartTime` | Nein | Gibt optional die Startzeit für die Abfrage an, die die Daten extrahiert. Dieser Parameter ist nur für einen Zeitausführungsbericht anwendbar, bei dem `ExecuteNow` auf `true` festgelegt ist. Das Format sollte wie folgt lauten: yyyy-MM-ddTHH:mm:ssZ. | Zeitstempel als Zeichenfolge |
| `QueryEndTime` | Nein | Gibt optional die Startzeit für die Abfrage an, die die Daten extrahiert. Dieser Parameter ist nur für einen Zeitausführungsbericht anwendbar, bei dem `ExecuteNow` auf `true` festgelegt ist. Das Format sollte wie folgt lauten: yyyy-MM-ddTHH:mm:ssZ. | Zeitstempel als Zeichenfolge |
|||||

*Beispiel für eine Antwort*

Die Antwortnutzdaten sind wie folgt strukturiert:

Antwortcode: 200, 400, 401, 403, 404, 500

Antwortnutzlast:

```json
{
  "Value": [
    {
            "reportId": "72fa95ab-35f5-4d44-a1ee-503abbc88003",
            "reportName": "ISVUsageReport",
            "description": "Normalized Usage and Estimated Financial Charges for PAID SKUs",
            "queryId": "78be43f2-e35f-491a-8cd5-78fe14194f9c",
            "query": "SELECT UsageDate, NormalizedUsage, EstimatedExtendedChargePC FROM ISVUsage WHERE SKUBillingType = 'Paid' ORDER BY UsageDate DESC TIMESPAN LAST_MONTH",
            "user": "142344300",
            "createdTime": "2021-01-06T05:46:00Z",
            "modifiedTime": null,
            "startTime": "2021-01-06T19:00:00Z",
            "reportStatus": "Active",
            "recurrenceInterval": 48,
            "recurrenceCount": 20,
            "callbackUrl": "https://<SampleCallbackUrl>",
            "format": "csv"    
    }
  ],
  "TotalCount": 1,
  "Message": "Report created successfully",
  "StatusCode": 200
}
```

*Glossar*

Diese Tabelle enthält die Schlüsseldefinitionen der Elemente in der Antwort.

| Parameter | Beschreibung |
| ------------ | ------------- |
| `ReportId` | Universally Unique Identifier (UUID) des von Ihnen erstellten Berichts |
| `ReportName` | Name für den Bericht in den Anforderungsnutzdaten |
| `Description` | Beschreibung, die während der Erstellung des Berichts angegeben wurde |
| `QueryId` | Die Abfrage-ID, die zum Zeitpunkt der Erstellung des Berichts übermittelt wurde |
| `Query` | Abfragetext, der für diesen Bericht ausgeführt wird |
| `User` | Benutzer-ID, die zum Erstellen des Berichts verwendet wird |
| `CreatedTime` | Die UTC-Zeit, zu der der Bericht erstellt wurde, im folgenden Format: yyyy-MM-ddTHH:mm:ssZ |
| `ModifiedTime` | Die UTC-Zeit, zu der der Bericht zuletzt geändert wurde, im folgenden Format: yyyy-MM-ddTHH:mm:ssZ |
| `StartTime` | Die UTC-Zeit, zu der die Berichtsausführung beginnt, im folgenden Format: yyyy-MM-ddTHH:mm:ssZ |
| `ReportStatus` | Status der Berichtsausführung. Die möglichen Werte lauten **Angehalten**, **Aktiv** und **Inaktiv**. |
| `RecurrenceInterval` | Wiederholungsintervall, das bei der Erstellung des Berichts angegeben wurde |
| `RecurrenceCount` | Anzahl der Wiederholungen, die bei der Erstellung des Berichts angegeben wurde |
| `CallbackUrl` | Rückruf-URL, die in der Anforderung angegeben wurde |
| `Format` | Format der Berichtsdateien Mögliche Werte sind „CSV“ oder „TSV“. |
| `TotalCount` | Anzahl von Datasets im Wertarray |
| `StatusCode` | Ergebniscode<br>Die möglichen Werte sind 200, 400, 401, 403, 500. |
| `message` | Statusmeldung der API-Ausführung |
|||

## <a name="get-report-executions-api"></a>API zum Abrufen von Berichtsausführungen

Sie können diese Methode verwenden, um den Status einer Berichtsausführung mithilfe der von der [API zum Erstellen von Berichten](#create-report-api) empfangenen `ReportId` abzufragen. Die Methode gibt den Link zum Herunterladen des Berichts zurück, wenn der Bericht zum Download bereit ist. Andernfalls gibt die Methode den Status zurück. Sie können diese API auch verwenden, um alle Ausführungen für einen bestimmten Bericht abzurufen.

> [!IMPORTANT]
> Diese API verfügt über Standardabfrageparameter, die für `executionStatus=Completed` und `getLatestExecution=true` festgelegt sind. Daher wird beim Aufrufen der API vor der ersten erfolgreichen Ausführung des Berichts der Fehler 404 zurückgegeben. Ausstehende Ausführungen können durch Festlegen von `executionStatus=Pending` abgerufen werden.

*Anforderungssyntax*

| Methode | Anforderungs-URI |
| ------------ | ------------- |
| Herunterladen | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledReport/execution/{reportId}?executionId={executionId}&executionStatus={executionStatus}&getLatestExecution={getLatestExecution}` |
|||

*Anforderungsheader*

| Header | type | BESCHREIBUNG |
| ------ | ------ | ------ |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken (Azure Active Directory). Das Format ist `Bearer <token>`. |
| Inhaltstyp | Zeichenfolge | `application/json` |
||||

*Pfadparameter*

Keine

*Query parameter (Abfrageparameter)*

| Parametername | Erforderlich | type | BESCHREIBUNG |
| ------------ | ------------- | ------------- | ------------- |
| `reportId` | Ja | Zeichenfolge | Filter, mit dem Sie ausschließlich Ausführungsdetails zu Berichten abrufen können, wobei die `reportId` in diesem Argument angegeben ist. Sie können mehrere `reportIds` angeben, indem Sie sie durch ein Semikolon („;“) voneinander trennen. |
| `executionId` | Nein | Zeichenfolge | Filter, mit dem Sie ausschließlich Details zu Berichten abrufen können, wobei die `executionId` in diesem Argument angegeben ist. Sie können mehrere `executionIds` angeben, indem Sie sie durch ein Semikolon („;“) voneinander trennen. |
| `executionStatus` | Nein | Zeichenfolge/Enumeration | Filter, mit dem Sie ausschließlich Details zu Berichten abrufen können, wobei die `executionStatus` in diesem Argument angegeben ist.<br>Gültige Werte sind `Pending`, `Running`, `Paused` und `Completed`. <br>Der Standardwert lautet `Completed`. Sie können mehrere Status angeben, indem Sie sie durch ein Semikolon („;“) voneinander trennen. |
| `getLatestExecution` | Nein | boolean | Die API gibt Details der aktuellen Berichtsausführung zurück.<br>Dieser Parameter ist standardmäßig auf `true` festgelegt. Wenn Sie den Wert dieses Parameters als `false` übergeben, gibt die API die Ausführungsinstanzen der letzten 90 Tage zurück. |
|||||

*Anforderungsnutzlast*

Keine

*Beispiel für eine Antwort*

Die Antwortnutzdaten sind wie folgt strukturiert:

Antwortcodes: 200, 400, 401, 403, 404, 500

Beispiel einer Antwortnutzlast:

```json
{
    "value": [
        {
            "executionId": "a0bd78ad-1a05-40fa-8847-8968b718d00f",
            "reportId": "72fa95ab-35f5-4d44-a1ee-503abbc88003",
            "recurrenceInterval": 4,
            "recurrenceCount": 10,
            "callbackUrl": null,
            "format": "csv",
            "executionStatus": "Completed",
            "reportAccessSecureLink": "https://<path to report for download>",
            "reportExpiryTime": null,
            "reportGeneratedTime": "2021-01-13T14:40:46Z"
        }
    ],
    "totalCount": 1,
    "message": null,
    "statusCode": 200
}
```

Wenn die Berichtsausführung beendet ist, wird der Ausführungsstatus `Completed` angezeigt. Sie können den Bericht herunterladen, indem Sie die URL im `reportAccessSecureLink` auswählen.

*Glossar*

Wichtige Definitionen der Elemente in der Antwort

| Parameter | Beschreibung |
| ------------ | ------------- |
| `ExecutionId` | Universally Unique Identifier (UUID) der Ausführungsinstanz |
| `ReportId` | Berichts-ID, die der Ausführungsinstanz zugeordnet ist |
| `RecurrenceInterval` | Wiederholungsintervall, das bei der Erstellung des Berichts angegeben wurde |
| `RecurrenceCount` | Anzahl der Wiederholungen, die bei der Erstellung des Berichts angegeben wurde |
| `CallbackUrl` | Rückruf-URL, die der Ausführungsinstanz zugeordnet ist |
| `Format` | Format der generierten Datei am Ende der Ausführung |
| `ExecutionStatus` | Status der Berichtsausführungsinstanz<br>Gültige Werte sind `Pending`, `Running`, `Paused` und `Completed`. |
| `ReportAccessSecureLink` | Link, über den sicher auf den Bericht zugegriffen werden kann |
| `ReportExpiryTime` | Die UTC-Zeit, nach der der Berichtslink abläuft, im folgenden Format: yyyy-mm-ddThh: mm: SSZ |
| `ReportGeneratedTime` | Die UTC-Zeit, zu der der Bericht generiert wurde, im folgenden Format: yyyy-mm-ddThh: mm: SSZ |
| `TotalCount` | Anzahl von Datasets im Wertarray |
| `StatusCode` | Ergebniscode <br>Die möglichen Werte sind 200, 400, 401, 403, 404 und 500 |
| `message` | Statusmeldung der API-Ausführung |
|||

## <a name="next-steps"></a>Nächste Schritte
- Sie können die APIs über die [Swagger-API-URL](https://api.partnercenter.microsoft.com/insights/v1/cmp/swagger/index.html) testen.
- Einstieg in den programmgesteuerten Zugriff auf Analysedaten
