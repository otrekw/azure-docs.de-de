---
title: Azure App Service als Event Grid-Quelle
description: In diesem Artikel wird beschrieben, wie Azure App Service als Event Grid-Ereignisquelle verwendet wird. Er enthält das Schema sowie Links zu Tutorials und Artikeln mit Vorgehensweisen.
services: event-grid
author: jasonfreeberg
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: jafreebe
ms.openlocfilehash: 0a24e8ba84739dbc1b5de5e0546a8fe0d2e826f1
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650696"
---
# <a name="azure-app-service-as-an-event-grid-source"></a>Azure App Service als Event Grid-Quelle

In diesem Artikel werden die Eigenschaften und das Schema für Azure App Service-Ereignisse beschrieben. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md). Außerdem erhalten Sie eine Liste mit Schnellstarts und Tutorials, die Azure App Service als Ereignisquelle verwenden.

## <a name="event-grid-event-schema"></a>Event Grid-Ereignisschema

### <a name="available-event-types"></a>Verfügbare Ereignistypen

Azure App Service gibt die folgenden Ereignistypen aus:

|    Ereignistyp                                             |    BESCHREIBUNG                                                     |
|-----------------------------------------------------------|--------------------------------------------------------------------|
|    Microsoft.Web/sites.BackupOperationStarted             |    Wird ausgelöst, wenn eine Sicherung gestartet wurde.                             |
|    Microsoft.Web/sites.BackupOperationCompleted           |    Wird ausgelöst, wenn eine Sicherung abgeschlossen wurde.                           |
|    Microsoft.Web/sites.BackupOperationFailed              |    Wird ausgelöst, wenn eine Sicherung fehlgeschlagen ist.                              |
|    Microsoft.Web/sites.RestoreOperationStarted            |    Wird ausgelöst, wenn eine Wiederherstellung auf Grundlage einer Sicherung gestartet wurde.        |
|    Microsoft.Web/sites.RestoreOperationCompleted          |    Wird ausgelöst, wenn eine Wiederherstellung auf Grundlage einer Sicherung abgeschlossen wurde.      |
|    Microsoft.Web/sites.RestoreOperationFailed             |    Wird ausgelöst, wenn eine Wiederherstellung auf Grundlage einer Sicherung fehlgeschlagen ist.         |
|    Microsoft.Web/sites.SlotSwapStarted                    |    Wird ausgelöst, wenn ein Slottausch gestartet wurde.                          |
|    Microsoft.Web/sites.SlotSwapCompleted                  |    Wird ausgelöst, wenn ein Slottausch abgeschlossen wurde.                      |
|    Microsoft.Web/sites.SlotSwapFailed                     |    Wird ausgelöst, wenn ein Slottausch fehlgeschlagen ist.                           |
|    Microsoft.Web/sites.SlotSwapWithPreviewStarted         |    Wird ausgelöst, wenn ein Slottausch mit Vorschau gestartet wurde.           |
|    Microsoft.Web/sites.SlotSwapWithPreviewCancelled       |    Wird ausgelöst, wenn ein Slottausch mit Vorschau abgebrochen wurde.    |
|    Microsoft.Web/sites.AppUpdated.Restarted               |    Wird ausgelöst, wenn eine Website neu gestartet wurde.                      |
|    Microsoft.Web/sites.AppUpdated.Stopped                 |    Wird ausgelöst, wenn eine Website angehalten wurde.                          |
|    Microsoft.Web/sites.AppUpdated.ChangedAppSettings      |    Wird ausgelöst, wenn die App-Einstellungen einer Website geändert wurden.             |
|    Microsoft.Web/serverfarms.AppServicePlanUpdated        |    Wird ausgelöst, wenn ein App Service-Plan aktualisiert wurde.                 |

### <a name="the-contents-of-an-event-response"></a>Der Inhalt einer Ereignisantwort

Wenn ein Ereignis ausgelöst wird, sendet der Event Grid-Dienst Daten zum Ereignis an den Endpunkt, der über ein entsprechendes Abonnement verfügt.
In diesem Abschnitt wird anhand eines Beispiels gezeigt, wie diese Daten für jedes Ereignis aussehen. Jedes Ereignis weist die folgenden Daten auf oberster Ebene auf:

