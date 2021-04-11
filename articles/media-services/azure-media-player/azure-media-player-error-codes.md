---
title: Azure Media Player-Fehlercodes
description: Eine Fehlercodereferenz für Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: error-reference
ms.date: 04/05/2021
ms.openlocfilehash: 77bd59782d12d7d73e6e882e929ae9a4c95250a7
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106449884"
---
# <a name="error-codes"></a>Fehlercodes #

Wenn die Wiedergabe nicht gestartet werden kann oder beendet wurde, wird ein Fehlerereignis ausgelöst, und die `error()`-Funktion gibt einen Code und eine optionale Nachricht zurück, um den App-Entwickler mit mehr Details zu versorgen. `error().message` ist nicht die Meldung, die dem Benutzer angezeigt wird.  Die für den Benutzer angezeigte Meldung basiert auf den `error().code`-Bits 27–20, siehe dazu die Tabelle unten.

```javascript

    var myPlayer = amp('vid1');
    myPlayer.addEventListener('error', function() {
        var errorDetails = myPlayer.error();
        var code = errorDetails.code;
        var message = errorDetails.message;
    }
```

## <a name="error-codes-bits-31-28-4-bits"></a>Fehlercodes, Bits [31–28] (4 Bits) ##

Beschreibung des Bereichs des FehlesD.

- 0 – Unbekannt
- 1: AMP
- 2: AzureHtml5JS
- 3: FlashSS
- 4: SilverlightSS
- 5: Html5
- 6: Html5FairPlayHLS

## <a name="error-codes-bits-27-0-28-bits"></a>Fehlercodes, Bits [27–0] (28 Bits) ##

Beschreibung der Fehlerdetails, Bits 27–20 enthalten eine allgemeine Beschreibung, Bits 19–0 bieten mehr Details, falls verfügbar.


| amp.errorCode.[name] | Codes, Bits [27–0] (28 Bits) | BESCHREIBUNG |
|---|---:|---|
| **MEDIA_ERR_ABORTED-Fehlerbereich (0x0100000 – 0x01FFFFF)** | | |
| abortedErrUnknown | 0x0100000 | Generischer Abbruchfehler |
| abortedErrNotImplemented | 0x0100001 | Abbruchfehler, nicht implementiert |
| abortedErrHttpMixedContentBlocked | 0x0100002 | Abbruchfehler, gemischte Inhalte blockiert – tritt im Allgemeinen auf, wenn `http://`-Stream von einer `https://`-Seite geladen wird |
| **MEDIA_ERR_NETWORK-Fehlerstartwert (0x0200000 – 0x02FFFFF)** | | |
| networkErrUnknown | 0x0200000 | Generischer Netzwerkfehler |
| networkErrHttpBadUrlFormat | 0x0200190 | HTTP 400-Fehlerantwort |
| networkErrHttpUserAuthRequired | 0x0200191 | HTTP 401-Fehlerantwort |
| networkErrHttpUserForbidden | 0x0200193 | HTTP 403-Fehlerantwort |
| networkErrHttpUrlNotFound | 0x0200194 | HTTP 404-Fehlerantwort |
| networkErrHttpNotAllowed | 0x0200195 | HTTP 405-Fehlerantwort |
| networkErrHttpGone | 0x020019A | HTTP 410-Fehlerantwort |
| networkErrHttpPreconditionFailed | 0x020019C | HTTP 412-Fehlerantwort |
| networkErrHttpInternalServerFailure | 0x02001F4 | HTTP 500-Fehlerantwort
| networkErrHttpBadGateway | 0x02001F6 | HTTP 502-Fehlerantwort |
| networkErrHttpServiceUnavailable | 0x02001F7 | HTTP 503-Fehlerantwort |
| networkErrHttpGatewayTimeout | 0x02001F8 | HTTP 504-Fehlerantwort |
| networkErrTimeout | 0x0200258 | Netzwerktimeoutfehler
| networkErrErr | 0x0200259 | Netzwerkverbindungs-Fehlerantwort |
| **MEDIA_ERR_DECODE-Fehler (0x0300000 – 0x03FFFFF)** | | |
| decodeErrUnknown | 0x0300000 | Generischer Fehler beim Decodieren |
| **MEDIA_ERR_SRC_NOT_SUPPORTED-Fehler (0x0400000 – 0x04FFFFF)** | | |
| srcErrUnknown | 0x0400000 | Generischer Fehler bei nicht unterstützter Quelle |
| srcErrParsePresentation | 0x0400001 | Präsentationsanalysefehler |
| srcErrParseSegment | 0x0400002 | Segmentanalysefehler |
| srcErrUnsupportedPresentation | 0x0400003 | Präsentation nicht unterstützt |
| srcErrInvalidSegment | 0x0400004 | Ungültiges Segment |
| **MEDIA_ERR_ENCRYPTED-Fehlerstartwert (0x0500000 – 0x05FFFFF)** | | |
| encryptErrUnknown | 0x0500000 | Generischer Verschlüsselungsfehler | 
| encryptErrDecrypterNotFound | 0x0500001 | Entschlüsselung nicht gefunden |
| encryptErrDecrypterInit | 0x0500002 | Initialisierungsfehler der Entschlüsselung |
| encryptErrDecrypterNotSupported | 0x0500003 | Entschlüsselung wird nicht unterstützt |
| encryptErrKeyAcquire | 0x0500004 | Fehler beim Abrufen des Schlüssels |
| encryptErrDecryption | 0x0500005 | Fehler beim Entschlüsseln des Segments |
| encryptErrLicenseAcquire | 0x0500006 | Fehler beim Abrufen der Schlüssellizenz |
| **SRC_PLAYER_MISMATCH-Fehlerstartwert (0x0600000 – 0x06FFFFF)** | | |
| srcPlayerMismatchUnknown | 0x0600000 | Generisch – kein Player mit passender Technologie zum Wiedergeben der Quelle |
| srcPlayerMismatchFlashNotInstalled | 0x0600001 |Das Flash-Plug-In ist nicht installiert, wird es installiert, kann die Quelle wiedergegeben werden. *ODER* Flash 30 ist installiert und gibt AES-Inhalt wieder.  Wenn dies der Fall ist, versuchen Sie es bitte mit einem anderen Browser. Flash 30 wird seit dem 7. Juni nicht mehr unterstützt. Weitere Details finden Sie unter [bekannte Probleme](azure-media-player-known-issues.md). Hinweis: Bei 0x00600003 sind weder Flash noch Silverlight installiert, sofern in der techOrder so angegeben.|
| srcPlayerMismatchSilverlightNotInstalled | 0x0600002 | Das Silverlight-Plug-In ist nicht installiert, wird es installiert, kann die Quelle wiedergegeben werden. Hinweis: Bei 0x00600003 sind weder Flash noch Silverlight installiert, sofern in der techOrder so angegeben. |
| | 0x00600003 | Weder Flash noch Silverlight ist installiert, sofern in techOrder so angegeben. |
| **Unbekannte Fehler (0x0ff00000)** | | |
| errUnknown | 0xFF00000 | Unbekannte Fehler |

