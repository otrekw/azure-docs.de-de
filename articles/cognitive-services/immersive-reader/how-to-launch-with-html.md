---
title: Starten des plastischen Readers mit HTML-Inhalt
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird gezeigt, wie Sie den plastischen Reader mit HTML-Inhalt starten.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metang
ms.custom: devx-track-js
ms.openlocfilehash: d028e9ef2087b866b3c168483d55556e38f9a7cd
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92636560"
---
# <a name="how-to-launch-the-immersive-reader-with-html-content"></a>Vorgehensweise: Starten des plastischen Readers mit HTML-Inhalt

Dieser Artikel zeigt, wie Sie den plastischen Reader mit HTML-Inhalt starten.

## <a name="prepare-the-html-content"></a>Vorbereiten des HTML-Inhalts

Platzieren Sie den Inhalt, den Sie im plastischen Reader innerhalb eines Containerelements rendern möchten. Stellen Sie sicher, dass das Containerelement über eine eindeutige `id` verfügt. Der plastische Reader bietet Unterstützung für grundlegende HTML-Elemente. Weitere Informationen finden Sie in der [Referenz](./reference.md#html-support).

```html
<div id='immersive-reader-content'>
    <b>Bold</b>
    <i>Italic</i>
    <u>Underline</u>
    <strike>Strikethrough</strike>
    <code>Code</code>
    <sup>Superscript</sup>
    <sub>Subscript</sub>
    <ul><li>Unordered lists</li></ul>
    <ol><li>Ordered lists</li></ol>
</div>
```

## <a name="get-the-html-content-in-javascript"></a>Abrufen des HTML-Inhalts in JavaScript

Verwenden Sie die `id` des Containerelements, um den HTML-Inhalt in Ihren JavaScript-Code zu übernehmen.

```javascript
const htmlContent = document.getElementById('immersive-reader-content').innerHTML;
```

## <a name="launch-the-immersive-reader-with-your-html-content"></a>Starten des plastischen Readers mit Ihrem HTML-Inhalt

Wenn Sie `ImmersiveReader.launchAsync` aufrufen, legen Sie die `mimeType`-Eigenschaft des Blocks auf `text/html` fest, um das HTML-Rendering zu aktivieren.

```javascript
const data = {
    chunks: [{
        content: htmlContent,
        mimeType: 'text/html'
    }]
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, data, YOUR_OPTIONS);
```

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [Referenz zum SDK für den plastischen Reader](./reference.md).