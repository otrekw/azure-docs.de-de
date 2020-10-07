---
title: Azure Media Player-Änderungsprotokoll
description: Enthält das Azure Media Player-Änderungsprotokoll.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 09/23/2020
ms.openlocfilehash: c63a8efc4bbcf6e5a124d439e6f9f91397e2fa53
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91315081"
---
# <a name="changelog"></a>Änderungsprotokoll

## <a name="236-official-update-september-21-2020"></a>2.3.6 (offizielles Update, 21. September 2020)

### <a name="features-236"></a>Features 2.3.6

Reine Audiounterstützung für die azureHtml5JS-Technologie (DASH), Unterstützung des Spätstarts der Livetranskription sowie Unterstützung des Änderns der Sprache in der Livetranskription hinzugefügt

### <a name="bug-fixes-236"></a>Fehlerbehebungen 2.3.6

Wenn bei der HLS-Wiedergabe auf Apple-Geräten „playsinline“ verwendet wird, wird das Video durch Klicken auf die Schaltfläche „LIVE“ neu gestartet. Durch das AMP-Posterbild wird manchmal eine Ausnahme ausgelöst. Die Lautstärkeschaltfläche war bei der HLS FairPlay-Wiedergabe nicht vorhanden. [Barrierefreiheit] Bei Verwendung der Tastatur sind keine QuickInfos für Schaltflächen definiert. [Barrierefreiheit] Das Helligkeitsverhältnis für die Statusleiste ist geringer als 1,3:1. [Barrierefreiheit] Der Tastaturfokus kehrt manchmal nicht zur Schaltfläche für die Videoqualität zurück. [Barrierefreiheit] Steuerelemente werden auf dem Videobildschirm nicht angezeigt und können daher von der Sprachausgabe nicht gefunden werden.

### <a name="changes-236"></a>Änderungen 2.3.6

An aufrufende Anwendungen werden aussagekräftige KeyDelivery-Fehler zurückgegeben.

## <a name="235-official-update-june-1-2020"></a>2.3.5 (Offizielles Update vom 1. Juni 2020)

### <a name="bug-fixes-235"></a>Fehlerbehebungen 2.3.5

- [Barrierefreiheit] Der Listener für die ESC-Taste ist im Bereich „Optionen“ an ein Dokument angefügt.
- [Barrierefreiheit] Verhindern, dass die Player-Benutzeroberfläche verschwindet, wenn die Steuerleiste oder das Optionsmenü den Fokus enthält.
- Die Steuerleiste zeigt die falsche Gesamtbetrachtungszeit an, wenn die Einstellungen für die Anzeige der Gesamtbetrachtungszeit aktiviert sind.

### <a name="changes-235"></a>Änderungen 2.3.5

- Die Fehlermeldung für den Fehlercode 0x00400005 wurde hinzugefügt und dokumentiert.

## <a name="234-official-update-march-4-2020"></a>2.3.4 (Offizielles Update vom 4. März 2020)

### <a name="bug-fixes-234"></a>Fehlerbehebungen 2.3.4

- PlayReady overrideLicenseAcquistionUrl konnte nicht festgelegt werden.
- Inhalte können nicht mit Unterbrechungen wiedergegeben werden.
- [Barrierefreiheit] Der ID-Attributwert für die Sprachausgabewarnung muss eindeutig sein.
- [Barrierefreiheit] Beim Navigieren durch das Dialogfeld mit den Einstellungen für Untertitel für Hörgeschädigte verlässt der Fokus das Dialogfeld.

### <a name="changes-234"></a>Änderungen 2.3.4

- „Content-Length“ wird nach dem erfolgreichen Herunterladen protokolliert, um bei der Analyse von Entschlüsselungsfehlern zu helfen. 2.3.3 (Offizielles Update vom 12. November 2019)

### <a name="features-234"></a>Features 2.3.4

- Die Unterstützung für die Anzeige der Gesamtbetrachtungszeit eines Videos als Überlagerung und für die Anzeige auf der Steuerleiste wurde hinzugefügt.

### <a name="bug-fixes-234"></a>Fehlerbehebungen 2.3.4

- Der Wechsel des Audiotitels funktioniert, gibt aber Fehler bei IE11 und Windows 7 aus: „Objekt unterstützt Eigenschaft oder Methode 'enabled' nicht“.
- Fehler beim Wechsel des Audiotitels, wenn der Puffer vollständig geladen ist.
- Beim Wechsel des Audiotitels tritt ein Fehler auf, wenn der Benutzer das Video anhält und sehr schnell zwischen Audiotiteln wechselt.
- [Barrierefreiheit] Die QuickInfos sind für das Videosteuerelement unter dem Videoplayer nicht definiert.
- Fehlende Lautstärkeregler unter Html5, je nachdem, wann „loadstart“ empfangen wird.
- [Barrierefreiheit] Keine Möglichkeit zum Festlegen des alternativen Texts für das Posterbild.
- [Barrierefreiheit] Der Fokus der Anwendung ging nach der Auswahl von „Fertig“ im Dialogfeld der Untertiteleinstellungen verloren.
- [Barrierefreiheit] Falsche ARIA-Attribute werden für „Video“ unter der Segmentvorschau definiert.

### <a name="changes-234"></a>Änderungen 2.3.4

- Bei der Wiedergabe von HLS unter iOS und macOS Safari wurde eine leere Untertitelbeschriftung/Spur entfernt.
- Die Anzahl von 412er für IMSC1-Untertitel wurde verringert.
- Ausgabewarnung in der Konsole für 10 aufeinanderfolgende leere IMSC1-Untertitelantworten zur Unterstützung des Livedebuggings.

## <a name="232-official-update-october-9-2019"></a>2.3.2 (Offizielles Update vom 9. Oktober 2019)

### <a name="features-232"></a>Features 2.3.2

– Die PlayReady-Unterstützung für die DASH-Wiedergabe wurde für Microsoft Edge-Browser (Chromium) hinzugefügt.

### <a name="bug-fixes-232"></a>Fehlerbehebungen 2.3.2

- Die aktuelle Wiedergabegeschwindigkeit wird im Menü für die Wiedergabegeschwindigkeit nicht visuell angezeigt, es sei denn, der Benutzer legt sie manuell fest.
- [Barrierefreiheit] Der Bereich „Einstellungen“ wird durch Betätigen der ESC-Taste nicht ausgeblendet.
- [Barrierefreiheit] Die AMP-Tastenkombination „M“ funktioniert nicht, wenn die Sprachausgabe aktiviert ist.

### <a name="changes-232"></a>Änderungen 2.3.2

- Bei Browsern, die den E-AC3-Audiocodec nicht unterstützen, werden E-AC3-Audiotiteln im Audiotitelmenü ausgeblendet.
- Bei Browsern, die den E-AC3-Audiocodec unterstützen, wird standardmäßig ein E-AC3-Audiotitel ausgewählt.
- Bei Browsern, die den Wechsel des Audiocodecs nicht unterstützen, werden Audiotitel mit einem anderen Codec als dem ausgewählten Titel im Audiotitelmenü ausgeblendet.

## <a name="231-official-update-august-12-2019"></a>2.3.1 (Offizielles Update vom 12. August 2019)

### <a name="features-231"></a>Features 2.3.1

- Signalisieren eines Ereignisses, wenn emsg-Felder bei der DASH-Wiedergabe empfangen werden – Unterstützung hinzugefügt, um EC-3-Audiotitel im Audiomenü von Browsern anzuzeigen, die EC-3 unterstützen, und der Wechsel des Audiotitels von AAC zu EC-3 und umgekehrt wurde nur für den auf Chromimum-basierenden Edge-Browser zugelassen.

### <a name="bug-fixes-231"></a>Fehlerbehebungen 2.3.1

