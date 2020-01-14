---
title: 'Schnellstart: Erstellen einer Wissensdatenbank: REST, C# – QnA Maker'
titleSuffix: Azure Cognitive Services
description: Diese C#-REST-basierte Schnellstartanleitung führt Sie durch das programmgesteuerte Erstellen einer Beispielwissensdatenbank für QnA Maker, die auf dem Azure-Dashboard Ihres Cognitive Services-API-Kontos angezeigt wird.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 12/16/2019
ms.author: diberry
ms.openlocfilehash: 2b3ec7611fec779fcd387f45204f2e1cada1161c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75447661"
---
# <a name="quickstart-create-a-knowledge-base-in-qna-maker-using-c-with-rest"></a>Schnellstart: Erstellen einer Wissensdatenbank in QnA Maker mithilfe von C# mit REST

In dieser Schnellstartanleitung wird Schritt für Schritt das programmgesteuerte Erstellen und Veröffentlichen eines Beispiels für eine QnA Maker-Wissensdatenbank beschrieben. QnA Maker extrahiert automatisch Fragen und Antworten aus teilweise strukturiertem Inhalt (z.B. häufig gestellten Fragen) von [Datenquellen](../Concepts/data-sources-supported.md). Das Modell für die Wissensdatenbank wird im JSON-Code definiert, der im Text der API-Anforderung gesendet wird.

In dieser Schnellstartanleitung werden QnA Maker-APIs aufgerufen:
* [Erstellen einer Wissensdatenbank](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create)
* [Abrufen von Vorgangsdetails](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails)

[Referenzdokumentation](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase) | [C#-Beispiel](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/blob/master/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).
* Sie benötigen eine [QnA Maker-Ressource](../How-To/set-up-qnamaker-service-azure.md). Wählen Sie für Ihre Ressource im Azure-Portal die Option **Schnellstart** aus, um den Schlüssel und den Endpunkt (der den Ressourcennamen enthält) abzurufen.

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Erstellen Sie eine neue .NET Core-Anwendung in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE.

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl `dotnet new` zum Erstellen einer neuen Konsolen-App mit dem Namen `qna-maker-quickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: *Program.cs*.

```dotnetcli
dotnet new console -n qna-maker-quickstart
```

Wechseln Sie zum Ordner der neu erstellten App. Sie können die Anwendung mit folgendem Befehl erstellen:

```dotnetcli
dotnet build
```

Die Buildausgabe sollte keine Warnungen oder Fehler enthalten.

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

## <a name="add-the-required-dependencies"></a>Hinzufügen der erforderlichen Abhängigkeiten

Ersetzen Sie am Anfang von „Program.cs“ die einzelne using-Anweisung durch die folgenden Zeilen, um dem Projekt die erforderlichen Abhängigkeiten hinzuzufügen:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=1-11 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Hinzufügen der erforderlichen Konstanten

Fügen Sie am Anfang der Program-Klasse die erforderlichen Konstanten für den Zugriff auf QnA Maker hinzu.

Legen Sie die folgenden Werte in Umgebungsvariablen fest:

* `QNA_MAKER_SUBSCRIPTION_KEY` – Der **Key** (Schlüssel) ist eine Zeichenfolge mit 32 Zeichen und im Azure-Portal in der QnA Maker-Ressource auf der Schnellstartseite verfügbar. Diese Ressource ist nicht mit dem Vorhersageendpunktschlüssel identisch.
* `QNA_MAKER_ENDPOINT` – Der **Endpoint** (Endpunkt) ist die URL für die Erstellung im Format `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`. Diese Ressource ist nicht die gleiche URL, die zum Abfragen des Vorhersageendpunkts verwendet wird.

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=17-26 "Add the required constants")]

## <a name="add-the-kb-definition"></a>Hinzufügen der Definition der Wissensdatenbank

Fügen Sie nach den Konstanten die folgende Definition für die Wissensdatenbank hinzu:

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=28-58 "Add the knowledge base definition")]

## <a name="add-supporting-functions-and-structures"></a>Hinzuzufügen von unterstützenden Funktionen und Strukturen
Fügen Sie innerhalb der Program-Klasse den folgenden Codeblock hinzu:

[!code-csharp[Add supporting functions and structures](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=60-99 "Add supporting functions and structures")]

## <a name="add-a-post-request-to-create-kb"></a>Hinzufügen einer POST-Anforderung zum Erstellen der Wissensdatenbank

Der folgende Code sendet eine HTTPS-Anforderung an die QnA Maker-API, um eine Wissensdatenbank zu erstellen, und empfängt die Antwort:

[!code-csharp[Add PostCreateKB to request via POST](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=145-165 "Add PostCreateKB to request via POST")]

[!code-csharp[Add a POST request to create KB](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=101-122 "Add a POST request to create KB")]

Mit diesem API-Aufruf wird eine JSON-Antwort zurückgegeben, die die Vorgangs-ID enthält. Verwenden Sie die Vorgangs-ID, um zu ermitteln, ob die Erstellung der Wissensdatenbank erfolgreich war.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## <a name="add-get-request-to-determine-creation-status"></a>Hinzufügen einer GET-Anforderung zum Ermitteln des Erstellungsstatus

Überprüfen Sie den Status des Vorgangs.

[!code-csharp[Add GetStatus to request via GET](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=167-187 "Add GetStatus to request via GET")]

[!code-csharp[Add GET request to determine creation status](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=124-143 "Add GET request to determine creation status")]

Dieser API-Aufruf gibt eine JSON-Antwort zurück, die den Vorgangsstatus enthält:

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

Wiederholen Sie den Aufruf, bis er erfolgreich abgeschlossen wird oder ein Fehler auftritt:

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-createkb-method"></a>Hinzufügen der CreateKB-Methode

Mit der folgenden Methode wird die Wissensdatenbank erstellt, und der Status wird wiederholt überprüft.  Die **Vorgangs-ID** für _create_ wird im Feld **Location** des POST-Antwortheaders zurückgegeben und anschließend als Teil der Route in der GET-Anforderung verwendet. Da die Erstellung der Wissensdatenbank einige Zeit dauern kann, müssen Sie die Aufrufe zum Überprüfen des Status wiederholen, bis der Status entweder „Erfolgreich“ oder „Fehler“ lautet. Wenn der Vorgang erfolgreich war, wird die ID der Wissensdatenbank in **resourceLocation** zurückgegeben.

[!code-csharp[Add CreateKB method](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=189-254 "Add CreateKB method")]

## <a name="add-the-createkb-method-to-main"></a>Hinzufügen der CreateKB-Methode zu „Main“

Ändern Sie die Main-Methode, um die CreateKB-Methode aufzurufen:

[!code-csharp[Add CreateKB method](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=256-265 "Add CreateKB method")]

## <a name="build-and-run-the-program"></a>Erstellen und Ausführen des Programms

Erstellen Sie das Programm, und führen Sie es aus. Die Anforderung wird automatisch an die QnA Maker-API gesendet, um die Wissensdatenbank zu erstellen, und dann werden die Ergebnisse alle 30 Sekunden abgefragt. Jede Antwort wird im Konsolenfenster ausgegeben.

Nach der Erstellung der Wissensdatenbank können Sie sie im QnA Maker-Portal auf der Seite [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) (Meine Wissensdatenbanken) anzeigen.


[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [REST-API-Referenz für QnA Maker (V4)](https://go.microsoft.com/fwlink/?linkid=2092179)
