---
title: 'Azure Media Services Video Indexer: Versionshinweise | Microsoft-Dokumentation'
description: Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Azure Media Services Video Indexer.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: na
ms.topic: article
ms.date: 04/20/2020
ms.author: juliako
ms.openlocfilehash: 8a9c1ba8e6316ea79941fbec1fbd339893afdb35
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82131204"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Azure Media Services Video Indexer: Versionshinweise

>Sie können eine Benachrichtigung erhalten, wann auf dieser Seite Updates vorhanden sind, indem Sie die URL `https://docs.microsoft.com/api/search/rss?search=%22Azure+Media+Services+Video+Indexer+release+notes%22&locale=en-us` kopieren und in Ihren RSS-Feedreader einfügen.

Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

* Neueste Versionen
* Bekannte Probleme
* Behebung von Programmfehlern
* Veraltete Funktionen

## <a name="april-2020"></a>April 2020

### <a name="new-widget-parameters-capabilities"></a>Neue Funktionen für Widgetparameter

Das Widget **Insights** weist neue Parameter auf: `language` und `control`.

Das Widget **Player** weist den neuen Parameter `locale` auf. Die Parameter `locale` und `language` steuern die Sprache des Players.

Weitere Informationen finden Sie im Abschnitt [Widgettypen](video-indexer-embed-widgets.md#widget-types). 

### <a name="new-player-skin"></a>Neue Playerskin

Eine neue Playerskin mit aktualisiertem Design wurde herausgegeben.

### <a name="prepare-for-upcoming-changes"></a>Vorbereitung auf bevorstehende Änderungen

* Heute geben die folgenden APIs ein Kontoobjekt zurück:

    * [Create-Paid-Account](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Paid-Account)
    * [Get-Account](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Account)
    * [Get-Accounts-Authorization](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Accounts-Authorization)
    * [Get-Accounts-With-Token](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Accounts-With-Token)
 
    Das Account-Objekt weist ein Feld `Url` auf, das auf den Speicherort der [Video Indexer-Website](https://www.videoindexer.ai/) verweist.
Für kostenpflichtige Konten verweist das Feld `Url` zurzeit auf eine interne URL anstelle der öffentlichen Website.
In den nächsten Wochen werden wir dies ändern und die URL der [Video Indexer-Website](https://www.videoindexer.ai/) für alle Konten zurückgeben (Testkonten und kostenpflichtige Konten).

    Verwenden Sie nicht die internen URLs, Sie sollten die [öffentlichen Video Indexer-APIs](https://api-portal.videoindexer.ai/) verwenden.
* Wenn Sie Video Indexer-URLs in Ihre Anwendungen einbetten und die URLs nicht auf die [Video Indexer-Website](https://www.videoindexer.ai/) oder den [Video Indexer-API-Endpunkt](https://api.videoindexer.ai) sondern stattdessen auf einen regionalen Endpunkt verweisen (beispielsweise `https://wus2.videoindexer.ai`), generieren Sie die URLs erneut.

   Dies kann auf einer der folgenden Weisen erfolgen:

    * Ersetzen der URL durch eine URL, die auf die Video Indexer-Widget-APIs verweist (beispielsweise das [Widget „Kognitive Erkenntnisse“](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Video-Insights-Widget))
    * Verwenden der Video Indexer-Website zum Generieren einer neuen eingebetteten URL:
         
         Drücken Sie **Wiedergabe**, um zur Seite Ihres Videos zu gelangen > klicken Sie auf die Schaltfläche **&lt;/&gt; Einbinden** > kopieren Sie die URL in Ihre Anwendung:
   
    Die regionalen URLs werden nicht unterstützt und werden in den nächsten Wochen blockiert.

## <a name="january-2020"></a>Januar 2020
 
### <a name="custom-language-support-for-additional-languages"></a>Benutzerdefinierte Sprachunterstützung für weitere Sprachen

Video Indexer unterstützt jetzt benutzerdefinierte Sprachmodelle für `ar-SY`, `en-UK` und `en-AU` (nur API).
 
### <a name="delete-account-timeframe-action-update"></a>Aktualisierung des Zeitrahmens der Aktion „Konto löschen“

Beim Ausführen der Aktion „Konto löschen“ wird das Konto jetzt innerhalb von 90 Tagen und nicht innerhalb von 48 Stunden gelöscht.
 
### <a name="new-video-indexer-github-repository"></a>Neues GitHub-Repository für Video Indexer

Ein neues Video Indexer-GitHub mit verschiedenen Projekten, Leitfäden mit ersten Schritten und Codebeispielen ist jetzt unter https://github.com/Azure-Samples/media-services-video-indexer verfügbar.
 
### <a name="swagger-update"></a>Swagger-Update

Video Indexer hat **Authentifizierungen** und **Vorgänge** in einer einzigen [Video Indexer-OpenAPI-Spezifikation (Swagger)](https://api-portal.videoindexer.ai/docs/services/Operations/export?DocumentFormat=OpenApiJson) vereinheitlicht. Entwickler finden die APIs im [Entwicklerportal für Video Indexer](https://api-portal.videoindexer.ai/).

## <a name="december-2019"></a>Dezember 2019

### <a name="update-transcript-with-the-new-api"></a>Aktualisieren des Transkripts mit der neuen API

Aktualisieren Sie einen bestimmten Abschnitt im Transkript mit der [Update-Video-Index](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Index?&pattern=update)-API.

### <a name="fix-account-configuration-from-the-video-indexer-portal"></a>Korrigieren der Kontokonfiguration über das Video Indexer-Portal

Sie können jetzt die Konfiguration der Media Services-Verbindung aktualisieren, um sich selbst zu helfen bei Problemen wie: 

* Falsche Azure Media Services-Ressource
* Kennwortänderungen
* Verschieben von Media Services-Ressourcen zwischen Abonnements  

Um die Kontokonfiguration zu korrigieren, navigieren Sie im Video Indexer-Portal zur Registerkarte „Einstellungen > Konto“ (als Eigentümer).

### <a name="configure-the-custom-vision-account"></a>Konfigurieren des Custom Vision-Kontos

Konfigurieren Sie das Custom Vision-Konto für kostenpflichtige Konten über das Video Indexer-Portal (zuvor wurde dies nur von der API unterstützt). Melden Sie sich dazu im Video Indexer-Portal an, wählen Sie „Modellanpassung > Animierte Zeichen > Konfigurieren“ aus. 

### <a name="scenes-shots-and-keyframes--now-in-one-insight-pane"></a>Szenen, Aufnahmen und Keyframes – jetzt in einem Erkenntnisbereich

Szenen, Aufnahmen und Keyframes werden jetzt zu einer Erkenntnis zusammengeführt, um Nutzung und Navigation zu erleichtern. Wenn Sie die gewünschte Szene auswählen, können Sie sehen, aus welchen Aufnahmen und Keyframes sie besteht. 

### <a name="notification-about-a-long-video-name"></a>Benachrichtigung über einen langen Videonamen

Wenn ein Videoname mehr als 80 Zeichen umfasst, zeigt der Video Indexer beim Hochladen einen beschreibenden Fehler an.

### <a name="streaming-endpoint-is-disabled-notification"></a>Benachrichtigung über deaktivierten Streamingendpunkt

Wenn der Streamingendpunkt deaktiviert ist, zeigt Video Indexer einen beschreibenden Fehler auf der Playerseite an.

### <a name="error-handling-improvement"></a>Verbesserung der Fehlerbehandlung

Der Statuscode 409 wird jetzt von den APIs [Video erneut indizieren](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Re-Index-Video?https://api-portal.videoindexer.ai/docs/services/Operations/operations/Re-Index-Video?) und [Videoindex aktualisieren](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Index?) zurückgegeben, falls ein Video aktiv indiziert wird, um zu verhindern, dass die aktuellen Änderungen der Neuindizierung versehentlich überschrieben werden.

## <a name="november-2019"></a>November 2019
 
* Unterstützung benutzerdefinierte koreanische Sprachmodelle

    Video Indexer unterstützt jetzt benutzerdefinierte Sprachmodelle in Koreanisch (`ko-KR`) sowohl in der API als auch im Portal. 
* Neue unterstützte Sprachen für die Spracherkennung (STT)

    Video Indexer-APIs unterstützen jetzt die Spracherkennung in Arabisch-Levantine (ar-SY), English UK-Dialekt (en-GB) und English Australiendialekt (en-AU).
    
    Für den Video Upload haben wir zh-HANS durch zh-CN ersetzt, zwar werden beide unterstützt, zh-CN wird jedoch empfohlen und ist genauer.
    
## <a name="october-2019"></a>Oktober 2019
 
* Suchen nach animierten Figuren im Katalog

    Wenn Sie animierte Figuren indizieren, können Sie sie jetzt im Videokatalog des Kontos danach suchen. Weitere Informationen finden Sie unter [Erkennung animierter Figuren](animated-characters-recognition.md).

## <a name="september-2019"></a>September 2019
 
Auf der IBC 2019 wurden mehrere Verbesserungen angekündigt:
 
* Erkennung animierter Figuren (öffentliche Vorschauversion)

    Die Fähigkeit zum Auffinden, Gruppieren und Erkennen von Figuren in animierten Inhalten mithilfe der Integration in Custom Vision. Weitere Informationen finden Sie unter [Erkennung animierter Figuren](animated-characters-recognition.md).
* Mehrsprachige Identifikation (öffentliche Vorschauversion)

    Erkennen von Segmenten in mehreren Sprachen in der Audiospur und Erstellen eines mehrsprachigen Transkripts auf ihrer Grundlage. Zu Anfang unterstützt: Englisch, Spanisch, Deutsch und Französisch. Weitere Informationen finden Sie unter [Automatisches Erkennen und Transkribieren mehrsprachiger Inhalte](multi-language-identification-transcription.md).
* Extraktion benannter Entitäten für Personen und Ort

    Extrahiert mithilfe der Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) Marken, Standorte und Personen aus Sprache und visuellem Text.
* Redaktionelle Typklassifizierung für Aufnahmen

    Markierung von Aufnahmen mit redaktionellen Typen wie Nahaufnahme, Halbtotale, zwei Personen, innen, außen usw. Weitere Informationen finden Sie unter [Erkennung von redaktionellen Aufnahmetypen](scenes-shots-keyframes.md#editorial-shot-type-detection).
* Verbesserte Themendeduktion – jetzt mit Abdeckung von Stufe 2
    
    Das Themendeduktionsmodell unterstützt jetzt mehr Detailebenen der IPTC-Taxonomie. Sämtliche Details finden Sie unter [Azure Media Services new AI-powered innovation](https://azure.microsoft.com/blog/azure-media-services-new-ai-powered-innovation/) (Neue, KI-gestützte Innovationen in Azure Media Services).

## <a name="august-2019"></a>August 2019
 
### <a name="video-indexer-deployed-in-uk-south"></a>Video Indexer bereitgestellt für „Vereinigtes Königreich, Süden“

Sie können jetzt ein kostenpflichtiges Video Indexer-Konto in der Region „Vereinigtes Königreich, Süden“ erstellen.

### <a name="new-editorial-shot-type-insights-available"></a>Neue redaktionelle Aufnahmearten verfügbar

Neue Tags, die Videoaufnahmen hinzugefügt werden, bieten redaktionelle „Aufnahmearten“, um sie mit gängigen redaktionellen Phrasen zu kennzeichnen, die im Workflow der Inhaltserstellung verwendet werden, z.B.: extreme Nahaufnahme (extreme closeup), Nahaufnahme (closeup), Weitwinkel (wide), Medium, Zwei-Personen-Aufnahme (two shot), Outdoor, Indoor, linkes Gesicht (left face) und rechtes Gesicht (right face) (verfügbar im JSON).

### <a name="new-people-and-locations-entities-extraction-available"></a>Neue Extraktion von Personen- und Standortentitäten verfügbar

Video Indexer kennzeichnet benannte Orte und Personen über Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) aus der OCR und der Transkription des Videos. Video Indexer verwendet einen Machine Learning-Algorithmus, um zu erkennen, wann bestimmte Standorte (z.B. der Eiffelturm) oder Personen (z.B. John Doe) in einem Video ausgesprochen werden.

### <a name="keyframes-extraction-in-native-resolution"></a>Extraktion von Keyframes in nativer Auflösung

Keyframes, die von Video Indexer extrahiert wurden, sind in der ursprünglichen Auflösung des Videos verfügbar.
 
### <a name="ga-for-training-custom-face-models-from-images"></a>Allgemeine Verfügbarkeit für Trainieren von benutzerdefinierten Gesichtern aus Bildern

Das Trainieren von Gesichtern aus Bildern wurde vom Vorschaumodus in die allgemeine Verfügbarkeit überführt (verfügbar über eine API und im Portal).

> [!NOTE]
> Der Übergang von „Vorschauversion in allgemeine Verfügbarkeit“ hat keine Auswirkungen auf die Preisgestaltung.

### <a name="hide-gallery-toggle-option"></a>Option zum Ausblenden von „Katalog“

Sie können nun die Registerkarte „Katalog“ im Portal ausblenden (ähnlich dem Ausblenden der Registerkarte „Beispiele“).
 
### <a name="maximum-url-size-increased"></a>Maximale URL-Größe erhöht

Unterstützung für URL-Abfragezeichenfolgen von 4096 (anstelle von 2048) bei der Indizierung eines Videos.
 
### <a name="support-for-multi-lingual-projects"></a>Unterstützung für mehrsprachige Projekte

Projekte können nun auf Basis von Videos erstellt werden, die in verschiedenen Sprachen indiziert sind (nur API).

## <a name="july-2019"></a>Juli 2019

### <a name="editor-as-a-widget"></a>Editor als Widget

Der Video Indexer-KI-Editor ist nun als Widget verfügbar und kann in Kundenanwendungen eingebettet werden.

### <a name="update-custom-language-model-from-closed-caption-file-from-the-portal"></a>Aktualisieren des benutzerdefinierten Sprachmodells aus der Untertiteldatei über das Portal

Kunden können im Portal auf der Seite zur Anpassung als Eingabe für Sprachmodelle die Dateiformate VTT, SRT und TTML angeben.

## <a name="june-2019"></a>Juni 2019

### <a name="video-indexer-deployed-to-japan-east"></a>Video Indexer bereitgestellt für „Japan, Osten“

Sie können jetzt ein kostenpflichtiges Video Indexer-Konto in der Region „Japan, Osten“ erstellen.

### <a name="create-and-repair-account-api-preview"></a>Erstellen und Reparieren der Konto-API (Vorschau)

Eine neue API wurde hinzugefügt, die es Ihnen ermöglicht, [den Azure Media Services-Verbindungsendpunkt oder -Schlüssel zu aktualisieren](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Paid-Account-Azure-Media-Services?&groupBy=tag).

### <a name="improve-error-handling-on-upload"></a>Verbesserung der Fehlerbehandlung beim Hochladen 

Eine beschreibende Meldung wird im Falle einer Fehlkonfiguration des zugrunde liegenden Azure Media Services-Kontos zurückgegeben.

### <a name="player-timeline-keyframes-preview"></a>Vorschau der Keyframes in der Player-Zeitachse 

Sie können jetzt eine Bildvorschau für jeden Zeitpunkt auf der Zeitachse des Players anzeigen.

### <a name="editor-semi-select"></a>Editor-Teilauswahl

Sie können jetzt eine Vorschau aller Erkenntnisse anzeigen, die als Ergebnis der Auswahl eines bestimmten Erkenntniszeitraums im Editor ausgewählt sind.

## <a name="may-2019"></a>Mai 2019

### <a name="update-custom-language-model-from-closed-caption-file"></a>Aktualisieren des benutzerdefinierte Sprachmodells aus der Untertiteldatei

Die APIs [Benutzerdefiniertes Sprachmodell erstellen](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?&groupBy=tag) und [Benutzerdefiniertes Sprachmodell aktualisieren](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Language-Model?&groupBy=tag) unterstützen jetzt VTT-, SRT-, und TTML-Dateiformate als Eingabe für Sprachmodelle.

Beim Aufrufen der [API „Video-Transkript aktualisieren“](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Transcript?&pattern=transcript) wird die Transkription automatisch hinzugefügt. Das dem Video zugeordnete Trainingsmodell wird ebenfalls automatisch aktualisiert. Informationen zum Anpassen und Trainieren Ihres Sprachmodells finden Sie unter [Anpassen eines Sprachmodells mit Video Indexer](customize-language-model-overview.md).

### <a name="new-download-transcript-formats--txt-and-csv"></a>Neue Transkriptformate zum Herunterladen – TXT und CSV

Zusätzlich zum bereits unterstützten Untertitelformat (SRT VTT und TTML) unterstützt Video Indexer jetzt das Herunterladen des Transkripts im TXT- und CSV-Format.

## <a name="next-steps"></a>Nächste Schritte

[Übersicht](video-indexer-overview.md)
