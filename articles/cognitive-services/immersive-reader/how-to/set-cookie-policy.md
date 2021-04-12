---
title: Festlegen der Cookierichtlinie für den plastischen Reader
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird gezeigt, wie Sie die Cookierichtlinie für den plastischen Reader festlegen.
services: cognitive-services
author: nitinme
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: nitinme
ms.custom: devx-track-js
ms.openlocfilehash: 8b0a1f4a948aa6fec565130acb5267476a1d4401
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105048645"
---
# <a name="how-to-set-the-cookie-policy-for-the-immersive-reader"></a>Vorgehensweise: Festlegen der Cookierichtlinie für den plastischen Reader

Der plastische Reader deaktiviert standardmäßig die Verwendung von Cookies. Wenn Sie die Cookieverwendung aktivieren, kann der plastische Reader Cookies verwenden, um Benutzereinstellungen beizubehalten und die Verwendung von Features zu verfolgen. Wenn Sie die Cookieverwendung im plastischen Reader aktivieren, sollten Sie die Anforderungen der Cookiekonformitätsrichtlinie der EU beachten. Es liegt in der Verantwortung der Hostanwendung, die erforderliche Zustimmung des Benutzers in Übereinstimmung mit der Cookiekonformitätsrichtlinie der EU einzuholen.

Die Cookierichtlinie kann über die [Optionen](../reference.md#options) für den plastischen Reader festgelegt werden.

## <a name="enable-cookie-usage"></a>Aktivieren der Cookienutzung

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Enable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="disable-cookie-usage"></a>Deaktivieren der Cookienutzung

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Disable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich die [Node.js-Schnellstartanleitung](../quickstarts/client-libraries.md?pivots=programming-language-nodejs) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Node.js bietet.
* Sehen Sie sich das [Android-Tutorial](../how-to-launch-immersive-reader.md) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Java oder Kotlin für Android bietet.
* Sehen Sie sich das [iOS-Tutorial](../how-to-launch-immersive-reader.md) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Swift für iOS bietet.
* Sehen Sie sich das [Python-Tutorial](../how-to-launch-immersive-reader.md) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Python bietet.
* Machen Sie sich mit dem [SDK für Plastischer Reader](https://github.com/microsoft/immersive-reader-sdk) und der [zugehörigen Referenz](../reference.md) vertraut.