## <a name="user-error-messages"></a>Fehlermeldungen für Benutzer ##

Die angezeigten Benutzermeldungen basieren auf den Bits 27–20 des Fehlercodes.

- MEDIA_ERR_ABORTED (1): „Sie haben die Videowiedergabe abgebrochen“
- MEDIA_ERR_NETWORK (2): „Ein Netzwerkfehler hat zu einem Fehler beim Videodownload geführt“
- MEDIA_ERR_DECODE (3): „Die Videowiedergabe wurde aufgrund eines Problems wegen Beschädigung abgebrochen oder weil das Video Features verwendet, die von Ihrem Browser nicht unterstützt werden“
- MEDIA_ERR_SRC_NOT_SUPPORTED (4): „Das Video konnte nicht geladen werden, entweder aufgrund eines Server- oder Netzwerkfehlers oder weil das Format nicht unterstützt wird“
- MEDIA_ERR_ENCRYPTED (5): „Das Video ist verschlüsselt, und wir besitzen die Schlüssel zu seiner Entschlüsselung nicht“
- SRC_PLAYER_MISMATCH (6): „Für dieses Video wurde keine kompatible Quelle gefunden“
- MEDIA_ERR_UNKNOWN (0xFF): „Unbekannter Fehler“

## <a name="examples"></a>Beispiele ##

### <a name="0x10600001"></a>0x10600001 ##

„Für dieses Video wurde keine kompatible Quelle gefunden“ wird dem Endbenutzer angezeigt.

Es gibt keinen Player mit einer Technologie zum Wiedergeben der angeforderten Quellen, wenn aber das Flash-Plug-In installiert wird, kann eine Quelle wahrscheinlich wiedergegeben werden.  

### <a name="0x20200194"></a>0x20200194 ###

„Ein Netzwerkfehler hat zu einem Fehler beim Videodownload geführt“ wird dem Endbenutzer angezeigt.

AzureHtml5JS konnte nicht aus einer HTTP 404-Antwort wiedergeben.

### <a name="categorizing-errors"></a>Kategorisieren von Fehlern ###

```javascript
    if(myPlayer.error().code & amp.errorCode.abortedErrStart) {
        // MEDIA_ERR_ABORTED errors
    }
    else if(myPlayer.error().code & amp.errorCode.networkErrStart) {
        // MEDIA_ERR_NETWORK errors
    }
    else if(myPlayer.error().code & amp.errorCode.decodeErrStart) {
        // MEDIA_ERR_DECODE errors
    }
    else if(myPlayer.error().code & amp.errorCode.srcErrStart) {
        // MEDIA_ERR_SRC_NOT_SUPPORTED errors
    }
    else if(myPlayer.error().code & amp.errorCode.encryptErrStart) {
        // MEDIA_ERR_ENCRYPTED errors
    }
    else if(myPlayer.error().code & amp.errorCode.srcPlayerMismatchStart) {
        // SRC_PLAYER_MISMATCH errors
    }
    else {
        // unknown errors
    }
```

### <a name="catching-a-specific-error"></a>Abfangen eines bestimmten Fehlers ###

Der folgende Code fängt nur 404-Fehler ab:

```javascript
    if(myPlayer.error().code & amp.errorCode.networkErrHttpUrlNotFound) {
        // all http 404 errors
    }
```

## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)