---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.date: 02/14/2020
ms.subservice: language-understanding
ms.topic: include
ms.author: diberry
ms.openlocfilehash: 5b1b361778de145a5e32a07bb0164ff2293d9a1a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77279670"
---
Kopieren Sie im Abschnitt **Verwalten** (Menü oben rechts) auf der Seite **Azure-Ressourcen** (Menü auf der linken Seite) die **Beispielabfrage**-URL, und fügen Sie sie in eine neue Browserregisterkarte ein.

Die Endpunkt-URL sieht wie folgt aus, wobei Ihre eigene App-ID und Ihr eigener Endpunktschlüssel APP-ID und KEY-ID ersetzen:

```console
https://westus.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```