|     Eigenschaft          |     type     |     BESCHREIBUNG                                                                                                                                |
|-----------------------|--------------|------------------------------------------------------------------------------------------------------------------------------------------------|
|    topic              |    Zeichenfolge    |    Vollständiger Ressourcenpfad zur Ereignisquelle. Dieses Feld ist nicht beschreibbar. Dieser Wert wird von Event Grid bereitgestellt.                                      |
|    subject            |    Zeichenfolge    |    Vom Herausgeber definierter Pfad zum Ereignisbetreff.                                                                                              |
|    eventType          |    Zeichenfolge    |    Einer der registrierten Ereignistypen für die Ereignisquelle.                                                                                  |
|    eventTime          |    Zeichenfolge    |    Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters.                                                                         |
|    id                 |    Zeichenfolge    |    Eindeutiger Bezeichner für das Ereignis.                                                                                                            |
|    data               |    Objekt (object)    |    Ereignisdaten für Blob Storage.                                                                                                                    |
|    dataVersion        |    Zeichenfolge    |    Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion.                                                          |
|    metadataVersion    |    Zeichenfolge    |    Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt.    |

#### <a name="backupoperationstarted-backupoperationcompleted-backupoperationfailed"></a>BackupOperationStarted, BackupOperationCompleted, BackupOperationFailed

```js
{
    id:'7c5d6de5-eb70-4de2-b788-c52a544e68b8',
    subject:'/Microsoft.Web/sites/<site-name>',
    eventType:'Microsoft.Web.BackupOperationStarted',
    eventTime:'2020-01-28T18:26:51.7194887Z',
    data: {
        "appEventTypeDetail": { "action": "Started" },
        "siteName": "<site-name>",
        "clientRequestId": "None",
        "correlationRequestId": "None",
        "requestId": "292f499d-04ee-4066-994d-c2df57b99198",
        "address": "None",
        "verb": "None"
    }
    topic:'/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>',
    dataVersion:'1',
    metaDataVersion:'1'
}
```

Das Datenobjekt enthält die folgenden Eigenschaften:

|    Eigenschaft                |    type      |    BESCHREIBUNG                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appEventTypeDetail      |    Objekt (object)    |    Detail einer Aktion für die App                                                                                       |
|    action                  |    Zeichenfolge    |    Typ der Aktion des Vorgangs                                                                                   |
|    name                    |    Zeichenfolge    |    Name der Website, auf der das Ereignis aufgetreten ist                                                                          |
|    clientRequestId         |    Zeichenfolge    |    Die Clientanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.         |
|    correlationRequestId    |    Zeichenfolge    |    Die Korrelationsanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.    |
|    requestId               |    Zeichenfolge    |    Die Anforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.                |
|    address                 |    Zeichenfolge    |    HTTP-Anforderungs-URL für diesen Vorgang                                                                                |
|    Verb                    |    Zeichenfolge    |    HTTP-Verb für diesen Vorgang                                                                                       |

#### <a name="restoreoperationstarted-restoreoperationcompleted-restoreoperationfailed"></a>RestoreOperationStarted, RestoreOperationCompleted, RestoreOperationFailed

```js
{
    id: '7c5d6de5-eb70-4de2-b788-c52a544e68b8',
    subject: '/Microsoft.Web/sites/<site-name>',
    eventType: 'Microsoft.Web.RestoreOperationStarted',
    eventTime: '2020-01-28T18:26:51.7194887Z',
    data: {
        appEventTypeDetail: { 
            action: "Started" 
        },
        siteName: "<site-name>",
        clientRequestId: "None",
        correlationRequestId: "None",
        requestId: "292f499d-04ee-4066-994d-c2df57b99198",
        address: "None",
        verb: "POST"
    }
    topic: '/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>',
    dataVersion: '1',
    metaDataVersion: '1'
}
```

Das Datenobjekt enthält die folgenden Eigenschaften:

|    Eigenschaft                |    type      |    BESCHREIBUNG                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appEventTypeDetail      |    Objekt (object)    |    Detail einer Aktion für die App                                                                                       |
|    action                  |    Zeichenfolge    |    Typ der Aktion des Vorgangs                                                                                   |
|    name                    |    Zeichenfolge    |    Name der Website, auf der das Ereignis aufgetreten ist                                                                          |
|    clientRequestId         |    Zeichenfolge    |    Die Clientanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.         |
|    correlationRequestId    |    Zeichenfolge    |    Die Korrelationsanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.    |
|    requestId               |    Zeichenfolge    |    Die Anforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.                |
|    address                 |    Zeichenfolge    |    HTTP-Anforderungs-URL für diesen Vorgang                                                                                |
|    Verb                    |    Zeichenfolge    |    HTTP-Verb für diesen Vorgang                                                                                       |

#### <a name="slotswapstarted-slotswapcompleted-slotswapfailed"></a>SlotSwapStarted, SlotSwapCompleted, SlotSwapFailed

