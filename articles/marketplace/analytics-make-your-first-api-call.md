---
title: Erstellen Ihres ersten API-Aufrufs für den Zugriff auf kommerzielle Marketplace-Analysedaten
description: Beispiele für die Verwendung der API für den Zugriff auf kommerzielle Marketplace-Analysedaten.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 9e5fbdfca80d19f026a014a89ffbf137bacb521c
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105639572"
---
# <a name="make-your-first-api-call-to-access-commercial-marketplace-analytics-data"></a>Erstellen Ihres ersten API-Aufrufs für den Zugriff auf kommerzielle Marketplace-Analysedaten

Eine Liste der APIs für den Zugriff auf kommerzielle Marketplace-Analysedaten finden Sie unter [APIs für den Zugriff auf kommerzielle Marketplace-Analysedaten](analytics-available-apis.md). Bevor Sie Ihren ersten API-Befehl durchführen, müssen Sie sicherstellen, dass die [Voraussetzungen](analytics-prerequisites.md) erfüllt sind, um programmgesteuert auf kommerzielle Marketplace-Analysedaten zuzugreifen.

## <a name="token-generation"></a>Tokengenerierung

Bevor Sie eine der Methoden aufrufen, müssen Sie zuerst ein Azure Active Directory-Zugriffstoken (Azure AD) abrufen. Sie müssen das Azure AD-Zugriffstoken an den Autorisierungsheader jeder Methode in der API übergeben. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Nach dem Ablauf können Sie das Token aktualisieren und weiterhin in zukünftigen Aufrufen der API verwenden.

Eine Beispielanforderung zum Generieren eines Tokens finden Sie unten. Die zum Generieren des Tokens erforderlichen drei Werte sind `clientId`, `clientSecret` und `tenantId`. Der `resource`-Parameter sollte auf `https://graph.windows.net` festgelegt werden.

***Anforderungsbeispiel***:

```json
curl --location --request POST 'https://login.microsoftonline.com/{TenantId}/oauth2/token' \
--header 'return-client-request-id: true' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'resource=https://graph.windows.net' \
--data-urlencode 'client_id={client_id}' \
--data-urlencode 'client_secret={client_secret}' \
--data-urlencode 'grant_type=client_credentials'
```

***Antwortbeispiel***:

```json
{
    "token_type": "Bearer",
    "expires_in": "3599",
    "ext_expires_in": "3599",
    "expires_on": "1612794445",
    "not_before": "1612790545",
    "resource": "https://graph.windows.net",
    "access_token": {Token}
}
```

