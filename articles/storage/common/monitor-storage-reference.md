---
title: Überwachen von Daten in Azure Storage – Referenz | Microsoft-Dokumentation
description: Protokoll- und Metrikenreferenz für die Überwachung von Daten in Azure Storage
author: normesta
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 05/01/2020
ms.author: normesta
ms.subservice: logs
ms.openlocfilehash: ba268e623a2858c2863ffc86eacfe25284a1e37a
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82710233"
---
# <a name="azure-storage-monitoring-data-reference"></a>Überwachen von Daten in Azure Storage – Referenz

Ausführliche Informationen über das Erfassen und Analysieren von Überwachungsdaten für Azure Storage finden Sie unter [Überwachen von Azure Storage](monitor-storage.md).

## <a name="metrics"></a>Metriken

In den nachstehenden Tabellen sind die für Azure Storage erfassten Plattformmetriken aufgelistet. 

### <a name="capacity-metrics"></a>Kapazitätsmetriken

Kapazitätsmetrikwerte werden stündlich an Azure Monitor gesendet. Die Werte werden täglich aktualisiert. Das Aggregationsintervall definiert das Zeitintervall, für das Metrikwerte dargestellt werden. Das unterstützte Aggregationsintervall für alle Kapazitätsmetriken beträgt eine Stunde (PT1H).

Azure Storage stellt in Azure Monitor folgende Kapazitätsmetriken bereit:

#### <a name="account-level"></a>Kontoebene

| Metrik | BESCHREIBUNG |
| ------------------- | ----------------- |
| UsedCapacity | Die vom Speicherkonto beanspruchte Speichermenge. Bei Standardspeicherkonten ist das die Summe der von Blob, Table, File und Queue beanspruchten Kapazität. Bei Storage Premium- und Blob Storage-Konten ist es der gleiche Wert wie für „BlobCapacity“. <br/><br/> Einheit: Byte <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |

#### <a name="blob-storage"></a>Blob Storage

