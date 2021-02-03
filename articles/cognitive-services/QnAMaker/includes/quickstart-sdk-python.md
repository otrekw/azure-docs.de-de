---
title: 'Schnellstart: QnA Maker-Clientbibliothek für Python'
description: In dieser Schnellstartanleitung werden die ersten Schritte mit der QnA Maker-Clientbibliothek für Python gezeigt.
ms.topic: include
ms.date: 06/18/2020
ms.openlocfilehash: cf3afcb9575f09b8c8d7b0b272dd738936756801
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98948674"
---
# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

Verwenden Sie die QnA Maker-Clientbibliothek für Python für folgende Zwecke:

* Erstellen einer Wissensdatenbank
* Aktualisieren einer Wissensdatenbank
* Veröffentlichen einer Wissensdatenbank
* Abrufen von Endpunktschlüsseln der Vorhersage-Runtime
* Warten auf Aufgaben mit langer Ausführungsdauer
* Herunterladen einer Wissensdatenbank
* Abrufen einer Antwort aus einer Wissensdatenbank
* Löschen einer Wissensdatenbank

[Referenzdokumentation](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-knowledge-qnamaker) | [Paket (PyPi)](https://pypi.org/project/azure-cognitiveservices-knowledge-qnamaker/0.2.0/) | [Python-Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/QnAMaker/sdk/quickstart.py)

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

Verwenden Sie die QnA Maker-Clientbibliothek für Python für folgende Zwecke:

* Erstellen einer Wissensdatenbank
* Aktualisieren einer Wissensdatenbank
* Veröffentlichen einer Wissensdatenbank
* Warten auf Aufgaben mit langer Ausführungsdauer
* Herunterladen einer Wissensdatenbank
* Abrufen einer Antwort aus einer Wissensdatenbank
* Löschen einer Wissensdatenbank

[Referenzdokumentation](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-knowledge-qnamaker) | [Paket (PyPi)](https://pypi.org/project/azure-cognitiveservices-knowledge-qnamaker/) | [Python-Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/QnAMaker/sdk/preview-sdk/quickstart.py)

---

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* [Python 3.x](https://www.python.org/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie im Azure-Portal eine [QnA Maker-Ressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker), um Ihren Erstellungsschlüssel und Endpunkt zu erhalten. Wählen Sie nach Abschluss der Bereitstellung **Zu Ressource wechseln** aus.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der QnA Maker-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* [Python 3.x](https://www.python.org/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie im Azure-Portal eine [QnA Maker-Ressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker), um Ihren Erstellungsschlüssel und Endpunkt zu erhalten.
    * HINWEIS: Das Kontrollkästchen **Verwaltet** muss aktiviert sein.
    * Wählen Sie nach der Bereitstellung der QnA Maker-Ressource **Zu Ressource wechseln** aus. Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der QnA Maker-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

---

## <a name="setting-up"></a>Einrichten

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

Nach der Installation von Python, können Sie die Clientbibliothek mit Folgendem installieren:

```console
pip install azure-cognitiveservices-knowledge-qnamaker==0.2.0
```

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

Nach der Installation von Python, können Sie die Clientbibliothek mit Folgendem installieren:

```console
pip install azure-cognitiveservices-knowledge-qnamaker
```

---

### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Erstellen Sie eine neue Python-Datei mit dem Namen `quickstart-file.py`, und importieren Sie die folgenden Bibliotheken:

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

[!code-python[Dependencies](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=Dependencies)]

# <a name="qna-maker-managed-preview-release"></a>[Mit QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

[!code-python[Dependencies](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/preview-sdk/quickstart.py?name=Dependencies)]

---

Erstellen Sie Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

> [!IMPORTANT]
> Navigieren Sie zum Azure-Portal, und suchen Sie den Schlüssel und den Endpunkt für die unter „Voraussetzungen“ erstellte QnA Maker-Ressource. Sie befinden sich auf der Seite mit dem **Schlüssel und Endpunkt** der Ressource unter **Ressourcenverwaltung**.

- Erstellen Sie die Umgebungsvariablen „QNA_MAKER_SUBSCRIPTION_KEY“, „QNA_MAKER_ENDPOINT“ und „QNA_MAKER_RUNTIME_ENDPOINT“, um diese Werte zu speichern.
- Der Wert von „QNA_MAKER_ENDPOINT“ hat das Format `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`. 
- Der Wert von „QNA_MAKER_RUNTIME_ENDPOINT“ hat das Format `https://YOUR-RESOURCE-NAME.azurewebsites.net`.
- In der Produktionsumgebung sollten Sie eine sichere Methode zum Speichern Ihrer Anmeldeinformationen sowie zum Zugriff darauf verwenden. Beispielsweise bietet der [Azure-Schlüsseltresor](../../../key-vault/general/overview.md) sichere Aufbewahrung von Schlüsseln.

[!code-python[Resource variables](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=Resourcevariables)]

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

> [!IMPORTANT]
> Navigieren Sie zum Azure-Portal, und suchen Sie den Schlüssel und den Endpunkt für die unter „Voraussetzungen“ erstellte QnA Maker-Ressource. Sie befinden sich auf der Seite mit dem **Schlüssel und Endpunkt** der Ressource unter **Ressourcenverwaltung**.

- Erstellen Sie die Umgebungsvariablen „QNA_MAKER_SUBSCRIPTION_KEY“ und „QNA_MAKER_ENDPOINT“, um diese Werte zu speichern.
- Der Wert von „QNA_MAKER_ENDPOINT“ hat das Format `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`. 
- In der Produktionsumgebung sollten Sie eine sichere Methode zum Speichern Ihrer Anmeldeinformationen sowie zum Zugriff darauf verwenden. Beispielsweise bietet der [Azure-Schlüsseltresor](../../../key-vault/general/overview.md) sichere Aufbewahrung von Schlüsseln.

[!code-python[Resource variables](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/preview-sdk/quickstart.py?name=Resourcevariables)]

---

## <a name="object-models"></a>Objektmodelle

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

Von [QnA Maker](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker) werden zwei unterschiedliche Objektmodelle verwendet:
* **[QnAMakerClient](#qnamakerclient-object-model)** ist das Objekt zum Erstellen, Verwalten, Veröffentlichen und Herunterladen der Wissensdatenbank.
* **[QnAMakerRuntime](#qnamakerruntimeclient-object-model)** ist das Objekt, das für Abfragen der Wissensdatenbank mit der GenerateAnswer-API und zum Senden neuer vorgeschlagener Fragen mithilfe der Train-API (im Rahmen des [aktiven Lernens](../concepts/active-learning-suggestions.md)) verwendet wird.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

Von [QnA Maker](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker) wird das folgende Objektmodell verwendet:
* **[QnAMakerClient](#qnamakerclient-object-model)** ist das Objekt zum Erstellen, Verwalten, Veröffentlichen, Herunterladen und Abfragen der Wissensdatenbank.

---

[!INCLUDE [Get KBinformation](./quickstart-sdk-cognitive-model.md)]

### <a name="qnamakerclient-object-model"></a>QnAMakerClient-Objektmodell

Der erstellende QnA Maker-Client ist ein [QnAMakerClient](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.qn_amaker_client.qnamakerclient)-Objekt, das mithilfe der „Microsoft.Rest.ServiceClientCredentials“-Klasse, die Ihren Schlüssel enthält, bei Azure authentifiziert wird.

Nachdem der Client erstellt wurde, verwenden Sie die [Knowledge Base](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebase_operations.knowledgebaseoperations)-Eigenschaft, um Ihre Knowledge Base zu erstellen, zu verwalten und zu veröffentlichen.

Verwalten Sie Ihre Knowledge Base durch Senden eines JSON-Objekts. Bei sofortigen Vorgängen gibt eine Methode in der Regel ein JSON-Objekt zurück, das den Status angibt. Bei zeitintensiven Vorgängen ist die Antwort die Vorgangs-ID. Rufen Sie die [operations.get_details](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebase_operations.knowledgebaseoperations#get-details-kb-id--custom-headers-none--raw-false----operation-config-)-Methode mit der Vorgangs-ID auf, um den [Status der Anforderung](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.operationstatetype) zu bestimmen.

### <a name="qnamakerruntimeclient-object-model"></a>QnAMakerRuntimeClient-Objektmodell

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

Der QnA Maker-Vorhersageclient ist ein `QnAMakerRuntimeClient`-Objekt, das sich bei Azure mithilfe von Microsoft.Rest.ServiceClientCredentials authentifiziert, die Ihren Schlüssel für die Vorhersage-Runtime enthalten, der nach dem Veröffentlichen der Wissensdatenbank vom Aufruf [client.EndpointKeysOperations.get_keys](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.endpoint_keys_operations.endpointkeysoperations#get-keys-custom-headers-none--raw-false----operation-config-) des erstellenden Clients zurückgegeben wurde.

Mithilfe der Methode `generate_answer` können Sie eine Antwort aus der Abfragelaufzeit abrufen.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

Für eine von QnA Maker verwaltete Ressource muss das Objekt „QnAMakerRuntimeClient“ nicht verwendet werden. Stattdessen kann [generate_answer](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebase_operations.knowledgebaseoperations#generate-answer-kb-id--generate-answer-payload--custom-headers-none--raw-false----operation-config-) direkt für das Objekt [QnAMakerClient](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.qn_amaker_client.qnamakerclient) aufgerufen werden.

---

## <a name="authenticate-the-client-for-authoring-the-knowledge-base"></a>Authentifizieren des Clients zum Erstellen der Wissensdatenbank

Instanziieren Sie einen Client mit Ihrem Endpunkt und Schlüssel. Erstellen Sie ein CognitiveServicesCredentials-Objekt mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein [QnAMakerClient](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.qn_amaker_client.qnamakerclient)-Objekt zu erstellen.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

[!code-python[Authorization to resource key](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=AuthorizationAuthor)]

# <a name="qna-maker-managed-preview-release"></a>[Mit QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

[!code-python[Authorization to resource key](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/preview-sdk/quickstart.py?name=AuthorizationAuthor)]

---

## <a name="create-a-knowledge-base"></a>Erstellen einer Wissensdatenbank

Verwenden Sie das Clientobjekt, um ein Objekt vom Typ [KnowledgebaseOperations](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations) abzurufen.

Eine Wissensdatenbank speichert Frage- und Antwortpaare für das [CreateKbDTO](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.createkbdto)-Objekt aus drei Quellen:

* Für **redaktionellen Inhalt** verwenden Sie das [QnADTO](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.qnadto)-Objekt.
    * Verwenden Sie für Metadaten und Folgeaufforderungen den redaktionellen Kontext, da diese Daten auf der Ebene der einzelnen QnA-Paare hinzugefügt werden.
* Für **Dateien** verwenden Sie das [FileDTO](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.filedto)-Objekt. Dieses Objekt enthält den Dateinamen sowie die öffentliche URL für den Zugriff auf die Datei.
* Verwenden Sie für **URLs** eine Liste von Zeichenfolgen zur Darstellung von öffentlich verfügbaren URLs.

Rufen Sie die [create](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations#create-create-kb-payload--custom-headers-none--raw-false----operation-config-)-Methode auf, und übergeben Sie dann die zurückgegebene Vorgangs-ID an die [Operations.getDetails](#get-status-of-an-operation)-Methode, um den Status abzurufen.

In der letzten Zeile des folgenden Codes wird die Knowledge Base-ID aus der Antwort von „MonitorOperation“ zurückgegeben.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

[!code-python[Create knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=CreateKBMethod)]

# <a name="qna-maker-managed-preview-release"></a>[Mit QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

[!code-python[Create knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/preview-sdk/quickstart.py?name=CreateKBMethod)]

---

Nehmen Sie zum erfolgreichen Erstellen einer Wissensdatenbank unbedingt die Funktion [`_monitor_operation`](#get-status-of-an-operation) mit auf, auf die im Code oben verwiesen wird.

## <a name="update-a-knowledge-base"></a>Aktualisieren einer Wissensdatenbank

Sie können eine Wissensdatenbank aktualisieren, indem Sie die Wissensdatenbank-ID und ein [UpdateKbOperationDTO](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdto)-Element, das [add](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoadd)-, [update](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoupdate)- und [delete](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtodelete)-DTO-Objekte enthält, als Eingabe an die [update](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebase_operations.knowledgebaseoperations)-Methode übergeben. Verwenden Sie die [Operation.getDetail](#get-status-of-an-operation) -Methode, um zu bestimmen, ob das Update erfolgreich war.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

[!code-python[Update a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=UpdateKBMethod)]

# <a name="qna-maker-managed-preview-release"></a>[Mit QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

[!code-python[Update a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/preview-sdk/quickstart.py?name=UpdateKBMethod)]

---

Nehmen Sie zum erfolgreichen Aktualisieren einer Wissensdatenbank unbedingt die Funktion [`_monitor_operation`](#get-status-of-an-operation) mit auf, auf die im Code oben verwiesen wird.

## <a name="download-a-knowledge-base"></a>Herunterladen einer Knowledge Base

Verwenden Sie die [download](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations)-Methode, um die Datenbank als Liste von [QnADocumentsDTO](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.qnadocumentsdto) herunterzuladen. Dies entspricht _nicht_ dem Export des QnA Maker-Portals von der Seite **Einstellungen**, da das Ergebnis dieser Methode keine TSV-Datei ist.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

[!code-python[Download a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=DownloadKB)]

# <a name="qna-maker-managed-preview-release"></a>[Mit QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

[!code-python[Download a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/preview-sdk/quickstart.py?name=DownloadKB)]

---

## <a name="publish-a-knowledge-base"></a>Veröffentlichen einer Wissensdatenbank

Veröffentlichen Sie die Wissensdatenbank mit der [publish](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations#publish-kb-id--custom-headers-none--raw-false----operation-config-)-Methode. Diese übernimmt das aktuell gespeicherte und trainierte Modell, auf das von der Knowledge Base-ID verwiesen wird, und veröffentlicht dieses an einem Endpunkt.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

[!code-python[Publish a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=PublishKB)]

# <a name="qna-maker-managed-preview-release"></a>[Mit QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

[!code-python[Publish a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/preview-sdk/quickstart.py?name=PublishKB)]

---

## <a name="query-a-knowledge-base"></a>Abfragen einer Knowledge Base

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

### <a name="get-query-runtime-key"></a>Abrufen des Abfrage-Runtimeschlüssels

Nach dem Veröffentlichen einer Wissensdatenbank benötigen Sie den Abfrage-Runtimeschlüssel, um die Runtime abzufragen. Dabei handelt es sich nicht um den Schlüssel, der zum Erstellen des ursprünglichen Clientobjekts verwendet wurde.

Verwenden Sie die Methode [EndpointKeysOperations.get_keys](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.endpointkeysoperations#get-keys-custom-headers-none--raw-false----operation-config-), um die Klasse [EndpointKeysDTO](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.endpointkeysdto) abzurufen.

Verwenden Sie eine der im Objekt zurückgegeben Schlüsseleigenschaften, um die Wissensdatenbank abzufragen.

[!code-python[Get query runtime key](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=GetQueryEndpointKey)]

### <a name="authenticate-the-runtime-for-generating-an-answer"></a>Authentifizieren der Runtime zum Erstellen einer Antwort

Erstellen Sie ein [QnAMakerRuntimeClient](/javascript/api/@azure/cognitiveservices-qnamaker-runtime/qnamakerruntimeclient)-Element, um die Wissensdatenbank abzufragen und eine Antwort zu generieren oder mit aktivem Lernen zu trainieren.

[!code-python[Authenticate the runtime](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=AuthorizationQuery)]

Verwenden Sie den QnAMakerRuntimeClient, um eine Antwort aus der Wissensdatenbank abzurufen oder neue Fragenvorschläge für [aktives Lernen](../concepts/active-learning-suggestions.md) an sie zu senden.

### <a name="generate-an-answer-from-the-knowledge-base"></a>Generieren einer Antwort auf der Grundlage der Wissensdatenbank

Generieren Sie mithilfe der Methode „QnAMakerRuntimeClient.runtime.generate_answer“ eine Antwort auf der Grundlage einer veröffentlichten Wissensdatenbank. Diese Methode akzeptiert die Wissensdatenbank-ID und die [QueryDTO](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.querydto)-Klasse. Greifen Sie auf weitere QueryDTO-Eigenschaften zu (etwa „Top“ und „Context“), und verwenden Sie sie in Ihrem Chatbot.

[!code-python[Generate an answer from a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=GenerateAnswer)]

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

### <a name="generate-an-answer-from-the-knowledge-base"></a>Generieren einer Antwort auf der Grundlage der Wissensdatenbank

Generieren Sie mithilfe der Methode [generate_answer](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations#generate-answer-kb-id--generate-answer-payload--custom-headers-none--raw-false----operation-config-) eine Antwort auf der Grundlage einer veröffentlichten Wissensdatenbank. Diese Methode akzeptiert die Wissensdatenbank-ID und die [QueryDTO](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.querydto)-Klasse. Greifen Sie auf weitere QueryDTO-Eigenschaften zu (etwa „Top“ und „Context“), und verwenden Sie sie in Ihrem Chatbot.

[!code-python[Generate an answer from a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/preview-sdk/quickstart.py?name=GenerateAnswer)]

---

Dies ist ein einfaches Beispiel zum Abfragen der Wissensdatenbank. Informationen zu erweiterten Abfrageszenarien finden Sie in [weiteren Abfragebeispielen](../quickstarts/get-answer-from-knowledge-base-using-url-tool.md?pivots=url-test-tool-curl#use-curl-to-query-for-a-chit-chat-answer).

## <a name="delete-a-knowledge-base"></a>Löschen einer Wissensdatenbank

Löschen Sie die Wissensdatenbank mit der [delete](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations#delete-kb-id--custom-headers-none--raw-false----operation-config-)-Methode mit einem Parameter der Wissensdatenbank-ID.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

[!code-python[Delete a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=DeleteKB)]

# <a name="qna-maker-managed-preview-release"></a>[Mit QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

[!code-python[Delete a knowledge base](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/preview-sdk/quickstart.py?name=DeleteKB)]

---

## <a name="get-status-of-an-operation"></a>Abrufen des Status eines Vorgangs

Einige Methoden, wie „create“ und „update“, können so viel Zeit in Anspruch nehmen, dass ein [Vorgang](/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.operation(class)) zurückgegeben wird, anstatt auf den Abschluss des Prozesses zu warten. Verwenden Sie die Vorgangs-ID des Vorgangs, um den Status der ursprünglichen Methode per Abruf (mit Wiederholungslogik) zu bestimmen.

Der _setTimeout_-Aufruf im folgenden Codeblock wird verwendet, um asynchronen Code zu simulieren. Ersetzen Sie dies durch Wiederholungslogik.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

[!code-python[Monitor an operation](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/quickstart.py?name=MonitorOperation)]

# <a name="qna-maker-managed-preview-release"></a>[Mit QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

[!code-python[Monitor an operation](~/cognitive-services-quickstart-code/python/QnAMaker/sdk/preview-sdk/quickstart.py?name=MonitorOperation)]

---

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Python-Befehl für die Schnellstartdatei aus.

```console
python quickstart-file.py
```

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/version-1)

Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/QnAMaker/sdk/quickstart.py).

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/version-2)

Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/QnAMaker/sdk/preview-sdk/quickstart.py).

---
