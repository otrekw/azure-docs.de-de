---
title: Übersicht über Language Understanding (LUIS)
description: Language Understanding (LUIS) ist ein cloudbasierter API-Dienst, der maschinelles Lernen auf Konversationstext in natürlicher Sprache anwendet, um die Bedeutung vorherzusagen und Informationen zu extrahieren.
keywords: Azure, künstliche Intelligenz, KI, Verarbeitung natürlicher Sprache, NLP, Verstehen natürlicher Sprache, NLU, LUIS, Konversations-KI, KI Chatbot, NLP KI, Azure LUIS
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: overview
ms.date: 04/13/2021
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: f46586b3f120cf191d88b7de9cf8686ca9b16cca
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107503767"
---
# <a name="what-is-language-understanding-luis"></a>Worum handelt es sich bei Language Understanding (LUIS)?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Language Understanding (LUIS) ist ein cloudbasierter Konversations-KI-Dienst, der benutzerdefinierte Machine Learning-Intelligenz auf natürliche Konversationssprachtexte eines Benutzers anwendet, um die allgemeine Bedeutung vorherzusagen sowie relevante und detaillierte Informationen abzurufen. LUIS bietet Zugriff über das [benutzerdefinierte Portal](https://www.luis.ai), über [APIs][endpoint-apis] und über [SDK-Clientbibliotheken](client-libraries-rest-api.md).

Wenn Sie LUIS zum ersten Mal verwenden, führen Sie die Schritte zum [Anmelden beim LUIS-Portal](sign-in-luis-portal.md "Anmelden beim LUIS-Portal") aus. Für den Einstieg können Sie [LUIS-Apps mit vordefinierter Domäne](luis-get-started-create-app.md) ausprobieren oder [Ihre eigene App erstellen](get-started-portal-build-app.md).

Diese Dokumentation enthält die folgenden Arten von Artikeln:  

* [**Schnellstarts**](luis-get-started-create-app.md) sind Anleitungen zu den ersten Schritten, die Sie durch das Senden von Anforderungen an den Dienst führen.  
* [**Schrittanleitungen**](luis-how-to-start-new-app.md) enthalten Anweisungen zur spezifischeren oder individuelleren Verwendung des Diensts.  
* Die Artikel zu [**Konzepten**](artificial-intelligence.md) enthalten ausführliche Erläuterungen der Dienstfunktionen und -features.  
* [**Tutorials**](tutorial-intents-only.md) sind ausführlichere Leitfäden, in denen die Verwendung des Diensts als Komponente in umfassenderen Unternehmenslösungen veranschaulicht wird.  

## <a name="what-does-luis-offer"></a>Das Angebot von LUIS 

