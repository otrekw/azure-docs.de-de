---
title: Konfigurieren der Authentifizierung für Webhookabonnenten – Azure Event Grid-IoT Edge | Microsoft-Dokumentation
description: Konfigurieren der Authentifizierung für Webhookabonnenten
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: d78b6e80005e9533ccd5ebfaea853f35ae6004be
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86171651"
---
# <a name="configure-webhook-subscriber-authentication"></a>Konfigurieren der Authentifizierung für Webhookabonnenten

Dieser Leitfaden enthält Beispiele zu möglichen Konfigurationen für Webhookabonnenten für ein Event Grid-Modul. Standardmäßig werden nur HTTPS-Endpunkte für Webhookabonnenten akzeptiert. Wenn der Abonnent ein selbstsigniertes Zertifikat präsentiert, wird dieses vom Event Grid-Modul abgelehnt.

## <a name="allow-only-https-subscriber"></a>Nur HTTPS-Abonnenten zulassen

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=false"
  ]
}
 ```

## <a name="allow-https-subscriber-with-self-signed-certificate"></a>HTTPS-Abonnenten mit selbstsigniertem Zertifikat zulassen

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Legen Sie die Eigenschaft `outbound__webhook__allowUnknownCA` nur in Testumgebungen auf `true` fest, da Sie u. U. normalerweise selbstsignierte Zertifikate verwenden. Für Produktionsworkloads empfiehlt es sich, sie auf **false** festzulegen.

## <a name="allow-https-subscriber-but-skip-certificate-validation"></a>HTTPS-Abonnenten zulassen, aber Zertifikatüberprüfung überspringen

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=true",
    "outbound__webhook__allowUnknownCA=false"
  ]
}
 ```

>[!NOTE]
>Legen Sie die Eigenschaft `outbound__webhook__skipServerCertValidation` nur in Testumgebungen auf `true` fest, da Sie möglicherweise kein Zertifikat präsentieren, das authentifiziert werden muss. Für Produktionsworkloads empfiehlt es sich, sie auf **false** festzulegen.

## <a name="allow-both-http-and-https-with-self-signed-certificates"></a>Sowohl HTTP als auch HTTPS mit selbstsignierten Zertifikaten zulassen

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=false",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Legen Sie die Eigenschaft `outbound__webhook__httpsOnly` nur in Testumgebungen auf `false` fest, da Sie möglicherweise erst einen HTTP-Abonnenten anzeigen möchten. Für Produktionsworkloads empfiehlt es sich, sie auf **true** festzulegen.
