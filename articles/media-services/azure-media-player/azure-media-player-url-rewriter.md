---
title: URL-Rewriter für Azure Media Player
description: Azure Media Player schreibt eine von Azure Media Services angegebene URL um, um Datenströme für SMOOTH, DASH, HLS v3 und HLS v4 bereitzustellen.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/05/2021
ms.custom: devx-track-js
ms.openlocfilehash: efa77ce7416b94331dce2bb4e7f77dd50c20d450
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106448575"
---
# <a name="url-rewriter"></a>URL-Rewriter #

Azure Media Player schreibt eine von Azure Media Services angegebene URL standardmäßig um, um Datenströme für SMOOTH, DASH, HLS v3 und HLS v4 bereitzustellen. Wenn die Quelle beispielsweise wie folgt angegeben ist, stellt Azure Media Player sicher, dass die Wiedergabe aller oben genannten Protokolle versucht wird:

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" />
    </video>
```

Wenn Sie den URL-Rewriter jedoch nicht verwenden möchten, fügen Sie dem Parameter die Eigenschaft `disableUrlRewriter` hinzu. Das bedeutet, dass alle an die Quellen übergebenen Informationen direkt ohne Änderungen an den Player übermittelt werden.  Nachfolgend sehen Sie ein Beispiel für das Hinzufügen von zwei Quellen (DASH und SMOOTH Streaming) zum Player:

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest(format=mpd-time-csf)" type="application/dash+xml" data-setup='{"disableUrlRewriter": true}'/>
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" data-setup='{"disableUrlRewriter": true}'/>
    </video>
```

oder

```javascript
    myPlayer.src([
        { src: "//example/path/to/myVideo.ism/manifest(format=mpd-time-csf)", type: "application/dash+xml", disableUrlRewriter: true },
        { src: "//example/path/to/myVideo.ism/manifest", type: "application/vnd.ms-sstr+xml", disableUrlRewriter: true }
    ]);
```

Sie können mithilfe des Parameters `streamingFormats` auch die spezifischen Streamingformate angeben, die Azure Media Player umschreiben soll. Zu den Optionen zählen `DASH`, `SMOOTH`, `HLSv3`, `HLSv4` und `HLS`. Der Unterschied zwischen HLS und HLSv3 bzw. v4 besteht darin, dass das HLS-Format die Wiedergabe von FairPlay-Inhalten unterstützt. v3 und v4 unterstützen FairPlay nicht. Dies ist nützlich, wenn keine Übermittlungsrichtlinie für ein bestimmtes Protokoll verfügbar ist.  Im folgenden Beispiel ist kein DASH-Protokoll für Ihr Medienobjekt aktiviert:

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" data-setup='{"streamingFormats": ["SMOOTH", "HLS","HLS-V3", "HLS-V4"] }'/>
    </video>
```

oder

```javascript
    myPlayer.src([
        { src: "//example/path/to/myVideo.ism/manifest", type: "application/vnd.ms-sstr+xml", streamingFormats: ["SMOOTH", "HLS","HLS-V3", "HLS-V4"]},
    ]);
```

Die beiden oben genannten Formate können in verschiedenen Szenarien abhängig von Ihrem spezifischen Medienobjekt kombiniert werden.

> [!NOTE]
> Die Informationen zum Widevine-Schutz werden nur im DASH-Protokoll dauerhaft gespeichert.

## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)