```js
{
    id: '7c5d6de5-eb70-4de2-b788-c52a544e68b8',
    subject: '/Microsoft.Web/sites/<site-name>',
    eventType: 'Microsoft.Web.SlotSwapStarted',
    eventTime: '2020-01-28T18:26:51.7194887Z',
    data: {
        appEventTypeDetail: null,
        siteName: '<site-name>',
        clientRequestId: '922f4841-20d9-4dd6-8c5b-23f0d85e5592',
        correlationRequestId: '9ac46505-2b8a-4e06-834c-05ffbe2e8c3a',
        requestId: '765117aa-eaf8-4bd2-a644-1dbf69c7b0fd',
        address: '/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/slots?Command=SWAP&targetSlot=production',
        verb: 'POST'
        sourceSlot: "staging",
        targetSlot: "production"
    },
    topic: '/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>',
    dataVersion: '1',
    metaDataVersion: '1'
}
```

Das Datenobjekt enthält die folgenden Eigenschaften:

|    Eigenschaft                |    type      |    BESCHREIBUNG                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appEventTypeDetail      |    Objekt (object)    |    Detail einer Aktion für die App                                                                                       |
|    action                  |    Zeichenfolge    |    Typ der Aktion des Vorgangs                                                                                   |
|    name                    |    Zeichenfolge    |    Name der Website, auf der das Ereignis aufgetreten ist                                                                          |
|    clientRequestId         |    Zeichenfolge    |    Die Clientanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.         |
|    correlationRequestId    |    Zeichenfolge    |    Die Korrelationsanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.    |
|    requestId               |    Zeichenfolge    |    Die Anforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.                |
|    address                 |    Zeichenfolge    |    HTTP-Anforderungs-URL für diesen Vorgang                                                                                |
|    Verb                    |    Zeichenfolge    |    HTTP-Verb für diesen Vorgang                                                                                       |
|    sourceSlot              |    Zeichenfolge    |    Der Quellslot des Tauschs                                                                                       |

#### <a name="slotswapwithpreviewstarted-slotswapwithpreviewcancelled"></a>SlotSwapWithPreviewStarted, SlotSwapWithPreviewCancelled

```js
{
    id: '7c5d6de5-eb70-4de2-b788-c52a544e68b8',
    subject: '/Microsoft.Web/sites/<site-name>',
    eventType: 'Microsoft.Web.SlotSwapWithPreviewStarted',
    eventTime: '2020-01-28T18:26:51.7194887Z',
    data: {
        appEventTypeDetail: null,
        siteName: '<site-name>',
        clientRequestId: '922f4841-20d9-4dd6-8c5b-23f0d85e5592',
        correlationRequestId: '9ac46505-2b8a-4e06-834c-05ffbe2e8c3a',
        requestId: '765117aa-eaf8-4bd2-a644-1dbf69c7b0fd',
        address: '/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/slots?Command=SWAP&targetSlot=production',
        verb: 'POST'
        sourceSlot: "staging",
        targetSlot: "production"
    },
    topic: '/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>',
    dataVersion: '1',
    metaDataVersion: '1'
}
```

Das Datenobjekt enthält die folgenden Eigenschaften:

|    Eigenschaft                |    type      |    BESCHREIBUNG                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appEventTypeDetail      |    Objekt (object)    |    Detail einer Aktion für die App                                                                                       |
|    action                  |    Zeichenfolge    |    Typ der Aktion des Vorgangs                                                                                   |
|    name                    |    Zeichenfolge    |    Name der Website, auf der das Ereignis aufgetreten ist                                                                          |
|    clientRequestId         |    Zeichenfolge    |    Die Clientanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.         |
|    correlationRequestId    |    Zeichenfolge    |    Die Korrelationsanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.    |
|    requestId               |    Zeichenfolge    |    Die Anforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.                |
|    address                 |    Zeichenfolge    |    HTTP-Anforderungs-URL für diesen Vorgang                                                                                |
|    Verb                    |    Zeichenfolge    |    HTTP-Verb für diesen Vorgang                                                                                       |

#### <a name="appupdatedrestarted-appupdatedstopped-appupdatedchangedappsettings"></a>AppUpdated.Restarted, AppUpdated.Stopped, AppUpdated.ChangedAppSettings

