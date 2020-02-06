---
title: Konfigurieren der Identität – Azure Event Grid-IoT Edge | Microsoft-Dokumentation
description: Konfigurieren der Identität des Event Grid-Moduls
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/05/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 0aedeea2a6ad08e1627c2d1a6ebde6c91a4d02d9
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76841764"
---
# <a name="configure-identity-for-the-event-grid-module"></a>Konfigurieren der Identität für das Event Grid-Modul

In diesem Artikel wird gezeigt, wie Sie die Identität für Event Grid in Edge konfigurieren. In der Standardeinstellung präsentiert das Event Grid-Modul sein Identitätszertifikat entsprechend der Konfiguration durch den IoT-Sicherheitsdaemon. Event Grid in Edge stellt das Identitätszertifikat mit ausgehenden Aufrufen beim Übermitteln von Ereignissen bereit. Ein Abonnent kann dann vor dem Akzeptieren überprüfen, ob es sich um das Event Grid-Modul handelt, das das Ereignis gesendet hat.

Informationen zu allen möglichen Konfigurationen finden Sie im Leitfaden zu [Sicherheit und Authentifizierung](security-authentication.md).

## <a name="always-present-identity-certificate"></a>Immer vorhandenes Identitätszertifikat
Dies ist eine Beispielkonfiguration, bei der bei ausgehenden Aufrufen immer ein Identitätszertifikat präsentiert wird. 

```json
 {
  "Env": [
    "outbound__clientAuth__clientCert__enabled=true",
    "outbound__clientAuth__clientCert__source=IoTEdge"
  ]
}
 ```

## <a name="dont-present-identity-certificate"></a>Kein Identitätszertifikat wird präsentiert
Dies ist eine Beispielkonfiguration, bei der bei ausgehenden Aufrufen kein Identitätszertifikat präsentiert wird. 

```json
 {
  "Env": [
    "outbound__clientAuth__clientCert__enabled=false"
  ]
}
 ```
