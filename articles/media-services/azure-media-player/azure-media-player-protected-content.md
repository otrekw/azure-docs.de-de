---
title: Geschützter Inhalt in Azure Media Player
description: Azure Media Player unterstützt derzeit Inhalte mit AES-128-Bit-Umschlagsverschlüsselung und Inhalte mit allgemeiner Verschlüsselung.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.custom: devx-track-js
ms.openlocfilehash: 83f144c06c23f3ab5507e3561be4a12350e20a42
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91329685"
---
# <a name="protected-content"></a>Geschützter Inhalt #

Azure Media Player unterstützt derzeit Inhalte mit AES-128-Bit-Umschlagsverschlüsselung, Inhalte mit allgemeiner Verschlüsselung (über PlayReady und Widevine) oder verschlüsselte Inhalte über FairPlay. Damit geschützte Inhalte ordnungsgemäß wiedergegeben werden können, müssen Sie `protectionInfo` an Azure Media Player übergeben. Diese Informationen sind pro Quelle vorhanden und können über `data-setup` direkt zum Tag `<source>` hinzugefügt werden.  Sie können `protectionInfo` auch direkt als Parameter hinzufügen, wenn die Quelle dynamisch festgelegt wird.

`protectionInfo` akzeptiert ein JSON-Objekt und umfasst Folgendes:

- `type`: `AES`, `PlayReady`, `Widevine` oder `FairPlay`
- `certificateUrl`: Hierfür sollte ein direkter Link zu Ihrem gehosteten FairPlay-Zertifikat angegeben werden.

- `authenticationToken`: Dies ist ein Optionsfeld zum Hinzufügen eines nicht codierten Authentifizierungstokens.

> [!IMPORTANT]
> Das Objekt **certificateUrl** ist nur für das FairPlay-DRM erforderlich.***
>[!NOTE]
> Die standardmäßige Technologiereihenfolge wurde angepasst, um die neue Technologie `html5FairPlayHLS` einzubeziehen, die insbesondere der nativen Wiedergabe von FairPlay-Inhalten in Browsern mit entsprechender Unterstützung (Safari unter OSX 8 oder höher) dient. Wenn Sie FairPlay-Inhalte wiedergeben möchten **UND** in Ihrer Anwendung die standardmäßige Technologiereihenfolge in eine benutzerdefinierte Reihenfolge geändert haben, müssen Sie diese neue Technologie Ihrem techOrder-Objekt hinzufügen. Es wird empfohlen, sie vor silverlightSS einzufügen, damit Ihre Inhalte nicht über PlayReady wiedergegeben werden.

## <a name="code-sample"></a>Codebeispiel ##

```html
Ex:

    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source
            src="//example/path/to/myVideo.ism/manifest"
            type="application/vnd.ms-sstr+xml"
            data-setup='{"protectionInfo": [{"type": "AES", "authenticationToken": "Bearer=urn%3amicrosoft%3aazure%3amediaservices%3acontentkeyidentifier=8130520b-c116-45a9-824e-4a0082f3cb3c&Audience=urn%3atest&ExpiresOn=1450207516&Issuer=http%3a%2f%2ftestacs.com%2f&HMACSHA256=eV7HDgZ9msp9H9bnEPGN91sBdU7XsZ9OyB6VgFhKBAU%3d"}]}'
        />
    </video>
or

```javascript
    var myPlayer = amp("vid1", /* Options */);
    myPlayer.src([{
        src: "//example/path/to/myVideo.ism/manifest",
        type: "application/vnd.ms-sstr+xml",
        protectionInfo: [{
            type: "PlayReady",
            authenticationToken: "Bearer=urn%3amicrosoft%3aazure%3amediaservices%3acontentkeyidentifier=d5646e95-63ee-4fbe-ba4e-295c8d9502e0&Audience=urn%3atest&ExpiresOn=1450222961&Issuer=http%3a%2f%2ftestacs.com%2f&HMACSHA256=4Jop3kNJdzVI8L5IZLgFtPdImyE%2fHTRil0x%2bEikSdPs%3d"
        }] }, ]
    );
```

Alternativ mit mehreren DRM:

```javascript
    var myPlayer = amp("vid1", /* Options */);
    myPlayer.src([{
        src: "//example/path/to/myVideo.ism/manifest",
        type: "application/vnd.ms-sstr+xml",
        protectionInfo: [{
                type: "PlayReady",
                authenticationToken: "Bearer=urn%3amicrosoft%3aazure%3amediaservices%3acontentkeyidentifier=d5646e95-63ee-4fbe-ba4e-295c8d9502e0&Audience=urn%3atest&ExpiresOn=1450222961&Issuer=http%3a%2f%2ftestacs.com%2f&HMACSHA256=4Jop3kNJdzVI8L5IZLgFtPdImyE%2fHTRil0x%2bEikSdPs%3d"
            },
            {
                type: "Widevine",
                authenticationToken: "Bearer=urn%3amicrosoft%3aazure%3amediaservices%3acontentkeyidentifier=d5646e95-63ee-4fbe-ba4e-295c8d9502e0&Audience=urn%3atest&ExpiresOn=1450222961&Issuer=http%3a%2f%2ftestacs.com%2f&HMACSHA256=4Jop3kNJdzVI8L5IZLgFtPdImyE%2fHTRil0x%2bEikSdPs%3d"
            },
            {
                   type: "FairPlay",
                  certificateUrl: "//example/path/to/myFairplay.der",
                   authenticationToken: "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1cm46bWljcm9zb2Z0OmF6dXJlOm1lZGlhc2VydmljZXM6Y29udGVudGtleWlkZW50aWZpZXIiOiIyMTI0M2Q2OC00Yjc4LTRlNzUtYTU5MS1jZWMzMDI0NDNhYWMiLCJpc3MiOiJodHRwOi8vY29udG9zbyIsImF1ZCI6InVybjp0ZXN0IiwiZXhwIjoxNDc0NTkyNDYzLCJuYmYiOjE0NzQ1ODg1NjN9.mE7UxgNhkieMMqtM_IiYQj-FK1KKIzB6lAptw4Mi67A"
        }] } ]
    );
```

> [!NOTE]
> Nicht alle Browser/Plattformen können geschützte Inhalte wiedergeben. Weitere Informationen zu den unterstützten Komponenten finden Sie im Abschnitt [Wiedergabetechnologie](azure-media-player-playback-technology.md).
> [!IMPORTANT]
> Das an den Player übergebene Token ist für geschützte Inhalte gedacht und wird nur für authentifizierte Benutzer verwendet. Es wird davon ausgegangen, dass die Anwendung SSL oder einen anderen Sicherheitsmechanismus verwendet. Darüber hinaus wird der Endbenutzer als vertrauenswürdig eingestuft und somit darauf vertraut, dass er das Token nicht missbraucht. Ist dies nicht der Fall, wenden Sie sich an Ihre Sicherheitsexperten.

## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)