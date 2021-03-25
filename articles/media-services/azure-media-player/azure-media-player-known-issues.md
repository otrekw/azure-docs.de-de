---
title: Bekannte Probleme mit Azure Media Player
description: Im aktuellen Release können die folgenden bekannten Probleme auftreten.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 05/11/2020
ms.openlocfilehash: a31c089971b7e70e70c5906480deb2b17c197b9f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87043609"
---
# <a name="known-issues"></a>Bekannte Probleme #

Im aktuellen Release können die folgenden bekannten Probleme auftreten:

## <a name="azure-media-player"></a>Azure Media Player ##

- Falsch konfigurierte Encoder können Probleme bei der Wiedergabe verursachen.
- Bei Streams mit größeren Zeitstempeln als 2^53 können Probleme bei der Wiedergabe auftreten.
  - Abhilfe: Verwenden Sie Zeitskalen von 90 KHz bei Video- und 44,1 KHz bei Audiostreams.
- Ohne Benutzerinteraktion findet auf mobilen Geräten keine automatische Wiedergabe statt. Sie wird von der Plattform blockiert.
- Die Suche in der Nähe von Unterbrechungen kann zu Fehlern bei der Wiedergabe führen.
- Das Herunterladen großer Präsentationen kann zu einer Sperre der Benutzeroberfläche führen.
- Nach dem Ende einer Präsentation können einige Quellen nicht automatisch wiedergegeben werden.
  - Um eine Quelle nach dem Ende erneut wiederzugeben, muss die Quelle erneut festgelegt werden.
- Leere Manifeste können Probleme mit dem Player verursachen.
  - Dieses Problem kann auftreten, wenn ein Livestream gestartet wird und im Manifest nicht genügend Blöcke gefunden werden.
- Die Wiedergabeposition kann sich außerhalb der Suchleiste auf der Benutzeroberfläche befinden.
- Die Ereignisreihenfolge ist nicht in allen Technologien konsistent.
- Die Buffered-Eigenschaft ist nicht in allen Technologien konsistent.
- nativeControlsForTouch muss „false“ lauten (Standardeinstellung), um diesen Fehler zu vermeiden: Objekt unterstützt Eigenschaft oder Methode „setControls“ nicht".
- Beiträge müssen jetzt absolute URLs sein.
- Bei Verwendung des Modus für hohen Kontrast auf dem Gerät können auf der Benutzeroberfläche kleinere Schönheitsfehler auftreten.
- Bei URLs, die in der vollständig decodierten Zeichenfolge die Zeichen „%“ oder „+“ enthalten, können Probleme beim Festlegen der Quelle auftreten.

## <a name="ad-insertion"></a>Einfügen von Werbeeinblendungen ##

- Beim Einfügen (bedarfsbasiert oder live) von Werbeeinblendungen können Probleme auftreten, wenn im Browser ein Anzeigenblocker installiert ist.
- Auf Mobilgeräten können Probleme bei der Wiedergabe von Werbeeinblendungen auftreten.
- MP4 Midroll-Anzeigen werden aktuell von Azure Media Player nicht unterstützt.

## <a name="azurehtml5js"></a>AzureHtml5JS ##

- Im DVR-Fenster von Live-Inhalten wird die Zeitachse über das Inhaltsende hinaus weitergeführt, bis eine Suche im Bereich durchgeführt wird oder das Ende der Präsentation erreicht ist.
- Bei Livepräsentationen in Firefox treten bei aktivierten MSE Probleme auf.

