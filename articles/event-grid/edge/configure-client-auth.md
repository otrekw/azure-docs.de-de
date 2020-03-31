---
title: Konfigurieren der Clientauthentifizierung von eingehenden Aufrufen – Azure Event Grid-IoT Edge | Microsoft-Dokumentation
description: Konfigurieren Sie API-Protokolle, die von Event Grid in IoT Edge verfügbar gemacht werden.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 3363db4557dd19e8d72747ccd62bb535abb7b1e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841790"
---
# <a name="configure-client-authentication-of-incoming-calls"></a>Konfigurieren der Clientauthentifizierung von eingehenden Aufrufen

Dieser Leitfaden enthält Beispiele zu möglichen Konfigurationen der Clientauthentifizierung für das Event Grid-Modul. Das Event Grid-Modul unterstützt zwei Arten von Clientauthentifizierung:

* SAS-Schlüssel-basiert (Shared Access Signature)
* Zertifikatbasiert

Informationen zu allen möglichen Konfigurationen finden Sie im Leitfaden zu [Sicherheit und Authentifizierung](security-authentication.md).

## <a name="enable-certificate-based-client-authentication-no-self-signed-certificates"></a>Aktivieren der zertifikatbasierten Clientauthentifizierung, keine selbstsignierten Zertifikate

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=false",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=false"
  ]
}
 ```

## <a name="enable-certificate-based-client-authentication-allow-self-signed-certificates"></a>Aktivieren der zertifikatbasierten Clientauthentifizierung, selbstsignierte Zertifikate zulassen

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=false",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=true"
  ]
}
```

>[!NOTE]
>Legen Sie die Eigenschaft **inbound__clientAuth__clientCert__allowUnknownCA** nur in Testumgebungen auf **true** fest, da Sie u. U. normalerweise selbstsignierte Zertifikate verwenden. Für Produktionsworkloads empfiehlt es sich, diese Eigenschaft auf **false** festzulegen und Zertifikate von einer Zertifizierungsstelle zu beziehen.

## <a name="enable-certificate-based-and-sas-key-based-client-authentication"></a>Aktivieren der zertifikatbasierten und der SAS-Schlüssel-basierten Clientauthentifizierung

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=true",
    "inbound__clientAuth__sasKeys__key1=<some-secret1-here>",
    "inbound__clientAuth__sasKeys__key2=<some-secret2-here>",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Die SAS-Schlüssel-basierte Clientauthentifizierung ermöglicht es einem Nicht-IoT Edge-Modul, Verwaltungs- und Runtimevorgänge auszuführen. Hierfür müssen natürlich die API-Ports außerhalb des IoT Edge-Netzwerks zugänglich sein.
