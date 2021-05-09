---
title: Datenreferenz zur Überwachung von Media Services
description: Wichtiges Referenzmaterial für die Überwachung von Media Services
author: IngridAtMicrosoft
ms.author: inhenkel
manager: femila
ms.topic: reference
ms.service: media-services
ms.custom: subject-monitoring
ms.date: 04/21/2021
ms.openlocfilehash: 1fff4cdf24f9be245026757e18050aa121b4d62a
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108138857"
---
# <a name="monitoring-media-services-data-reference"></a>Datenreferenz zur Überwachung von Media Services

In diesem Artikel werden die Daten behandelt, die für die Überwachung von Media Services nützlich sind. Weitere Informationen zu allen Plattformmetriken, die in Azure Monitor unterstützt werden, finden Sie unter [In Azure Monitor unterstützte Metriken](../../../azure-monitor/essentials/metrics-supported.md).

## <a name="metrics"></a>Metriken

Metriken werden in regelmäßigen Abständen gesammelt, unabhängig davon, ob sich der Wert ändert oder nicht. Sie sind für Warnmeldungen gut geeignet, da sie häufig abgetastet werden können und eine Warnung mit relativ einfacher Logik schnell ausgelöst werden kann.


Media Services unterstützt das Überwachen von Metriken für die folgenden Ressourcen:

|Metriktyp | Ressourcenanbieter/Typnamespace<br/> und Link zu einzelnen Metriken |
|-------|-----|
| Media Services: Allgemein | [Allgemein](../../../azure-monitor/essentials/metrics-supported.md#microsoftmediamediaservices) |
| Liveereignisse | [Microsoft.Media/mediaservices/liveEvents](../../../azure-monitor/essentials/metrics-supported.md#microsoftmediamediaservicesliveevents) 
| Streamingendpunkte | [Microsoft.Media/mediaservices/streamingEndpoints](../../../azure-monitor/essentials/metrics-supported.md#microsoftmediamediaservicesstreamingendpoints), die für die [REST-API der Streamingendpunkte](/rest/api/media/streamingendpoints) relevant sind. 


Informieren Sie sich außerdem über [Kontingente und Grenzwerte für Konten](../limits-quotas-constraints-reference.md).


## <a name="metric-dimensions"></a>Metrikdimensionen

Weitere Informationen zu Metrikdimensionen finden Sie unter [Mehrdimensionale Metriken](../../../azure-monitor/essentials/data-platform-metrics.md#multi-dimensional-metrics).

Media Services umfasst die folgenden Metrikdimensionen.  Sie sind selbsterklärend, basierend auf den Metriken, die sie unterstützen.  Weitere Informationen finden Sie über die oben angegebenen [Metriklinks](#metrics).   
- OutputFormat
- HttpStatusCode 
- ErrorCode 
- TrackName 

## <a name="resource-logs"></a>Ressourcenprotokolle

Ressourcenprotokolle liefern regelmäßig erfasste umfassende Daten zum Betrieb einer Azure-Ressource. Weitere Informationen finden Sie unter [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](../../../azure-monitor/essentials/platform-logs-overview.md).

Media Services unterstützt die folgenden Ressourcenprotokolle: [Microsoft.Media/mediaservices](../../../azure-monitor/essentials/resource-logs-categories.md#microsoftmediamediaservices)

## <a name="schemas"></a>Schemas

Eine detaillierte Beschreibung des Schemas der obersten Ebene für Diagnoseprotokolle finden Sie unter [Unterstützte Dienste, Schemas und Kategorien für Azure-Diagnoseprotokolle](../../../azure-monitor/essentials/resource-logs-schema.md).

## <a name="key-delivery-log-schema-properties"></a>Eigenschaften des Protokollschemas der Schlüsselübermittlung

Diese Eigenschaften sind spezifisch für das Protokollschema für Schlüsselübermittlung.

|Name|BESCHREIBUNG|
|---|---|
|keyId|Die ID des angeforderten Schlüssels.|
|keyType|Kann einen der folgenden Werte aufweisen: „Clear“ (keine Verschlüsselung), „FairPlay“, „PlayReady“ oder „Widevine“.|
|policyName|Der Azure Resource Manager-Name der Richtlinie.|
|tokenType|Der Tokentyp.|
|statusMessage|Die Statusmeldung.|

### <a name="example"></a>Beispiel

Eigenschaften des Schemas für Schlüsselübermittlungsanforderungen.

```json
{
    "time": "2019-01-11T17:59:10.4908614Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Succeeded",
    "resultSignature": "OK",
    "durationMs": 315,
    "identity": {
        "authorization": {
            "issuer": "http://testacs",
            "audience": "urn:test"
        },
        "claims": {
            "urn:microsoft:azure:mediaservices:contentkeyidentifier": "3321e646-78d0-4896-84ec-c7b98eddfca5",
            "iss": "http://testacs",
            "aud": "urn:test",
            "exp": "1547233138"
        }
    },
    "level": "Informational",
    "location": "uswestcentral",
    "properties": {
        "requestId": "b0243468-d8e5-4edf-a48b-d408e1661050",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "JWT",
        "statusMessage": "OK"
    }
} 
```

```json
 {
    "time": "2019-01-11T17:59:33.4676382Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Failed",
    "resultSignature": "Unauthorized",
    "durationMs": 2,
    "level": "Error",
    "location": "uswestcentral",
    "properties": {
        "requestId": "875af030-b77c-416b-b7e1-58f23ebec182",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "None",
        "statusMessage": "No token present in authorization header or URL."
    }
} 
```

>[!NOTE]
> Widevine ist ein von Google Inc. bereitgestellter Dienst, der den Vertragsbedingungen und der Datenschutzrichtlinie von Google, Inc. unterliegt.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [monitoring-next-steps](../includes/monitoring-next-steps.md)]