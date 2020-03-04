---
title: Azure Media Services-Terminologie und -Konzepte – Azure | Microsoft-Dokumentation
description: Dieses Thema bietet eine kurze Übersicht über die Terminologie und die Konzepte von Azure Media Services und stellt Links zu weiteren Details zur Verfügung.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/18/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: e2c718b3b5ee27b5781f5f6287e0ee45fa405170
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562376"
---
# <a name="media-services-concepts"></a>Media Services-Konzepte

Dieses Thema bietet eine kurze Übersicht über die Azure Media Services-Terminologie und -Konzepte. Darüber hinaus enthält der Artikel Links zu Artikeln mit ausführlicheren Erläuterungen zu Konzepten und Funktionen von Media Services v3. 

Es empfiehlt sich, sich vor Entwicklungsbeginn mit den grundlegenden Konzepten vertraut zu machen, die in diesen Themen behandelt werden.

> [!NOTE]
> Derzeit können Sie das Azure-Portal nicht für die Verwaltung von v3-Ressourcen verwenden. Verwenden Sie die [REST-API](https://aka.ms/ams-v3-rest-ref), die [Befehlszeilenschnittstelle](https://aka.ms/ams-v3-cli-ref) oder eines der unterstützten [SDKs](media-services-apis-overview.md#sdks).

## <a name="media-services-v3-terminology"></a>Media Services v3 – Terminologie

|Begriff|BESCHREIBUNG|
|---|---|
|Liveereignis|Ein **Liveereignis** stellt eine Pipeline zum Erfassen, Transcodieren (optional) und Packen von Video- und Audiolivestreams sowie von Echtzeit-Metadaten dar.<br/><br/>Für Kunden, die von Media Services v2-APIs migrieren, ersetzt das **Liveereignis** die **Channel**-Entität in v2. Weitere Informationen finden Sie unter [Hinweise zur Migration von Media Services v2 zu v3](migrate-from-v2-to-v3.md).|
|Streamingendpunkt/Paketerstellung/Ursprung|Ein **Streamingendpunkt** stellt einen dynamischen (Just-In-Time-) Paketerstellungs- und Ursprungsdienst dar, der Ihre Live- und On-Demand-Inhalte direkt an eine Clientplayeranwendung bereitstellen kann und dabei eines der allgemeinen Streamingmedienprotokolle (HLS oder DASH) verwendet. Zudem sorgt der **Streamingendpunkt** für eine dynamische (Just-In-Time-)Verschlüsselung zu branchenführenden DRMs.<br/><br/>In der Medienstreamingbranche wird dieser Dienst häufig als **Paketerstellungs-Manager** oder **Ursprung** bezeichnet.  JITP (Just-in-time-Packager, Just-In-Time-Paketerstellungs-Manager) oder JITE (Just-in-time-Encryption, Just-In-Time-Verschlüsselung) sind weitere in der Branche häufig verwendete Begriffe für diese Funktion. 

## <a name="media-services-v3-concepts"></a>Azure Media Services v3 – Konzepte

|Konzepte|BESCHREIBUNG|Links|
|---|---|---|
|Medienobjekte und Hochladen von Inhalten|Um mit dem Verwalten, Verschlüsseln, Codieren, Analysieren und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen und Ihre digitalen Dateien in **Medienobjekte** hochladen.|[Clouduploads und Cloudspeicherung](storage-account-concept.md)<br/><br/>[Medienobjektkonzept](assets-concept.md)|
|Codieren von Inhalt|Sobald Sie Ihre hochwertigen digitalen Mediendateien in Medienobjekten hochgeladen haben, können Sie sie in Formaten codieren, die mit einer Vielzahl von Browsern und Geräten wiedergegeben werden können. <br/>Um mit Media Services v3 codieren zu können, müssen Sie **Transformationen** und **Aufträge** erstellen.|[Transformationen und Aufträge](transforms-jobs-concept.md)<br/><br/>[Codierung mit Media Services](encoding-concept.md)|
|Analysieren von Inhalten (Video Indexer)|Mit Media Services v 3 können Sie Inhalte aus Video- und Audiodateien mit Voreinstellungen von Media Services v3 extrahieren. Um Ihre Inhalte mit den Voreinstellungen von Media Services v3 zu analysieren, müssen Sie **Transformationen** und **Aufträge** erstellen.<br/><br/>Wenn Sie detailliertere Erkenntnisse möchten, verwenden Sie [Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) direkt.|[Analysieren von Video- und Audiodateien](analyzing-video-audio-files-concept.md)|
|Verpackung und Bereitstellung|Sobald Ihre Inhalte codiert sind, können Sie die Vorteile der **dynamischen Paketerstellung** nutzen. Bei Media Services ist ein **Streamingendpunkt** der dynamische Paketerstellungsdienst, mit dem Medieninhalte für Clientplayer bereitgestellt werden. Um Videos im Ausgabeobjekt für die Wiedergabe durch Clients verfügbar zu machen, müssen Sie einen **Streaminglocator** erstellen und dann die Streaming-URLs erstellen. <br/><br/>Beim Erstellen des **Streaminglocators** müssen Sie zusätzlich zum Namen des Objekts eine **Streamingrichtlinie** angeben. Mithilfe von **Streamingrichtlinien** können Sie Streamingprotokolle und Verschlüsselungsoptionen (falls zutreffend) für Ihre **Streaminglocators** definieren. Die dynamische Paketerstellung wird unabhängig davon verwendet, ob Sie für Ihre Inhalte das Live- oder On-Demand-Streaming nutzen. <br/><br/>Sie können **dynamische Manifeste** von Media Services verwenden, um nur eine bestimmte Wiedergabe oder Subclips Ihres Videos zu streamen.|[Dynamische Paketerstellung](dynamic-packaging-overview.md)<br/><br/>[Streamingendpunkte](streaming-endpoint-concept.md)<br/><br/>[Streaminglocators](streaming-locators-concept.md)<br/><br/>[Streamingrichtlinien](streaming-policy-concept.md)<br/><br/>[Dynamische Manifeste](filters-dynamic-manifest-overview.md)<br/><br/>[Filter](filters-concept.md)|
|Inhaltsschutz|Mit Media Services können Sie Ihre zu übermittelnden Live- und On-Demand-Inhalte dynamisch mit Advanced Encryption Standard (AES-128) oder/und einem der drei wichtigsten DRM-Systeme verschlüsseln: Microsoft PlayReady, Google Widevine und Apple FairPlay. Media Services bietet auch einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen (PlayReady, Widevine und FairPlay) an autorisierte Clients. <br/><br/>Wenn Sie Verschlüsselungsoptionen für Ihren Datenstrom festlegen, erstellen Sie die **Inhaltsschlüsselrichtlinie** und verknüpfen sie mit Ihrem **Streaminglocator**. Mit der **Richtlinie für den Inhaltsschlüssel** können Sie konfigurieren, wie der Inhaltsschlüssel an Endclients verteilt wird.<br/><br/> Versuchen Sie, Richtlinien immer dann wiederzuverwenden, wenn dieselben Optionen erforderlich sind.| [Richtlinien für Inhaltsschlüssel](content-key-policy-concept.md)<br/><br/>[Inhaltsschutz](content-protection-overview.md)|
|Livestreaming|Mit Media Services können Sie Ihren Kunden Liveereignisse in der Azure-Cloud anbieten. **Liveereignisse** sorgen für das Erfassen und Verarbeiten von Livevideofeeds. Wenn Sie ein **Liveereignis** erstellen, wird ein Eingangsendpunkt erstellt, mit dem Sie ein Livesignal von einem Remoteencoder senden können. Wenn der Stream an das **Liveereignis** übertragen wird, können Sie das Streamingereignis starten, indem Sie ein **Medienobjekt**, eine **Liveausgabe** und einen **Streaminglocator** erstellen. Durch die **Liveausgabe** wird der Datenstrom in das **Medienobjekt** archiviert und über den **Streamingendpunkt** für die Zuschauer verfügbar gemacht. Für ein **Liveereignis** ist einer von zwei Typen möglich: **Pass-Through** und **Livecodierung**.|[Übersicht über das Livestreaming](live-streaming-overview.md)<br/><br/>[Liveereignisse und Liveausgaben](live-events-outputs-concept.md)|
|Überwachen mit Event Grid|Zum Anzeigen des Auftragsfortschritts sollten Sie **Event Grid** verwenden. Media Services gibt auch die Live-Ereignistypen aus. Mit Event Grid können Ihre Apps Ereignisse aus praktisch allen Azure-Diensten sowie aus benutzerdefinierten Quellen überwachen und darauf reagieren. |[Behandeln von Event Grid-Ereignissen](reacting-to-media-services-events.md)<br/><br/>[Schemas](media-services-event-schemas.md)|
|Überwachen mit Azure Monitor|Überwachen von Metriken und Diagnoseprotokollen, die Ihnen zu verstehen helfen, wie sich Ihre Anwendungen mit Azure Monitor verhalten.|[Metriken und Diagnoseprotokolle](media-services-metrics-diagnostic-logs.md)<br/><br/>[Schemas für Diagnoseprotokolle](media-services-diagnostic-logs-schema.md)|
|Beteiligte Kunden|Mit dem Azure Media Player können Sie von Media Services gestreamte Medieninhalte auf einer Vielzahl von Browsern und Geräten wiedergeben. Der Azure Media Player nutzt Industriestandards wie HTML5, MSE (Media Source Extensions) und EME (Encrypted Media Extensions) für ein funktionsreiches adaptives Streaming. |[Übersicht über Azure Media Player](use-azure-media-player.md)|

## <a name="ask-questions-give-feedback-get-updates"></a>Fragen stellen, Feedback geben, Updates abrufen

Im Artikel [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services zu bekommen.

## <a name="next-steps"></a>Nächste Schritte

* [Codieren von Remotedateien und Streamen von Videos: REST](stream-files-tutorial-with-rest.md)
* [Codieren von hochgeladenen Dateien und Streamen von Videos: .NET](stream-files-tutorial-with-api.md)
* [Livestreaming: .NET](stream-live-tutorial-with-api.md)
* [Analysieren Ihres Videos: .NET](analyze-videos-tutorial-with-api.md)
* [Dynamische AES-128-Verschlüsselung: .NET](protect-with-aes128.md)
* [Dynamische Verschlüsselung mit Multi-DRM: .NET](protect-with-drm.md) 
