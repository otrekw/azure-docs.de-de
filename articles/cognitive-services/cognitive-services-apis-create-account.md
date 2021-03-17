---
title: Erstellen einer Cognitive Services-Ressource im Azure-Portal
titleSuffix: Azure Cognitive Services
description: Beginnen Sie mit Azure Cognitive Services, indem Sie im Azure-Portal eine Ressource erstellen und abonnieren.
services: cognitive-services
author: aahill
manager: nitinme
keywords: Cognitive Services, kognitive Intelligenz, kognitive Lösungen, KI-Dienste
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 03/15/2021
ms.author: aahi
ms.openlocfilehash: 69c83e9172a8369b7ff31116ee4db74fc33d86bb
ms.sourcegitcommit: 66ce33826d77416dc2e4ba5447eeb387705a6ae5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2021
ms.locfileid: "103472127"
---
# <a name="quickstart-create-a-cognitive-services-resource-using-the-azure-portal"></a>Schnellstart: Erstellen einer Cognitive Services-Ressource mithilfe des Azure-Portals

Verwenden Sie diese Schnellstartanleitung, um mit der Verwendung von Azure Cognitive Services zu beginnen. Nachdem Sie im Azure-Portal eine Cognitive Service-Ressource erstellt haben, erhalten Sie einen Endpunkt und einen Schlüssel zum Authentifizieren Ihrer Anwendungen.