| Metrik | BESCHREIBUNG |
| ------------------- | ----------------- |
| BlobCapacity | Der gesamte im Speicherkonto beanspruchte Blobspeicher. <br/><br/> Einheit: Byte <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 <br/> Abmessungen: **BlobType** und **BlobTier** ([Definition](#metrics-dimensions)) |
| BlobCount    | Die Anzahl von im Speicherkonto gespeicherten Blobs. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 <br/> Abmessungen: **BlobType** und **BlobTier** ([Definition](#metrics-dimensions)) |
| ContainerCount    | Die Anzahl von Containern im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |
| IndexCapacity     | Die vom hierarchischen ADLS Gen2-Index verwendete Speichermenge <br/><br/> Einheit: Byte <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |

#### <a name="table-storage"></a>Table Storage

| Metrik | BESCHREIBUNG |
| ------------------- | ----------------- |
| TableCapacity | Der vom Speicherkonto beanspruchte Table-Speicher. <br/><br/> Einheit: Byte <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |
| TableCount   | Die Anzahl von Tabellen im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |
| TableEntityCount | Die Anzahl von Tabellenentitäten im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |

#### <a name="queue-storage"></a>Queue Storage

| Metrik | BESCHREIBUNG |
| ------------------- | ----------------- |
| QueueCapacity | Der vom Speicherkonto beanspruchte Queue-Speicher. <br/><br/> Einheit: Byte <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |
| QueueCount   | Die Anzahl von Warteschlangen im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |
| QueueMessageCount | Die Anzahl nicht abgelaufener Warteschlangennachrichten im Speicherkonto. <br/><br/>Einheit: Anzahl <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |

#### <a name="file-storage"></a>File Storage

| Metrik | BESCHREIBUNG |
| ------------------- | ----------------- |
| FileCapacity | Der vom Speicherkonto beanspruchte File-Speicher. <br/><br/> Einheit: Byte <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |
| FileCount   | Die Anzahl von Dateien im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |
| FileShareCount | Die Anzahl von Dateifreigaben im Speicherkonto. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |

### <a name="transaction-metrics"></a>Transaktionsmetriken

Transaktionsmetriken werden für jede Anforderung an ein Speicherkonto aus Azure Storage an Azure Monitor ausgegeben. Falls in Ihrem Speicherkonto keine Aktivität stattfindet, gibt es in diesem Zeitraum keine Daten zu Transaktionsmetriken. Alle Transaktionsmetriken stehen sowohl auf der Konto- als auch auf der Dienstebene (Blob Storage, Table Storage, Azure Files und Queue Storage) zur Verfügung. Das Aggregationsintervall definiert das Zeitintervall für die dargestellten Metrikwerte. Für alle Transaktionsmetriken werden die Aggregationsintervalle PT1H und PT1M unterstützt.

Azure Storage stellt in Azure Monitor folgende Transaktionsmetriken bereit:

| Metrik | BESCHREIBUNG |
| ------------------- | ----------------- |
| Transaktionen | Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und nicht erfolgreiche Anforderungen sowie Anforderungen, die zu Fehlern geführt haben. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Gesamt <br/> Verfügbare Dimensionen: ResponseType, GeoType, ApiName, Authentication ([Definition](#metrics-dimensions))<br/> Beispielwert: 1024 |
| Eingehende Daten | Die eingehende Datenmenge. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure. <br/><br/> Einheit: Byte <br/> Aggregationstyp: Gesamt <br/> Verfügbare Dimensionen: GeoType, ApiName, Authentication ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| Ausgehende Daten | Die ausgehende Datenmenge. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar. <br/><br/> Einheit: Byte <br/> Aggregationstyp: Gesamt <br/> Verfügbare Dimensionen: GeoType, ApiName, Authentication ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| SuccessServerLatency | Die durchschnittliche Verarbeitungszeit einer erfolgreichen Anforderung durch Azure Storage. Dieser Wert beinhaltet nicht die in „SuccessE2ELatency“ angegebene Netzwerklatenz. <br/><br/> Einheit: Millisekunden <br/> Aggregationstyp: Average <br/> Verfügbare Dimensionen: GeoType, ApiName, Authentication ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| SuccessE2ELatency | Die durchschnittliche End-to-End-Wartezeit (in Millisekunden) bei erfolgreichen Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung. <br/><br/> Einheit: Millisekunden <br/> Aggregationstyp: Average <br/> Verfügbare Dimensionen: GeoType, ApiName, Authentication ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| Verfügbarkeit | Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Zur Berechnung der Verfügbarkeit wird der Wert der gesamten gebührenpflichtigen Anforderungen durch die Anzahl entsprechender Anforderungen geteilt. Dabei werden auch Anforderungen einbezogen, die zu unerwarteten Fehlern geführt haben. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang. <br/><br/> Einheit: Percent <br/> Aggregationstyp: Average <br/> Verfügbare Dimensionen: GeoType, ApiName, Authentication ([Definition](#metrics-dimensions)) <br/> Beispielwert: 99,99 |

<a id="metrics-dimensions" />

## <a name="metrics-dimensions"></a>Metrikdimensionen

Azure Storage unterstützt folgende Dimensionen für Metriken in Azure Monitor:

| Dimensionsname | BESCHREIBUNG |
| ------------------- | ----------------- |
| **BlobType** | Die Art des Blobs (nur für Blobmetriken). Die unterstützten Werte sind **BlockBlob**, **PageBlob**, und **Azure Data Lake Storage**. Anfügeblob ist in „BlockBlob“ enthalten. |
| **BlobTier** | Azure Storage bietet unterschiedliche Zugriffsebenen, die Ihnen das Speichern von Blobobjektdaten auf die kostengünstigste Art ermöglichen. Weitere Informationen finden Sie in [Azure Storage-Blobtarif](../blobs/storage-blob-storage-tiers.md). Die unterstützten Werte umfassen: <br/> <li>**Hot**: Heiße Zugriffsebene</li> <li>**Cool**: Kalte Zugriffsebene</li> <li>**Archive**: Archivzugriffsebene</li> <li>**Premium**: Premium-Tarif für Blockblob</li> <li>**P4/P6/P10/P15/P20/P30/P40/P50/P60**: Zugriffsebenentypen für Premium-Seitenblob</li> <li>**Standard**: Zugriffsebenentypen für Standard-Seitenblob</li> <li>**Kein Tarif**: Zugriffsebenentyp für Speicherkonto des Typs „Allgemein v1“</li> |
| **GeoType** | Transaktion aus dem primären oder sekundären Cluster. Die verfügbaren Werte sind **Primary** und **Secondary**. Gilt für georedundanten Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS) beim Lesen von Objekten aus dem sekundären Mandanten. |
| **ResponseType** | Transaktionsantworttyp. Verfügbaren Werte: <br/><br/> <li>**ServerOtherError**: Alle weiteren serverseitigen Fehler (mit Ausnahme beschriebener Fehler). </li> <li>**ServerBusyError**: Authentifizierte Anforderung, die den HTTP-Statuscode 503 zurückgegeben hat. </li> <li>**ServerTimeoutError**: Authentifizierte Anforderung mit Timeoutfehler, die den HTTP-Statuscode 500 zurückgegeben hat. Das Timeout ist auf einen Serverfehler zurückzuführen. </li> <li>**AuthorizationError**: Authentifizierte Anforderung, die aufgrund eines nicht autorisierten Datenzugriffs oder aufgrund eines Autorisierungsfehlers nicht erfolgreich war. </li> <li>**NetworkError**: Authentifizierte Anforderung, die aufgrund von Netzwerkfehlern nicht erfolgreich war. Tritt üblicherweise auf, wenn ein Client vor Ablauf des Timeouts vorzeitig eine Verbindung trennt. </li><li>**ClientAccountBandwidthThrottlingError**: Die Bandbreite für die Anforderung wird aufgrund einer Überschreitung der [Skalierbarkeitslimits für Speicherkonten](scalability-targets-standard-account.md) gedrosselt.</li><li>**ClientAccountRequestThrottlingError**: Die Anforderungsrate für die Anforderung wird aufgrund einer Überschreitung der [Skalierbarkeitslimits für Speicherkonten](scalability-targets-standard-account.md) gedrosselt.<li>**ClientThrottlingError**: Anderer Fehler bei der clientseitigen Drosselung. ClientAccountBandwidthThrottlingError und ClientAccountRequestThrottlingError sind ausgeschlossen.</li> <li>**ClientTimeoutError**: Authentifizierte Anforderung mit Timeoutfehler, die den HTTP-Statuscode 500 zurückgegeben hat. Wenn das Netzwerktimeout des Clients oder das Anforderungstimeout auf einen niedrigeren Wert festgelegt ist als vom Speicherdienst erwartet, handelt es sich um ein erwartetes Timeout. Andernfalls wird „ServerTimeoutError“ gemeldet. </li> <li>**ClientOtherError**: Alle weiteren clientseitigen Fehler (mit Ausnahme beschriebener Fehler). </li> <li>**Erfolg**: Erfolgreiche Anforderung.</li> <li> **SuccessWithThrottling**: Erfolgreiche Anforderung, wenn ein SMB-Client bei den ersten Versuchen gedrosselt wird, der Vorgang aber letztendlich erfolgreich ist.</li> |
| **ApiName** | Der Name des Vorgangs. Beispiel: <br/> <li>**CreateContainer**</li> <li>**DeleteBlob**</li> <li>**GetBlob**</li> Eine Liste mit allen Vorgangsnamen finden Sie [hier](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages). |
| **Authentifizierung** | Der in Transaktionen verwendete Authentifizierungstyp. Verfügbaren Werte: <br/> <li>**AccountKey**: Die Transaktion wird mit dem Speicherkontoschlüssel authentifiziert.</li> <li>**SAS**: Die Transaktion wird mit Shared Access Signatures authentifiziert.</li> <li>**OAuth**: Die Transaktion wird mit OAuth-Zugriffstoken authentifiziert.</li> <li>**Anonymous**: Die Transaktion wird anonym angefordert. Sie enthält keine Preflight-Anforderungen.</li> <li>**AnonymousPreflight**: Die Transaktion ist eine Preflightanforderung.</li> |

Bei Metriken mit Dimensionsunterstützung muss der Dimensionswert angegeben werden, um die entsprechenden Metrikwerte anzeigen zu können. Wenn Sie sich also beispielsweise erfolgreiche Antworten für **Transaktionen** ansehen möchten, müssen Sie die Dimension **ResponseType** nach **Success** filtern. Anderes Beispiel: Wenn Sie sich **BlobCount** für „Blockblob“ ansehen möchten, müssen Sie die Dimension **BlobType** nach **BlockBlob** filtern.

## <a name="resource-logs-preview"></a>Ressourcenprotokolle (Vorschau)

> [!NOTE]
> Azure Storage-Protokolle in Azure Monitor befinden sich in der öffentlichen Vorschauphase und stehen in allen öffentlichen Cloudregionen für Vorschautests zur Verfügung. Informationen zum Registrieren für die Vorschauversion finden Sie auf [dieser Seite](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u).  Diese Vorschau ermöglicht Protokolle für Blobs (einschließlich Azure Data Lake Storage Gen2), Dateien, Warteschlangen, Tabellen, Universell V1-Premium-Speicherkonten und Universell V2-Speicherkonten. Klassische Speicherkonten werden nicht unterstützt.

In der nachstehenden Tabelle sind die Eigenschaften für Azure Storage-Ressourcenprotokolle aufgelistet, wenn diese in Azure Monitor-Protokollen oder Azure Storage gesammelt werden. Diese Eigenschaften beschreiben den Vorgang, den Dienst und den Autorisierungstyp, der zum Ausführen des Vorgangs verwendet wurde.

### <a name="fields-that-describe-the-operation"></a>Den Vorgang beschreibende Felder

```json
{
    "time": "2019-02-28T19:10:21.2123117Z",
    "resourceId": "/subscriptions/12345678-2222-3333-4444-555555555555/resourceGroups/mytestrp/providers/Microsoft.Storage/storageAccounts/testaccount1/blobServices/default",
    "category": "StorageWrite",
    "operationName": "PutBlob",
    "operationVersion": "2017-04-17",
    "schemaVersion": "1.0",
    "statusCode": 201,
    "statusText": "Success",
    "durationMs": 5,
    "callerIpAddress": "192.168.0.1:11111",
    "correlationId": "ad881411-201e-004e-1c99-cfd67d000000",
    "location": "uswestcentral",
    "uri": "http://mystorageaccount.blob.core.windows.net/cont1/blobname?timeout=10"
}
```

| Eigenschaft | BESCHREIBUNG |
|:--- |:---|
|**time** | Hierbei handelt es sich um die Uhrzeit gemäß koordinierter Weltzeit (UTC), zu der die Anforderung von Storage empfangen wurde. Beispiel: `2018/11/08 21:09:36.6900118`.|
|**Ressourcen-ID** | Hierbei handelt es sich um die Ressourcen-ID des Speicherkontos. Beispiel: `/subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/`<br>`myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/storageAccounts/blobServices/default`|
|**category** | Hierbei handelt es sich um die Kategorie des angeforderten Vorgangs. Beispiel: `StorageRead`, `StorageWrite` oder `StorageDelete`.|
|**operationName** | Hierbei handelt es sich um den Typ des ausgeführten REST-Vorgangs. <br> Eine vollständige Liste der Vorgänge finden Sie im Thema [Mit Storage Analytics protokollierte Vorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages). |
|**operationVersion** | Hierbei handelt es sich um die Speicherdienstversion, die beim Erstellen der Anforderung angegeben wurde. Diese entspricht dem Wert des **x-ms-version**-Headers. Beispiel: `2017-04-17`.|
|**schemaVersion** | Die Schemaversion des Protokolls Beispiel: `1.0`.|
|**statusCode** | Hierbei handelt es sich um den HTTP-Statuscode für die Anforderung. Wenn die Anforderung unterbrochen wurde, ist dieser Wert möglicherweise auf `Unknown` festgelegt. <br> Beispiel: `206` |
|**statusText** | Hierbei handelt es sich um den Status des angeforderten Vorgangs.  Eine vollständige Liste der Statusmeldungen finden Sie im Thema [Mit Storage Analytics protokollierte Vorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages). In Version 2017-04-17 und höher wird die Statusmeldung `ClientOtherError` nicht verwendet. Stattdessen enthält dieses Feld einen Fehlercode. Beispiel: `SASSuccess`  |
|**durationMs** | Hierbei handelt es sich um die zum Ausführen des angeforderten Vorgangs benötigte Gesamtzeit in Millisekunden. Dieser Wert schließt die Zeit mit ein, die für das Lesen der eingehenden Anforderung und das Senden der Antwort an den Anforderer benötigt wurde. Beispiel: `12`.|
|**callerIpAddress** | Hierbei handelt es sich um die IP-Adresse des Anforderers einschließlich der Portnummer. Beispiel: `192.100.0.102:4362`. |
|**correlationId** | Hierbei handelt es sich um die ID, die zum ressourcenübergreifenden Korrelieren von Protokollen verwendet wird. Beispiel: `b99ba45e-a01e-0042-4ea6-772bbb000000`. |
|**location** | Hierbei handelt es sich um den Speicherort des Speicherkontos. Beispiel: `North Europe`. |
|**protocol**|Hierbei handelt es sich um das für den Vorgang verwendete Protokoll, z. B. `HTTP`, `HTTPS`, `SMB` oder `NFS`.|
| **uri** | Hierbei handelt es sich um den angeforderten Uniform Resource Identifier. Beispiel: http://myaccountname.blob.core.windows.net/cont1/blobname?timeout=10. |

### <a name="fields-that-describe-how-the-operation-was-authenticated"></a>Felder, die beschreiben, wie der Vorgang authentifiziert wurde

```json
{
    "identity": {
        "authorization": [
            {
                "action": "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
                "principals": [
                    {
                        "id": "fde5ba15-4355-4223-b811-cccccccccccc",
                        "type": "User"
                    }
                ],
                "roleAssignmentId": "ecf75cb8-491c-4a25-ad6e-aaaaaaaaaaaa",
                "roleDefinitionId": "b7e6dc6d-f1e8-4753-8033-ffffffffffff"
            }
        ],
        "requester": {
            "appId": "691458b9-1327-4635-9f55-bbbbbbbbbbbb",
            "audience": "https://storage.azure.com/",
            "objectId": "fde5ba15-4355-4223-b811-cccccccccccc",
            "tenantId": "72f988bf-86f1-41af-91ab-dddddddddddd",
            "tokenIssuer": "https://sts.windows.net/72f988bf-86f1-41af-91ab-eeeeeeeeeeee/"
           },
        "type": "OAuth"
    },
}

```

| Eigenschaft | BESCHREIBUNG |
|:--- |:---|
|**identity / type** | Hierbei handelt es sich um den für die Anforderung verwendeten Authentifizierungstyp, z. B. `OAuth`, `SAS Key`, `Account Key` oder `Anonymous`. |
|**identity / tokenHash**|Dieses Feld ist ausschließlich zur internen Verwendung vorgesehen. |
|**authorization / action** | Hierbei handelt es sich um die der Anforderung zugewiesene Aktion. Beispiel: `Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read` |
|**authorization / roleAssignmentId** | Hierbei handelt es sich um die Rollenzuweisungs-ID. Beispiel: `4e2521b7-13be-4363-aeda-111111111111`.|
|**authorization / roleDefinitionId** | Hierbei handelt es sich um die Rollendefinitions-ID. Beispiel: `ba92f5b4-2d11-453d-a403-111111111111"`.|
|**principals / id** | Hierbei handelt es sich um die ID des Sicherheitsprinzipals. Beispiel: `a4711f3a-254f-4cfb-8a2d-111111111111`.|
|**principals / type** | Hierbei handelt es sich um den Typ des Sicherheitsprinzipals. Beispiel: `ServicePrincipal`. |
|**requester / appID** | Hierbei handelt es sich um die OAuth-Anwendungs-ID (Open Authorization), die als Anforderer verwendet wird. <br> Beispiel: `d3f7d5fe-e64a-4e4e-871d-333333333333`.|
|**requester / audience** | Hierbei handelt es sich um die OAuth-Zielgruppe der Anforderung. Beispiel: `https://storage.azure.com`. |
|**requester / objectId** | Hierbei handelt es sich um die OAuth-Objekt-ID des Anforderers. Bei Kerberos-Authentifizierung stellt sie den Objektbezeichner des mit Kerberos authentifizierten Benutzers dar. Beispiel: `0e0bf547-55e5-465c-91b7-2873712b249c`. |
|**requester / tenantId** | Hierbei handelt es sich um die OAuth-Mandanten-ID der Identität. Beispiel: `72f988bf-86f1-41af-91ab-222222222222`.|
|**requester / tokenIssuer** | Hierbei handelt es sich um den Aussteller des OAuth-Tokens. Beispiel: `https://sts.windows.net/72f988bf-86f1-41af-91ab-222222222222/`.|
|**requester / upn** | Hierbei handelt es sich um den Benutzerprinzipalnamen (UPN) des Anforderers. Beispiel: `someone@contoso.com`. |
|**requester / userName** | Dieses Feld ist ausschließlich zur internen Verwendung vorgesehen.|

### <a name="fields-that-describe-the-service"></a>Den Dienst beschreibende Felder

```json
{
    "properties": {
        "accountName": "testaccount1",
        "requestUrl": "https://testaccount1.blob.core.windows.net:443/upload?restype=container&comp=list&prefix=&delimiter=%2F&marker=&maxresults=30&include=metadata&_=1551405598426",
        "userAgentHeader": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.140 Safari/537.36 Edge/17.17134",
        "referrerHeader": "blob:https://ms.portal.azure.com/6f50025f-3b88-488d-b29e-3c592a31ddc9",
        "clientRequestId": "",
        "etag": "",
        "serverLatencyMs": 63,
        "serviceType": "blob",
        "operationCount": 0,
        "requestHeaderSize": 2658,
        "requestBodySize": 0,
        "responseHeaderSize": 295,
        "responseBodySize": 2018,
        "contentLengthHeader": 0,
        "requestMd5": "",
        "serverMd5": "",
        "lastModifiedTime": "",
        "conditionsUsed": "",
        "smbTreeConnectID" : "0x3",
        "smbPersistentHandleID" : "0x6003f",
        "smbVolatileHandleID" : "0xFFFFFFFF00000065",
        "smbMessageID" : "0x3b165",
        "smbCreditsConsumed" : "0x3",
        "smbCommandDetail" : "0x2000 bytes at offset 0xf2000",
        "smbFileId" : " 0x9223442405598953",
        "smbSessionID" : "0x8530280128000049",
        "smbCommandMajor" : "0x6",
        "smbCommandMinor" : "DirectoryCloseAndDelete"
    }

}
```

| Eigenschaft | BESCHREIBUNG |
|:--- |:---|
|**accountName** | Der Name des Speicherkontos. Beispiel: `mystorageaccount`.  |
|**requestUrl** | Hierbei handelt es sich um die angeforderte URL. Beispiel: `http://mystorageaccount.blob.core.windows.net/cont1/blobname?timeout=10`.|
|**userAgentHeader** | Hierbei handelt es sich um den Wert des **Benutzer-Agent-Headers** in Anführungszeichen. Beispiel: `WA-Storage/6.2.0 (.NET CLR 4.0.30319.42000; Win32NT 6.2.9200.0)`.|
|**referrerHeader** | Hierbei handelt es sich um den Wert des **Verweiserheaders**. Beispiel: `http://contoso.com/about.html`.|
|**clientRequestId** | Hierbei handelt es sich um den Wert des **x-ms-client-request-id**-Headers der Anforderung. Beispiel: `360b66a6-ad4f-4c4a-84a4-0ad7cb44f7a6`. |
|**etag** | Hierbei handelt es sich um den ETag-Bezeichner für das zurückgegebene Objekt in Anführungszeichen. Beispiel: `0x8D101F7E4B662C4`.  |
|**serverLatencyMs** | Hierbei handelt es sich um die zum Ausführen des angeforderten Vorgangs benötigte Gesamtzeit in Millisekunden. Dieser Wert umfasst nicht die Netzwerklatenzzeit (die Zeit, die für das Lesen der eingehenden Anforderung und das Senden der Antwort an den Anforderer benötigt wurde). Beispiel: `22`. |
|**serviceType** | Hierbei handelt es sich um den der Anforderung zugeordneten Dienst, z. B. `blob`, `table`, `files` oder `queue`. |
|**operationCount** | Hierbei handelt es sich um die Anzahl von an der Anforderung beteiligten protokollierten Vorgängen. Die Zählung beginnt mit einem Index von `0`. Manche Anforderungen benötigen mehr als einen Vorgang, z. B. eine Anforderung zum Kopieren eines Blobs. Die meisten Anforderungen führen nur einen Vorgang aus. Beispiel: `1`. |
|**requestHeaderSize** | Hierbei handelt es sich um die Größe des Anforderungsheaders in Byte. Beispiel: `578`. <br>Wenn bei einer Anforderung ein Fehler aufgetreten ist, ist dieser Wert möglicherweise leer. |
|**requestBodySize** | Hierbei handelt es sich um die Größe der vom Speicherdienst gelesenen Anforderungspakete in Byte. <br> Beispiel: `0`. <br>Wenn bei einer Anforderung ein Fehler aufgetreten ist, ist dieser Wert möglicherweise leer.  |
|**responseHeaderSize** | Hierbei handelt es sich um die Größe des Antwortheaders in Byte. Beispiel: `216`. <br>Wenn bei einer Anforderung ein Fehler aufgetreten ist, ist dieser Wert möglicherweise leer.  |
|**responseBodySize** | Hierbei handelt es sich um die Größe der vom Speicherdienst geschriebenen Antwortpakete in Byte. Wenn bei einer Anforderung ein Fehler aufgetreten ist, kann dieser Wert leer sein. Beispiel: `216`.  |
|**requestMd5** | Hierbei handelt es sich entweder um den Wert des **Content-MD5**-Headers oder den des **x-ms-content-md5**-Headers in der Anforderung. Der MD5-Hashwert, der in diesem Feld angegeben ist, stellt den Inhalt der Anforderung dar. Beispiel: `788815fd0198be0d275ad329cafd1830`. <br>Dieses Feld kann leer sein.  |
|**serverMd5** | Hierbei handelt es sich um den Wert des vom Speicherdienst berechneten MD5-Hashs. Beispiel: `3228b3cf1069a5489b298446321f8521`. <br>Dieses Feld kann leer sein.  |
|**lastModifiedTime** | Hierbei handelt es sich um die Uhrzeit der letzten Änderung für das zurückgegebene Objekt.  Beispiel: `Tuesday, 09-Aug-11 21:13:26 GMT`. <br>Dieses Feld ist für Vorgänge leer, die mehrere Objekte zurückgeben können. |
|**conditionsUsed** | Hierbei handelt es sich um eine Liste von Schlüssel-Wert-Paaren mit Semikolons als Trennzeichen, die eine Bedingung darstellen. Dabei kann es sich um folgende Bedingungen handeln: <li> If-Modified-Since <li> If-Unmodified-Since <li> If-Match <li> If-None-Match  <br> Beispiel: `If-Modified-Since=Friday, 05-Aug-11 19:11:54 GMT`. |
|**contentLengthHeader** | Hierbei handelt es sich um den Wert des Content-Length-Headers für die an den Speicherdienst gesendete Anforderung. Wenn die Anforderung erfolgreich war, entspricht dieser Wert dem von requestBodySize. Wenn bei einer Anforderung ein Fehler aufgetreten ist, entspricht dieser Wert möglicherweise nicht dem von requestBodySize, oder er ist leer. |
|**tlsVersion** | Hierbei handelt es sich um die bei der Anforderung für die Verbindung verwendete TLS-Version. Beispiel: `TLS 1.2`. |
|**smbTreeConnectID** | Hierbei handelt es sich um die beim Herstellen einer Strukturverbindung erstellte SMB-**treeConnectId** (Server Message Block). Beispiel: `0x3` |
|**smbPersistentHandleID** | Hierbei handelt es sich um eine persistente Handle-ID aus einer SMB2-CREATE-Anforderung, die das erneute Herstellen einer Verbindung mit dem Netzwerk übersteht.  Auf diese ID wird in [MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/f1d9b40d-e335-45fc-9d0b-199a31ede4c3) 2.2.14.1 mit **SMB2_FILEID.Persistent** verwiesen. Beispiel: `0x6003f` |
|**smbVolatileHandleID** | Hierbei handelt es sich um eine flüchtige Handle-ID aus einer SMB2-CREATE-Anforderung, die beim erneuten Herstellen einer Verbindung mit dem Netzwerk wiederverwendet wird.  Auf diese ID wird in [MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/f1d9b40d-e335-45fc-9d0b-199a31ede4c3) 2.2.14.1 mit **SMB2_FILEID.Volatile** verwiesen. Beispiel: `0xFFFFFFFF00000065` |
|**smbMessageID** | Hierbei handelt es sich um die **MessageId** für die Verbindung. Beispiel: `0x3b165` |
|**smbCreditsConsumed** | Hierbei handelt es sich um den von der Anforderung genutzten eingehenden oder ausgehenden Datenverkehr in Einheiten von je 64 KB. Beispiel: `0x3` |
|**smbCommandDetail** | Hierbei handelt es sich um weitere Informationen zur konkreten Anforderung statt zum allgemeinen Typ der Anforderung. Beispiel: `0x2000 bytes at offset 0xf2000` |
|**smbFileId** | Hierbei handelt es sich um die der Datei oder dem Verzeichnis zugeordnete **FileId**.  Diese entspricht ungefähr einer NTFS-FileId. Beispiel: `0x9223442405598953` |
|**smbSessionID** | Hierbei handelt es sich um die beim Einrichten der Sitzung erstellte SMB2-**SessionId**. Beispiel: `0x8530280128000049` |
|**smbCommandMajor  uint32** | Hierbei handelt es sich um einen Wert in **SMB2_HEADER.Command**. Derzeit ist dies eine Zahl im Bereich von 0 bis einschließlich 18. Beispiel: `0x6` |
|**smbCommandMinor** | Hierbei handelt es sich um eine etwaige Unterklasse von **SmbCommandMajor**. Beispiel: `DirectoryCloseAndDelete` |

## <a name="see-also"></a>Weitere Informationen

- Eine Beschreibung der Überwachung von Azure Storage finden Sie unter [Überwachen von Azure Storage](monitor-storage.md).
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md).