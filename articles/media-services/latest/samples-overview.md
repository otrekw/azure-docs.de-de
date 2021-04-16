---
title: Media Services v3-Beispiele
description: Dieser Artikel enthält eine Liste aller verfügbaren Beispiele für Media Services V3, geordnet nach Methode und SDK.  Es gibt Beispiele für .NET, Node.JS, Python und Java sowie REST mit Postman.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: overview
ms.date: 03/24/2021
ms.author: inhenkel
ms.openlocfilehash: 1d827d734c434204ff6b7ec60d27e507ae626abd
ms.sourcegitcommit: b28e9f4d34abcb6f5ccbf112206926d5434bd0da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107227686"
---
# <a name="media-services-v3-samples"></a>Media Services v3-Beispiele

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Dieser Artikel enthält eine Liste aller verfügbaren Beispiele für Media Services, geordnet nach Methode und SDK.  Es gibt Beispiele für .NET, Node.JS, Python und Java sowie REST mit Postman.

## <a name="rest-postman-collection"></a>REST Postman-Sammlung

Die [REST-Postman](https://github.com/Azure-Samples/media-services-v3-rest-postman)-Beispiele beinhalten eine Postman-Umgebung und -Sammlung, die Sie in den Postman-Client importieren können.

## <a name="samples-by-sdk"></a>Beispiele nach SDK

Auf den einzelnen Registerkarten finden Sie eine Beschreibung und Links zu den Beispielen, die Sie möglicherweise suchen.

## <a name="net"></a>[.NET](#tab/net/)

| Ordner | BESCHREIBUNG |
|-------------|-------------|
| [VideoEncoding/EncodingWithMESPredefinedPreset](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/VideoEncoding/EncodingWithMESPredefinedPreset)|Vorgehensweise zum Übermitteln eines Auftrags mithilfe einer integrierten Voreinstellung und der Eingabe einer HTTP-URL, zum Veröffentlichen des Ausgabeobjekts für das Streaming und zum Herunterladen der Ergebnisse zur Überprüfung.|
| [VideoEncoding/EncodingWithMESCustomPreset_H264](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/VideoEncoding/EncodingWithMESCustomPreset_H264)|Vorgehensweise zum Übermitteln eines Auftrags mithilfe einer benutzerdefinierten H.264-Codierungsvoreinstellung und Eingabe einer HTTP-URL, zum Veröffentlichen des Ausgabeobjekts für das Streaming und zum Herunterladen der Ergebnisse zur Überprüfung.|
| [VideoEncoding/EncodingWithMESCustomPreset_HEVC](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/VideoEncoding/EncodingWithMESCustomPreset_HEVC)|Vorgehensweise zum Übermitteln eines Auftrags mithilfe einer benutzerdefinierten HEVC-Codierungsvoreinstellung und Eingabe einer HTTP-URL, zum Veröffentlichen des Ausgabeobjekts für das Streaming und zum Herunterladen der Ergebnisse zur Überprüfung.|
| [VideoEncoding/EncodingWithMESCustomStitchTwoAssets](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/VideoEncoding/EncodingWithMESCustomStitchTwoAssets)|Vorgehensweise zum Übermitteln eines Auftrags mithilfe einer JobInputSequence zum Zusammenfügen von zwei oder mehr Objekten, die möglicherweise durch die Start- oder Endzeit abgeschnitten werden. Die sich ergebende codierte Datei ist ein einzelnes Video, in dem alle Objekte zusammengefügt sind.  Im Beispiel werden zudem das Ausgabeobjekt für das Streaming veröffentlicht und die Ergebnisse zur Überprüfung heruntergeladen.|
| [VideoEncoding/EncodingWithMESCustomPresetAndSprite](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/VideoEncoding/EncodingWithMESCustomPresetAndSprite)|Vorgehensweise zum Übermitteln eines Auftrags mithilfe einer benutzerdefinierten Voreinstellung mit Miniaturbild-Sprite und Eingabe einer HTTP-URL, zum Veröffentlichen des Ausgabeobjekts für das Streaming und zum Herunterladen der Ergebnisse zur Überprüfung.|
| [Live/LiveEventWithDVR](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/Live/LiveEventWithDVR)|Vorgehensweise zum Erstellen eines LiveEvents mit einem vollständigen Archiv bis zu 25 Stunden und einem Filter für das Objekt mit 5-Minuten-DVR-Fenster. Vorgehensweise zum Verwenden eines Filters zum Erstellen eines Locators für das Streaming.|
| [VideoAnalytics/VideoAnalyzer](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/VideoAnalytics/VideoAnalyzer)|Vorgehensweise zum Erstellen einer Videoanalysetransformation, zum Hochladen einer Videodatei in ein Eingabeobjekt, zum Einreichen eines Auftrags mit der Transformation und zum Herunterladen der Ergebnisse zur Überprüfung.|
| [AudioAnalytics/AudioAnalyzer](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/AudioAnalytics/AudioAnalyzer)|Vorgehensweise zum Erstellen einer Audioanalysetransformation, zum Hochladen einer Mediendatei in ein Eingabeobjekt, zum Übermitteln eines Auftrags mit der Transformation und zum Herunterladen der Ergebnisse zur Überprüfung.|
| [ContentProtection/BasicAESClearKey](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/ContentProtection/BasicAESClearKey)|Vorgehensweise zum Erstellen einer Transformation mit integrierter AdaptiveStreaming-Voreinstellung, zum Übermitteln eines Auftrags, zum Erstellen einer ContentKeyPolicy mithilfe eines geheimen Schlüssels, zum Zuordnen der ContentKeyPolicy zum StreamingLocator, zum Abrufen eines Tokens und zum Ausdrucken einer URL für die Wiedergabe in Azure Media Player. Wenn von einem Player ein Stream angefordert wird, verwendet Media Services den angegebenen Schlüssel, um Ihre Inhalte dynamisch mit AES-128 zu verschlüsseln, und Azure Media Player verwendet das Token zum Entschlüsseln.|
| [ContentProtection/BasicWidevine](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/ContentProtection/BasicWidevine)|Vorgehensweise zum Erstellen einer Transformation mit integrierter AdaptiveStreaming-Voreinstellung, zum Übermitteln eines Auftrags, zum Erstellen einer ContentKeyPolicy mit Widevine-Konfiguration mithilfe eines geheimen Schlüssels, zum Zuordnen der ContentKeyPolicy mit StreamingLocator, zum Abrufen eines Tokens und zum Ausdrucken einer URL für die Wiedergabe in einem Widevine-Player. Wenn ein Benutzer die mit Widevine geschützten Inhalte anfordert, fordert die Playeranwendung eine Lizenz vom Media Services-Lizenzdienst an. Wenn die Playeranwendung autorisiert wurde, gibt der Media Services-Lizenzdienst eine Lizenz für den Player aus. Eine Widevine-Lizenz enthält den Entschlüsselungsschlüssel, der vom Clientplayer zum Entschlüsseln und Streamen des Inhalts verwendet werden kann.|
| [ContentProtection/BasicPlayReady](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/ContentProtection/BasicPlayReady)|Vorgehensweise zum Erstellen einer Transformation mit integrierter AdaptiveStreaming-Voreinstellung, zum Übermitteln eines Auftrags, zum Erstellen einer ContentKeyPolicy mit PlayReady-Konfiguration mithilfe eines geheimen Schlüssels, zum Zuordnen der ContentKeyPolicy mit StreamingLocator, zum Abrufen eines Tokens und zum Ausdrucken einer URL für die Wiedergabe in einem Azure Media Player. Wenn ein Benutzer die mit PlayReady geschützten Inhalte anfordert, fordert die Playeranwendung eine Lizenz vom Media Services-Lizenzdienst an. Wenn die Playeranwendung autorisiert wurde, gibt der Media Services-Lizenzdienst eine Lizenz für den Player aus. Eine PlayReady-Lizenz enthält den Entschlüsselungsschlüssel, der vom Clientplayer zum Entschlüsseln und Streamen des Inhalts verwendet werden kann.|
| [ContentProtection/OfflinePlayReadyAndWidevine](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/ContentProtection/OfflinePlayReadyAndWidevine)|Vorgehensweise zum dynamischen Verschlüsseln von Inhalten mit PlayReady- und Widevine-DRM und zum Wiedergeben der Inhalte ohne Anforderung einer Lizenz vom Lizenzdienst. Vorgehensweise zum Erstellen einer Transformation mit integrierter AdaptiveStreaming-Voreinstellung, zum Übermitteln eines Auftrags, zum Erstellen einer ContentKeyPolicy mit offener Einschränkung und persistenter PlayReady/Widevine-Konfiguration, zum Zuordnen einer ContentKeyPolicy zu einem StreamingLocator und zum Ausdrucken einer URL für die Wiedergabe.|
| [Streaming/AssetFilters](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/Streaming/AssetFilters)|Vorgehensweise zum Erstellen einer Transformation mit integrierter AdaptiveStreaming-Voreinstellung, zum Übermitteln eines Auftrags, zum Erstellen eines Ressourcenfilters und Kontofilters, zum Zuordnen der Filter zu Streaming-Locators und zum Ausdrucken von URLs für die Wiedergabe.|
| [Streaming/StreamHLSAndDASH](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/Streaming/StreamHLSAndDASH)|Vorgehensweise zum Erstellen einer Transformation mit integrierter AdaptiveStreaming-Voreinstellung, zum Übermitteln eines Auftrags und zum Veröffentlichen eines Ausgabeobjekts für HLS- und DASH-Streaming.|
| [HighAvailabilityEncodingStreaming](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/HighAvailabilityEncodingStreaming/) | Anleitungen und bewährte Methoden für ein Produktionssystem, das bedarfsgesteuerte Codierung oder Analysen verwendet. Als Leser sollten Sie mit dem Begleitartikel [Hochverfügbarkeit mit Media Services und VOD](https://docs.microsoft.com/azure/media-services/latest/architecture-high-availability-encoding-concept) beginnen. Es gibt eine separate Lösungsdatei, die für das Beispiel [HighAvailabilityEncodingStreaming](https://github.com/Azure-Samples/media-services-v3-dotnet/blob/main/HighAvailabilityEncodingStreaming/README.md) bereitgestellt wird. |

## <a name="nodejs"></a>[Node.JS](#tab/node/)

|Ordner|BESCHREIBUNG|
|---|---|
|[HelloWorld-ListAssets](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/main/AMSv3Samples/HelloWorld-ListAssets) |Vorgehensweise zum Verbinden und Auflisten von Assets |
|[Live](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/main/AMSv3Samples/Live)| Vorgehensweise für einfaches Livestreaming. **WARNUNG**: Stellen Sie sicher, dass alle Ressourcen bereinigt sind und im Portal nicht mehr abgerechnet werden, wenn Sie Livestreaming verwenden.|
|[StreamFilesSample](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/main/AMSv3Samples/StreamFilesSample)| Vorgehensweise zum Hochladen einer lokalen Datei oder zum Codierung aus einer Quell-URL, zum Verwenden des Speicher-SDK zum Herunterladen von Inhalten und zum Streamen auf einen Player. |
|[StreamFilesWithDRMSample](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/main/AMSv3Samples/StreamFilesWithDRMSample)| Vorgehensweise zum Codieren und Streamen mithilfe von Widevine- und PlayReady-DRM |
|[VideoIndexerSample](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/main/AMSv3Samples/VideoIndexerSample)| Vorgehensweise zum Verwenden von Voreinstellungen für das Video- und Audioanalysetool, um Metadaten und Erkenntnissen aus einer Video- oder Audiodatei zu gewinnen. |

## <a name="python"></a>[Python](#tab/python)

Zurzeit gibt es ein Python-Beispiel, eine [Einfache Codierung mit Python](https://github.com/Azure-Samples/media-services-v3-python).

## <a name="java"></a>[Java](#tab/java)

|Ordner|BESCHREIBUNG|
|---|---|
|[AudioAnalytics/AudioAnalyzer/](https://github.com/Azure-Samples/media-services-v3-java/tree/master/AudioAnalytics/AudioAnalyzer)|Vorgehensweise zum Analysieren von Audiodaten in einer Mediendatei. |
|Content Protection|
|ContentProtection||
|[BasicAESClearKey](https://github.com/Azure-Samples/media-services-v3-java/tree/master/ContentProtection/BasicAESClearKey)|Vorgehensweise zum dynamischen Verschlüsseln von Inhalten mit AES-128.|
|[BasicPlayReady](https://github.com/Azure-Samples/media-services-v3-java/tree/master/ContentProtection/BasicPlayReady)|Vorgehensweise zum dynamischen Verschlüsseln von Inhalten mit PlayReady-DRM.|
|[BasicWidevine](https://github.com/Azure-Samples/media-services-v3-java/tree/master/ContentProtection/BasicWidevine)|Vorgehensweise zum dynamischen Verschlüsseln von Inhalten mit Widevine-DRM.|
|[OfflineFairPlay](https://github.com/Azure-Samples/media-services-v3-java/tree/master/ContentProtection/OfflineFairPlay)|Vorgehensweise zum dynamischen Verschlüsseln von Inhalten mit FairPlay-DRM und zum Wiedergeben der Inhalte ohne Anforderung einer Lizenz vom Lizenzdienst.|
|[OfflinePlayReadyAndWidevine](https://github.com/Azure-Samples/media-services-v3-java/tree/master/ContentProtection/OfflinePlayReadyAndWidevine)|Vorgehensweise zum dynamischen Verschlüsseln von Inhalten mit PlayReady- und Widevine-DRM und zum Wiedergeben der Inhalte ohne Anforderung einer Lizenz vom Lizenzdienst.|
|DynamicPackagingVODContent||
|[AssetFilters](https://github.com/Azure-Samples/media-services-v3-java/tree/master/DynamicPackagingVODContent/AssetFilters)|Vorgehensweise zum Filtern von Inhalten mithilfe von Objekt- und Kontofiltern.|
|[StreamHLSAndDASH](https://github.com/Azure-Samples/media-services-v3-java/tree/master/DynamicPackagingVODContent/StreamHLSAndDASH)|Vorgehensweise zum dynamischen Verpacken von VOD-Inhalten in HLS/Dash für das Streaming.|
|[LiveIngest/LiveEventWithDVR](https://github.com/Azure-Samples/media-services-v3-java/tree/master/LiveIngest/LiveEventWithDVR)|Vorgehensweise zum Erstellen Sie eines Liveereignisses mit einem vollständigen Archiv bis zu 25 Stunden und einem Filter für das Objekt mit einem 5-Minuten-DVR-Fenster und zum Erstellen eines Locators für das Streaming und Verwenden des Filters.|
|[VideoAnalytics/VideoAnalyzer](https://github.com/Azure-Samples/media-services-v3-java/tree/master/VideoAnalytics/VideoAnalyzer)|Vorgehensweise zum Erstellen Sie eines Liveereignisses mit einem vollständigen Archiv bis zu 25 Stunden und einem Filter für das Objekt mit einem 5-Minuten-DVR-Fenster und zum Erstellen eines Locators für das Streaming und Verwenden des Filters.|
|VideoEncoding||
|[EncodingWithMESCustomPreset](https://github.com/Azure-Samples/media-services-v3-java/tree/master/VideoEncoding/EncodingWithMESCustomPreset)|Vorgehensweise zum Erstellen einer benutzerdefinierten Codierungstransformation mithilfe der StandardEncoderPreset-Einstellungen.|
|[EncodingWithMESPredefinedPreset](https://github.com/Azure-Samples/media-services-v3-java/tree/master/VideoEncoding/EncodingWithMESPredefinedPreset)|Vorgehensweise zum Übermitteln eines Auftrags mithilfe einer integrierten Voreinstellung und der Eingabe einer HTTP-URL, zum Veröffentlichen des Ausgabeobjekts für das Streaming und zum Herunterladen der Ergebnisse zur Überprüfung.|

---
