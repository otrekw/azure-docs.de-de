---
title: Häufige Fehlercodes für den Azure Instance Metadata Service (IMDS)
titleSuffix: Azure Load Balancer
description: Übersicht über häufige Fehlercodes und die entsprechenden Methoden zur Entschärfung für Azure Instance Metadata Service (IMDS)
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: troubleshooting
ms.date: 02/12/2021
ms.author: allensu
ms.openlocfilehash: e932e211996a05b2740613381735a7de3492e5bf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100519183"
---
# <a name="error-codes-common-error-codes-when-using-imds-to-retrieve-load-balancer-information"></a>Fehlercodes: Häufige Fehlercodes beim Verwenden von IMDS beim Abrufen von Lastenausgleichsinformationen

In diesem Artikel werden häufige Bereitstellungsfehler beschrieben und erläutert, wie Sie diese Fehler beheben können, wenn Sie Azure Instance Metadata Service (IMDS) verwenden.

## <a name="error-codes"></a>Fehlercodes

| Fehlercode | Fehlermeldung | Details und Entschärfung |
| --- | ---------- | ----------------- |
| 400 | Erforderlicher Parameter fehlt: „\<ParameterName>“. Erledigen Sie bitte die Anfrage, und versuchen Sie es noch einmal. | Der Fehlercode weist auf einen fehlenden Parameter hin. </br> Weitere Informationen zum Hinzufügen des fehlenden Parameters finden Sie unter [Abrufen von Lastenausgleichs-Metadaten mithilfe des Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response).
| 400 | Der Parameterwert ist nicht zulässig, oder der Parameterwert „\<ParameterValue>“ ist für den Parameter „ParameterName“ nicht zulässig. Erledigen Sie bitte die Anfrage, und versuchen Sie es noch einmal. | Der Fehlercode gibt an, dass das Anforderungsformat nicht ordnungsgemäß konfiguriert ist. </br> Weitere Informationen zum Korrigieren des Anforderungstexts und zum Auslösen eines Wiederholungsversuchs finden Sie unter [Abrufen von Lastenausgleichs-Metadaten mithilfe des Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response). |
| 400 | Unerwartete Anforderung. Überprüfen Sie die Abfrageparameter, und versuchen Sie es noch einmal. | Der Fehlercode gibt an, dass das Anforderungsformat nicht ordnungsgemäß konfiguriert ist. </br> Weitere Informationen zum Korrigieren des Anforderungstexts und zum Auslösen eines Wiederholungsversuchs finden Sie unter [Abrufen von Lastenausgleichs-Metadaten mithilfe des Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response). |
| 404 | Es wurden keine Lastenausgleichs-Metadaten gefunden. Überprüfen Sie, ob Ihre VM eine Load Balancer-Instanz mit einer anderen SKU als „Basic“ verwendet, und versuchen Sie es später erneut. | Der Fehlercode gibt an, dass die VM keiner Load Balancer-Instanz zugeordnet ist oder dass diese die Basic-SKU anstelle der Standard-SKU verwendet. </br> Weitere Informationen finden Sie unter [Quickstart: Erstellen eines öffentlichen Lastenausgleichs für VMs über das Azure-Portal](quickstart-load-balancer-standard-public-portal.md?tabs=option-1-create-load-balancer-standard). Dort erfahren Sie, wie Sie Load Balancer Standard bereitstellen.|
| 404 | API wurde nicht gefunden: Pfad = „\<UrlPath>“, Methode = „\<Method>“ | Der Fehlercode gibt an, dass der Pfad falsch konfiguriert wurde. </br> Weitere Informationen zum Korrigieren des Anforderungstexts und zum Auslösen eines Wiederholungsversuchs finden Sie unter [Abrufen von Lastenausgleichs-Metadaten mithilfe des Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response).|
| 405 | Die HTTP-Methode ist nicht zulässig. Pfad = „\<UrlPath>“, Methode = „\<Method>“ | Der Fehlercode weist auf ein nicht unterstütztes HTTP-Verb hin. </br> Weitere Informationen zu unterstützten Verben finden Sie unter [Azure Instance Metadata Service (IMDS)](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#http-verbs). |
| 429 | Zu viele Anforderungen | Der Fehlercode weist auf ein Ratenlimit hin. </br> Weitere Informationen zur Ratenbegrenzung finden Sie unter [Azure Instance Metadata Service (IMDS)](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#rate-limiting).|
| 400 | Der Anforderungstext ist größer als MaxBodyLength: … | Der Fehlercode gibt an, dass eine Anforderung größer ist als MaxBodyLength. </br> Weitere Informationen zur Textlänge finden Sie unter [Abrufen von Lastenausgleichs-Metadaten mithilfe des Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response).|
| 400 | Die Länge des Parameterschlüssels ist größer als MaxParameterKeyLength: … | Der Fehlercode gibt an, dass die Länge eines Parameterschlüssels größer ist als MaxParameterKeyLength. </br> Weitere Informationen zur Textlänge finden Sie unter [Abrufen von Lastenausgleichs-Metadaten mithilfe des Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response). |
| 400 | Die Länge des Parameterwerts ist größer als MaxParameterValueLength: … | Der Fehlercode gibt an, dass die Länge eines Parameterwerts größer ist als MaxParameterValueLength. </br> Weitere Informationen zur Wertlänge finden Sie unter [Abrufen von Lastenausgleichs-Metadaten mithilfe des Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response).|
| 400 | Die Länge des Parameterheaderwerts ist größer als MaxHeaderValueLength: … | Der Fehlercode gibt an, dass die Länge eines Parameterheaderwerts größer ist als MaxHeaderValueLength. </br> Weitere Informationen zur Wertlänge finden Sie unter [Abrufen von Lastenausgleichs-Metadaten mithilfe des Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response).|
| 404 | Die Load Balancer-Metadaten-API ist momentan nicht verfügbar. Versuchen Sie es später noch einmal. | Der Fehlercode gibt an, dass die API möglicherweise gerade bereitgestellt wird. Wiederholen Sie Ihre Anforderung später noch einmal. |
| 404 | /metadata/loadbalancer ist zurzeit nicht verfügbar. | Der Fehlercode gibt an, dass die API gerade aktiviert wird. Wiederholen Sie Ihre Anforderung später noch einmal. |
| 503 | Interner Dienst ist nicht verfügbar. Versuchen Sie es später noch einmal.  | Der Fehlercode gibt an, dass die API vorübergehend nicht verfügbar ist. Wiederholen Sie Ihre Anforderung später noch einmal. |
|  |  |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md)