```js
{
    id: 'b74ea56b-2a3f-4de5-a5d7-38e60c81cf23',
    subject: '/Microsoft.Web/sites/<site-name>',
    eventType: 'Microsoft.Web.AppUpdated',
    eventTime: '2020-01-28T18:22:30.2760952Z',
    data: {
        appEventTypeDetail: {
            action: 'Stopped'
        },
        siteName: '<site-name>',
        clientRequestId: '64a5e0aa-7cee-4ff1-9093-b9197b820014',
        correlationRequestId: '25bb36a5-8f6c-4f04-b615-e9a0ee045756',
        requestId: 'f2e8eb3f-b190-42de-b99e-6acefe587374',
        address: '/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/stop',
        verb: 'POST'
    },
    topic: '/subscriptions/<id>/resourceGroups/<group>/providers/Microsoft.Web/sites/<site-name>',
    dataVersion: '1',
    metaDataVersion: '1'
}
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

|    Eigenschaft                |    type      |    BESCHREIBUNG                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appEventTypeDetail      |    Objekt (object)    |    Detail einer Aktion für die App                                                                                       |
|    action                  |    Zeichenfolge    |    Typ der Aktion des Vorgangs                                                                                   |
|    name                    |    Zeichenfolge    |    Name der Website, auf der das Ereignis aufgetreten ist                                                                          |
|    clientRequestId         |    Zeichenfolge    |    Die Clientanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.         |
|    correlationRequestId    |    Zeichenfolge    |    Die Korrelationsanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.    |
|    requestId               |    Zeichenfolge    |    Die Anforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.                |
|    address                 |    Zeichenfolge    |    HTTP-Anforderungs-URL für diesen Vorgang                                                                                |
|    Verb                    |    Zeichenfolge    |    HTTP-Verb für diesen Vorgang                                                                                       |

#### <a name="serverfarmsappserviceplanupdated"></a>Serverfarms.AppServicePlanUpdated

```js
{
   id: "56501672-9150-40e1-893a-18420c7fdbf7",
   subject: "/Microsoft.Web/serverfarms/<plan-name>",
   eventType: "Microsoft.Web.AppServicePlanUpdated",
   eventTime: "2020-01-28T18:22:23.5516004Z",
   data: {
        serverFarmEventTypeDetail: {
            stampKind: "Public",
            action: "Updated",
            status: "Started"
        },
        serverFarmId: "0",
        sku: {
            name: "P1v2",
            tier: "PremiumV2",
            size: "P1v2",
            family: "Pv2",
            capacity: 1
        },
        clientRequestId: "8f880321-a991-45c7-b743-6ff63fe4c004",
        correlationRequestId: "1995c3be-ba7f-4ccf-94af-516df637ec8a",
        requestId: "b973a8e6-6949-4783-b44c-ac778be831bb",
        address: "/websystems/WebSites/serverfarms/subscriptions/<id>/webspaces/<webspace-id>/serverfarms/<plan-name>/async",
        verb: "PUT"
   },
   topic: "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/serverfarms/<serverfarm-name>",
   dataVersion: "1",
   metaDataVersion: "1"
}
```

Das Datenobjekt weist die folgenden Eigenschaften auf:

|    Eigenschaft                         |    type      |    BESCHREIBUNG                                                                                                       |
|-------------------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appServicePlanEventTypeDetail    |    Objekt (object)    |    Detail einer Aktion für den App Service-Plan                                                                          |
|    stampKind                        |    Zeichenfolge    |    Art der Umgebung, in der sich der App Service-Plan befindet                                                                     |
|    action                           |    Zeichenfolge    |    Typ der Aktion für den App Service-Plan                                                                            |
|    status                           |    Zeichenfolge    |    Status des Vorgangs für den App Service-Plan                                                                   |
|    sku                              |    Objekt (object)    |    SKU des App Service-Plans                                                                                       |
|    name                             |    Zeichenfolge    |    Name des App Service-Plans                                                                                      |
|    Tarif                             |    Zeichenfolge    |    Dienstebene des App Service-Plans                                                                                      |
|    Size                             |    Zeichenfolge    |    Größe des App Service-Plans                                                                                      |
|    Familie                           |    Zeichenfolge    |    Familie des App Service-Plans                                                                                        |
|    Capacity                         |    Zeichenfolge    |    Kapazität des App Service-Plans                                                                                      |
|    action                           |    Zeichenfolge    |    Typ der Aktion des Vorgangs                                                                                   |
|    name                             |    Zeichenfolge    |    Name der Website, auf der das Ereignis aufgetreten ist                                                                          |
|    clientRequestId                  |    Zeichenfolge    |    Die Clientanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.         |
|    correlationRequestId             |    Zeichenfolge    |    Die Korrelationsanforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.    |
|    requestId                        |    Zeichenfolge    |    Die Anforderungs-ID, die vom App-Dienst für den Website-API-Vorgang generiert wird, der das Ereignis ausgelöst hat.                |
|    address                          |    Zeichenfolge    |    HTTP-Anforderungs-URL für diesen Vorgang                                                                                |
|    Verb                             |    Zeichenfolge    |    HTTP-Verb für diesen Vorgang                                                                                       |
