---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/11/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 020e59f029b09f3c7656f67039731e4141e68d31
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "67178695"
---
Wenn Python beim Starten der Anwendung einen Fehler erkennt, wird nur eine einfache Fehlerseite zurückgegeben (z. B. „Die Seite kann aufgrund eines internen Serverfehlers nicht angezeigt werden“).

So erfassen Sie Python-Anwendungsfehler

1. Wählen Sie im Azure-Portal in Ihrer Web-App **Einstellungen** aus.
2. Klicken Sie auf der Registerkarte **Einstellungen** auf **Anwendungseinstellungen**.
3. Geben Sie unter **App-Einstellungen** das folgende Schlüssel/Wert-Paar ein:
    * Schlüssel: WSGI_LOG
    * Wert: D:\home\site\wwwroot\logs.txt (geben Sie den Dateinamen Ihrer Wahl ein)

Fehler sollten jetzt im Ordner „wwwroot“ in der Datei „logs.txt“ angezeigt werden.
