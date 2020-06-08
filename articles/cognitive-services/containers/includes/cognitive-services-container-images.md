---
title: Containerrepositorys und -images
services: cognitive-services
author: aahill
manager: nitinme
description: Zwei Tabellen, die die Containerregistrierungen, Repositorys und Imagenamen für alle Cognitive Service-Angebote darstellen.
ms.service: cognitive-services
ms.topic: include
ms.date: 04/24/2020
ms.author: aahi
ms.openlocfilehash: 6d47829d32ed0b7ded545c73748cda1019d94bde
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83721374"
---
### <a name="container-repositories-and-images"></a>Containerrepositorys und -images

Die folgenden Tabellen stellen eine Liste der verfügbaren Containerimages dar, die von Azure Cognitive Services geboten werden. Eine vollständige Liste aller verfügbaren Namen für Containerimages und ihrer verfügbaren Tags finden Sie unter [Cognitive Services-Containerimagetags](../container-image-tags.md). 

#### <a name="generally-available"></a>Allgemein verfügbar 

Die Microsoft-Containerregistrierung (MCR) syndiziert alle allgemein verfügbaren Container für Cognitive Services. Die Container sind auch direkt über den [Docker-Hub](https://hub.docker.com/_/microsoft-azure-cognitive-services)verfügbar.

#### <a name="luis"></a>[LUIS](#tab/luis)

| LUIS-Container | Container Registry/Repository/Imagename |
|--|--|
| LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |

Weitere Informationen finden Sie unter [Installieren und Ausführen von Docker-Containern für LUIS](../../LUIS/luis-container-howto.md).

#### <a name="text-analytics"></a>[Textanalyse](#tab/text-analytics)

| Textanalysecontainer | Container Registry/Repository/Imagename |
|--|--|
| Standpunktanalyse v3 (Englisch) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-en` |
| Standpunktanalyse v3 (Spanisch) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-es` |
| Standpunktanalyse v3 (Französisch) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-fr` |
| Standpunktanalyse v3 (Italienisch) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-it` |
| Standpunktanalyse v3 (Deutsch) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-de` |
| Standpunktanalyse v3 (Chinesisch, vereinfacht) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-zh` |
| Standpunktanalyse v3 (Chinesisch, traditionell) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-zht` |
| Standpunktanalyse v3 (Japanisch) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-ja` |
| Standpunktanalyse v3 (Portugiesisch) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-pt` |
| Standpunktanalyse v3 (Niederländisch) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-nl` |

Weitere Informationen finden Sie unter [Installieren und Ausführen von Containern für die Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md).

---

#### <a name="public-ungated-preview-container-registry-mcrmicrosoftcom"></a>Öffentliche „nicht verwaltete“ Vorschau (Containerregistrierung: `mcr.microsoft.com`)

Die folgenden Vorschaucontainer sind öffentlich verfügbar. Die Microsoft-Containerregistrierung (MCR) syndiziert alle öffentlich verfügbaren, nicht verwalteten Container für Cognitive Services. Die Container sind auch direkt über den [Docker-Hub](https://hub.docker.com/_/microsoft-azure-cognitive-services)verfügbar.

| Dienst | Container | Container Registry/Repository/Imagename |
|--|--|--|
| [Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Schlüsselwortextraktion | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Spracherkennung | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Anomalieerkennung](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomalieerkennung | `mcr.microsoft.com/azure-cognitive-services/anomaly-detector` |

#### <a name="public-gated-preview-container-registry-containerpreviewazurecrio"></a>Öffentliche „verwaltete“ Vorschau (Containerregistrierung: `containerpreview.azurecr.io`)

Die folgenden Container für die verwaltete Vorschau werden in der Registrierung der Containervorschau gehostet und setzen eine Zugriffsbewerbung voraus. Weitere Informationen zu Containern finden Sie in den folgenden Artikeln.

| Dienst | Container | Container Registry/Repository/Imagename |
|--|--|--|
| [Maschinelles Sehen](../../Computer-vision/computer-vision-how-to-install-containers.md) | Lesen | `containerpreview.azurecr.io/microsoft/cognitive-services-read` |
| [Gesichtserkennung](../../face/face-how-to-install-containers.md) | Gesicht | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Formularerkennung](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Formularerkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=stt) | Spracherkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=cstt) | Benutzerdefinierte Spracherkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=tts) | Text-zu-Sprache | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=ctts) | Benutzerdefinierte Sprachsynthese | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` |
