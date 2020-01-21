---
title: 'Schnellstart: QnA Maker-Clientbibliothek für Node.js'
description: Dieser Schnellstart zeigt Ihnen die ersten Schritte mit der QnA Maker-Clientbibliothek für Node.js.
ms.topic: quickstart
ms.date: 01/13/2020
ms.openlocfilehash: 05e6d2c77a351c22c73f0bdb54daeaf0e03f4a9d
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76020912"
---
Verwenden Sie die QnA Maker-Clientbibliothek für Node.js für folgende Zwecke:

* Erstellen einer Wissensdatenbank
* Aktualisieren einer Wissensdatenbank
* Veröffentlichen einer Wissensdatenbank
* Abrufen eines veröffentlichten Endpunktschlüssels
* Warten auf Aufgaben mit langer Ausführungsdauer
* Löschen einer Wissensdatenbank

[Referenzdokumentation](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/?view=azure-node-latest) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-qnamaker) | [Paket (npm)](https://www.npmjs.com/package/@azure/cognitiveservices-qnamaker) | [Node.js-Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Die aktuelle Version von [Node.js](https://nodejs.org).

## <a name="setting-up"></a>Einrichten

### <a name="create-a-qna-maker-azure-resource"></a>Erstellen einer QnA Maker-Azure-Ressource

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie eine Ressource für QnA Maker mithilfe des [Azure-Portals](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oder der [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) auf Ihrem lokalen Computer.

Rufen Sie nach dem Abrufen des Schlüssels und des Endpunkts aus der Ressource die Werte für Ihre neue Ressource im Azure-Portal auf der Schnellstartseite ab.

[Erstellen Sie Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) mit den Namen `QNAMAKER_AUTHORING_KEY` und `QNAMAKER_ENDPOINT`. Sie können die Datei [.env.sample](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/.env.sample) in `.env` kopieren und die Umgebungsvariablen in dieser Datei verwenden.

### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Erstellen Sie in einem Konsolenfenster (etwa cmd, PowerShell oder Bash) ein neues Verzeichnis für Ihre App, und rufen Sie es auf.

```console
mkdir qnamaker_quickstart && cd qnamaker_quickstart
```

Führen Sie den Befehl `npm init -y` aus, um eine Knotenanwendung mit der Datei `package.json` zu erstellen.

```console
npm init -y
```

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Installieren Sie die erforderlichen und optionalen NPM-Pakete:

```console
npm install @azure/cognitiveservices-qnamaker @azure/ms-rest-js dotenv
```

Die Datei `package.json` Ihrer App wird mit den Abhängigkeiten aktualisiert. `dotenv` ist optional und wird verwendet, damit Sie die Umgebungsvariablen in einer Textdatei festlegen können. Checken Sie `.env` nicht in die Quellcodeverwaltung ein.


## <a name="object-model"></a>Objektmodell

Der QnA Maker-Client ist ein [QnAMakerClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest)-Objekt, das per „ServiceClientCredentials“-Element, in dem Ihr Schlüssel enthalten ist, bei Azure authentifiziert wird.

Nachdem der Client erstellt wurde, verwenden Sie die [Knowledge Base](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest#knowledgebase)-Eigenschaft, um Ihre Knowledge Base zu erstellen, zu verwalten und zu veröffentlichen.

Verwalten Sie Ihre Knowledge Base durch Senden eines JSON-Objekts. Bei sofortigen Vorgängen gibt eine Methode in der Regel ein JSON-Objekt zurück, das den Status angibt. Bei zeitintensiven Vorgängen ist die Antwort die Vorgangs-ID. Rufen Sie die [client.Operations.getDetails](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest#getdetails-string--servicecallback-operation--)-Methode mit der Vorgangs-ID auf, um den [Status der Anforderung](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operationstatetype?view=azure-node-latest) zu bestimmen.


## <a name="code-examples"></a>Codebeispiele

Mit den Codeausschnitten wird veranschaulicht, wie folgende Vorgänge mit der QnA Maker-Clientbibliothek für Node.js durchgeführt werden:

* [Erstellen einer Wissensdatenbank](#create-a-knowledge-base)
* [Aktualisieren einer Wissensdatenbank](#update-a-knowledge-base)
* [Veröffentlichen einer Wissensdatenbank](#publish-a-knowledge-base)
* [Löschen einer Wissensdatenbank](#delete-a-knowledge-base)
* [Abrufen eines veröffentlichten Endpunkts](#get-published-endpoint)
* [Abrufen des Status eines Vorgangs](#get-status-of-an-operation)

## <a name="add-the-dependencies"></a>Hinzufügen der Abhängigkeiten

Erstellen Sie eine Datei mit dem Namen `index.js`. Fügen Sie der Datei die QnA Maker-Bibliothek und die Abhängigkeiten hinzu.

[!code-javascript[Require statements](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=dependencies)]

Erstellen Sie Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource. Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellt haben, müssen der Editor, die IDE oder die Shell, in dem bzw. der sie ausgeführt wird, geschlossen und erneut geöffnet werden, damit der Zugriff auf die Variable möglich ist.

|Umgebungsvariable|Node.js-Variable|Beispiel|
|--|--|--|
|`QNAMAKER_AUTHORING_KEY`|`authoring_key`|Der Schlüssel ist eine Zeichenfolge mit 32 Zeichen und ist im Azure-Portal in der QnA Maker-Ressource auf der Schnellstartseite verfügbar. Diese Ressource ist nicht mit dem Vorhersageendpunktschlüssel identisch.|
|`QNAMAKER_ENDPOINT`|`endpoint`| Der Erstellungsendpunkt im Format `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` enthält Ihren **Ressourcennamen**. Diese Ressource ist nicht die gleiche URL, die zum Abfragen des Vorhersageendpunkts verwendet wird.|
||||

[!code-javascript[Azure resource variables](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=resourceSettings)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Erstellen Sie als Nächstes ein ApiKeyCredentials-Objekt mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein [QnAMakerClient](https://docs.microsoft.com/javascript/api/%40azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest#qnamakerclient-serviceclientcredentials--string--msrest-serviceclientoptions-)-Objekt zu erstellen. Verwenden Sie das Clientobjekt, um ein Objekt vom Typ [knowledge base client](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest) abzurufen.


[!code-javascript[Authorization to resource key](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=authorization)]

## <a name="create-a-knowledge-base"></a>Erstellen einer Wissensdatenbank

Eine Knowledge Base speichert Frage- und Antwortpaare für das [CreateKbDTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/createkbdto?view=azure-node-latest)-Objekt aus drei Quellen:

* Für **redaktionellen Inhalt** verwenden Sie das [QnADTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnadto?view=azure-node-latest)-Objekt.
* Für **Dateien** verwenden Sie das [FileDTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/filedto?view=azure-node-latest)-Objekt.
* Für **URLs** verwenden Sie eine Liste mit Zeichenfolgen.

Rufen Sie die [create](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#create-createkbdto--servicecallback-operation--)-Methode mit den Informationen der Wissensdatenbank auf. Bei den Informationen der Wissensdatenbank handelt es sich im Grunde um ein JSON-Objekt.

Übergeben Sie bei Rückgabe der create-Methode die zurückgegebene Vorgangs-ID an die [wait_for_operation](#get-status-of-an-operation)-Methode, um den Status abzurufen. Die Rückgabe der Methode „wait_for_operation“ erfolgt bei Abschluss des Vorgangs. Analysieren Sie den `resourceLocation`-Headerwert des zurückgegebenen Vorgangs, um die neue Wissensdatenbank-ID abzurufen.

[!code-javascript[Create a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=createKnowledgeBase&highlight=15,30)]

Nehmen Sie zum erfolgreichen Erstellen einer Wissensdatenbank unbedingt die Funktion [`wait_for_operation`](#get-status-of-an-operation) mit auf, auf die im Code oben verwiesen wird.

## <a name="update-a-knowledge-base"></a>Aktualisieren einer Wissensdatenbank

Sie können eine Wissensdatenbank aktualisieren, indem Sie die Wissensdatenbank-ID und ein [UpdateKbOperationDTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest)-Element, das [add](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#add)-, [update](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#update)- und [delete](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#deleteproperty)-DTO-Objekte enthält, als Eingabe an die [update](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#update-string--updatekboperationdto--msrest-requestoptionsbase-)-Methode übergeben. Die DTOs sind im Grunde ebenfalls JSON-Objekte. Verwenden Sie die [wait_for_operation](#get-status-of-an-operation)-Methode, um zu bestimmen, ob das Update erfolgreich war.

[!code-javascript[Update a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=updateKnowledgeBase&highlight=28)]

Nehmen Sie zum erfolgreichen Aktualisieren einer Wissensdatenbank unbedingt die Funktion [`wait_for_operation`](#get-status-of-an-operation) mit auf, auf die im Code oben verwiesen wird.

## <a name="publish-a-knowledge-base"></a>Veröffentlichen einer Wissensdatenbank

Veröffentlichen Sie die Wissensdatenbank mit der [publish](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#publish-string--msrest-requestoptionsbase-)-Methode. Diese übernimmt das aktuell gespeicherte und trainierte Modell, auf das von der Knowledge Base-ID verwiesen wird, und veröffentlicht dieses an einem Endpunkt. Überprüfen Sie den HTTP-Antwortcode, um sich zu vergewissern, dass die Veröffentlichung erfolgreich war.

[!code-javascript[Publish a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=publishKnowledgeBase&highlight=3)]


## <a name="get-published-endpoint"></a>Abrufen eines veröffentlichten Endpunkts

Greifen Sie nach der Veröffentlichung der Wissensdatenbank über die generateAnswer-API der Abfragevorhersagelaufzeit auf die veröffentlichte Wissensdatenbank zu. Dazu benötigen Sie den Endpunktschlüssel der Laufzeit. Dieser unterscheidet sich vom Erstellungsschlüssel.

[!code-javascript[Get endpoint keys](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=getEndpointKeys&highlight=3)]

Vom Aufruf werden zwei Endpunktschlüssel zurückgegeben. Nur einer ist für den Zugriff auf den Laufzeitendpunkt erforderlich.

## <a name="delete-a-knowledge-base"></a>Löschen einer Wissensdatenbank

Löschen Sie die Wissensdatenbank mit der [delete](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#deletemethod-string--msrest-requestoptionsbase-)-Methode mit einem Parameter der Wissensdatenbank-ID.

[!code-javascript[Delete a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=deleteKnowledgeBase&highlight=3)]

## <a name="get-status-of-an-operation"></a>Abrufen des Status eines Vorgangs

Einige Methoden, wie „create“ und „update“, können so viel Zeit in Anspruch nehmen, dass ein [Vorgang](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest) zurückgegeben wird, anstatt auf den Abschluss des Prozesses zu warten. Verwenden Sie die [Vorgangs-ID](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operation?view=azure-node-latest#operationid) des Vorgangs, um den Status der ursprünglichen Methode mittels Abrufen (mit Wiederholungslogik) zu bestimmen.

Der _delayTimer_-Aufruf im folgenden Codeblock wird verwendet, um die Wiederholungslogik zu simulieren. Ersetzen Sie ihn durch Ihre eigene Wiederholungslogik.

[!code-javascript[Monitor an operation](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=monitorOperation&highlight=8,13)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `node index.js` über das Anwendungsverzeichnis aus.

Alle Codeausschnitte in diesem Artikel sind [verfügbar](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js) und können als einzelne Datei ausgeführt werden.

```console
node index.js
```

Das Programm gibt den Status in der Konsole aus:

```console
qnamaker_quickstart@1.0.0 start C:\samples\cognitive-services-quickstart-code\javascript\QnAMaker\sdk> node index.js

Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Succeeded
Create operation 200, KB ID 99df758d-f23f-4931-ab83-e738fe978e69
Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Succeeded
Update operation state 200 - HTTP status 200
Publish request succeeded - HTTP status 204
GetEndpointKeys request succeeded - HTTP status 200 - primary key 8482830b-681e-400e-b8a3-4016278aba64
QnA Maker FAQ stored in English language with 1 sources, last updated 2020-01-12T16:54:40Z
New KB name stored in English language with 1 sources, last updated 2020-01-12T17:32:16Z
New KB name stored in English language with 1 sources, last updated 2020-01-13T00:27:46Z
Delete operation state succeeded - HTTP status 204
done
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)