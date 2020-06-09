---
title: 'Azure Media Services v3: Versionshinweise | Microsoft-Dokumentation'
description: Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: na
ms.topic: article
ms.date: 05/28/2020
ms.author: juliako
ms.openlocfilehash: e072bcb0edc741b7843f470f14c3c37153338efb
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83647637"
---
# <a name="azure-media-services-v3-release-notes"></a>Versionshinweise zu Azure Media Services v3

>Sie können eine Benachrichtigung erhalten, wann auf dieser Seite Updates vorhanden sind, indem Sie die URL `https://docs.microsoft.com/api/search/rss?search=%22Azure+Media+Services+v3+release+notes%22&locale=en-us` kopieren und in Ihren RSS-Feedreader einfügen.

Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

* Neueste Versionen
* Bekannte Probleme
* Behebung von Programmfehlern
* Veraltete Funktionen

## <a name="known-issues"></a>Bekannte Probleme

> [!NOTE]
> Sie können das [Azure-Portal](https://portal.azure.com/) für Folgendes nutzen: Verwalten von v3-[Liveereignissen](live-events-outputs-concept.md), Anzeigen von v3-[Objekten](assets-concept.md) und Abrufen von Informationen zum Zugreifen auf APIs. Verwenden Sie für alle anderen Verwaltungsaufgaben (etwa für Transformationen und Aufträge) die [REST-API](https://aka.ms/ams-v3-rest-ref), die [CLI](https://aka.ms/ams-v3-cli-ref) oder eins der unterstützten [SDKs](media-services-apis-overview.md#sdks).

Weitere Informationen finden Sie unter [Hinweise zur Migration von Media Services v2 zu v3](migrate-from-v2-to-v3.md#known-issues).

## <a name="may-2020"></a>Mai 2020

Azure Media Services ist jetzt in den folgenden Regionen allgemein verfügbar: „Deutschland, Norden“, „Deutschland, Westen-Mitte“, „Schweiz, Norden“ und „Schweiz, Westen“. Kunden können Media Services mithilfe des Azure-Portals in diesen Regionen bereitstellen.

## <a name="april-2020"></a>April 2020

### <a name="improvements-in-documentation"></a>Verbesserungen bei der Dokumentation

Die Azure Media Player-Dokumentation wurde zur [Azure-Dokumentation](../azure-media-player/azure-media-player-overview.md) migriert.

## <a name="january-2020"></a>Januar 2020

### <a name="improvements-in-media-processors"></a>Verbesserungen bei Medienprozessoren

- Verbesserte Unterstützung für verschachtelte Quellen in der Videoanalyse: Diese Inhalte werden jetzt ordnungsgemäß entschachtelt, bevor sie an Inferenzmodule gesendet werden.
- Beim Generieren von Miniaturansichten im „besten“ Modus sucht der Encoder jetzt über 30 Sekunden, um einen nicht monochromen Frame auszuwählen.

### <a name="azure-government-cloud-updates"></a>Updates für die Azure Government-Cloud

Media Services ist nun in den folgenden Azure Government-Regionen allgemein verfügbar: *USGov Arizona* und *USGov Texas*.

## <a name="december-2019"></a>Dezember 2019

CDN-Unterstützung für *Origin-Assist Prefetch*-Header für Livestreaming und On-Demand-Videostreaming wurde hinzugefügt und ist für Kunden mit direktem Vertrag mit Akamai CDN verfügbar. Die Funktion „Origin-Assist CDN-Prefetch“ umfasst den folgenden HTTP-Header-Austausch zwischen Akamai CDN und dem Azure Media Services-Ursprung:

|HTTP-Header|Werte|Sender|Receiver|Zweck|
| ---- | ---- | ---- | ---- | ----- |
|CDN-Origin-Assist-Prefetch-Enabled | „1“ (Standardwert) oder „0“ |CDN|Origin|Angabe, dass CDN für den Vorabruf aktiviert ist|
|CDN-Origin-Assist-Prefetch-Path| Beispiel: <br/>Fragments(video=1400000000,format=mpd-time-cmaf)|Origin|CDN|Angabe des Vorabrufpfads zu CDN|
|CDN-Origin-Assist-Prefetch-Request|„1“ (Vorabrufanforderung) oder „0“ (reguläre Anforderung)|CDN|Origin|Angabe, dass die Anforderung von CDN ein Vorabruf ist|

Um einen Teil des Headeraustausches in Aktion zu sehen, können Sie folgende Schritte ausführen:

1. Verwenden Sie Postman oder cURL, um eine Anforderung an den Media Services-Ursprung für ein Audio- oder Videosegment oder Audio- oder Videofragment auszugeben. Fügen Sie den Header „CDN-Origin-Assist-Prefetch-Enabled: 1“ in der Anforderung hinzu.
2. In der Antwort sollte der Header „CDN-Origin-Assist-Prefetch-Path“ mit einem relativen Pfad als Wert angezeigt werden.

## <a name="november-2019"></a>November 2019

### <a name="live-transcription-preview"></a>Livetranskription Preview

Die Livetranskription befindet sich jetzt in der öffentlichen Vorschau und ist in der Region „USA, Westen 2“ verfügbar.

Die Livetranskription ist so konzipiert, dass sie zusammen mit Liveereignissen als Add-On-Funktion funktioniert.  Sie wird sowohl für Pass-Through-Liveereignisse als auch für Liveereignisse mit Standard- oder Premium-Codierung unterstützt.  Wenn dieses Feature aktiviert wird, nutzt der Dienst das [Spracherkennungsfeature](../../cognitive-services/speech-service/speech-to-text.md) von Cognitive Services, um den gesprochenen Text in der eingehenden Audiodatei in Text zu konvertieren. Dieser Text wird dann mitsamt Video- und Audiodaten in den MPEG-DASH- und HLS-Protokollen für die Übermittlung zur Verfügung gestellt. Die Abrechnung basiert auf einer neuen Add-On-Verbrauchseinheit, die sich als zusätzliche Kosten für das Liveereignis niederschlägt, wenn es sich im Zustand „Wird ausgeführt“ befindet.  Details zur Livetranskription und Abrechnung finden Sie unter [Livetranskription](live-transcription.md)

> [!NOTE]
> Derzeit ist die Livetranskription nur als Vorschaufunktion in der Region „USA, Westen 2“ verfügbar. Sie unterstützt zu diesem Zeitpunkt nur die Transkription von gesprochener englischer Sprache (en-us).

### <a name="content-protection"></a>Inhaltsschutz

Die Funktion *Verhindern der Tokenwiedergabe* (Token Replay Prevention), die in begrenzten Regionen im September veröffentlicht wurde, ist jetzt in allen Regionen verfügbar.
Media Services-Kunden können jetzt einen Grenzwert festlegen, der bestimmt, wie oft ein Token zum Anfordern eines Schlüssels oder einer Lizenz verwendet werden kann. Weitere Informationen finden Sie unter [Token Replay Prevention](content-protection-overview.md#token-replay-prevention) (Verhindern der Tokenwiedergabe).

### <a name="new-recommended-live-encoder-partners"></a>Neue, empfohlene Liveencoder-Partner

Unterstützung für die folgenden neuen, empfohlenen Partner-Encoder für RTMP-Livestreaming wurde hinzugefügt:

- [Cambria Live 4.3](https://www.capellasystems.net/products/cambria-live/)
- [GoPro Hero7/8 und Max Action-Kameras](https://gopro.com/help/articles/block/getting-started-with-live-streaming)
- [Restream.io](https://restream.io/)

### <a name="file-encoding-enhancements"></a>Verbesserungen bei der Dateicodierung

- Eine neue Voreinstellung für Content Aware Encoding ist jetzt verfügbar. Sie erzeugt mithilfe der inhaltsbezogenen Codierung einen Satz von GOP-orientierten MP4s. Bei jeglichen eingegebenen Inhalten führt der Dienst eine erste einfache Analyse des eingegebenen Inhalts durch. Er verwendet diese Ergebnisse, um die optimale Anzahl der Ebenen, die geeignete Bitrate und die Auflösungseinstellungen für die Bereitstellung durch adaptives Streaming zu bestimmen. Diese Voreinstellung ist besonders effektiv für Videos mit geringer und mittlerer Komplexität, bei denen die Ausgabedateien mit niedrigeren Bitraten, aber in einer Qualität vorliegen, die dem Betrachter dennoch ein gutes Erlebnis bietet. Die Ausgabe enthält MP4-Dateien mit überlappendem Video und Audio. Weitere Informationen finden Sie unter [Öffnen der API-Spezifikationen](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json).
- Verbesserte Leistung und Multithreading für das Größenänderungsmodul in Media Encoder Standard. Unter bestimmten Bedingungen sollte der Kunde eine Leistungssteigerung zwischen 5 und 40 % VOD-Codierung sehen. Inhalte von geringer Komplexität, die in mehrere Bitraten codiert werden, erfahren die höchsten Leistungssteigerungen. 
- Die Standardcodierung behält nun bei Verwendung der zeitbasierten GOP-Einstellung einen regelmäßigen GOP-Rhythmus für Inhalte mit variablen Frameraten (VFR) bei der VOD-Codierung bei.  Dies bedeutet, dass Kunden, die Inhalte mit gemischten Frameraten übermitteln, die beispielsweise zwischen 15 und 30 fps variieren, nun feststellen sollten, dass bei der Ausgabe in MP4-Dateien mit Adaptive Bitrate Streaming regelmäßige GOP-Abstände berechnet werden. Hierdurch wird die Möglichkeit verbessert, nahtlos zwischen den Spuren zu wechseln, wenn über HLS oder DASH geliefert wird. 
-  Verbesserte AV-Synchronisierung für Quellinhalte mit variabler Framerate (VFR)

### <a name="video-indexer-video-analytics"></a>Video Indexer, Videoanalyse

- Keyframes, die mithilfe der VideoAnalyzer-Voreinstellung extrahiert wurden, sind jetzt in der ursprünglichen Auflösung des Videos, anstatt dass ihre Größe angepasst wird. Die hochauflösende Keyframe-Extraktion bietet Ihnen Bilder in Originalqualität und ermöglicht Ihnen die Verwendung bildbasierter Künstlicher Intelligenz-Modelle, die von den Microsoft-Diensten Maschinelles Sehen und Custom Vision bereitgestellt werden, um noch mehr Erkenntnisse zu Ihrem Video zu erhalten.

## <a name="september-2019"></a>September 2019

###  <a name="media-services-v3"></a>Media Services v3  

#### <a name="live-linear-encoding-of-live-events"></a>Lineare Livecodierung von Liveereignissen

Media Services v3 gibt die Vorschauversion von 24 Stunden x 365 Tagen linearer Livecodierung von Liveereignisssen bekannt.

###  <a name="media-services-v2"></a>Media Services v2  

#### <a name="deprecation-of-media-processors"></a>Einstellung von Medienprozessoren

Wir kündigen die Einstellung von *Azure Media Indexer* und *Azure Media Indexer 2 Preview* an. Die Daten zur Einstellung finden Sie unter dem Thema [Legacykomponenten](../previous/legacy-components.md). [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) ersetzt diese Legacy-Medienprozessoren.

Weitere Informationen finden Sie unter [Migration von Azure Media Indexer v1 und Azure Media Indexer v2 zu Azure Media Services Video Indexer](../previous/migrate-indexer-v1-v2.md).

## <a name="august-2019"></a>August 2019

###  <a name="media-services-v3"></a>Media Services v3  

#### <a name="south-africa-regional-pair-is-open-for-media-services"></a>Südafrika-Regionspaar für Media Services verfügbar 

Media Services ist jetzt in den Regionen „Südafrika, Norden“ und „Südafrika, Westen“ verfügbar.

Weiter Informationen finden Sie unter [Clouds und Regionen, in denen Media Services v3 verfügbar ist](azure-clouds-regions.md).

###  <a name="media-services-v2"></a>Media Services v2  

#### <a name="deprecation-of-media-processors"></a>Einstellung von Medienprozessoren

Wir kündigen die Einstellung der Medienprozessoren *Windows Azure Media Encoder* (WAME) und *Azure Media Encoder* (AME) an, die eingestellt werden. Die Daten zur Einstellung finden Sie unter dem Thema [Legacykomponenten](../previous/legacy-components.md).

Weitere Informationen finden Sie unter [Migrieren von WAME zu Media Encoder Standard](https://go.microsoft.com/fwlink/?LinkId=2101334) und [Migrieren von AME zu Media Encoder Standard](https://go.microsoft.com/fwlink/?LinkId=2101335).
 
## <a name="july-2019"></a>Juli 2019

### <a name="content-protection"></a>Inhaltsschutz

Beim Streamen von Inhalten, die mittels Tokeneinschränkung geschützt sind, benötigen Endbenutzer ein Token, das im Rahmen der Schlüsselübermittlungsanforderung gesendet wird. Mit dem Feature zum *Verhindern der Tokenwiedergabe* können Media Services-Kunden einen Grenzwert festlegen, der bestimmt, wie oft ein Token zum Anfordern eines Schlüssels oder einer Lizenz verwendet werden kann. Weitere Informationen finden Sie unter [Token Replay Prevention](content-protection-overview.md#token-replay-prevention) (Verhindern der Tokenwiedergabe).

Seit Juli war die Previewfunktion nur in „USA, Mitte“ und „USA, Westen-Mitte“ verfügbar.

## <a name="june-2019"></a>Juni 2019

### <a name="video-subclipping"></a>Video-Subclips

Sie können jetzt mithilfe eines [Auftrags](https://docs.microsoft.com/rest/api/media/jobs) Videos beim Codieren zuschneiden oder Subclips für diese erstellen. 

Dies funktioniert mit jeder [Transformation](https://docs.microsoft.com/rest/api/media/transforms), die entweder mit den [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset)-Voreinstellungen oder mit den [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset)-Voreinstellungen erstellt wird. 

Beispiele finden Sie hier:

* [Erstellen von Subclips mit .NET](subclip-video-dotnet-howto.md)
* [Erstellen von Subclips mit REST](subclip-video-rest-howto.md)

## <a name="may-2019"></a>Mai 2019

### <a name="azure-monitor-support-for-media-services-diagnostic-logs-and-metrics"></a>Azure Monitor-Unterstützung für Media Services-Diagnoseprotokolle und -Metriken

Sie können jetzt Azure Monitor verwenden, um die von Media Services ausgegebenen Telemetriedaten anzuzeigen.

* Nutzen Sie die Azure Monitor-Diagnoseprotokolle, um Anforderungen zu überwachen, die vom Endpunkt für die Media Services-Schlüsselbereitstellung gesendet werden. 
* Überwachen Sie die Metriken, die von den [Streamingendpunkten](streaming-endpoint-concept.md) von Media Services ausgegeben werden.   

Ausführliche Informationen finden Sie unter [Überwachen von Media Services-Metriken und -Diagnoseprotokollen](media-services-metrics-diagnostic-logs.md).

### <a name="multi-audio-tracks-support-in-dynamic-packaging"></a>Unterstützung von mehreren Audiospuren bei der dynamischen Paketerstellung 

Beim Streamen von Medienobjekten, die mehrere Audiospuren mit mehreren Codecs und Sprachen aufweisen, unterstützt die [dynamische Paketerstellung](dynamic-packaging-overview.md) jetzt mehrere Audiospuren für die HLS-Ausgabe (Version 4 oder höher).

### <a name="korea-regional-pair-is-open-for-media-services"></a>Korea-Regionspaar für Media Services verfügbar 

Microsoft Azure Media Services ist jetzt in den Regionen „Südkorea, Mitte“ und „Südkorea, Süden“ verfügbar. 

Weiter Informationen finden Sie unter [Clouds und Regionen, in denen Media Services v3 verfügbar ist](azure-clouds-regions.md).

### <a name="performance-improvements"></a>Leistungsverbesserungen

Updates wurden hinzugefügt, die Verbesserungen an der Media Services-Leistung umfassen.

* Die maximal unterstützte Dateigröße für die Verarbeitung wurde aktualisiert. Mehr dazu finden Sie unter [Kontingente und Grenzwerte](limits-quotas-constraints.md).
* [Verbesserungen der Codierungsgeschwindigkeit](media-reserved-units-cli-how-to.md#choosing-between-different-reserved-unit-types).

## <a name="april-2019"></a>April 2019

### <a name="new-presets"></a>Neue Voreinstellungen

* [FaceDetectorPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#facedetectorpreset) wurde zu den Voreinstellungen des integrierten Analysetools hinzugefügt.
* [ContentAwareEncodingExperimental](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset) wurde zu den Voreinstellungen des integrierten Encoders hinzugefügt. Weitere Informationen finden Sie unter [Inhaltsbezogene Codierung](content-aware-encoding.md). 

## <a name="march-2019"></a>März 2019

Die dynamische Paketerstellung unterstützt jetzt Dolby Atmos. Weitere Informationen finden Sie unter [Von der dynamischen Paketerstellung unterstützte Audiocodecs](dynamic-packaging-overview.md#audio-codecs).

Sie können nun eine Liste von Medienobjekt- oder Kontenfiltern angeben, die für Ihren Streaminglocator gelten würden. Weitere Informationen finden Sie unter [Zuordnen von Filtern mit Streaminglocator](filters-concept.md#associating-filters-with-streaming-locator).

## <a name="february-2019"></a>Februar 2019

Media Services v3 wird jetzt in nationalen Azure-Clouds unterstützt. Noch sind nicht alle Funktionen in allen Clouds verfügbar. Ausführliche Informationen finden Sie unter [Clouds und Regionen, in denen Azure Media Services v3 verfügbar ist](azure-clouds-regions.md).

Das Ereignis [Microsoft.Media.JobOutputProgress](media-services-event-schemas.md#monitoring-job-output-progress) wurde den Azure Event Grid-Schemas für Media Services hinzugefügt.

## <a name="january-2019"></a>Januar 2019

### <a name="media-encoder-standard-and-mpi-files"></a>Media Encoder Standard und MPI-Dateien 

Bei der Codierung mit Media Encoder Standard zum Erzeugen von MP4-Dateien wird eine neue MPI-Datei generiert und dem Ausgabeasset hinzugefügt. Diese MPI-Datei dient zum Verbessern der Leistung für die [dynamische Paketerstellung](dynamic-packaging-overview.md) und Streamingszenarios.

Sie sollten weder die MPI-Datei ändern oder entfernen noch beliebige Abhängigkeiten vom Vorhandensein (oder Nichtvorhandensein) einer solchen Datei in Ihren Dienst integrieren.

## <a name="december-2018"></a>Dezember 2018

Zu den Updates in der GA-Version der V3-API gehören:
       
* Die **PresentationTimeRange**-Eigenschaften sind nicht mehr für **Medienobjektfilter** und **Kontofilter** erforderlich. 
* Die Abfrageoptionen „$top“ und „$skip“ für **Aufträge** und **Transformationen** wurden entfernt, und „$orderby“ wurde hinzugefügt. Beim Hinzufügen der neuen Sortierungsfunktionalität wurde festgestellt, dass die Optionen „$top“ und „$skip“ versehentlich zuvor verfügbar gemacht wurden, obwohl sie nicht implementiert sind.
* Enumerationserweiterbarkeit wurde erneut aktiviert. Dieses Feature wurde in den Vorschauversionen des SDK aktiviert und wurde in der GA-Version versehentlich deaktiviert.
* Zwei vordefinierte Streamingrichtlinien wurden umbenannt. **SecureStreaming** ist jetzt **MultiDrmCencStreaming**. **SecureStreamingWithFairPlay** ist jetzt **Predefined_MultiDrmStreaming**.

## <a name="november-2018"></a>November 2018

Das CLI 2.0-Modul ist ab sofort für [Azure Media Services v3 (allgemein verfügbar)](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest) v2.0.50 verfügbar.

### <a name="new-commands"></a>Neue Befehle

- [az ams account](https://docs.microsoft.com/cli/azure/ams/account?view=azure-cli-latest)
- [az ams account-filter](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest)
- [az ams asset](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest)
- [az ams asset-filter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest)
- [az ams content-key-policy](https://docs.microsoft.com/cli/azure/ams/content-key-policy?view=azure-cli-latest)
- [az ams job](https://docs.microsoft.com/cli/azure/ams/job?view=azure-cli-latest)
- [az ams live-event](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)
- [az ams live-output](https://docs.microsoft.com/cli/azure/ams/live-output?view=azure-cli-latest)
- [az ams streaming-endpoint](https://docs.microsoft.com/cli/azure/ams/streaming-endpoint?view=azure-cli-latest)
- [az ams streaming-locator](https://docs.microsoft.com/cli/azure/ams/streaming-locator?view=azure-cli-latest)
- [az ams account mru:](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) Hiermit können Sie reservierte Einheiten für Medien verwalten. Weitere Informationen finden Sie unter [Skalieren von reservierten Einheiten für Medien](media-reserved-units-cli-how-to.md).

### <a name="new-features-and-breaking-changes"></a>Neue Features und Breaking Changes

#### <a name="asset-commands"></a>Assetbefehle

- Die Argumente ```--storage-account``` und ```--container``` wurden hinzugefügt.
- Standardwerte für Ablaufzeit (aktueller Zeitpunkt + 23 Std.) und Berechtigungen (Lesen) im Befehl ```az ams asset get-sas-url``` wurden hinzugefügt.

#### <a name="job-commands"></a>Auftragsbefehle

- Die Argumente ```--correlation-data``` und ```--label``` wurden hinzugefügt.
- ```--output-asset-names``` wurde in ```--output-assets``` umbenannt. Ab sofort wird eine durch Leerzeichen getrennte Ressourcenliste im Format „assetName=Bezeichnung“ akzeptiert. Assets ohne Bezeichnung können wie folgt gesendet werden: „assetName=“.

#### <a name="streaming-locator-commands"></a>streaming locator-Befehle

- Der Basisbefehl ```az ams streaming locator``` wurde durch ```az ams streaming-locator``` ersetzt.
- Die Argumente ```--streaming-locator-id``` und ```--alternative-media-id support``` wurden hinzugefügt.
- Das Argument ```--content-keys argument``` wurde aktualisiert.
- ```--content-policy-name``` wurde in ```--content-key-policy-name``` umbenannt.

#### <a name="streaming-policy-commands"></a>streaming policy-Richtlinien

- Der Basisbefehl ```az ams streaming policy``` wurde durch ```az ams streaming-policy``` ersetzt.
- Unterstützung für Verschlüsselungsparameter für ```az ams streaming-policy create``` wurde hinzugefügt.

#### <a name="transform-commands"></a>transform-Befehle

- Das Argument ```--preset-names``` wurde durch ```--preset``` ersetzt. Ab sofort kann nur noch eine einzelne Ausgabe/Voreinstellung festgelegt werden. (Wenn Sie weitere hinzufügen möchten, müssen Sie ```az ams transform output add``` ausführen.) Darüber hinaus können Sie eine benutzerdefinierte Voreinstellung für den Standard-Encoder (StandardEncoderPreset) festlegen, indem Sie den Pfad an Ihr benutzerdefiniertes JSON-Objekt übergeben.
- Für ```az ams transform output remove``` kann der zu entfernende Ausgabeindex übergeben werden.
- Die Argumente ```--relative-priority, --on-error, --audio-language and --insights-to-extract``` wurden in ```az ams transform create``` und den Befehlen ```az ams transform output add``` hinzugefügt.

## <a name="october-2018---ga"></a>Oktober 2018 – Allgemeine Verfügbarkeit (GA)

In diesem Abschnitt werden die Oktober-Updates für Azure Media Services (AMS) beschrieben.

### <a name="rest-v3-ga-release"></a>REST v3-GA-Release

Das [REST v3-GA-Release](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01) umfasst mehr APIs für den Livebetrieb, Manifestfilter für die Konto-/Ressourcenebene und DRM-Support.

#### <a name="azure-resource-management"></a>Azure-Ressourcenverwaltung 

Die Unterstützung der Azure-Ressourcenverwaltung ermöglicht eine einheitliche Verwaltung und Vorgangs-API (jetzt alles an einem Ort).

Ab dieser Version können Sie Resource Manager-Vorlagen verwenden, um Liveereignisse zu erstellen.

#### <a name="improvement-of-asset-operations"></a>Verbesserung von Medienobjektvorgängen 

Die folgenden Verbesserungen wurden eingeführt:

- Erfassen von HTTP(S)-URLs oder Azure Blob Storage-SAS-URLs
- Angeben von eigenen Containernamen für Medienobjekte 
- Einfachere Unterstützung der Ausgabe zur Erstellung von benutzerdefinierten Workflows mit Azure Functions

#### <a name="new-transform-object"></a>Neues Transform-Objekt

Mit dem neuen **Transform**-Objekt wird das Codierungsmodell vereinfacht. Es ist mit dem neuen Objekt einfach, Resource Manager-Vorlagen und -Voreinstellungen für die Codierung zu erstellen und gemeinsam zu nutzen. 

#### <a name="azure-active-directory-authentication-and-rbac"></a>Azure Active Directory-Authentifizierung und rollenbasierte Zugriffssteuerung

Mit der Azure AD-Authentifizierung und rollenbasierten Zugriffssteuerung (RBAC) werden sichere Transformationen, Liveereignisse, Richtlinien für Inhaltsschlüssel oder Medienobjekte nach Rolle oder Benutzer in Azure AD ermöglicht.

#### <a name="client-sdks"></a>Client-SDKs  

In Media Services v3 unterstützte Sprachen: .NET Core, Java, Node.js, Ruby, TypeScript, Python, Go.

#### <a name="live-encoding-updates"></a>Updates der Livecodierung

Die folgenden Updates für die Livecodierung wurden eingeführt:

- Neuer Modus mit geringer Latenz für den Livebetrieb (10 Sekunden für den End-to-End-Vorgang).
- Verbesserte RTMP-Unterstützung (höhere Stabilität und bessere Unterstützung für Quellcodierer).
- Sichere RTMPS-Erfassung.

    Bei der Erstellung eines Liveereignisses erhalten Sie jetzt vier Erfassungs-URLs. Die vier Erfassungs-URLs sind nahezu identisch und verfügen über das gleiche Streamingtoken (AppId). Nur der Portnummernteil unterscheidet sich. Zwei der URLs dienen als primäre URL und Backup-URL für RTMPS. 
- Unterstützung der 24-Stunden-Transcodierung. 
- Verbesserte Unterstützung für Werbeeinblendungen in RTMP über SCTE35.

#### <a name="improved-event-grid-support"></a>Verbesserte Event Grid-Unterstützung

Folgende Verbesserungen der Event Grid-Unterstützung wurden eingeführt:

- Azure Event Grid-Integration, um die Entwicklung mit Logic Apps und Azure Functions zu vereinfachen. 
- Abonnieren von Ereignissen für Codierung, Livekanäle und mehr.

### <a name="cmaf-support"></a>CMAF-Unterstützung

CMAF- und „cbcs“-Verschlüsselungsunterstützung für Apple HLS- (iOS 11 und höher) und MPEG-DASH-Player, die CMAF unterstützen.

### <a name="video-indexer"></a>Video Indexer

Das Video Indexer-GA-Release wurde im August angekündigt. Neue Informationen zu derzeit unterstützten Funktionen finden Sie unter [Was ist Video Indexer?](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json). 

### <a name="plans-for-changes"></a>Pläne für Änderungen

#### <a name="azure-cli-20"></a>Azure CLI 2.0
 
Das Azure CLI 2.0-Modul mit Vorgängen für alle Features (z.B. Livebetrieb, Richtlinien für Inhaltsschlüssel, Konto-/Ressourcenfilter, Streamingrichtlinien) ist in Kürze verfügbar. 

### <a name="known-issues"></a>Bekannte Probleme

Nur Kunden, die die Vorschauversion der API für Ressourcen- oder Kontofilter verwendet haben, sind vom folgenden Problem betroffen.

Wenn Sie zwischen dem 28.09. und dem 12.10. mit der Media Services v3 CLI oder den zugehörigen APIs Ressourcen- oder Kontofilter erstellt haben, müssen Sie aufgrund eines Versionskonflikts alle Ressourcen- und Kontofilter entfernen und neu erstellen. 

## <a name="may-2018---preview"></a>Mai 2018 – Vorschau

### <a name="net-sdk"></a>.NET SDK

Das .NET SDK enthält die folgenden Funktionen:

* **Transformationen** und **Aufträge** zum Codieren oder Analysieren von Medieninhalten. Beispiele finden Sie in den Tutorials zum [Streamen von Dateien](stream-files-tutorial-with-api.md) und [Analysieren](analyze-videos-tutorial-with-api.md).
* **Streaminglocators** zum Veröffentlichen und Streamen von Inhalten auf Endbenutzergeräten
* **Streamingrichtlinien** und **Inhaltsschlüssel-Richtlinien** zum Konfigurieren der Schlüsselübermittlung und des Inhaltsschutzes (DRM) bei der Inhaltsbereitstellung
* **Liveereignisse** und **Liveausgaben** zum Konfigurieren der Erfassung und Archivierung von Livestreaminginhalten
* **Medienobjekte** zum Speichern und Veröffentlichen von Medieninhalten in Azure Storage 
* **Streamingendpunkte** zum Konfigurieren und Skalieren der dynamischen Paketerstellung, der Verschlüsselung und des Streamings von Live- und On-Demand-Medieninhalten

### <a name="known-issues"></a>Bekannte Probleme

* Wenn Sie einen Auftrag übermitteln, können Sie angeben, dass Ihr Quellvideo mithilfe von HTTPS, URLs, SAS-URLs oder Pfaden zu Dateien in Azure Blob Storage erfasst wird. AMS v3 unterstützt derzeit keine segmentierte Transfercodierung über HTTPS-URLs.

## <a name="ask-questions-give-feedback-get-updates"></a>Fragen stellen, Feedback geben, Updates abrufen

Im Artikel [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services zu bekommen.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht](media-services-overview.md)
- [Updates der Media Services v3-Dokumentation](docs-release-notes.md)
- [Versionsanmerkungen zu Media Services v2](../previous/media-services-release-notes.md)