* **Einfachheit:** Dank LUIS benötigen Sie keine interne KI-Erfahrung und keinerlei Machine Learning-Vorkenntnisse. Mit einigen wenigen Klicks können Sie Ihre eigene Anwendung mit Konversations-KI erstellen. Zur Erstellung Ihrer benutzerdefinierten Anwendung können Sie eine unserer [Schnellstartanleitungen](get-started-portal-build-app.md) durchlaufen oder eine unserer [Apps mit vordefinierter Domäne](luis-get-started-create-app.md) verwenden.
* **Sicherheit, Datenschutz und Compliance:** LUIS basiert auf der Azure-Infrastruktur und bietet Sicherheit, Datenschutz und Compliance auf Unternehmensniveau. Ihre Daten gehören Ihnen und können jederzeit von Ihnen gelöscht werden. Im gespeicherten Zustand sind Ihre Daten verschlüsselt. Weitere Informationen dazu finden Sie [hier](https://azure.microsoft.com/support/legal/cognitive-services-compliance-and-privacy).
* **Integration:** Ihre LUIS-App lässt sich problemlos in andere Microsoft-Dienste wie [Microsoft Bot Framework](https://docs.microsoft.com/composer/tutorial/tutorial-luis), [QnA Maker](../QnAMaker/choose-natural-language-processing-service.md) und [Speech](../Speech-Service/quickstarts/intent-recognition.md) integrieren.


## <a name="luis-scenarios"></a>LUIS-Szenarien
* [Erstellen eines für Unternehmen konzipierten interaktiven Bots:](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/conversational-bot) Diese Referenzarchitektur beschreibt, wie Sie mit Azure Bot Framework einen interaktiven Bot (Chatbot) auf Unternehmensniveau erstellen.
* [Kommerzieller Chatbot für Kundendienst:](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/commerce-chatbot) Wenn Entwickler den Azure Bot Service und den Language Understanding-Dienst gemeinsam nutzen, können sie Konversationsschnittstellen für verschiedene Szenarien wie Bankgeschäfte, Reisen und Unterhaltung erstellen.
* [Steuern von IoT-Geräten mithilfe eines Sprachassistenten:](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/iot-controlling-devices-with-voice-assistant) Erstellen Sie nahtlose Konversationsschnittstellen mit allen Ihren internetfähigen Geräten – von Ihrem vernetzten Fernsehgerät oder Kühlschrank bis hin zu Geräten in einem vernetzten Kraftwerk.


## <a name="application-development-life-cycle"></a>Anwendungsentwicklungszyklus

![LUIS-App-Entwicklungszyklus](./media/luis-overview/luis-dev-lifecycle.png "LUIS-Anwendungsentwicklungszyklus")

-   **Planen:** Identifizieren Sie die möglichen Verwendungsszenarien für Ihre Anwendung. Definieren Sie die Aktionen und relevanten Informationen, die berücksichtigt werden müssen.
-   **Erstellen:** Verwenden Sie Ihre Erstellungsressource, um Ihre App zu entwickeln. Definieren Sie zunächst [Absichten](luis-concept-intent.md) und [Entitäten](luis-concept-entity-types.md). Fügen Sie dann [Trainingsäußerungen](luis-concept-utterance.md) für die einzelnen Absichten hinzu. 
-   **Testen und Verbessern:** Testen Sie Ihr Modell mit anderen Äußerungen, um ein Gefühl für das Verhalten der App zu bekommen, und entscheiden Sie, ob Verbesserungsbedarf besteht. Bewährte Methoden zur Verbesserung Ihrer Anwendung finden Sie [hier](luis-concept-best-practices.md). 
-   **Veröffentlichen:** Stellen Sie Ihre App für Vorhersagen bereit, und fragen Sie den Endpunkt mithilfe Ihrer Vorhersageressource ab. Weitere Informationen zu Erstellungs- und Vorhersageressourcen finden Sie [hier](luis-how-to-azure-subscription.md#luis-resources). 
-   **Verbinden:** Stellen Sie eine Verbindung mit anderen Diensten wie [Microsoft Bot Framework](https://docs.microsoft.com/composer/tutorial/tutorial-luis), [QnA Maker](../QnAMaker/choose-natural-language-processing-service.md) und [Speech](../Speech-Service/quickstarts/intent-recognition.md) her. 
-   **Optimieren:** [Überprüfen Sie Endpunktäußerungen](luis-concept-review-endpoint-utterances.md), um Ihre Anwendung mit Beispielen aus der Praxis zu verbessern.

Weitere Informationen zur Planung und Erstellung Ihrer Anwendung finden Sie [hier](luis-how-plan-your-app.md).

## <a name="next-steps"></a>Nächste Schritte

* [Neuerungen](whats-new.md "Neues") beim Dienst und bei der Dokumentation
* [Erstellen einer LUIS-App](tutorial-intents-only.md)
* [API-Referenz][endpoint-apis]
* [bewährten Methoden](luis-concept-best-practices.md)
* [Entwicklerressourcen](developer-reference-resource.md "Entwicklerressourcen") für LUIS.
* [Planen Ihrer App](luis-how-plan-your-app.md "Planen der App") mit [Absichten](luis-concept-intent.md "Absichten") und [Entitäten](luis-concept-entity-types.md "entities")

[bot-framework]: /bot-framework/
[flow]: /connectors/luis/
[authoring-apis]: https://go.microsoft.com/fwlink/?linkid=2092087
[endpoint-apis]: https://go.microsoft.com/fwlink/?linkid=2092356
[qnamaker]: https://qnamaker.ai/