- Das Audiotitelmenü ist nach dem Entfernen von EC-3-Titeln beschädigt.
- Die aktuelle Zeit kann größer sein als die Dauer des Videos.
- Das Festlegen der Wiedergabegeschwindigkeit über „initialSpeed“ funktioniert nicht.
- Gelegentlich scheint der Player nach einer Suche zu hängen.
- Bei Edge und IE wird auf einem Touchscreen nach dem Vergrößern einer Seite das richtige Segment des Videos durch Drücken oder Zeigen auf die Suchleiste nicht ordnungsgemäß angezeigt.
- [Barrierefreiheit] Aria-Bezeichnung für Wiedergabe/Pause ist für Videoplayer nicht aussagekräftig. Fehler „Livesegment nicht gefunden“ für flashSS zum richtigen amp-Fehler zugeordnet.
- [Barrierefreiheit] Die für Wiedergabe/Pause verwendeten Aria-Rollen müssen mit gültigen Werten übereinstimmen (.vjs-text-track-display).
- [Barrierefreiheit] Bestimmte Aria-Rollen müssen von bestimmten übergeordneten Elementen übernommen werden.
- [Barrierefreiheit] Es ist keine QuickInfo für die Wiedergabe-/Pause-Schaltfläche des Videoplayers definiert. IMSC1-Untertitel können nach der Suche innerhalb des aktuellen Video-/Audiopuffers verschwinden.

### <a name="changes-231"></a>Änderungen 2.3.1

- Wenn er einen „segmentDecryptError“ erhält und der Player sich bereits am Liveedge befindet, aktualisiert der Player jetzt das Manifest, anstatt das nächste Segment zu versuchen.
- Die Protokollierung für die Diagnose wurde erweitert.
- Die Dokumentation wurde aktualisiert, um die FairPlay-Unterstützung für iOS Safari einzubeziehen.
- Ein Beispiel für „srclang“ der IMSC1-Option wurde hinzugefügt.
- Außerkraftsetzungen für „padding“, „textPadding“, „boxShadow“ wurden für Texttitel hinzugefügt.
- Ein Fehlercode (0x0020025B) wurde hinzugefügt, um zu unterscheiden, dass beim Segmentdownload aufgrund einer unterbrochenen Internetverbindung ein Fehler aufgetreten ist, anstatt nur 0x00200259 auszulösen.

## <a name="230-official-release-april-30-2019"></a>2.3.0 (Offizielle Version vom 30. April 2019)

### <a name="features-230"></a>Features 2.3.0

- Unterstützung für IMSC1-Untertitel für DASH wurde hinzugefügt.
- Unterstützung für reine Videomedienobjekte für DASH wurde hinzugefügt.
- API presentationTimeOffsetInSec wurde hinzugefügt.

### <a name="bug-fixes-230"></a>Fehlerbehebungen 2.3.0

- Das Heuristikprofil „AMP LowLatency“ steht im Konflikt mit der iOS-Videowiedergabefunktion „Stummschalten“ und „Stummschaltung aufheben“ für einige Sprachen mit falschen Übersetzungen.
- Der aria-valuenow-Wert des Schiebereglers der Statusleiste ist manchmal falsch.
- Der Aria-Rollenwert der Texttitelanzeige ist falsch.

### <a name="changes-230"></a>Änderungen 2.3.0

- Die Protokolle enthalten jetzt die Größe der heruntergeladenen Medienfragmente.
- Unterstützung für IE 9 und IE 10 wurde entfernt.
- Beispiel CEA708 wurde aktualisiert, um Untertitel linksbündig anzuzeigen.
- MediaError.message wurde in Protokollen für Wiedergabefehler einbezogen.

## <a name="224-official-update-february-22-2019"></a>2.2.4 (Offizielles Update vom 22. Februar 2019) ##

### <a name="bug-fixes-224"></a>Fehlerbehebungen in Version 2.2.4 ###

- [Fehlerbehebung][AMP][Barrierefreiheit] Erreichbare Phantom-Registerkarte bei Anzeige des Fehlerbildschirms entfernt
- [Fehlerbehebung][AMP] Fehler bei Hotkey „M“ für IE11 und Microsoft Edge behoben
- [Fehlerbehebung][AMP] Ausnahme für CEA708-Untertitel behoben
- [Fehlerbehebung][AMP] Problem in Bezug auf das Einfrieren von Videos für den Microsoft Edge-Browser behoben

### <a name="changes-224"></a>Änderungen in Version 2.2.4 ###

- [Änderung][AMP] Bei einem Fragmententschlüsselungsfehler versucht der Player, aktuelle bzw. verschiedene Fragmente zu wiederholen, um die Wiedergabe wiederherzustellen
- [Änderung][AMP] Toleranz von AMP in Bezug auf sich überlappende Video- oder Audiofragmente erhöht

## <a name="223-official-update-january-9-2019"></a>2.2.3 (Offizielles Update vom 9. Januar 2019) ##

### <a name="features-223"></a>Features 2.2.3 ###

- [Feature][HLS] Audiotitelmenü für Safari-HLS-Wiedergabe hinzugefügt

### <a name="bug-fixes-223"></a>Fehlerbehebungen in Version 2.2.3 ###

- [Fehlerbehebung][AMP][Barrierefreiheit] Bei der Wiedergabe von Livesendungen konnte die Schaltfläche „Live“ nicht über die Tastatur ausgewählt werden
- [Fehlerbehebung][AMP] False Positive-Fehler (0x0400003) aufgrund eines nicht erfolgreichen MSE-Tests behoben
- [Fehlerbehebung][AMP] Problem behoben, bei dem das Video beim Starten eines Livestreams ggf. eingefroren ist

### <a name="changes-223"></a>Änderungen in Version 2.2.3 ###

- [Änderung][AMP] Im Protokoll weitere Informationen hinzugefügt, um die Diagnose zu verbessern
- [Änderung][AMP] Wenn bei gleicher Bildschirmauflösung mehr als eine Bitrate verfügbar ist, können alle Bitraten ausgewählt werden

## <a name="222-official-update"></a>2.2.2 (Offizielles Update) ##

### <a name="bug-fixes-222"></a>Fehlerbehebungen in Version 2.2.2 ###

- [Fehlerbehebung][AMP] Wenn es für den Player zu einem vorübergehenden Netzwerkausfall gekommen ist, wurde die Wiedergabe sofort beendet
- [Fehlerbehebung][AMP][Barrierefreiheit] Auf das Fehlerdialogfeld konnte nicht über die Tastatur zugegriffen werden
- [Fehlerbehebung][AMP] Anzeige eines unendlichen Wartekreisels anstelle des Fehlers „Keine Unterstützung“, wenn ein reines Audioobjekt wiedergegeben wurde

### <a name="changes-222"></a>Änderungen in Version 2.2.2 ###

- [Änderung][AMP] Lokalisierte Zeichenfolgen für Benutzeroberfläche für Ankündigungen hinzugefügt

## <a name="221-official-update"></a>2.2.1 (Offizielles Update) ##

### <a name="features-221"></a>Features in Version 2.2.1 ###

- [Feature][CMAF] Unterstützung für HLS-CMAF hinzugefügt

### <a name="bug-fixes"></a>Fehlerbehebungen ###

- [Fehlerbehebung][AMP] Nicht gelöschte Zeitgeber in der Wiederholungslogik haben zu Wiedergabefehlern geführt
- [Fehlerbehebung][AMP][Firefox] Beendigungsereignis wurde für Firefox und Chrome nicht ausgelöst, wenn das Liveprogramm beendet wurde
- [Fehlerbehebung][AMP] Steuerelemente wurden nach SetSource auch dann angezeigt, wenn für die Steuerelemente in den Player-Optionen „false“ festgelegt war

### <a name="changes"></a>Änderungen ###

- [Änderung][Live-Untertitelung] API-Name für CEA-Untertitel von 608 in 708 geändert. Weitere Informationen finden Sie auf der [Seite mit den Einstellungen für CEA708-Untertitel](/javascript/api/azuremediaplayer/amp.player.cea708captionssettings)-->.

## <a name="220-official-release"></a>2.2.0 (Offizielle Version) ##

### <a name="features-220"></a>Features in Version 2.2.0 ###

- [Feature][Azurehtml5JS][Flash][LiveCaptions]Unterstützung für CEA708-Untertitel in Azurehtml5JS- und FlashSS-Technologie, um eindeutigen Inhalt bzw. AES-Inhalt sicherzustellen

### <a name="bug-fixes-220"></a>Fehlerbehebungen in Version 2.2.0 ###

- [Fehlerbehebung] Erkennung der Flash-Version in Chrome/Microsoft Edge hat nicht funktioniert

