---
title: Containerrepositorys und -images
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Zwei Tabellen, die die Containerregistrierungen, Repositorys und Imagenamen für alle Cognitive Service-Angebote darstellen.
ms.service: cognitive-services
ms.topic: include
ms.date: 01/10/2020
ms.author: dapine
ms.openlocfilehash: 7c229ea306d89f85fb37a68fc84e3e4f6770e5ad
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75866934"
---
### <a name="container-repositories-and-images"></a>Containerrepositorys und -images

Die folgenden Tabellen stellen eine Liste der verfügbaren Containerimages dar, die von Azure Cognitive Services geboten werden. Eine vollständige Liste aller verfügbaren Namen für Containerimages und ihrer verfügbaren Tags finden Sie unter [Cognitive Services-Containerimagetags](../container-image-tags.md). Zurzeit gibt es keine allgemein verfügbaren Cognitive Services-Container. Bis auf Weiteres sind Container entweder als *öffentlich, nicht geschlossen* oder *öffentliche Vorschau, geschlossen* verfügbar.

 - *Öffentlich, nicht geschlossen*: Container sind ohne Beschränkungsmechanismus öffentlich verfügbar.
 - *Öffentliche Vorschau, geschlossen*: Container sind öffentlich verfügbar, erfordern jedoch eine formelle Anforderung für den Zugriff auf die Containerregistrierung.

#### <a name="public-ungated-container-registry-mcrmicrosoftcom"></a>Öffentliche „nicht verwaltete“ Vorschau (Containerregistrierung: `mcr.microsoft.com`)

Die Microsoft-Containerregistrierung (MCR) syndikalisiert alle öffentlich verfügbaren, „nicht verwalteten“ Container für Cognitive Services. Die Container sind auch direkt über den [Docker-Hub](https://hub.docker.com/_/microsoft-azure-cognitive-services)verfügbar.

| Dienst | Container | Container Registry/Repository/Imagename |
|--|--|--|
| [LUIS](../../LUIS/luis-container-howto.md) | LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |
| [Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Schlüsselwortextraktion | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Spracherkennung | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Standpunktanalyse | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

#### <a name="public-gated-preview-container-registry-containerpreviewazurecrio"></a>Öffentliche "verwaltete" Vorschau (Containerregistrierung: `containerpreview.azurecr.io`)

Die Vorschauversion der Containerregistrierung hostet alle öffentlich verfügbaren, „verwalteten“ Container für Cognitive Services. Diese Container erfordern für den Zugriff eine formelle Anforderung über die Containerregistrierung.

| Dienst | Container | Container Registry/Repository/Imagename |
|--|--|--|
| [Anomalieerkennung](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomalieerkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector` |
| [Maschinelles Sehen](../../Computer-vision/computer-vision-how-to-install-containers.md) | Lesen | `containerpreview.azurecr.io/microsoft/cognitive-services-read` |
| [Gesichtserkennung](../../face/face-how-to-install-containers.md) | Gesicht | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Formularerkennung](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Formularerkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=stt) | Spracherkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=cstt) | Benutzerdefinierte Spracherkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=tts) | Text-zu-Sprache | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=ctts) | Benutzerdefinierte Sprachsynthese | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` |
