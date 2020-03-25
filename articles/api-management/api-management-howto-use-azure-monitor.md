---
title: Überwachen von veröffentlichten APIs in Azure API Management | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie Ihre API in API Management überwachen.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: b06301ab424a29d8f0e31e8f4dee26265327896b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "79221927"
---
# <a name="monitor-published-apis"></a>Überwachen von veröffentlichten APIs

Mit Azure Monitor können Sie Metriken oder Protokolle aus Azure-Ressourcen visualisieren, abfragen, weiterleiten und archivieren und ggf. notwendige Maßnahmen ergreifen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Anzeigen von Aktivitätsprotokollen
> * Anzeigen von Diagnoseprotokollen
> * Anzeigen von Metriken Ihrer API 
> * Einrichten einer Warnungsregel, wenn Ihre API nicht autorisierte Aufrufe empfängt

Im folgenden Video wird die Überwachung von API Management mithilfe von Azure Monitor veranschaulicht. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]

## <a name="prerequisites"></a>Voraussetzungen

+ Machen Sie sich mit der [Azure API Management-Terminologie](api-management-terminology.md) vertraut.
+ Absolvieren Sie den folgenden Schnellstart: [Erstellen einer Azure API Management-Instanz](get-started-create-service-instance.md).
+ Schließen Sie darüber hinaus das folgende Tutorial ab: [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="view-metrics-of-your-apis"></a>Anzeigen von Metriken Ihrer API

Von API Management werden jede Minute Metriken ausgegeben, sodass Sie einen Überblick über den Zustand und die Integrität Ihrer APIs nahezu in Echtzeit erhalten. Hier sehen Sie eine Übersicht über einige der verfügbaren Metriken:

* Kapazität: Unterstützt Sie beim Treffen von Entscheidungen in Bezug auf Upgrades/Downgrades Ihrer APIM-Dienste. Die Metrik wird minütlich ausgegeben und spiegelt die Gatewaykapazität zum Zeitpunkt der Meldung wider. Der Wert der Metrik kann zwischen 0 und 100 liegen und wird basierend auf Gatewayressourcen wie CPU und Speicherauslastung berechnet.
* Gatewayanforderungen gesamt: Anzahl der API-Anforderungen innerhalb des Zeitraums. 
* Erfolgreiche Gatewayanforderungen: Anzahl der API-Anforderungen, die erfolgreiche HTTP-Antwortcodes einschließlich 304, 307 und alle unter 301 (z.B. 200) erhalten haben.
* Fehlerhafte Gatewayanforderungen: Anzahl der API-Anforderungen, die fehlerhafte HTTP-Antwortcodes einschließlich 400 und alle über 500 erhalten haben.
* Nicht autorisierte Gatewayanforderungen: Anzahl der API-Anforderungen, die HTTP-Antwortcodes einschließlich 401, 403 und 429 erhalten haben.
* Andere Gateway-Anforderungen: Anzahl der API-Anforderungen, die HTTP-Antwortcodes erhalten haben, die keiner der oben genannten Kategorien angehören (z.B. 418).

![Metrikdiagramm](./media/api-management-azure-monitor/apim-monitor-metrics.png)

So greifen Sie auf Metriken zu:

1. Klicken Sie im Menü am unteren Seitenrand auf **Metriken**.

    ![metrics](./media/api-management-azure-monitor/api-management-metrics-blade.png)

1. Wählen Sie im Dropdownmenü die gewünschten Metriken aus. Beispiel: **Anforderungen**. 
1. Im Diagramm ist die Gesamtanzahl von API-Aufrufen gezeigt.
1. Das Diagramm kann anhand der Dimensionen der Metrik **Anforderungen** gefiltert werden. Klicken Sie beispielsweise auf **Filter hinzufügen**, wählen Sie **Backend Response Code** (Antwortcode des Back-Ends) aus, und geben Sie als Wert 500 ein. Das Diagramm zeigt nun die Anzahl von Anforderungen an, für die im API-Back-End ein Fehler auftrat.   

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Einrichten einer Warnungsregel für nicht autorisierte Anforderungen

Sie können die Konfiguration so durchführen, dass Warnungen basierend auf Metriken und Aktivitätsprotokollen empfangen werden. Mit Azure Monitor können Sie eine Warnung so konfigurieren, dass Folgendes erfolgt, wenn sie ausgelöst wird:

* Senden einer E-Mail-Benachrichtigung
* Aufrufen eines Webhooks
* Aufrufen einer Azure-Logik-App

So konfigurieren Sie Warnungen:

1. Klicken Sie im unteren Seitenbereich auf der Menüleiste auf **Warnungen**.

    ![alerts](./media/api-management-azure-monitor/alert-menu-item.png)

2. Klicken Sie für diese Warnung auf **Neue Warnungsregel**.
3. Klicken Sie auf **Bedingung hinzufügen**.
4. Wählen Sie in der Dropdownliste für den Signaltyp die Option **Metriken** aus.
5. Wählen Sie **Unauthorized Gateway Request** (Nicht autorisierte Gatewayanforderung) als zu überwachendes Signal aus.

    ![alerts](./media/api-management-azure-monitor/signal-type.png)

6. Geben Sie in der Ansicht **Signallogik konfigurieren** einen Schwellenwert an, nach dem die Warnung ausgelöst werden soll, und klicken Sie anschließend auf **Fertig**.

    ![alerts](./media/api-management-azure-monitor/threshold.png)

7. Wählen Sie eine vorhandene Aktionsgruppe aus, oder erstellen Sie eine neue Aktionsgruppe. Im folgenden Beispiel wird eine E-Mail an die Administratoren gesendet. 

    ![alerts](./media/api-management-azure-monitor/action-details.png)

8. Geben Sie einen Namen und eine Beschreibung für die Warnungsregel an, und wählen Sie den Schweregrad aus. 
9. Klicken Sie auf **Warnungsregel erstellen**.
10. Versuchen Sie nun, die Konferenz-API ohne API-Schlüssel aufzurufen. Die Warnung wird ausgelöst, und eine E-Mail wird an die Administratoren gesendet. 

## <a name="activity-logs"></a>Aktivitätsprotokolle

Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre API Management-Dienste ausgeführt wurden. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für Ihre API Management-Dienste durchgeführt wurden.

> [!NOTE]
> Aktivitätsprotokolle enthalten keine Lesevorgänge (GET) oder Vorgänge, die im Azure-Portal oder mithilfe der ursprünglichen Management-APIs durchgeführt wurden.

Sie können in Ihrem API Management-Dienst auf Aktivitätsprotokolle oder in Azure Monitor auf Protokolle all Ihrer Azure-Ressourcen zugreifen. 

![Aktivitätsprotokolle](./media/api-management-azure-monitor/apim-monitor-activity-logs.png)

So zeigen Sie Aktivitätsprotokolle an:

1. Wählen Sie Ihre APIM-Dienstinstanz aus.
2. Klicken Sie auf **Aktivitätsprotokoll**.

    ![Aktivitätsprotokoll](./media/api-management-azure-monitor/api-management-activity-logs-blade.png)

3. Wählen Sie den gewünschten Filterungsbereich, und klicken Sie auf **Anwenden**.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

Diagnoseprotokolle bieten umfassende Informationen zu Vorgängen und Fehlern, die zur Überwachung und Problembehandlung relevant sind. Diagnoseprotokolle unterscheiden sich von Aktivitätsprotokollen. Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre Azure-Ressourcen ausgeführt wurden. Diagnoseprotokolle bieten Einblick in Vorgänge, die Ihre Ressource selbst ausgeführt hat.

So konfigurieren Sie Diagnoseprotokolle:

1. Wählen Sie Ihre APIM-Dienstinstanz aus.
2. Klicken Sie auf **Diagnoseeinstellungen**.

    ![Diagnoseprotokolle](./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png)

3. Klicken Sie auf **Diagnose aktivieren**. Sie können Diagnoseprotokolle zusammen mit Metriken in einem Speicherkonto archivieren, an einen Event Hub streamen oder an Azure Monitor-Protokolle senden. 

API Management bietet derzeit Diagnoseprotokolle (stündlich erfasst) zu einzelnen API-Anforderungen, bei denen jeder Eintrag das folgende Schema aufweist:

```json
{  
    "isRequestSuccess" : "",
    "time": "",
    "operationName": "",
    "category": "",
    "durationMs": ,
    "callerIpAddress": "",
    "correlationId": "",
    "location": "",
    "httpStatusCodeCategory": "",
    "resourceId": "",
    "properties": {   
        "method": "", 
        "url": "", 
        "clientProtocol": "", 
        "responseCode": , 
        "backendMethod": "", 
        "backendUrl": "", 
        "backendResponseCode": ,
        "backendProtocol": "",  
        "requestSize": , 
        "responseSize": , 
        "cache": "", 
        "cacheTime": "", 
        "backendTime": , 
        "clientTime": , 
        "apiId": "",
        "operationId": "", 
        "productId": "", 
        "userId": "", 
        "apimSubscriptionId": "", 
        "backendId": "",
        "lastError": { 
            "elapsed" : "", 
            "source" : "", 
            "scope" : "", 
            "section" : "" ,
            "reason" : "", 
            "message" : ""
        } 
    }      
}  
```

| Eigenschaft  | type | BESCHREIBUNG |
| ------------- | ------------- | ------------- |
| isRequestSuccess | boolean | „True“, wenn die HTTP-Anforderung mit einem Antwortstatuscode im Bereich 2xx oder 3xx abgeschlossen wird |
| time | date-time | Der Zeitstempel des Zeitpunkts, zu dem das Gateway mit dem Verarbeiten der Anforderung beginnt |
| operationName | string | Konstanter Wert „Microsoft.ApiManagement/GatewayLogs“ |
| category | string | Konstanter Wert „GatewayLogs“ |
| durationMs | integer | Anzahl von Millisekunden zwischen dem Zeitpunkt, zu dem das Gateway die Anforderung empfangen hat, und dem Zeitpunkt, zu dem die Antwort vollständig gesendet wurde. Dies beinhalt clientTime, cacheTime und backendTime. |
| callerIpAddress | string | IP-Adresse des unmittelbaren Gateway-Aufrufers (kann auch ein Zwischenaufrufer sein) |
| correlationId | string | Von API Management zugewiesene eindeutige HTTP-Anforderungs-ID |
| location | string | Name der Azure-Region, in der sich das Gateway befindet, das die Anforderung verarbeitet hat |
| httpStatusCodeCategory | string | Kategorie des HTTP-Anforderungsstatuscode: Erfolgreich (301 oder darunter, 304 oder 307), Nicht autorisiert (401, 403, 429), Erroneous (Fehler) (400, zwischen 500 und 600), Other (Sonstiges) |
| resourceId | string | ID der API Management-Ressource: /SUBSCRIPTIONS/\<Abonnement>/RESOURCEGROUPS/\<Ressourcengruppe>/PROVIDERS/MICROSOFT.APIMANAGEMENT/SERVICE/\<Name> |
| properties | Objekt (object) | Eigenschaften der aktuellen Anforderung |
| method | string | HTTP-Methode der eingehenden Anforderung |
| url | string | URL der eingehenden Anforderung |
| clientProtocol | string | HTTP-Protokollversion der eingehenden Anforderung |
| responseCode | integer | Statuscode der an einen Client gesendeten HTTP-Antwort |
| backendMethod | string | HTTP-Methode der an ein Back-End gesendeten Anforderung |
| backendUrl | string | URL der an ein Back-End gesendeten Anforderung |
| backendResponseCode | integer | Code der von einem Back-End empfangenen HTTP-Antwort |
| backendProtocol | string | HTTP-Protokollversion der an ein Back-End gesendeten Anforderung | 
| requestSize | integer | Anzahl der Bytes, die von einem Client während der Anforderungsverarbeitung empfangen werden | 
| responseSize | integer | Anzahl der Bytes, die während der Anforderungsverarbeitung an einen Client gesendet werden | 
| cache | string | Status der API Management-Cachenutzung bei der Anforderungsverarbeitung (d.h. Treffer, Fehler, Keiner) | 
| cacheTime | integer | Anzahl von Millisekunden für alle API Management-Cache-E/A-Vorgänge (Verbindungsherstellung, Senden und Empfangen von Bytes) | 
| backendTime | integer | Anzahl von Millisekunden für alle Back-End-E/A-Vorgänge (Verbindungsherstellung, Senden und Empfangen von Bytes) | 
| clientTime | integer | Anzahl von Millisekunden für alle Client-E/A-Vorgänge (Verbindungsherstellung, Senden und Empfangen von Bytes) | 
| apiId | string | API-Entitätsbezeichner für die aktuelle Anforderung | 
| operationId | string | Vorgangsentitätsbezeichner für die aktuelle Anforderung | 
| productId | string | Produktentitätsbezeichner für die aktuelle Anforderung | 
| userId | string | Benutzerentitätsbezeichner für die aktuelle Anforderung | 
| apimSubscriptionId | string | Abonnemententitätsbezeichner für die aktuelle Anforderung | 
| backendId | string | Back-End-Entitätsbezeichner für die aktuelle Anforderung | 
| lastError | Objekt (object) | Letzter Anforderungsverarbeitungsfehler | 
| elapsed | integer | Anzahl von Millisekunden, die zwischen dem Eingehen der Anforderung beim Gateway und dem Auftreten des Fehlers vergangen sind | 
| source | string | Der Fehler wurde durch den Namen der Richtlinie oder die Verarbeitung des internen Handlers verursacht. | 
| scope | string | Der Fehler wurde durch den Bereich des Richtliniendokuments verursacht, das die Richtlinie enthält. | 
| section | string | Der Fehler wurde durch den Abschnitt des Richtliniendokuments verursacht, der die Richtlinie enthält. | 
| reason | string | Fehlerursache | 
| message | string | Fehlermeldung | 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Anzeigen von Aktivitätsprotokollen
> * Anzeigen von Diagnoseprotokollen
> * Anzeigen von Metriken Ihrer API
> * Einrichten einer Warnungsregel, wenn Ihre API nicht autorisierte Aufrufe empfängt

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Verwenden des API-Inspektors zur Verfolgung von Aufrufen in Azure API Management](api-management-howto-api-inspector.md)