Azure Cognitive Services sind cloudbasierte Dienste mit REST-APIs und Clientbibliothek-SDKs, die Entwickler bei der Erstellung intelligenter Anwendungen unterstützen, ohne dass direkte KI- oder Data Science-Fähigkeiten oder -Kenntnisse erforderlich sind. Mit Cognitive Services können Entwickler ganz einfach kognitive Features in ihre Anwendungen integrieren – mit kognitiven Lösungen, die sehen, hören, sprechen, verstehen und sogar schlussfolgern können.

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Ein gültiges Azure-Abonnement: [Erstellen Sie ein kostenloses Abonnement](https://azure.microsoft.com/free/cognitive-services/).

## <a name="create-a-new-azure-cognitive-services-resource"></a>Erstellen einer neuen Azure Cognitive Services-Ressource

1. Dient zum Erstellen einer Ressource.

### <a name="multi-service-resource"></a>[Ressource für mehrere Dienste](#tab/multiservice)

Die Ressource für mehrere Dienste wird im Portal **Cognitive Services** benannt. [Erstellen Sie eine Cognitive Services-Ressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne).

Zu diesem Zeitpunkt ermöglicht die Ressource für mehrere Dienste den Zugriff auf die folgenden Cognitive Services:

* Maschinelles Sehen
* Content Moderator
* Gesicht
* Language Understanding (LUIS)
* Textanalyse
* Übersetzer

### <a name="single-service-resource"></a>[Ressource für einen einzelnen Dienst](#tab/singleservice)

Verwenden Sie die folgenden Links, um eine Ressource für die verfügbaren Cognitive Services zu erstellen:

| Bildanalyse                      | Spracheingabe                  | Sprache                          | Entscheidung             |
|-----------------------------|-------------------------|-----------------------------------|----------------------|
| [Maschinelles Sehen](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision)         | [Speech-Dienste](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices)     | [Plastischer Reader](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesImmersiveReader)              | [Anomalieerkennung](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) | 
| [Custom Vision Service](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesCustomVision) | [Sprechererkennung](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeakerRecognition) | [Language Understanding (LUIS)](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) | [Content Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) | 
| [Gesichtserkennung](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace)                    |                         | [QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker)                     | [Personalisierung](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)     |
| [Freihanderkennung](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesInkRecognizer)        |                         | [Textanalyse](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)                |  [Metrics Advisor](https://go.microsoft.com/fwlink/?linkid=2142156)                    |

---

2. Geben Sie auf der Seite **Erstellen** die folgenden Informationen ein:
<!-- markdownlint-disable MD024 -->

### <a name="multi-service-resource"></a>[Ressource für mehrere Dienste](#tab/multiservice)

|Projektdetails| BESCHREIBUNG   |
|--|--|
| **Abonnement** | Wählen Sie eines Ihrer verfügbaren Azure-Abonnements aus. |
| **Ressourcengruppe** | Die Azure-Ressourcengruppe, die Ihre Cognitive Services-Ressource enthält. Sie können eine neue Gruppe erstellen oder sie einer bereits bestehenden Gruppe hinzufügen. |
| **Region** | Der Speicherort Ihrer Cognitive Services-Instanz. Verschiedene Speicherorte können Wartezeiten verursachen, haben aber keinen Einfluss auf die Laufzeitverfügbarkeit Ihrer Ressource. |
| **Name** | Ein beschreibender Name für Ihre Cognitive Services-Ressource. Beispiel: *MyCognitiveServicesResource*. |
| **Preisstufe** | Die Kosten für Ihr Cognitive Services-Konto hängen von den ausgewählten Optionen und Ihrer Nutzung ab. Weitere Informationen finden Sie unter API-[Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).

![Bildschirm „Ressourcenerstellung“ für mehrere Dienste](media/cognitive-services-apis-create-account/resource_create_screen-multi.png)

Klicken Sie auf **Erstellen**.

### <a name="single-service-resource"></a>[Ressource für einen einzelnen Dienst](#tab/singleservice)

|Projektdetails| BESCHREIBUNG   |
|--|--|
| **Abonnement** | Wählen Sie eines Ihrer verfügbaren Azure-Abonnements aus. |
| **Ressourcengruppe** | Die Azure-Ressourcengruppe, die Ihre Cognitive Services-Ressource enthält. Sie können eine neue Gruppe erstellen oder sie einer bereits bestehenden Gruppe hinzufügen. |
| **Region** | Der Speicherort Ihrer Cognitive Services-Instanz. Verschiedene Speicherorte können Wartezeiten verursachen, haben aber keinen Einfluss auf die Laufzeitverfügbarkeit Ihrer Ressource. |
| **Name** | Ein beschreibender Name für Ihre Cognitive Services-Ressource. Beispiel: *MyCognitiveServicesResource*. |
| **Preisstufe** | Die Kosten für Ihr Cognitive Services-Konto hängen von den ausgewählten Optionen und Ihrer Nutzung ab. Weitere Informationen finden Sie unter API-[Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).

![Bildschirm „Ressourcenerstellung“ für einen einzelnen Dienst](media/cognitive-services-apis-create-account/resource_create_screen.png)

Klicken Sie auf **Erstellen**.

---

[!INCLUDE [Register Azure resource for subscription](./includes/register-resource-subscription.md)]

## <a name="get-the-keys-for-your-resource"></a>Abrufen der Schlüssel für die Ressource

1. Klicken Sie nach erfolgreicher Ressourcenbereitstellung unter **Nächste Schritte** auf **Zu Ressource** wechseln.

    ![Suchen nach Cognitive Services](media/cognitive-services-apis-create-account/resource-next-steps.png)

2. Von dem Schnellstartbereich, der geöffnet wird, können Sie auf Ihren Schlüssel und Endpunkt zugreifen.

    ![Schlüssel und Endpunkt abrufen](media/cognitive-services-apis-create-account/get-cog-serv-keys.png)

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen in der Gruppe enthaltenen Ressourcen gelöscht.

1. Erweitern Sie im Azure-Portal das Menü auf der linken Seite, um das Menü mit den Diensten zu öffnen, und klicken Sie auf **Ressourcengruppen**, um die Liste mit Ihren Ressourcengruppen anzuzeigen.
2. Suchen Sie die Ressourcengruppe, die die zu löschende Ressource enthält.
3. Klicken Sie mit der rechten Maustaste auf die Ressourcengruppenauflistung. Klicken Sie auf **Ressourcengruppe löschen**, und bestätigen Sie den Vorgang.

## <a name="see-also"></a>Weitere Informationen

* [Authentifizieren von Anforderungen an Azure Cognitive Services](authentication.md)
* [Was ist Azure Cognitive Services?](./what-are-cognitive-services.md)
* [Schnellstart: Erstellen einer Azure Cognitive Services-Ressource mit der Clientbibliothek für die Azure-Verwaltung](.\cognitive-services-apis-create-account-client-library.md)
* [Unterstützung für natürliche Sprachen in Azure Cognitive Services](language-support.md)
* [Containerunterstützung in Azure Cognitive Services](cognitive-services-container-support.md)