Weitere Informationen zum Abrufen eines Azure AD-Tokens für Ihre Anwendung finden Sie unter [Zugriff auf Analysedaten mithilfe von Speicherdiensten](/windows/uwp/monetize/access-analytics-data-using-windows-store-services#step-2-obtain-an-azure-ad-access-token).

## <a name="programmatic-api-call"></a>Programmgesteuerter API-Aufruf

Nachdem Sie das Azure AD-Token wie im vorherigen Abschnitt beschrieben abgerufen haben, führen Sie die folgenden Schritte aus, um den ersten programmgesteuerten Zugriffsbericht zu erstellen.

Daten können aus den folgenden Datasets (datasetName) heruntergeladen werden:

- ISVCustomer
- ISVMarketplaceInsights
- Isvusage
- ISVOrder

In den folgenden Abschnitten finden Sie Beispiele für den programmgesteuerten Zugriff auf `OrderId` über das ISVOrder-Dataset.

### <a name="step-1-make-a-rest-call-using-the-get-datasets-api"></a>Schritt 1: Erstellen eines REST-Aufrufs mithilfe der API zum Abrufen von Datasets

Die API-Antwort enthält den Namen des Datasets, von dem aus Sie den Bericht herunterladen können. Die API-Antwort für das jeweilige Dataset enthält auch die Liste der auswählbaren Spalten, die für Ihre benutzerdefinierte Berichtsvorlage verwendet werden können. Sie können auch auf die folgenden Tabellen verweisen, um die Namen der Spalten zu erhalten:

- [Tabelle „Auftragsdetails“](orders-dashboard.md#orders-details-table)
- [Tabelle „Nutzungsdetails“](usage-dashboard.md#usage-details-table)
- [Tabelle der Kundendetails](customer-dashboard.md#customer-details-table)
- [Marketplace-Erkenntnisse: Tabelle „Details“](insights-dashboard.md#marketplace-insights-details-table)

***Anforderungsbeispiel***:

```json
curl 
--location 
--request GET 'https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledDataset ' \ 
--header 'Authorization: Bearer <AzureADToken>'
```

***Antwortbeispiel***:

```json
{
    "value": [
        {
            "datasetName": "ISVOrder",
            "selectableColumns": [
                "MarketplaceSubscriptionId",
                "MonthStartDate",
                "OfferType",
                "AzureLicenseType",
                "Sku",
                "CustomerCountry",
                "IsPreviewSKU",
                "OrderId",
                "OrderQuantity",
                "CloudInstanceName",
                "IsNewCustomer",
                "OrderStatus",
                "OrderCancelDate",
                "CustomerCompanyName",
                "CustomerName",
                "OrderPurchaseDate",
                "OfferName",
                "TrialEndDate",
                "CustomerId",
                "BillingAccountId"
            ],
            "availableMetrics": [],
            "availableDateRanges": [
                "LAST_MONTH",
                "LAST_3_MONTHS",
                "LAST_6_MONTHS",
                "LIFETIME"
            ]
        },
    ],
    "totalCount": 1,
    "message": "Dataset fetched successfully",
    "statusCode": 200
}
```

### <a name="step-2-create-the-custom-query"></a>Schritt 2: Erstellen der benutzerdefinierten Abfrage

In diesem Schritt verwenden wir die Auftrags-ID aus dem Bericht „Bestellungen“, um eine benutzerdefinierte Abfrage für den gewünschten Bericht zu erstellen. Wenn der Standardwert für `timespan` nicht in der Abfrage angegeben ist, beträgt er sechs Monate.

***Anforderungsbeispiel***:

```json
curl 
--location 
--request POST ' https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledQueries' \ 
--header ' Authorization: Bearer <AzureAD_Token>' \ 
--header 'Content-Type: application/json' \ 
--data-raw 
            '{ 
                "Query": "SELECT OrderId from ISVOrder", 
                "Name": "ISVOrderQuery1", 
                "Description": "Get a list of all Order IDs" 
             }'
```

***Antwortbeispiel***:

```json
{
    "value": [
        {
            "queryId": "78be43f2-e35f-491a-8cd5-78fe14194f9c",
            "name": "ISVOrderQuery1",
            "description": "Get a list of all Order IDs”,
            "query": "SELECT OrderId from ISVOrder",
            "type": "userDefined",
            "user": "142344300",
            "createdTime": "2021-01-06T05:38:34",
            "modifiedTime": null
        }
    ],
    "totalCount": 1,
    "message": "Query created successfully",
    "statusCode": 200
}
```

Bei erfolgreicher Ausführung der Abfrage wird eine `queryId` generiert, die zum Generieren des Berichts verwendet werden muss.

### <a name="step-3-execute-test-query-api"></a>Schritt 3: Ausführen der Testabfrage-API

In diesem Schritt verwenden wir die Testabfrage-API, um die obersten 100 Zeilen für die erstellte Abfrage abzurufen.

***Anforderungsbeispiel***:

```json
curl 
--location 
--request GET 'https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledQueries/testQueryResult?exportQuery=SELECT%20OrderId%20from%20ISVOrder' \ 
--header ' Authorization: Bearer <AzureADToken>'
```

***Antwortbeispiel***:

```json
{
    "value": [
        {
            "OrderId": "086365c6-9c38-4fba-904a-6228f6cb2ba8"
        },
        {
            "OrderId": "086365c6-9c38-4fba-904a-6228f6cb2bb8"
        },
        {
            "OrderId": "086365c6-9c38-4fba-904a-6228f6cb2bc8"
        },
        {
            "OrderId": "086365c6-9c38-4fba-904a-6228f6cb2bd8"
        },
        {
            "OrderId": "086365c6-9c38-4fba-904a-6228f6cb2be8"
        },
               .
               .
               .

        {
            "OrderId": "086365c6-9c38-4fba-904a-6228f6cb2bf0"
        },
        {
            "OrderId": "086365c6-9c38-4fba-904a-6228f6cb2bf1"
        },
        {
            "OrderId": "086365c6-9c38-4fba-904a-6228f6cb2bf2"
        },
        {
            "OrderId": "086365c6-9c38-4fba-904a-6228f6cb2bf3"
        },
        {
            "OrderId": "086365c6-9c38-4fba-904a-6228f6cb2bf4"
        }
    ],
    "totalCount": 100,
    "message": null,
    "statusCode": 200
}
```

### <a name="step-4-create-the-report"></a>Schritt 4: Erstellen des Berichts

In diesem Schritt verwenden wir die zuvor generierte `QueryId`, um den Bericht zu erstellen.

***Anforderungsbeispiel***:

```json
curl 
--location 
--request POST 'https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledReport' \ 
--header ' Authorization: Bearer <AzureADToken>' \ 
--header 'Content-Type: application/json' \ 
--data-raw 
                 '{
                   “ReportName": "ISVReport1",
                   "Description": "Report for getting list of Order Ids",
                   "QueryId": "78be43f2-e35f-491a-8cd5-78fe14194f9c",
                   "StartTime": "2021-01-06T19:00:00Z”,
                   "RecurrenceInterval": 48,
                   "RecurrenceCount": 20,
                    "Format": "csv"
                  }'
```

<br>

_**Tabelle 1: Beschreibung der in diesem Anforderungsbeispiel verwendeten Parameter**_

| Parameter | Beschreibung |
| ------------ | ------------- |
| `Description` | Geben Sie eine kurze Beschreibung des Berichts, der generiert wird. |
| `QueryId` | Dies ist die `queryId`, die beim Erstellen der Abfrage in „Schritt 2: Erstellen der benutzerdefinierten Abfrage“ generiert wurde. |
| `StartTime` | Zeitpunkt, zu dem die erste Ausführung des Berichts begonnen hat. |
| `RecurrenceInterval` | Bei der Erstellung des Berichts angegebenes Wiederholungsintervall. |
| `RecurrenceCount` | Bei der Erstellung des Berichts angegebene Wiederholungsanzahl. |
| `Format` | CSV- und TSV-Dateiformate werden unterstützt. |
|||

***Antwortbeispiel***:

```json
{
    "value": [
        {
            "reportId": "72fa95ab-35f5-4d44-a1ee-503abbc88003",
            "reportName": "ISVReport1",
            "description": "Report for getting list of Order Ids",
            "queryId": "78be43f2-e35f-491a-8cd5-78fe14194f9c",
            "query": "SELECT OrderId from ISVOrder",
            "user": "142344300",
            "createdTime": "2021-01-06T05:46:00Z",
            "modifiedTime": null,
            "startTime": "2021-01-06T19:00:00Z",
            "reportStatus": "Active",
            "recurrenceInterval": 48,
            "recurrenceCount": 20,
            "callbackUrl": null,
            "format": "csv"
        }
    ],
    "totalCount": 1,
    "message": "Report created successfully",
    "statusCode": 200
}
```

Bei erfolgreicher Ausführung wird eine `reportId` generiert, die verwendet werden muss, um einen Download des Berichts zu planen.

### <a name="step-5-execute-report-executions-api"></a>Schritt 5: Ausführen der Berichtsausführungs-API

Um den sicheren Speicherort (URL) des Berichts abzurufen, führen wir nun die Berichtsausführungs-API aus.

***Anforderungsbeispiel***:

```json
Curl
--location
--request GET 'https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledReport/execution/72fa95ab-35f5-4d44-a1ee-503abbc88003' \
--header ' Authorization: Bearer <AzureADToken>' \
```

***Antwortbeispiel***:

```json
{
    "value": [
        {
            "executionId": "1f18b53b-df30-4d98-85ee-e6c7e687aeed",
            "reportId": "72fa95ab-35f5-4d44-a1ee-503abbc88003",
            "recurrenceInterval": 48,
            "recurrenceCount": 20,
            "callbackUrl": null,
            "format": "csv",
            "executionStatus": "Pending",
            "reportAccessSecureLink": null,
            "reportExpiryTime": null,
            "reportGeneratedTime": null
        }
    ],
    "totalCount": 1,
    "message": null,
    "statusCode": 200
}
```

## <a name="next-steps"></a>Nächste Schritte

- Sie können die APIs über die [Swagger-API-URL](https://swagger.io/docs/specification/api-host-and-base-path/) testen.
- [Paradigma für den programmgesteuerten Zugriff](analytics-programmatic-access.md)
