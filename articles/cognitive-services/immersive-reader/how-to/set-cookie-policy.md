---
title: Festlegen der Cookierichtlinie für den plastischen Reader
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird gezeigt, wie Sie die Cookierichtlinie für den plastischen Reader festlegen.
services: cognitive-services
author: pasta
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: pasta
ms.openlocfilehash: 876379a211038933cf665bd1a4a4daae9c9b2787
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267068"
---
# <a name="how-to-set-the-cookie-policy-for-the-immersive-reader"></a>Vorgehensweise: Festlegen der Cookierichtlinie für den plastischen Reader

Der plastische Reader deaktiviert standardmäßig die Verwendung von Cookies. Wenn Sie die Cookieverwendung aktivieren, kann der plastische Reader Cookies verwenden, um Benutzereinstellungen beizubehalten und die Verwendung von Features zu verfolgen. Wenn Sie die Cookieverwendung im plastischen Reader aktivieren, sollten Sie die Anforderungen der Cookiekonformitätsrichtlinie der EU beachten. Es liegt in der Verantwortung der Hostanwendung, die erforderliche Zustimmung des Benutzers in Übereinstimmung mit der Cookiekonformitätsrichtlinie der EU einzuholen.

Die Cookierichtlinie kann über die [Optionen](../reference.md#options) für den plastischen Reader festgelegt werden. Weitere Informationen finden Sie unter [CookiePolicy-Enumeration](../reference.md#cookiepolicy-enum).

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
* Sehen Sie sich das [Python-Tutorial](../tutorial-python.md) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Python bietet.
* Schauen Sie sich das [Swift-Tutorial](../tutorial-ios-picture-immersive-reader.md) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Swift bietet.
* Machen Sie sich mit dem [SDK für Plastischer Reader](https://github.com/microsoft/immersive-reader-sdk) und der [zugehörigen Referenz](../reference.md) vertraut.