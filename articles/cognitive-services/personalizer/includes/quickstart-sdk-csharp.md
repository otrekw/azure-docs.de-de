---
title: include file
description: include file
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 07/30/2020
ms.openlocfilehash: 55bc13caec8a48ed058ea38bdc0ebe487839eef3
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87461109"
---
[Referenzdokumentation](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.Personalizer?view=azure-dotnet-preview) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Personalizer) | [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Personalizer/) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/Personalizer)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="using-this-quickstart"></a>Verwenden dieser Schnellstartanleitung

Diese Schnellstartanleitung umfasst mehrere Schritte:

* Erstellen Sie im Azure-Portal eine Personalisierungsressource.
* Ändern Sie im Azure-Portal auf der Seite **Konfiguration** die Häufigkeit der Modellaktualisierung für die Personalisierungsressource in ein sehr kurzes Intervall.
* Erstellen Sie in einem Code-Editor eine Codedatei, und bearbeiten Sie sie.
* Installieren Sie in der Befehlszeile oder im Terminal das SDK über die Befehlszeile.
* Führen Sie die Codedatei in der Befehlszeile oder im Terminal aus.

[!INCLUDE [Create Azure resource for Personalizer](create-personalizer-resource.md)]

[!INCLUDE [!Change model frequency](change-model-frequency.md)]

## <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Erstellen Sie eine neue .NET Core-Anwendung in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE.

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl dotnet `new` zum Erstellen einer neuen Konsolen-App mit dem Namen `personalizer-quickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: `Program.cs`.

```console
dotnet new console -n personalizer-quickstart
```

Wechseln Sie zum Ordner der neu erstellten App. Sie können die Anwendung mit folgendem Befehl erstellen:

```console
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

## <a name="install-the-sdk"></a>Installieren des SDKs

