---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.date: 05/06/2020
ms.subservice: language-understanding
ms.topic: include
ms.author: diberry
ms.openlocfilehash: a6e6c89c34723fb9e11b0c7e4ab8c9bedb8aa9ca
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85959025"
---
Kopieren Sie im Abschnitt **Verwalten** (Menü rechts oben) auf der Seite **Azure-Ressourcen** (linkes Menü) die URL der **Beispielabfrage**, und fügen Sie sie in einen neuen Browsertab ein.

Die Endpunkt-URL sieht wie folgt aus (mit Ihrer eigene App-ID und Ihrem eigenen Endpunktschlüssel anstelle von „APP-ID“ und „KEY-ID“):

```console
https://YOUR-CUSTOM-SUBDOMAIN.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```