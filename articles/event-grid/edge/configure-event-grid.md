---
title: Konfiguration – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Konfiguration in Event Grid in IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 841b5092775353bbe3340dbbd55610026f998a15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76846473"
---
# <a name="event-grid-configuration"></a>Event Grid-Konfiguration

Event Grid bietet viele Konfigurationen, die über die Umgebung geändert werden können. Im folgenden Abschnitt finden Sie eine Referenz für alle verfügbaren Optionen und ihre Standardwerte.

## <a name="tls-configuration"></a>TLS-Konfiguration

Informationen zur Clientauthentifizierung im Allgemeinen finden Sie unter [Sicherheit and Authentifizierung](security-authentication.md). Beispiele für die Verwendung finden Sie in [diesem Artikel](configure-api-protocol.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
|`inbound__serverAuth__tlsPolicy`| TLS-Richtlinie des Event Grid-Moduls. Der Standardwert ist nur „HTTPS“.
|`inbound__serverAuth__serverCert__source`| Quelle des Serverzertifikats, das vom Event Grid-Modul für seine TLS-Konfiguration verwendet wird. Der Standardwert ist „IoT Edge“.

## <a name="incoming-client-authentication"></a>Eingehende Clientauthentifizierung

Informationen zur Clientauthentifizierung im Allgemeinen finden Sie unter [Sicherheit and Authentifizierung](security-authentication.md). Beispiele finden Sie in [diesem Artikel](configure-client-auth.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
|`inbound__clientAuth__clientCert__enabled`| Zum De-/Aktivieren der zertifikatbasierten Clientauthentifizierung. Der Standardwert ist true.
|`inbound__clientAuth__clientCert__source`| Quelle für die Überprüfung von Clientzertifikaten. Der Standardwert ist „IoT Edge“.
|`inbound__clientAuth__clientCert__allowUnknownCA`| Richtlinie zum Zulassen eines selbstsignierten Clientzertifikats. Der Standardwert ist true.
|`inbound__clientAuth__sasKeys__enabled`| Zum De-/Aktivieren der SAS-Schlüssel-basierten Clientauthentifizierung. Der Standardwert ist „Aus“.
|`inbound__clientAuth__sasKeys__key1`| Einer der Werte, um eingehende Anforderungen zu überprüfen.
|`inbound__clientAuth__sasKeys__key2`| Optionaler zweiter Wert, um eingehende Anforderungen zu überprüfen.

## <a name="outgoing-client-authentication"></a>Ausgehende Clientauthentifizierung
Informationen zur Clientauthentifizierung im Allgemeinen finden Sie unter [Sicherheit and Authentifizierung](security-authentication.md). Beispiele finden Sie in [diesem Artikel](configure-identity-auth.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
|`outbound__clientAuth__clientCert__enabled`| Zum De-/Aktivieren des Anfügens eines Identitätszertifikats für ausgehende Anforderungen. Der Standardwert ist true.
|`outbound__clientAuth__clientCert__source`| Quelle zum Abrufen des ausgehenden Zertifikats des Event Grid-Moduls. Der Standardwert ist „IoT Edge“.

## <a name="webhook-event-handlers"></a>Webhook-Ereignishandler

Informationen zur Clientauthentifizierung im Allgemeinen finden Sie unter [Sicherheit and Authentifizierung](security-authentication.md). Beispiele finden Sie in [diesem Artikel](configure-webhook-subscriber-auth.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
|`outbound__webhook__httpsOnly`| Richtlinie zur Steuerung, ob nur HTTPS-Abonnenten zulässig sind. Der Standardwert ist „true“ (nur HTTPS).
|`outbound__webhook__skipServerCertValidation`| Flag zur Steuerung, ob das Zertifikat des Abonnenten überprüft werden soll. Der Standardwert ist true.
|`outbound__webhook__allowUnknownCA`| Richtlinie zur Steuerung, ob ein selbstsigniertes Zertifikat von einem Abonnenten vorgewiesen werden kann. Der Standardwert ist true. 

## <a name="delivery-and-retry"></a>Übermittlung und Wiederholung

Informationen zu dieser Funktion im Allgemeinen finden Sie unter [Übermittlung und Wiederholung](delivery-retry.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
| `broker__defaultMaxDeliveryAttempts` | Die maximale Anzahl der Versuche zur Übermittlung eines Ereignisses. Standardwert: 30.
| `broker__defaultEventTimeToLiveInSeconds` | Gültigkeitsdauer (Time-to-Live, TTL) in Sekunden, nach der ein Ereignis gelöscht wird, wenn es nicht übermittelt wird. Der Standardwert beträgt **7200** Sekunden.

## <a name="output-batching"></a>Ausgabebatches

Weitere Informationen zu dieser Funktion im Allgemeinen finden Sie unter [Übermittlung und Ausgabebatchverarbeitung](delivery-output-batching.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
| `api__deliveryPolicyLimits__maxBatchSizeInBytes` | Der zulässige Höchstwert für den Knopf `ApproxBatchSizeInBytes`. Der Standardwert ist `1_058_576`.
| `api__deliveryPolicyLimits__maxEventsPerBatch` | Der zulässige Höchstwert für den Knopf `MaxEventsPerBatch`. Der Standardwert ist `50`.
| `broker__defaultMaxBatchSizeInBytes` | Maximale Größe der Übermittlungsanforderung, wenn nur `MaxEventsPerBatch` angegeben ist. Der Standardwert ist `1_058_576`.
| `broker__defaultMaxEventsPerBatch` | Maximale Anzahl der einem Batch hinzuzufügenden Ereignisse, wenn nur `MaxBatchSizeInBytes` angegeben ist. Der Standardwert ist `10`.

## <a name="metrics"></a>metrics

Weitere Informationen zur Verwendung von Metriken mit Event Grid in IoT Edge finden Sie unter [Überwachen von Themen und Abonnements](monitor-topics-subscriptions.md).

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
| `metrics__reporterType` | Berichterstattertyp für den Metrikendpunkt. Der Standardwert ist `none` und deaktiviert Metriken. Wenn Sie ihn auf `prometheus` festlegen, werden Metriken im Prometheus-Offenlegungsformat aktiviert.