Installieren Sie im Anwendungsverzeichnis mit dem folgenden Befehl die Personalisierungsclientbibliothek für .NET:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Personalizer --version 0.8.0-preview
```

Bei Verwendung der Visual Studio-IDE ist die Clientbibliothek als herunterladbares NuGet-Paket verfügbar.

## <a name="object-model"></a>Objektmodell

Der Personalisierungsclient ist ein Objekt vom Typ [PersonalizerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclient?view=azure-dotnet), das mithilfe der Klasse „Microsoft.Rest.ServiceClientCredentials“, die Ihren Schlüssel enthält, bei Azure authentifiziert wird.

Erstellen Sie zum Anfordern des besten Inhaltselements eine Rangfolgeanforderung ([RankRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rankrequest?view=azure-dotnet-preview)), und übergeben Sie sie an die Methode [client.Rank](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.rank?view=azure-dotnet-preview). Die Methode „Rank“ gibt eine Rangfolgeantwort (RankResponse) zurück.

Erstellen Sie eine Relevanzanforderung ([RewardRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rewardrequest?view=azure-dotnet-preview)), und übergeben Sie sie an die Methode [client.Reward](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.reward?view=azure-dotnet-preview), um eine Relevanzbewertung an die Personalisierung zu senden.

Im Rahmen dieser Schnellstartanleitung ist die Bestimmung der Relevanzbewertung ganz einfach. In einem Produktionssystem kann die Bestimmung der Einflussfaktoren für die [Relevanzbewertung](../concept-rewards.md) sowie deren jeweilige Gewichtung allerdings eine komplexe Angelegenheit sein und muss unter Umständen im Laufe der Zeit überarbeitet werden. Diese Entwurfsentscheidung sollte eine der wichtigsten Entscheidungen im Zusammenhang mit Ihrer Personalisierungsarchitektur sein.

## <a name="code-examples"></a>Codebeispiele

In diesen Codeausschnitten werden folgende Aufgaben mit der Personalisierungsclientbibliothek für .NET veranschaulicht:

* [Erstellen eines Personalisierungsclients](#create-a-personalizer-client)
* [Rangfolge-API](#request-the-best-action)
* [Relevanz-API](#send-a-reward)

## <a name="add-the-dependencies"></a>Hinzufügen der Abhängigkeiten

Öffnen Sie aus dem Projektverzeichnis die Datei **Program.cs** in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Ersetzen Sie den vorhandenen `using`-Code durch die folgenden `using`-Anweisungen:

[!code-csharp[Using statements](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Hinzufügen von Informationen zur Personalisierungsressource

Bearbeiten Sie in der Klasse **Program** die Schlüssel- und Endpunktvariablen am Anfang der Codedatei für den Azure-Schlüssel und -Endpunkt Ihrer Ressource. 

[!code-csharp[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=classVariables)]

## <a name="create-a-personalizer-client"></a>Erstellen eines Personalisierungsclients

Erstellen Sie als Nächstes eine Methode zum Zurückgeben eines Personalisierungsclients. Der Parameter für die Methode lautet `PERSONALIZER_RESOURCE_ENDPOINT`, und der API-Schlüssel (ApiKey) ist `PERSONALIZER_RESOURCE_KEY`.

[!code-csharp[Create the Personalizer client](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=authorization)]

## <a name="get-food-items-as-rankable-actions"></a>Abrufen von Lebensmitteln als Aktionen, denen ein Rang zugewiesen werden kann

Aktionen stellen die Inhaltsoptionen dar, aus denen die Personalisierung das beste Inhaltselement auswählen soll. Fügen Sie der Klasse „Program“ die folgenden Methoden hinzu, um die Gruppe der Aktionen und deren Features darzustellen. 

[!code-csharp[Food items as actions](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=createAction)]

## <a name="get-user-preferences-for-context"></a>Abrufen von Benutzerwünschen für den Kontext

Fügen Sie der Klasse „Program“ die folgenden Methoden hinzu, um die Benutzereingaben für Tageszeit und aktuellen Essenswunsch aus der Befehlszeile abzurufen. Diese werden als Kontextfeatures verwendet.

[!code-csharp[Present time out day preference to the user](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=createUserFeatureTimeOfDay)]

[!code-csharp[Present food taste preference to the user](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=createUserFeatureTastePreference)]

Bei beiden Methoden wird die Methode `GetKey` verwendet, um die Auswahl des Benutzers aus der Befehlszeile zu lesen.

[!code-csharp[Read user's choice from the command line](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=readCommandLine)]

## <a name="create-the-learning-loop"></a>Erstellen der Lernschleife

Die Lernschleife der Personalisierung ist ein Zyklus aus [Rangfolge-](#request-the-best-action) und [Relevanzaufrufen](#send-a-reward). In dieser Schnellstartanleitung folgt auf jeden Rangfolgeaufruf zur Personalisierung des Inhalts ein Relevanzaufruf, um der Personalisierung mitzuteilen, wie gut der Dienst abgeschnitten hat.

Der folgende Code durchläuft einen Zyklus in Form einer Schleife: Der Benutzer wird an der Befehlszeile nach seinen Präferenzen befragt, die Angaben werden zur Wahl der besten Aktion an die Personalisierung gesendet, die Auswahl wird dem Kunden in einer Auswahlliste angezeigt, und anschließend wird eine Relevanzbewertung an die Personalisierung gesendet, die angibt, wie gut die Wahl des Diensts war.

[!code-csharp[Learning loop](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=mainLoop)]

Fügen Sie vor dem Ausführen der Codedatei die folgenden Methoden hinzu, die die [Inhaltsoptionen abrufen](#get-food-items-as-rankable-actions):

* `GetActions`
* `GetUsersTimeOfDay`
* `GetUsersTastePreference`
* `GetKey`

## <a name="request-the-best-action"></a>Anfordern der besten Aktion

Für die Rangfolgeanforderung erfragt das Programm die Präferenzen des Benutzers, um für die Inhaltsoptionen ein Element vom Typ `currentContent` zu erstellen. Der Prozess kann Inhalte erstellen, die von den Aktionen ausgeschlossen werden sollen (angezeigt als `excludeActions`). Die Rangfolgeanforderung muss die Aktionen und ihre Features, die Features des aktuellen Kontexts (currentContext), die auszuschließenden Aktionen (excludeActions) und eine eindeutige Ereignis-ID enthalten, um die Antwort zu erhalten.

In dieser Schnellstartanleitung werden die einfachen Kontextmerkmale „Tageszeit“ und „Essenswunsch des Benutzers“ verwendet. In Produktionssystemen kann die Bestimmung und [Auswertung](../concept-feature-evaluation.md) von [Aktionen und Merkmalen](../concepts-features.md) allerdings eine komplexe Angelegenheit sein.

[!code-csharp[The Personalizer learning loop ranks the request.](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=rank)]

## <a name="send-a-reward"></a>Senden einer Relevanz

Um die Relevanzbewertung zu erhalten, die in der Relevanzanforderung gesendet wird, ruft das Programm die Auswahl des Benutzers aus der Befehlszeile ab, weist der Auswahl einen numerischen Wert zu und sendet dann die eindeutige Ereignis-ID und die Relevanzbewertung als numerischen Wert an die Relevanz-API.

In dieser Schnellstartanleitung wird als Relevanzbewertung eine einfache Zahl (0 oder 1) zugewiesen. In Produktionssystemen ist die Entscheidung, was wann an den [Relevanzaufruf](../concept-rewards.md) gesendet werden soll (abhängig von Ihren spezifischen Anforderungen), unter Umständen etwas komplizierter.

[!code-csharp[The Personalizer learning loop ranks the request.](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=reward)]

## <a name="run-the-program"></a>Ausführen des Programms

Führen Sie die Anwendung mit dem Befehl dotnet `run` über das Anwendungsverzeichnis aus.

```console
dotnet run
```

![Das Schnellstartprogramm stellt ein paar Fragen zum Sammeln von Benutzereinstellungen – auch bekannt als Features – und stellt dann die am besten bewertete Aktion bereit.](../media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

Der [Quellcode für diese Schnellstartanleitung](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/Personalizer) ist verfügbar.