### <a name="changes-220"></a>Änderungen in Version 2.2.0 ###

- [Änderung][AMP][Heuristik] Heuristikprofilname von QuickStartive in LowLatency geändert
- [Änderung][Flash] Änderung im Flash-Player in Bezug auf die Versionserkennung, um die Wiedergabe von AES-Inhalt mit dem neuen Adobe Flash-Update zu ermöglichen

## <a name="219-official-hotfix"></a>2.1.9 (Offizieller Hotfix) ##

### <a name="bug-fixes-219"></a>Fehlerbehebungen in Version 2.1.9 ###

- [Fehlerbehebung][Live] Ausnahme beim Übergang von Livestreams zu Video on Demand-/Live-Archiven

### <a name="changes-219"></a>Änderungen in Version 2.1.9 ###

- [Änderung][Flash][AES] Flash-Technologielogik geändert, damit „sharedbytearrays“ nicht für die AES-Entschlüsselung genutzt wird (Adobe hat die Nutzung von Flash 30 blockiert). Beachten Sie, dass die Wiedergabe erst funktioniert, nachdem von Adobe eine neue Version von Flash bereitgestellt wird (aufgrund eines Fehlers in Version 30). Ausführlichere Informationen hierzu finden Sie unter [Bekannte Probleme](azure-media-player-known-issues.md).

## <a name="218-official-update"></a>2.1.8 (Offizielles Update) ##

### <a name="bug-fixes-218"></a>Fehlerbehebungen in Version 2.1.8 ###

- [Fehlerbehebung] Wartekreisel wurde nach der Suche und vor der Wiedergabe gelegentlich nicht angezeigt
- [Fehlerbehebung] Player wurde nicht im Modus „Stumm“ gestartet, wenn die Stummschaltung aktiviert war
- [Fehlerbehebung] Schieberegler „Lautstärke“ wurde angezeigt, wenn Steuerelemente auf „false“ festgelegt waren
- [Fehlerbehebung] Wiedergabe wurde gelegentlich wiederholt, wenn Benutzer zum Liveedge gesprungen sind
- [Fehlerbehebung][Firefox] Player hat beim Laden gelegentlich eine JavaScript-Ausnahme ausgelöst
- [Fehlerbehebung][Barrierefreiheit] Fokusrahmen für Schaltfläche „Play/Pause/Lautstärke“ ist bei Auswahl über die Tastatur verloren gegangen
- [Fehlerbehebung] Arbeitsspeicherverlust beim Schließen des Players behoben
- [Fehlerbehebung] Beim Aufrufen von „src()“ nach Player-Fehler wurde die Quelle nicht zurückgesetzt
- [Fehlerbehebung][Live] AMP hat sich in einem konstanten Ladezustand befunden, wenn Benutzer nach Beendigung der Sendung auf die Schaltfläche „Live“ geklickt haben
- [Fehlerbehebung][Chrome] Player hat gehangen und Wiedergabe ist fehlgeschlagen, wenn der Browser minimiert wurde und sich im Hintergrund befunden hat

### <a name="changes-218"></a>Änderungen in Version 2.1.8 ###

- [Änderung] Fehler 0x0600001 aktualisiert. Er wird angezeigt, wenn AES-Inhalt mit Flash 30 wiedergegeben wird, da dies derzeit nicht unterstützt wird. Ausführlichere Informationen hierzu finden Sie unter [Bekannte Probleme](azure-media-player-known-issues.md).
- [Änderung] Zusätzliche Wiederholungsversuche für Liveszenarien hinzugefügt, wenn vom Manifest 404 angefordert wird oder leere Manifeste zurückgegeben werden

## <a name="217-official-update"></a>2.1.7 (Offizielles Update) ##

### <a name="features-217"></a>Features in Version 2.1.7 ###

- [Feature][AzureHtml5JS] Konfigurationsoption hinzugefügt, mit der veraltete Daten im Medienquellpuffer gelöscht werden

### <a name="bug-fixes-217"></a>Fehlerbehebungen in Version 2.1.7 ###

- [Fehlerbehebung][Barrierefreiheit][Sprachausgabe] Leeren Header entfernt, der vom Player eingefügt wurde, wenn kein Titel festgelegt war
- [Fehlerbehebung][UWA] AMP hat bei der Wiedergabe in einer universellen Windows-App eine Ausnahme ausgelöst
- [Fehlerbehebung][OSX] „setActiveTextTrack()“ hat in Safari unter OSx nicht funktioniert
- [Fehlerbehebung][Live] Beim Klicken auf den Liveedge nach dem Schließen und erneuten Initialisieren des Players ist eine Ausnahme aufgetreten
- [Fehlerbehebung][Design] Aktuelle Uhrzeit wurde für bestimmte Medienobjekte abgeschnitten
- [Fehlerbehebung][DRM] Unterstützung für die Wiedergabe in Browsern hinzugefügt, die mehrere CENC-DRM-Instanzen unterstützen

### <a name="changes-217"></a>Änderungen in Version 2.1.7 ###

- [Änderung][Beispiele][Barrierefreiheit] Sprachtag für alle Beispiele hinzugefügt

## <a name="216-official-update"></a>2.1.6 (Offizielles Update) ##

### <a name="bug-fixes-216"></a>Fehlerbehebungen in Version 2.1.6 ###

- [Fehlerbehebung] AMP hat für bestimmte Medienobjekte die falsche Dauer angezeigt
- [Fehlerbehebung][FairPlay-HLS] FairPlay-Fehler wurden nicht an die Benutzeroberfläche weitergegeben
- [Fehlerbehebung] Benutzerdefinierte Heuristikeigenschaften wurden in AMP 2.1.5 ignoriert

### <a name="changes-216"></a>Änderungen in Version 2.1.6 ###

- [Änderung][FairPlayDRM] Timeout für Zertifikat- und Lizenzanforderung für FairPlay entfernt, um Parität mit PlayReady- und Widevine-Implementierungen zu erzielen
- [Änderung] Verschiedene Änderungen in Bezug auf die Heuristik, um die unscharfe Anzeige von Inhalten zu vermeiden

### <a name="features-216"></a>Features in Version 2.1.6 ###

- [Feature] Unterstützung für das Format „mpd-time-cmaf“ hinzugefügt

## <a name="215-official-hotfix"></a>2.1.5 (Offizieller Hotfix) ##

### <a name="bug-fixes-215"></a>Fehlerbehebungen in Version 2.1.5 ###

- [Fehlerbehebung][Untertitel] VTT-Formatierungen wurden vom Player nicht richtig gerendert
- [Fehlerbehebung][Barrierefreiheit] ARIA-Bezeichnung für Schaltfläche „Live“ hat gefehlt

## <a name="214-official-update"></a>2.1.4 (Offizielles Update) ##

### <a name="bug-fixes-214"></a>Fehlerbehebungen in Version 2.1.4 ###

- [Fehlerbehebung][Barrierefreiheit][Fokus] Benutzer konnten mit der TAB-TASTE den Fokus nicht auf benutzerdefinierte Schaltflächen verschieben, die in der Steuerleiste rechts von der Schaltfläche „Vollbild“ hinzugefügt wurden
- [Fehlerbehebung][IE11][Lautstärkeleiste] Beim Zugreifen auf das Popupelement für die Lautstärke per TAB-TASTE hat in IE11 das gesamte Videofenster geblinkt, wenn der Vollbildmodus aktiviert war
- [Fehlerbehebung][Design|Flush] Angezeigte freie Fläche zwischen Steuerleiste und Popupelement für die Lautstärkeleiste
- [Fehlerbehebung][AMP][Untertitel] Alte eingebettete Titel wurden nicht gelöscht, wenn die Quelle in einem vorhandenen Player geändert wurde
- [Fehlerbehebung][Barrierefreiheit][Sprachausgabe] Sprachausgabe hat Inhalt des Steuerelements für die Lautstärke nicht richtig vorgelesen
- [Fehlerbehebung][FlashSS] Wiedergabeereignis wurde gelegentlich von der Flash-Technologie nicht ausgelöst
- [Fehlerbehebung][AMP][Fokus] Für „Play/Pause“ waren zwei Klicks erforderlich, wenn der Player den Fokus hatte und der Vollbildmodus aktiviert war
- [Fehlerbehebung][AMP][Design] Für ein spezifisches Medienobjekt wurde in der Statusanzeige eine falsche Dauer angezeigt
- [Fehlerbehebung][Anzeigen][Ad Butler] VAST-Parser hat keine VAST-Dateien ohne Statusereignis verarbeitet
- [Fehlerbehebung][SDN][AMP 2.1.1] Problem in Bezug auf die Unterstützung des Hive SDN-Plug-Ins behoben
- [Fehlerbehebung][Barrierefreiheit] Sprachausgabe hat „Midnight Mute Button“ (Schaltfläche Mitternacht stumm) vorgelesen, wenn der Benutzer den Fokus auf die Schaltfläche „Lautstärke“ verschoben hat