- Medienobjekte, die nur aus Audiodaten bestehen, werden über die AzureHtml5JS-Technologie nicht wiedergegeben.
  - Wenn Sie Medienobjekte ohne Audio wiedergeben möchten, fügen Sie über das [Azure Media Services-Explorer-Tool](https://aka.ms/amse) leere Audiospuren ein.
  - Informationen zum Einfügen einer stillen Audiospur finden Sie [hier](../previous/media-services-advanced-encoding-with-mes.md#silent_audio).

## <a name="flash"></a>Blinken ##

- AES-Inhalte werden aufgrund eines Fehlers in der Cachinglogik von Adobe in der Flash-Version 30.0.0.134 nicht wiedergegeben. Adobe hat das Problem in Version 30.0.0.154 behoben.
- Bei technischen und HTTP-Fehlern (z. B. Fehler 404 oder Netzwerkzeitüberschreitungen) dauert die Wiederherstellung des Players länger als bei anderen Technologien.
- Durch Klicken auf einen Videobereich mit flashSS-Technologie erfolgt keine Wiedergabe/Pause des Players.
- Wenn Flash installiert ist, der Benutzer aber keine Berechtigung zum Laden auf der Website besitzt, kann eine Schleife entstehen. Dies liegt daran, dass der Player davon ausgeht, dass das Plug-In installiert und verfügbar ist und den Inhalt ausführt. Es wurde JavaScript-Code gesendet, aber die Browsereinstellungen haben die Ausführung des Plug-Ins blockiert, bis der Benutzer die Aufforderung akzeptiert, das Plug-In zuzulassen. Dies kann in allen Browsern auftreten.  

## <a name="silverlight"></a>Silverlight ##

- Fehlende Features
- Bei technischen und HTTP-Fehlern (z. B. Fehler 404 oder Netzwerkzeitüberschreitungen) dauert die Wiederherstellung des Players länger als bei anderen Technologien.
- Für die Wiedergabe mit Silverlight in Safari und Firefox auf Mac-Computern muss `"http://` oder `https://` explizit für die Quelle definiert werden.
- Wenn für diese Technologie eine API fehlt, wird im Allgemeinen NULL zurückgegeben.
- Wenn Flash installiert ist, der Benutzer aber keine Berechtigung zum Laden auf der Website besitzt, kann eine Schleife entstehen. Dies liegt daran, dass der Player davon ausgeht, dass das Plug-In installiert und verfügbar ist und den Inhalt ausführt. Es wurde JavaScript-Code gesendet, aber die Browsereinstellungen haben die Ausführung des Plug-Ins blockiert, bis der Benutzer die Aufforderung akzeptiert, das Plug-In zuzulassen. Dies kann in allen Browsern auftreten.  

## <a name="native-html5"></a>Natives HTML5 ##

- HTML5-Technologie sendet ein canplaythrough-Ereignis nur für die erste festgelegte Quelle.
- Diese Technologie unterstützt nur Features, die im Browser implementiert sind.  Einige Features fehlen in dieser Technologie möglicherweise.  
- Wenn für diese Technologie eine API fehlt, wird im Allgemeinen NULL zurückgegeben.
- In dieser Technologie treten Probleme mit Beschriftungen und Untertiteln auf. Diese können in dieser Technologie möglicherweise nicht genutzt oder angezeigt werden.
- Unzureichende Bandbreite in Szenarien mit HLS/HTML5-Technologie führt zur Wiedergabe von Audioinhalten ohne Videospur.

### <a name="microsoft"></a>Microsoft ###

- Die Wiedergabe in IE8 funktioniert aufgrund einer Inkompatibilität mit ECMAScript 5 derzeit nicht.
- In Internet Explorer und einigen Microsoft Edge-Versionen kann die Vollbildanzeige nicht aktiviert werden, indem die Schaltfläche über die TAB-Taste angesteuert und dann ausgewählt oder indem die Zugriffstaste F/f gedrückt wird.

## <a name="google"></a>Google ##

- Mehrere Codierungsprofile im selben Manifest verursachen Wiedergabeprobleme in Chrome und werden nicht empfohlen.
- Chrome kann HE-AAC mit AzureHtml5JS nicht wiedergeben. Weitere Details finden Sie im [Bugtracker](https://bugs.chromium.org/p/chromium/issues/detail?id=534301).
- Ab Chrome v58 müssen Widevine-Inhalte mit dem https://-Protokoll geladen bzw. wiedergegeben werden, da sonst die Wiedergabe nicht möglich ist.

## <a name="mozilla"></a>Mozilla ##

- Für einen Wechsel des Audiostreams müssen Audiostreams dieselben privaten Codecdaten aufweisen, wenn AzureHtml5JS verwendet wird. Dies ist eine Anforderung der Firefox-Plattform.

## <a name="apple"></a>Apple ##

- Safari auf Mac aktiviert bei der Einstellung „Plug-Ins zum Stromsparen stoppen“ häufig standardmäßig den Energiesparmodus. Dadurch werden Plug-Ins wie Flash und Silverlight in der Annahme blockiert, dass diese nicht im Sinne des Benutzers sind. Es werden nicht die Plug-Ins selbst, sondern ihre Funktionen blockiert. Aufgrund der standardmäßigen Technologiereihenfolge kann dies bei der Wiedergabe zu Problemen führen.
  - Lösung 1: Wenn sich der Videoplayer mittig im Vordergrund befindet (innerhalb eines Pixelbereichs von 3000 × 3000 ab der oberen linken Ecke des Dokuments), sollte die Wiedergabe funktionieren.
  - Lösung 2: Ändern Sie die Technologiereihenfolge für Safari zu ["azureHtml5JS", "html5"]. Diese Entschärfung hat den Nachteil, dass nicht alle Features, die in der FlashSS-Technologie verfügbar sind, abgerufen werden.
- Bei PlayReady-Inhalten über Silverlight können bei der Wiedergabe in Safari Probleme auftreten.
- AES-Inhalte und eingeschränkte Tokeninhalte werden auf iOS- und älteren Android-Geräten nicht wiedergegeben.
  - Um dieses Szenario zu erzielen, muss dem Dienst ein Proxy hinzugefügt werden.
- In iOS scheint die Standardskin durch.
- Auf iPhones erfolgt die Wiedergabe immer im nativen Player im Vollbildmodus.
  - Features wie z. B. Untertitel werden in dieser Nicht-Inline-Wiedergabe möglicherweise nicht beibehalten.
- Wenn eine Livepräsentation zu Ende ist, beenden iOS-Geräte diese möglicherweise nicht ordnungsgemäß.
- iOS lässt keine DVR-Funktionen zu.
- Der Wechsel von iOS-Audiostreams kann nur über die iOS-Benutzeroberfläche des nativen Players erfolgen und setzt voraus, dass die Audiostreams dieselben privaten Codecdaten aufweisen.
- Bei älteren Safari-Versionen können möglicherweise Probleme bei der Wiedergabe von Livestreams auftreten.

## <a name="older-android"></a>Ältere Android-Versionen ##

- AES-Inhalte und eingeschränkte Tokeninhalte werden auf iOS- und älteren Android-Geräten nicht wiedergegeben.
  - Um dieses Szenario zu erzielen, muss dem Dienst ein Proxy hinzugefügt werden.

## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)
