---
title: Konfigurieren der Identität – Azure Event Grid-IoT Edge | Microsoft-Dokumentation
description: Konfigurieren der Identität des Event Grid-Moduls
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: 2418c8518bbf298a102d4f29b4a973f6121de2eb
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171685"
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