### <a name="changes-214"></a>Änderungen in Version 2.1.4 ###

- [Fehlerbehebung][Barrierefreiheit][Hilfstechnologie] Schaltflächen verfügen jetzt über eine „aria-live“-Eigenschaft zur Verbesserung der Benutzerfreundlichkeit in Bezug auf Hilfstechnologie
- [Änderung][Barrierefreiheit][Schaltfläche „Lautstärke“|Sprachausgabe] Barrierefreiheit für Schaltfläche „Lautstärke“ wurde verbessert, indem das Verhalten der TAB-TASTE und des Schiebereglers geändert wurde. Aufgrund dieser Änderungen ist es für Tastaturbenutzer einfacher, die Lautstärke des Players zu ändern.
- [Änderung] Timeout für Inaktivität des Kontextmenüs von drei auf fünf Sekunden erhöht
- [Änderung][Barrierefreiheit][Helligkeit] Kontrastverhältnis für Helligkeit in Dropdownmenüs der Untertiteleinstellungen verbessert

## <a name="213-official-update"></a>2.1.3 (Offizielles Update) ##

### <a name="bug-fixes-213"></a>Fehlerbehebungen in Version 2.1.3 ###

- [Fehlerbehebung][Plug-Ins|Titelüberlagerung] Plug-In für Titelüberlagerungen hat JS-Ausnahmen bei AMP v2.X+ ausgelöst
- [Fehlerbehebung] SourceSet-Ereignis wurde auch dann an die JavaScript-Konsole gesendet, wenn die Protokollierung deaktiviert war
- [Fehlerbehebung][Design] Tipps zur Player-Zeit wurden außerhalb des Player-Kontexts gerendert, wenn auf ein Ende der Leiste für die Anzeige der Dauer gezeigt wurde
- [Fehlerbehebung][Barrierefreiheit][Sprachausgabe] Sprachausgabe hat „Region Landmark“ (Wahrzeichen der Region) oder „Video Player Region Landmark (Wahrzeichen der Videoplayer-Region) vorgelesen, wenn der Fokus auf den Player verschoben wurde
- [Fehlerbehebung][AMP] Player-Rahmen konnte nicht per CSS deaktiviert werden
- [Fehlerbehebung][Barrierefreiheit] Fokus konnte per TAB-TASTE nicht auf den gesamten Player festgelegt werden, wenn der Vollbildmodus aktiviert war
- [Fehlerbehebung][Design][Live] Design hat nicht auf lokalisierten Live-Text auf Japanisch reagiert
- [Fehlerbehebung][Design] Dauer und aktuelle Uhrzeit wurden abgeschnitten, wenn der Stream länger als 60 Minuten war [Fehlerbehebung][iPhone|Live] Player hat Text für die aktuelle Uhrzeit/Dauer in der Steuerleiste angezeigt
- [Fehlerbehebung][AMP] Aufruf der APIs für die Player-Heuristik hat zu JavaScript-Ausnahmen geführt
- [Fehlerbehebung][Natives HTML5|iOS] Videotag-Eigenschaft „playsinline“ wurde nicht an Player weitergegeben
- [Fehlerbehebung][iOS|iframe] Player konnte auf einem iPhone nicht in den Vollbildmodus versetzt werden, wenn der Player in einem iframe geladen wurde
- [Fehlerbehebung][AMP][Heuristik] AMP wurde unabhängig von den Player-Optionen immer mit einem Hybridprofil betrieben
- [Fehlerbehebung][AMP|Win8.1] Beim Hosten in einer Win 8.1-App mit einer Webansicht wurde ein Fehler ausgelöst

### <a name="changes-213"></a>Änderungen in Version 2.1.3 ###

- [Änderung][AMP] Informationen zum CDN-Endpunkt im „FragmentDownloadComplete“-Ereignis hinzugefügt
- [Änderung][AMP][Live] Latenz beim Livestreaming verbessert und optimiert

## <a name="212-official-hotfix"></a>2.1.2 (Offizieller Hotfix) ##

### <a name="bug-fixes-212"></a>Fehlerbehebungen in Version 2.1.2 ####

- [Fehlerbehebung][Barrierefreiheit][Windows-Sprachausgabe] Sprachausgabe hat „Progress midnight“ (Status Mitternacht) vorgelesen, wenn sich der Benutzer im Kontext „Statusleiste“ befunden hat und die aktuelle Zeit „0:00 Uhr“ war
- [Fehlerbehebung][Design] Logogröße war in JavaScript-Code hartcodiert
- [Barrierefreiheit][Hotkeys] Hotkeys waren nicht aktiviert, wenn auf den Player geklickt wurde

### <a name="changes-212"></a>Änderungen in Version 2.1.2 ####

- [Änderung][Protokollierung] Manifest-URL wird protokolliert, wenn das Manifest vom Player nicht geladen werden kann

### <a name="features-212"></a>Features in Version 2.1.2 ###

- [Änderung][Leistung][Optimierung] Kürzere Zeiten beim Laden und Starten des Players

## <a name="211-official-update"></a>2.1.1 (Offizielles Update) ##

### <a name="bug-fixes-211"></a>Fehlerbehebungen in Version 2.1.1 ####

- [Fehlerbehebung][iOS] Festlegen der automatischen Wiedergabe auf „false“ hat in Safari für iOS zur unendlichen Anzeige des Wartekreisels geführt
- [Fehlerbehebung] Suche nach einem Zeitpunkt, der die Dauer des Inhalts übersteigt, hat zu unendlichem Wartekreisel geführt
- [Fehlerbehebung] Für Hotkeys waren mehrere Tastenanschläge erforderlich, damit der Kontext des Players funktioniert hat
- [Fehlerbehebung] Bei bestimmten Medienobjekten ist das Video nach dem Ändern der Player-Größe einige Sekunden lang eingefroren
- [Fehlerbehebung] Unendlicher Wartekreisel (nach Abschluss der Suche), wenn vom Benutzer in schneller Folge mehrere Suchen durchgeführt wurden
- [Fehlerbehebung] Steuerleiste ist bei Inaktivität nicht ausgeblendet
- [Fehlerbehebung] Das Öffnen einer Web-App mit Hosting von AMP konnte dazu führen, dass die Webseite zweimal geladen wurde
- [Fehlerbehebung] Unendlicher Wartekreisel bei der Wiedergabe bestimmter Medienobjekte per Flash-Technologie
- [Fehlerbehebung] Menü „Weitere Optionen“ wurde bei Drittanbieter-Plug-Ins nicht angezeigt
- [Fehlerbehebung][Design|Tube][Live] Zwei Livesymbole wurden angezeigt, wenn sich der Player im Liveedge eines Programms befunden hat
- [Fehlerbehebung][Design] Logo konnte nicht deaktiviert werden
- [Fehlerbehebung][DD+-Inhalte] Unendlicher Wartekreisel wurde für Medienobjekte mit Dolby Digital-Audiotiteln angezeigt
- [Fehlerbehebung] Aktuelle AMP-Version ist eingefroren, wenn während eines Livestreams Audiosprachtitel gewechselt wurden
- [Fehlerbehebung] Ausblendung im Hintergrund für Wartekreisel behoben
- [Fehlerbehebung] Fehler mit unendlichem Wartekreisel in statischen Beispielen für AES-Flashtoken behoben

### <a name="changes-211"></a>Änderungen in Version 2.1.1 ####

