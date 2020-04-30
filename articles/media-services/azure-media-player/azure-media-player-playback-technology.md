---
title: Azure Media Player-Wiedergabetechnologie
description: Hier erfahren Sie mehr über die Technologie zum Wiedergeben von Video- und Audiodateien.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 85eaa04836774b838da67e073017f4af3d2fe179
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727507"
---
# <a name="playback-technology-tech"></a>Wiedergabetechnologie („Technologie“) #

Der Begriff „Wiedergabetechnologie“ bezieht sich auf die spezifische Browser- oder Plug-In-Technologie, die für die Video- oder Audiowiedergabe verwendet wird.

- **azureHtml5JS**: Nutzt MSE- und EME-Standards in Kombination mit dem Videoelement für eine Plug-In-freie Wiedergabe von DASH-Inhalten mit Unterstützung von Inhalten mit AES-128-Bit-Umschlagsverschlüsselung oder allgemeiner DRM-Verschlüsselung (über PlayReady und Widevine, sofern vom Browser unterstützt) über Azure Media Services.
- **flashSS**: Nutzt Flash Player-Technologie für die Wiedergabe von Smooth-Inhalten mit Unterstützung von AES-128-Bit-Umschlagsentschlüsselung über Azure Media Services (mindestens Flash-Version 11.4 erforderlich).
- **html5FairPlayHLS**: Nutzt Safari-spezifische browserbasierte Wiedergabetechnologie über HLS mit dem Videoelement. Diese Technologie ist für die Wiedergabe von FairPlay-geschützten Inhalten über Azure Media Services erforderlich und wurde der Technologiereihenfolge am 19.10.2016 hinzugefügt.
- **silverlightSS**: Nutzt Silverlight-Technologie für die Wiedergabe von Smooth-Inhalten mit Unterstützung von PlayReady-geschützten Inhalten über Azure Media Services.
- **html5**: Nutzt browserbasierte Wiedergabetechnologie mit dem Videoelement.  Bei Apple iOS-oder Android-Geräten ermöglicht diese Technologie die Wiedergabe von HLS-Datenströmen mit grundlegender Unterstützung von AES-128-Bit-Umschlagverschlüsselung oder DRM-Inhalten (über FairPlay, sofern vom Browser unterstützt).

## <a name="tech-order"></a>Technologiereihenfolge ##

Um sicherzustellen, dass Ihr Medienobjekt auf verschiedensten Geräten wiedergegeben kann, wird die folgende Technologiereihenfolge empfohlen und standardmäßig verwendet, wenn `techOrder: ["azureHtml5JS", "flashSS", "html5FairPlayHLS","silverlightSS", "html5"]` gilt und direkt für `<video>` oder programmgesteuert in den Optionen festgelegt werden kann:

`<video data-setup='{"techOrder": ["azureHtml5JS", "flashSS", "html5FairPlayHLS, "silverlightSS", "html5"]}`

oder

```javascript
    amp("vid1", {
          techOrder: ["azureHtml5JS", "flashSS", "html5FairPlayHLS, "silverlightSS", "html5"]
    });
```

## <a name="compatibility-matrix"></a>Kompatibilitätsmatrix ##

Angesichts der empfohlenen Technologiereihenfolge für das Streamen von Inhalten über Azure Media Services wird für die Wiedergabe die folgende Kompatibilitätsmatrix erwartet:

| Browser        | Betriebssystem                                                       | Erwartete Technologie (unverschlüsselt)  | Erwartete Technologie (AES)  | Erwartete Technologie (DRM)          |
|----------------|----------------------------------------------------------|------------------------|----------------------|------------------------------|
| Edge, IE 11      | Windows 10, Windows 8.1, Windows Phone 10.1               | azureHtml5JS           | azureHtml5JS         | azureHtml5JS (PlayReady)     |
| IE 11, IE 9–10.1  | Windows 7, Windows Vista<sup>1</sup>                     | flashSS                | flashSS              | silverlightSS (PlayReady)    |
| IE 11          | Windows Phone 8.1                                        | azureHtml5JS           | azureHtml5JS         | Nicht unterstützt                |
| Microsoft Edge           | Xbox One<sup>1</sup> (Update von Nov 2015)                   | azureHtml5JS           | azureHtml5JS         | Nicht unterstützt                |
| Chrome ab Version 37     | Windows 10, Windows 8.1, macOS X Yosemite<sup>1</sup>   | azureHtml5JS           | azureHtml5JS         | azureHtml5JS (Widevine)      |
| Firefox ab Version 47    | Windows 10, Windows 8.1, macOS X ab Yosemite<sup>1</sup>  | azureHtml5JS           | azureHtml5JS         | azureHtml5JS (Widevine)      |
| Firefox 42–46  | Windows 10, Windows 8.1, macOS X ab Yosemite<sup>1</sup>  | azureHtml5JS           | azureHtml5JS         | silverlightSS (PlayReady)    |
| Firefox 35–41  | Windows 10, Windows 8.1                                  | flashSS                | flashSS              | silverlightSS (PlayReady)    |
| Safari         | iOS ab Version 6                                                   | html5                  | html5 (ohne Token)³    | Nicht unterstützt                |
| Safari ab Version 8      | OS X ab Yosemite                                           | azureHtml5JS           | azureHtml5JS         | html5FairPlayHLS (FairPlay)  |
| Safari 6       | OS X Mountain Lion<sup>1</sup>                           | flashSS                | flashSS              | silverlightSS (PlayReady)    |
| Chrome ab Version 37     | Android ab Version 4.4.4<sup>2</sup>                               | azureHtml5JS           | azureHtml5JS         | azureHtml5JS (Widevine)      |
| Chrome ab Version 37     | Android 4.02                                             | html5                  | html5 (ohne Token)<sup>3</sup>    | Nicht unterstützt                |
| Firefox ab Version 42    | Android ab Version 5.0<sup>2</sup>                                 | azureHtml5JS           | azureHtml5JS         | Nicht unterstützt                |
| IE 8           | Windows                                                  | Nicht unterstützt          | Nicht unterstützt        | Nicht unterstützt                |

<sup>1</sup> Konfiguration wird nicht unterstützt oder wurde nicht getestet und ist hier nur der Vollständigkeit halber angegeben.

<sup>2</sup> Für die erfolgreiche Wiedergabe auf Android-Geräten ist eine Kombination aus Gerätefunktionen, Grafikunterstützung, Codec-Rendering, Betriebssystemunterstützung und weiteren Aspekten erforderlich. Da es sich bei Android um eine Open-Source-Plattform handelt, die Smartphoneherstellern eine Änderung des von Google bereitgestellten Android-Betriebssystems ermöglicht, ist die Android-Umgebung inzwischen etwas fragmentiert, und einige Geräte werden aufgrund fehlender Features ggf. nicht unterstützt. Außerdem werden von einigen Android-Geräten nicht alle Codecs unterstützt.  

<sup>3</sup> In Fällen ohne Tokenunterstützung kann diese Funktion mithilfe eines Proxys hinzugefügt werden. Weitere Informationen zu dieser Lösung finden Sie in [diesem Blog](https://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

> [!NOTE]
> Wenn für die gewählte erwartete Technologie auf dem Computer des Benutzers ein Plug-In (beispielsweise Flash) installiert werden muss, werden von AMP die Funktionen der nächsten Technologie in der Technologieliste überprüft (zusammen mit Quelltypen und Schutzinformationen). Wenn Sie also beispielsweise versuchen, einen ungeschützten On-Demand-Datenstrom in Safari 8 unter OS X Yosemite anzuzeigen, und weder Flash noch Silverlight installiert ist, wird von AMP die native HTML5-Technologie für die Wiedergabe verwendet.<br/><br/>Da immer wieder neue Browsertechnologien hinzukommen, kann sich diese Matrix jederzeit ändern.

## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)