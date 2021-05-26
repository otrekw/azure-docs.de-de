---
title: Anzeige mathematischer Inhalte im plastischen Reader
titleSuffix: Azure Applied AI Services
description: In diesem Artikel wird gezeigt, wie Sie mathematische Inhalte im plastischen Reader anzeigen.
author: nitinme
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: nitinme
ms.custom: devx-track-js
ms.openlocfilehash: 5dad8a27e9a3b38ce70acb01e765561456e1a1b8
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110373873"
---
# <a name="how-to-display-math-in-the-immersive-reader"></a>Vorgehensweise: Anzeige mathematischer Inhalte im plastischen Reader

Der plastische Reader kann mathematische Inhalte anzeigen, wenn sie in Form der Mathematical Markup Language ([MathML](https://developer.mozilla.org/docs/Web/MathML)) bereitgestellt werden.
Der MIME-Typ kann über den [Block](../reference.md#chunk) für den plastischen Reader festgelegt werden. Weitere Informationen finden Sie unter [Unterstützte MIME-Typen](../reference.md#supported-mime-types).

## <a name="send-math-to-the-immersive-reader"></a>Senden mathematischer Inhalte an den plastischen Reader
Um mathematische Inhalte an den plastischen Reader zu senden, geben Sie einen MathML enthaltenden Block an, und legen Sie den MIME-Typ auf ```application/mathml+xml``` fest.

Wenn Ihr Inhalt z. B. dem folgenden ähneln würde:

```html
<div id='ir-content'>
    <math xmlns='http://www.w3.org/1998/Math/MathML'>
        <mfrac>
            <mrow>
                <msup>
                    <mi>x</mi>
                    <mn>2</mn>
                </msup>
                <mo>+</mo>
                <mn>3</mn>
                <mi>x</mi>
                <mo>+</mo>
                <mn>2</mn>
            </mrow>
            <mrow>
                <mi>x</mi>
                <mo>−</mo>
                <mn>3</mn>
            </mrow>
        </mfrac>
        <mo>=</mo>
        <mn>4</mn>
    </math>
</div>
```

Dann könnten Sie Ihren Inhalt mithilfe des folgenden JavaScripts anzeigen.

```javascript
const data = {
    title: 'My Math',
    chunks: [{
        content: document.getElementById('ir-content').innerHTML.trim(),
        mimeType: 'application/mathml+xml'
    }]
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, data, YOUR_OPTIONS);
```

Beim Starten des plastischen Readers sollte Folgendes angezeigt werden:

![Mathematische Inhalte im plastischen Reader](../media/how-tos/1-math.png)

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit dem [SDK für Plastischer Reader](https://github.com/microsoft/immersive-reader-sdk) und der [zugehörigen Referenz](../reference.md) vertraut.