- [Änderung] Fehlercode für Widevine-HTTPS-Anforderung hinzugefügt: Ab Chrome v58 müssen Widevine-Inhalte mit dem `https://`-Protokoll geladen bzw. wiedergegeben werden, da sonst die Wiedergabe nicht möglich ist
- [Änderung] ARIA-Bezeichnung für das Laden des Wartekreisels hinzugefügt, damit Hilfstechnologie „Video wird geladen“ per Sprache ausgeben kann, wenn Inhalt geladen wird  

## <a name="210-official-release"></a>2.1.0 (Offizielle Version) ##

### <a name="features-210"></a>Features in Version 2.1.0 ###

- [Feature][AzureHtml5JS] Unterstützung für VOD-Anzeigen für Pre-/Mid-/Post-Roll
- [Feature][Beta][AzureHtml5JS] Unterstützung für Livevideo-Anzeigen für Pre-/Mid-/Post-Roll
- [Feature] Neue Designoption hinzugefügt: AMP-Flush
- [Feature] Verbesserte ARIA-Bezeichnungen hinzugefügt, um die Integration mit Sprachausgabe/Hilfstechnologie zu optimieren
- [Feature][Design] Im Design werden jetzt alle Symbole und Schaltflächen im Modus „Hoher Kontrast“ richtig angezeigt

### <a name="bug-fixes-210"></a>Fehlerbehebungen in Version 2.1.0 ###

- [Fehlerbehebung] Verschiedene Fehlerbehebungen in Bezug auf Barrierefreiheit und Benutzeroberfläche
- [Fehlerbehebung] AMP wurde in IE9 nicht richtig geladen

### <a name="changes-210"></a>Änderungen in Version 2.1.0 ###

- [Änderung] DOM-Elemente für Anzeigen im Player neu strukturiert
- [Änderung] Wechsel von CSS zu SCSS für Designentwicklung
- [Änderung][Beispiele] Beispiel für VOD-Anzeigen hinzugefügt
- [Änderung][Beispiele] Beispiel für Wiedergabegeschwindigkeit hinzugefügt
- [Änderung][Beispiele] Beispiel für Flush-Design hinzugefügt

## <a name="200-beta-release"></a>2.0.0 (Betaversion) ##

- [Änderung] Update auf VJS5
- [Feature] Neue Fluid-API für Reaktionsfähigkeit des Players hinzugefügt
- [Feature] Wiedergabegeschwindigkeit
- [Änderung] Wechsel von CSS zu SCSS für Design

## <a name="183-official-hotfix-update"></a>1.8.3 (Offizielles Hotfix-Update) ##

### <a name="bug-fixes-183"></a>Fehlerbehebungen in Version 1.8.3 ###

- [Fehlerbehebung][AzureHtml5JS] Bestimmte Medienobjekte mit negativen DTS wurden in Chrome nicht wiedergegeben

## <a name="182-official-hotfix-update"></a>1.8.2 (Offizielles Hotfix-Update) ##

### <a name="bug-fixes-182"></a>Fehlerbehebungen in Version 1.8.2 ###

- [Fehlerbehebung][AzureHtml5JS] Höhere Audiobitraten wurden per AzureHtml5JS nicht wiedergegeben

## <a name="181-official-update"></a>1.8.1 (Offizielles Update) ##

### <a name="bug-fixes-181"></a>Fehlerbehebungen in Version 1.8.1 ###

- [Fehlerbehebung][iOS] Untertitel wurden in nativem Player nicht angezeigt
- [Fehlerbehebung][AMP] CDN-basierte Streaming-URLs mit angefügten Authentifizierungstoken wurden nicht wiedergegeben
- [Fehlerbehebung][FairPlay] Tech-ID (Bits 31 - 28 des Fehlercodes) haben für FairPlay-Fehlercode gefehlt (siehe Fehlercodes)
- [Fehlerbehebung][Safari][PlayReady] PlayReady-Inhalt in Safari hat zu unendlichem Wartekreisel geführt

### <a name="changes-181"></a>Änderungen in Version 1.8.1 ###

- [Änderung][Html5] Native ausführliche Html5-Technologieprotokolle geändert, damit sie Videotagereignisse enthalten

## <a name="180-official-update"></a>1.8.0 (Offizielles Update) ##

### <a name="features-180"></a>Features in Version 1.8.0 ###

- [Features][DRM] FairPlay-Unterstützung hinzugefügt (siehe [Geschützter Inhalt](azure-media-player-protected-content.md))

### <a name="bug-fixes-180"></a>Fehlerbehebungen in Version 1.8.0 ###

- [Fehlerbehebung][AMP] Bei Benutzersuche wurde kein Warteereignis ausgelöst, wenn das Netzwerk gedrosselt war
- [Fehlerbehebung][FlashSS] Auswahl der Qualität in Flash-Technologie hat zu Ausnahme geführt
- [Fehlerbehebung][AMP] Dynamische Auswahl der Qualität wurde im Kontextmenü angezeigt
- [Fehlerbehebung][Design] Es war schwierig, in Kontextmenüs das letzte Menüelement auszuwählen

### <a name="changes-180"></a>Änderungen in Version 1.8.0 ###

- [Änderung] Player auf aktuelle Chrome/EME-Anforderungen aktualisiert
- [Änderung] Standardeinstellung für techOrder geändert, um neue Technologie abzudecken: html5FairPlayHLS (siehe [Geschützter Inhalt](azure-media-player-protected-content.md))
- [Änderung][AzureHtml5JS] MPEG-Dash-Wiedergabe in Safari aktiviert
- [Änderung][Beispiele] Multi-DRM-Beispiele geändert, um FairPlay abzudecken

## <a name="174-official-hotfix-update"></a>1.7.4 (Offizielles Hotfix-Update) ##

### <a name="bug-fixes-174"></a>Fehlerbehebungen in Version 1.7.4 ###

- [Fehlerbehebung][Chrome] Blauer Rahmen für Suchhandle wurde angezeigt, wenn sich der Benutzer im Player-Kontext befunden hat
- [Fehlerbehebung][IE9] JavaScript-Ausnahme wurde ausgelöst, wenn der Player in IE9 geladen wurde

## <a name="173-official-hotfix-update"></a>1.7.3 (Offizielles Hotfix-Update) ##

### <a name="bug-fixes-173"></a>Fehlerbehebungen in Version 1.7.3 ###

- [Fehlerbehebung][AzureHtml5JS] Timeout für Player in eingeschränkten Netzwerken

### <a name="changes-173"></a>Änderungen in Version 1.7.3 ###

- [Änderung] Webcrypto im Edgebereich zur Entschlüsselung von AES-Inhalt aktiviert
- [Änderung] Optimierung der AMP-Heuristik zur Abdeckung von zwischengespeicherten Blöcken
- [Änderung][AzureHtml5JS] Optimierung der Heuristik durch Reduzierung der Latenz bei der Bandbreitenschätzung

## <a name="172-official-hotfix-update"></a>1.7.2 (Offizielles Hotfix-Update) ##

### <a name="features-172"></a>Features in Version 1.7.2 ###
<!---API needs onboarding. Removed link to API until remedied.--->
- [Feature][AzureHtml5JS|Firefox] Widevine-Wiedergabe mit EME für Firefox 47+ aktiviert
- [Feature] Ereignis für Schließen des Players hinzugefügt
<!-- ([disposing](index.html#static-amp.eventname.disposing)) -->

### <a name="bug-fixes-172"></a>Fehlerbehebungen in Version 1.7.2 ###

- [Fehlerbehebung] Codierte Akamai-CDN-URL-Abfrageparameter wurden nicht richtig decodiert
- [Fehlerbehebung] Ausnahme wurde für „manifestPlayableWindowLength()“ ausgelöst
- [Fehlerbehebung] Benutzer konnte am Ende des Videos nicht immer auf „Play“ klicken, um es erneut wiederzugeben
- [Fehlerbehebung] Dynamische Größenanpassung hat nicht auf schnelle Änderungen der Fenstergrößen reagiert
- [Fehlerbehebung][Edge|IE] Dynamische Größenanpassung hatte keine Auswirkung beim Laden von Seiten für „width=x, height=auto“
- [Fehlerbehebung][Android|Chrome] Chrome hat nach Berechtigungen zum Wiedergeben von DRM-Inhalt gefragt, wenn dieser nicht verschlüsselt war
- [Fehlerbehebung][Barrierefreiheit][Edge] Kontextmenüelemente konnten per Tastatur nicht richtig ausgewählt werden
- [Fehlerbehebung][Barrierefreiheit] Fehlende Rahmenanzeige im Modus „Hoher Kontrast“
- [Fehlerbehebung][FlashSS] Ereignislistener für Loslassen der Maustaste wurde nach dem Schließen des Players nicht entfernt und hat zu Ausnahme geführt
- [Fehlerbehebung][FlashSS] Problem beim Analysieren der Manifest-URL mit codierten Leerzeichen
- [Fehlerbehebung][iOS] Typfehler beim Auswerten von „tech.featuresVolumeControl“

### <a name="changes-172"></a>Änderungen in Version 1.7.2 ###

- [Änderung][DRM] DRM-Überprüfungen hinter das Festlegen der Quelle verschoben, damit diese nur erfolgen, wenn Inhalt verschlüsselt ist
- [Änderung][AES] Undefinierten type/plain-Text aus Anforderung zur Schlüsselübermittlung entfernt
- [Änderung][Barrierefreiheit] Windows-Sprachausgabe liest jetzt „Media Player“ vor, wenn der Kontext auf den Player festgelegt ist (anstatt auf die Eigenschaften)

## <a name="171-official-hotfix-update"></a>1.7.1 (Offizielles Hotfix-Update) ##

### <a name="features-171"></a>Features in Version 1.7.1 ###

- [Feature] Option für Heuristik-Hybridprofil hinzugefügt (Profil wird standardmäßig festgelegt)

### <a name="bug-fixes-171"></a>Fehlerbehebungen in Version 1.7.1 ###

- [Fehlerbehebung] Dynamischer Entwurf hat nicht gemäß HTML5-Standard (width=100%, height=auto) funktioniert
- [Fehlerbehebung] Prozentwerte für Breite und Höhe haben sich in Version 1.7.0 nicht wie erwartet verhalten

## <a name="170-official-update"></a>1.7.0 (Offizielles Update) ##

### <a name="features-170"></a>Features in Version 1.7.0 ###
<!---API needs onboarding. Removed link until remedied.--->
- [Feature][AzureHtml5JS][FlashSS] „currentMediaTime()“ hinzugefügt, um die Encodermedienzeit für die aktuelle Uhrzeit in Sekunden abzurufen
- [Feature][FlashSS] APIs für Telemetriedaten-Downloads mit „videoBufferData()“ und „audioBufferData()“ implementiert<!-- (see [BufferData](index.html#amp.bufferdata) for more details) -->
- [Feature][FlashSS] Ereignis „downloadbitratechanged“ hinzugefügt
- [Feature] Ladedauer gegenüber früheren Versionen des Players verbessert
- [Feature] Fehler werden in der JavaScript-Konsole protokolliert

### <a name="bug-fixes-170"></a>Fehlerbehebungen in Version 1.7.0 ###

- [Fehlerbehebung] Codierte Poster-URL mit Abfragezeichenfolge-Parametern wurden im Player nicht angezeigt
- [Fehlerbehebung] Ausnahme wurde ausgelöst, wenn keine Technologie geladen war und API „amp.Player.poster()“ aufgerufen wurde
- [Fehlerbehebung] Ausnahme wurde ausgelöst, wenn von Funktionen versucht wurde, nach dem Schließen auf den Player zuzugreifen
- [Fehlerbehebung][Barrierefreiheit] Fehlende Kontur bei Fokus auf Suchkopfbereich der Statusanzeige
- [Fehlerbehebung][Barrierefreiheit] Kontextmenüs haben im Modus „Hoher Kontrast“ über einen Schatten verfügt
- [Fehlerbehebung][iOS] Native Wiedergabe von WebVTT-Untertiteln im Player hat nicht funktioniert
- [Fehlerbehebung][AzureHtml5JS] Fehler 0x0100002 sollte bei der Wiedergabe eines HTTP-Streams auf einer HTTPS-Website angezeigt werden (anstatt eines unendlichen Wartekreisels aufgrund von gemischtem Inhalt)
- [Fehlerbehebung][AzureHtml5JS] Fehlendes Endsegment hat zu einem Schleifenfehler bei der Integritätsprüfung geführt, der als unendlicher Puffervorgang wahrgenommen wurde
- [Fehlerbehebung][AzureHtml5JS] Falscher Audiotitelname im Menü, wenn „useManifestForLabel=false“ und dreistellige Sprachcodes verwendet wurden
- [Fehlerbehebung][AzureHtml5JS|Chrome] Scheinbarer unendlicher Puffervorgang am Ende des Inhalts, der durch eine Gleitkommawert-Ungenauigkeit bei der Dauer im JavaScript-Code in Chrome verursacht wurde
- [Fehlerbehebung][FlashSS] Vorübergehende Anzeige eines nicht kritischen zeitweiligen Fehlers bei der Flash Player-Erstellung
- [Fehlerbehebung][FlashSS] Fehler bei der Wiedergabe aufgrund von Rundungsungenauigkeiten, wenn für Video- und Audiostreams unterschiedliche Zeitskalen verwendet werden: Fehler „Fehler beim Generieren von FLVTags durch Fragment-URL (...)“
- [Fehlerbehebung][FlashSS] Probleme beim Analysieren der Manifest-URLs mit codierten Leerzeichen
- [Fehlerbehebung][FlashSS] Fehlende Überprüfung auf Flash Player-Version >= 11.4 hat zu Fehler bei der Wiedergabe geführt, anstatt die nächste Technologie gemäß „techOrder“ zu verwenden
- [Fehlerbehebung][FlashSS][AES] Probleme beim Akzeptieren von AES-Token, die Unterstriche enthalten haben
- [Fehlerbehebung][SilverlightSS|OSX] „//“ als Präfix eines Manifests anstelle des Protokolls (HTTP oder HTTPS) wurde als lokale Datei erkannt und hat zu unendlichem Wartekreisel geführt

### <a name="changes-170"></a>Änderungen in Version 1.7.0 ###

- [Änderung][FlashSS] SWF-Skripts („MSAdaptiveStreamingPlugin-osmf2.0.swf“ und „StrobeMediaPlayback.2.0.swf“) wurden zu einer SWF-Datei mit dem Namen „StrobeMediaPlayback.2.0.swf“ zusammengefasst
- [Änderung][FlashSS] Fehlercodeverteilung aktualisiert, um genauere Fehlercodes zu erhalten (z. B. führen 404-Fehler jetzt zu 0x30200194, anstatt zum generischen Fehler 0x30200000)

## <a name="163-official-hotfix-update"></a>1.6.3 (Offizielles Hotfix-Update) ##

### <a name="bug-fixes-163"></a>Fehlerbehebungen in Version 1.6.3 ###

- [Fehlerbehebung] JavaScript-Laufzeitausnahme, wenn der Ereignishandler für Hotkeys nach dem Schließen des Players ausgeführt wurde
- [Fehlerbehebung][Android][AzureHtml5JS] Keine Wiedergabe auf mobilem Gerät bei Nutzung eines Mobilfunknetzes
- [Fehlerbehebung] Aktualisierung von Forge zur Ausführung als Webworker, um die Benutzeroberfläche zu entlasten

## <a name="162-official-hotfix-update"></a>1.6.2 (Offizielles Hotfix-Update) ##

### <a name="features-162"></a>Features in Version 1.6.2 ###

- [Feature] Weitere Sprachen für Lokalisierung hinzugefügt (weitere Details in der Dokumentation)

### <a name="bug-fixes-162"></a>Fehlerbehebungen in Version 1.6.2 ###

- [Fehlerbehebung][IE9-10] Beim Klicken auf Bereiche rund um den Player wurde das Browserfenster aufgrund eines IE9/IE10-Fehlers minimiert, der bei „window.blur()“ auftritt
- [Fehlerbehebung][FlashSS] AES-Token mit Unterstrichen wurden nicht akzeptiert

## <a name="161-official-hotfix-update"></a>1.6.1 (Offizielles Hotfix-Update) ##

### <a name="bug-fixes-161"></a>Fehlerbehebungen in Version 1.6.1 ###

- [Fehlerbehebung][FlashSS|Edge,IE][SilverlightSS|IE] Festlegung des Fokus in IE/Microsoft Edge auf andere Elemente der Benutzeroberfläche für Eingaben oder andere Vorgänge war nicht möglich
- [Fehlerbehebung] Fehler bei AES-Wiedergabe, wenn Forge nicht definiert war
- [Fehlerbehebung][Android][AzureHtml5JS|Chrome] Anzeige des Wartekreisels und keine Wiedergabe von Inhalt während der Schleife zur Integritätsprüfung
- [Fehlerbehebung][IE9] Keine Unterstützung von „console.log()“, weil von IE 9 eine Ausnahme ausgelöst wurde

## <a name="160-official-update"></a>1.6.0 (Offizielles Update) ##

### <a name="features-160"></a>Features in Version 1.6.0 ###

- [Feature] Größenreduzierung von 33 % für „azuremediaplayer.min.js“
- [Feature][AzureHtml5JS|Edge][Nicht getestet] Unterstützung für DD+-Audiostreams in Microsoft Edge (kein Codec-Wechsel nach anfänglicher Auswahl). Für die App muss an diesem Punkt der richtige Audiostream ausgewählt werden.
- [Feature] Hotkey-Steuerelemente (weitere Details in der Dokumentation)
- [Feature] Tipp beim Zeigen auf Statuszeit für präzise Zeitpunktsuche
- [Feature] Asynchrone Erkennung von Plug-Ins, wenn das Plug-In die „setupDone“-Methode enthält

### <a name="bug-fixes-160"></a>Fehlerbehebungen in Version 1.6.0 ###

- [Fehlerbehebung] Arbeitsspeicherprotokoll für „getMemoryLog(true)“ wurde nicht geleert
- [Fehlerbehebung] Feld für Bitratenauswahl wurde bei Mausbewegung zurückgesetzt und hat zu Problemen beim Auswählen von niedrigeren Bitraten per Maus geführt
- [Fehlerbehebung] Mac Office in App ist bei Durchführung einer DRM-Prüfung abgestürzt
- [Fehlerbehebung] CSS-Klassen konnten leicht versehentlich überschrieben werden
- [Fehlerbehebung][Chrome] Identifizierung aus User-Agent-Zeichenfolge für Browser auf Microsoft Edge aktualisiert
- [Fehlerbehebung][AzureHtml5JS] Schaltfläche „Untertitel“ wird in der Symbolleiste in Microsoft Edge (Win10) bzw. Chrome (Mac) nicht angezeigt
- [Fehlerbehebung][Android][AzureHtml5JS|Chrome] Ausnahme „InvalidStateError“ bei Aufruf „endOfStream()“ in kurzen Videos
- [Fehlerbehebung][Firefox] DRM-Warnung entfernt, die von Firefox beim Überprüfen der Browserfunktionen verursacht wurde
- [Fehlerbehebung][Html5] Untertitel wurden für Progressive MP4-Inhalt nicht angezeigt
- [Fehlerbehebung][FlashSS] Meldungen mit übereinstimmenden Zeitstempeln wurden in umgekehrter Reihenfolge protokolliert
- [Fehlerbehebung][Barrierefreiheit][Chrome|Firefox] Automatische Auswahl des ersten Menüelements mit TAB-TASTE und Auswahlsteuerelementen
- [Fehlerbehebung][Barrierefreiheit] Zugriff auf Schaltfläche „Lautstärke“ per TAB-TASTE

### <a name="changes-160"></a>Änderungen in Version 1.6.0 ###

- [Änderung] Verwendung der AES-Entschlüsselungszeit bei Auswahl der Qualitätsstufe
- [Änderung] Aktualisierung des URL-Rewriters auf Bevorzugung von HLS v4 gegenüber HLS v3 für mehrere Audiostreams
- [Änderung] „nativeControlsForTouch“ standardmäßig auf „false“ festgelegt (für richtige Funktionsweise erforderlich)

## <a name="150-official-update"></a>1.5.0 (Offizielles Update) ##

### <a name="features-150"></a>Features in Version 1.5.0 ###

- [Feature] Verbesserungen in Bezug auf allgemeine Websicherheit (Verhinderung von Einschleusung, XSS usw.)
- [Feature] Hooks für Integration des SDN-Plug-Ins für „sourceset“-Ereignis und „options.sdn“
- [Feature] Stabile Verarbeitung von 5XX- und 4XX-Fehlern während der Wiedergabe

### <a name="bug-fixes-150"></a>Fehlerbehebungen in Version 1.5.0 ###

- [Fehlerbehebung] Update für CSS-Minimierung durchgeführt, damit für Schaltflächen Schriftartcodes der HTML-Entität anstelle von Unicode verwendet werden
- [Fehlerbehebung][AzureHtml5JS] Für Multi-DRM-Inhalt wurde aus „protectionInfo“ immer das Token des ersten Elements ausgewählt, sodass für den zweiten DRM-Vorgang ein Fehler aufgetreten ist
- [Fehlerbehebung][AzureHtml5JS] Suchvorgang wurde nicht beendet, wenn in einem Bereich mit fehlenden Segmenten gesucht wurde
- [Fehlerbehebung][AzureHtml5JS|Edge] EME mit Präfix in Microsoft Edge für PlayReady-Wiedergabe aktualisiert
- [Fehlerbehebung][AzureHtml5JS|Firefox] EME-Überprüfung aktualisiert, damit für Firefox v42+ (mit MSE) für geschützten Inhalt auf Silverlight zurückgegriffen werden kann
- [Fehlerbehebung][FlashSS] „error.message“ von Zahl auf ausführliche Zeichenfolge aktualisiert

### <a name="changes-150"></a>Änderungen in Version 1.5.0 ###

- [Änderung] Poster funktionieren derzeit nur als absolute URLs

## <a name="140-official-update"></a>1.4.0 (Offizielles Update) ##

### <a name="features-140"></a>Features in Version 1.4.0 ###

- [Feature][AzureHtml5JS|Chrome] Einfache Widevine-DRM-Unterstützung
- [Feature][AzureHtml5JS] Stabile Verarbeitung von 404/412-Fehlern während der Wiedergabe

### <a name="bug-fixes-140"></a>Fehlerbehebungen in Version 1.4.0 ###

- [Fehlerbehebung][FlashSS] Verbesserung der Parameterüberprüfung

## <a name="130-official-update"></a>1.3.0 (Offizielles Update) ##

### <a name="features-130"></a>Features in Version 1.3.0 ###

- [Feature][AzureHtml5JS][FlashSS] Audiowechsel für Multi-Audioinhalt mit demselben Codec

### <a name="bug-fixes-130"></a>Fehlerbehebungen in Version 1.3.0 ###

- [Fehlerbehebung][AzureHtml5JS|Chrome] Zeitweiliger unendlicher Wartekreisel
- [Fehlerbehebung][AzureHtml5JS|IE][Windows Phone] Ausnahme hat für Windows Phone zu Problemen bei der Wiedergabe geführt
- [Fehlerbehebung][FlashSS] Fehler für zusätzliche Instanzen, wenn automatische Wiedergabe auf „false“ festgelegt war
- [Fehlerbehebung] Probleme bei der Größenanpassung für Menüs der Benutzeroberfläche

## <a name="120-official-update"></a>1.2.0 (Offizielles Update) ##

### <a name="features-120"></a>Features in Version 1.2.0 ###

- [Feature][AzureHtml5JS|Firefox] Unterstützung bei Aktivierung von MSE
- [Feature] App muss keine Pfade für Binärdateien (swf, xap) von alternativen Technologien mehr angeben. Der Pfad ist relativ zum Azure Media Player-Skript.

### <a name="bug-fixes-120"></a>Fehlerbehebungen in Version 1.2.0 ###

- [Fehlerbehebung][AzureHtml5JS|Chrome] Player wurde hinter Liveedge verschoben, wenn er sich im Hintergrund befunden hat
- [Fehlerbehebung][AzureHtml5JS|Edge] Vollbildmodus hat nicht funktioniert
- [Fehlerbehebung][AzureHtml5JS] Protokollierung wurde bei Festlegung in den Optionen nicht richtig aktiviert
- [Fehlerbehebung][Flash] Bei Warteereignis wurde sowohl „Wird gepuffert“ als auch das Symbol für die Pufferung angezeigt
- [Fehlerbehebung] Wiedergabe kann fortgesetzt werden, falls die erste Bandbreitenanforderung nicht erfolgreich ist
- [Fehlerbehebung] Fehler beim Laden des Players, wenn die Initialisierung mit nicht definierten Optionen durchgeführt wurde
- [Fehlerbehebung] Beim Versuch, den Player zu schließen, nachdem das Schließen bereits erfolgt ist, ist eine vdata-Ausnahme aufgetreten
- [Fehlerbehebung] Fehlerhafte Zuordnung der Symbole für die Qualitätsleiste

## <a name="111-official-hotfix-update"></a>1.1.1 (Offizielles Hotfix-Update) ##

### <a name="bug-fixes-111"></a>Fehlerbehebungen in Version 1.1.1 ###

- [Fehlerbehebung] Problem mit Vollbild in früherer IE-Version
- [Fehlerbehebung] Plug-Ins werden nicht mehr überschrieben

## <a name="110-official-update"></a>1.1.0 (Offizielles Update) ##

### <a name="features-110"></a>Features in Version 1.1.0 ###

- [Feature] Lokalisierungszeichenfolgen für Benutzeroberfläche aktualisiert

### <a name="bug-fixes-110"></a>Fehlerbehebungen in Version 1.1.0 ###

- [Fehlerbehebung] Nicht genügend Kontrast für große Schaltfläche „Play“
- [Fehlerbehebung] Visuelle Anzeige für Fokuswechsel per TAB-TASTE
- [Fehlerbehebung] Menü für die Auswahl der Bitrate enthält jetzt die richtigen Informationen zur Auflösung
- [Fehlerbehebung] Größe des Menüs „Weitere Optionen“ wird jetzt dynamisch angepasst
- [Fehlerbehebung] Verschiedene Probleme mit der Benutzeroberfläche

## <a name="100-official-release"></a>1.0.0 (Offizielle Version) ##

### <a name="features-100"></a>Features in Version 1.0.0 ###

- [Feature] Grundlegende Tests für Barrierefreiheit in Bezug auf Registerkartensteuerung, Fokussteuerung, Sprachausgabe, Benutzeroberfläche mit hohem Kontrast
- [Feature] Benutzeroberfläche aktualisiert
- [Feature] Protokollierung für Entwicklung
- [Feature] API für dynamisches Festlegen von Spuren für Untertitel
- [Feature] Grundlegende Features für die Lokalisierung
- [Feature] Vereinheitlichung der Fehlercodes für verschiedene Technologien
- [Feature] Neuer Fehlercode für den Fall, dass Plug-Ins (z. B. Flash oder Silverlight) nicht installiert sind
- [Feature][AzureHtml5JS] Grundlegende Diagnoseereignisse implementiert

### <a name="bug-fixes-100"></a>Fehlerbehebungen in Version 1.0.0 ###
<!---What is that actually supposed to say?--->
- [Fehlerbehebung][AzureHtml5JS] Livewiedergabe ist bei MPD-Updates eingefroren, wenn der Zeitstempel kleinere Ungenauigkeiten aufgewiesen hat
- [Fehlerbehebung][AzureHtml5JS] Behebung mehrerer Probleme mit der Livewiedergabe
- [Fehlerbehebung][AzureHtml5JS] Leeren der Puffer, wenn Heuristik für Fenstergröße aktiviert ist, und Wechseln zu einem Bildschirm mit höherer Auflösung
- [Fehlerbehebung][AzureHtml5JS] In Chrome wird das Ereignis für das Ende jetzt richtig angezeigt. Verknüpft mit vorherigem bekanntem Problem *Chrome sendet das beendete Ereignis nicht richtig, wenn „AzureHtml5JS“ verwendet wird. Es besteht ein Problem im zugrunde liegenden Browser.*
- [Fehlerbehebung][AzureHtml5JS] Safari für diese Technologie deaktiviert, um das folgende Problem zu beheben: *Wiedergabeproblem bei OSX Yosemite mit der Technologie „AzureHtml5JS tech“. Es bestehen Probleme bei der MSE-Implementierung. Vorübergehende Lösung: Erzwingen von „flashSS“, „silverlightSS“ als Technologiereihenfolge für diese User-Agents*
- [Fehlerbehebung][FlashSS] „loadstart“ wird nach Auftreten eines Fehlers ausgelöst

## <a name="020-beta"></a>0.2.0 (Beta) ##

### <a name="features-020"></a>Features in Version 0.2.0 ###

- [Feature] Tests für PlayReady und AES für On-Demand und Live abgeschlossen (siehe Kompatibilitätsmatrix)
- [Feature] Verarbeitung von Diskontinuitäten
- [Feature] Unterstützung für Zeitstempel, die größer als 2^53 sind
- [Feature] URL-Abfrageparameter wird in Manifestanforderung beibehalten
- [Feature][Nicht getestet] Unterstützung für Heuristikprofile `QuickStart` und `HighQuality`
- [Feature][Nicht getestet] Bereitstellung von Videostreaminformationen für Bitraten, Breite und Höhe bei „AzureHtml5JS“ und „FlashSS“
- [Feature][Nicht getestet] Auswahl der Bitrate für „AzureHtml5JS“ und „FlashSS“ (siehe API-Dokumentation)

### <a name="bug-fixes-020"></a>Fehlerbehebungen in Version 0.2.0 ###

- [Fehlerbehebung] Große Schaltfläche „Play“ wird unter WP 8.1 jetzt angezeigt
- [Fehlerbehebung] Mehrere Probleme mit der Livewiedergabe behoben
- [Fehlerbehebung] Schaltfläche für Aufhebung der Stummschaltung funktioniert jetzt für die Benutzeroberfläche
- [Fehlerbehebung] Ladevorgang der Benutzeroberfläche für den Modus „Automatische Wiedergabe“ aktualisiert
- [Fehlerbehebung] Problem bei AMD-Ladevorgang und Definition von Methodenkonflikten
- [Fehlerbehebung] Problem beim Laden der Cordova-App für WP 8.1
- [Fehlerbehebung] Für geschützten Inhalt wird der von der Plattform/Technologie unterstützte ProtectionType abgefragt, um die passende Technologie für die Wiedergabe auszuwählen.  Hierdurch wird das folgende vorherige bekannte Problem behoben: „_Für PlayReady-Inhalt in Chrome (Desktop)/Safari 8 (OSX Yosemite) wird derzeit kein Fallback auf den Silverlight-Player durchgeführt_“.
- [Fehlerbehebung] Nicht abgefangene Ausnahme unter WinServer 2012 R2, weil auf dem Computer standardmäßig keine Media Foundation-Installation vorhanden ist.  Es wird versucht, nicht implementierte HTML-Videotag-APIs zu verwenden, und dies führt zu einem Fehler. Die derzeitige Lösung besteht darin, den Fehler abzufangen und „false“ zurückzugeben, anstatt den Fehler auszulösen.
- [Fehlerbehebung] init-Segment wird nach einem Such- oder HTTP-Fehler immer abgerufen, um Störungen bei der Wiedergabe zu verhindern
- [Fehlerbehebung] Nachverfolgung des simulierten Status und von „timeupdates“ deaktivieren, wenn ein Fehler aufgetreten ist
- [Fehlerbehebung] Kontextmenü entfernt
- [Fehlerbehebung][AzureHtml5JS] Fehlermeldung wurde nicht angezeigt, wenn für PlayReady-Inhalt ein ungültiges Token festgelegt wurde
- [Fehlerbehebung][AzureHtml5JS] Beim Umschalten auf den Vollbildmodus während der Livewiedergabe wurde die Heuristik für die Fenstergröße nicht berücksichtigt
- [Fehlerbehebung][FlashSS] Vom Strobe Media Player angezeigte Meldungen entfernt, damit nur Meldungen von Azure Media Player angezeigt werden
- [Fehlerbehebung][SilverlightSS] „seeked“-Ereignis wurde nicht abgerufen, wenn über die Dauer hinaus oder mit einem kleineren Wert als 0 gesucht wurde

## <a name="010-beta-release"></a>0.1.0 (Betaversion) ##

Erste Vorabversion

## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)
