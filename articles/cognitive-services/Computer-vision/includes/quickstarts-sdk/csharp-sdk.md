---
title: 'Schnellstart: Client Bibliothek zur optischen Zeichenerkennung für .NET'
description: Erste Schritte mit der optischen Zeichen Erkennungs Client-Bibliothek für .NET. mit dieser Schnellstartanleitung.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 12/15/2020
ms.author: pafarley
ms.custom: devx-track-csharp
ms.openlocfilehash: 538b3ce5a268464b9f014dd00b924875824cab3b
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107327589"
---
<a name="HOLTop"></a>

Verwenden Sie die OCR-Client Bibliothek, um gedruckten und handschriftlichen Text aus Bildern zu lesen.

[Referenzdokumentation](/dotnet/api/overview/azure/cognitiveservices/client/computervision) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.ComputerVision) | [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) | [Beispiele](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: [Kostenloses Azure-Konto](https://azure.microsoft.com/free/cognitive-services/)
* Die [Visual Studio-IDE](https://visualstudio.microsoft.com/vs/) oder die aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title="Erstellen einer Ressource für maschinelles Sehen"  target="_blank"> im Azure-Portal eine Ressource für maschinelles Sehen </a> erstellen, um Ihren Schlüssel und Endpunkt abzurufen. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um eine Verbindung Ihrer Anwendung mit dem Dienst für maschinelles Sehen herzustellen. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

#### <a name="visual-studio-ide"></a>[Visual Studio-IDE](#tab/visual-studio)

Erstellen Sie mit Visual Studio eine neue .NET Core-Anwendung. 

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek 

Installieren Sie nach der Erstellung eines neuen Projekts die Clientbibliothek, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe klicken und **NuGet-Pakete verwalten** auswählen. Wählen Sie im daraufhin geöffneten Paket-Manager die Option **Durchsuchen** aus, aktivieren Sie das Kontrollkästchen **Vorabversion einbeziehen**, und suchen Sie nach `Microsoft.Azure.CognitiveServices.Vision.ComputerVision`. Wählen Sie die Version `7.0.0` und anschließend **Installieren** aus. 

#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/cli)

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl `dotnet new` zum Erstellen einer neuen Konsolen-App mit dem Namen `computer-vision-quickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: *Program.cs*.

```console
dotnet new console -n computer-vision-quickstart
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

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Installieren Sie im Anwendungsverzeichnis mit dem folgenden Befehl die Clientbibliothek für maschinelles Sehen für .NET:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Vision.ComputerVision --version 7.0.0
```

---

> [!TIP]
> Möchten Sie sich sofort die gesamte Codedatei für die Schnellstartanleitung ansehen? Die Datei steht [auf GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs) zur Verfügung. Dort finden Sie die Codebeispiele aus dieser Schnellstartanleitung.

Öffnen Sie aus dem Projektverzeichnis die Datei *Program.cs* in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE.

### <a name="find-the-subscription-key-and-endpoint"></a>Ermitteln des Abonnementschlüssels und Endpunkts

Öffnen Sie das Azure-Portal. Wenn die im Abschnitt **Voraussetzungen** erstellte Ressource für maschinelles Sehen erfolgreich bereitgestellt wurde, klicken Sie unter **Nächste Schritte** auf die Schaltfläche **Zu Ressource wechseln**. Ihren Abonnementschlüssel und den Endpunkt finden Sie auf der Seite mit dem **Schlüssel und dem Endpunkt** der Ressource unter **Ressourcenverwaltung**. 

Erstellen Sie in der **Program**-Klasse der Anwendung Variablen für den Abonnementschlüssel und den Endpunkt für Maschinelles Sehen. Fügen Sie Ihren Abonnementschlüssel und -endpunkt an der angegeben Stelle in den folgenden Code ein. Der Maschinelles Sehen-Endpunkt hat das Format `https://<your_computer_vision_resource_name>.cognitiveservices.azure.com/`.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_using_and_vars)]

> [!IMPORTANT]
> Denken Sie daran, den Abonnementschlüssel aus Ihrem Code zu entfernen, wenn Sie fertig sind, und ihn niemals zu veröffentlichen. In der Produktionsumgebung sollten Sie eine sichere Methode zum Speichern Ihrer Anmeldeinformationen sowie zum Zugriff darauf verwenden. Beispielsweise [Azure Key Vault](../../../../key-vault/general/overview.md).

Fügen Sie in der `Main`-Methode der Anwendung Aufrufe für die Methoden hinzu, die in dieser Schnellstartanleitung verwendet werden. Die Erstellung führen Sie zu einem späteren Zeitpunkt durch.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_client)]

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_extracttextinmain)]

> [!div class="nextstepaction"]
> [Ich habe den Client eingerichtet.](?success=set-up-client#object-model) [Bei mir ist ein Problem aufgetreten.](https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=Csharp&Section=set-up-client)

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen verarbeiten einige der Hauptfunktionen des OCR. NETSDK.

|name|BESCHREIBUNG|
|---|---|
| [ComputerVisionClient](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient) | Diese Klasse wird für alle Funktionen der Maschinelles Sehen-API benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie für die meisten Bildvorgänge.|
|[ComputerVisionClientExtensions](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclientextensions)| Diese Klasse enthält zusätzliche Methoden für **ComputerVisionClient**.|

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der OCR Clientbibliothek für .NET. ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Lesen von gedrucktem und handschriftlichem Text](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Instanziieren Sie in einer neuen Methode in der Klasse **Program** einen Client mit Ihrem Endpunkt und Abonnementschlüssel. Erstellen Sie ein Objekt vom Typ **[ApiKeyServiceClientCredentials](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.apikeyserviceclientcredentials)** mit Ihrem Abonnementschlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein Objekt vom Typ **[ComputerVisionClient](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient)** zu erstellen.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_auth)]

> [!div class="nextstepaction"]
> [Ich habe den Client authentifiziert.](?success=authenticate-client#read-printed-and-handwritten-text) [Bei mir ist ein Problem aufgetreten.](https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=Csharp&Section=authenticate-client)

## <a name="read-printed-and-handwritten-text"></a>Lesen von gedrucktem und handschriftlichem Text

Der OCR-Dienst kann sichtbaren Text in einem Bild lesen und in eine Zeichenfolge konvertieren. Weitere Informationen zur Texterkennung finden Sie in der Übersicht [Optische Zeichenerkennung (OCR)](../../overview-ocr.md). Der Code in diesem Abschnitt nutzt das aktuelle [SDK-Release für maschinelles Sehen für Read 3.0](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) und definiert die Methode `BatchReadFileUrl`. Hierbei wird das Clientobjekt verwendet, um Text im Bild zu erkennen und zu extrahieren.

> [!TIP]
> Sie können auch Text aus einem lokalen Bild extrahieren. Sehen Sie sich die [ComputerVisionClient](/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient)-Methoden an, etwa **ReadInStreamAsync**. Alternativ finden Sie im Beispielcode auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs) Szenarien zu lokalen Bildern.

### <a name="set-up-test-image"></a>Einrichten eines Testbilds

Speichern Sie in der Klasse **Program** einen Verweis auf die URL des Bilds, aus dem Sie Text extrahieren möchten. Dieser Codeausschnitt enthält Beispielbilder für gedruckten und handschriftlichen Text.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_readtext_url)]

### <a name="call-the-read-api"></a>Aufrufen der Lese-API

Hiermit wird die neue Methode zum Lesen von Text definiert. Fügen Sie den folgenden Code hinzu, mit dem die **ReadAsync**-Methode für das angegebene Bild aufgerufen wird. Dies gibt eine Vorgangs-ID zurück und startet einen asynchronen Prozess, um den Inhalt des Bilds zu lesen.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_readfileurl_1)]

### <a name="get-read-results"></a>Abrufen der Leseergebnisse

Rufen Sie als Nächstes die Vorgangs-ID ab, die vom Aufruf **ReadAsync** zurückgegeben wurde, und verwenden Sie sie zum Abfragen des Diensts auf Vorgangsergebnisse. Der folgende Code überprüft den Vorgang, bis die Ergebnisse zurückgegeben werden. Anschließend werden die extrahierten Textdaten in der Konsole ausgegeben.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_readfileurl_2)]

### <a name="display-read-results"></a>Anzeigen der Leseergebnisse

Fügen Sie den folgenden Code hinzu, um die abgerufenen Textdaten zu analysieren und anzuzeigen, und schließen Sie die Methodendefinition ab.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_readfileurl_3)]

## <a name="run-the-application"></a>Ausführen der Anwendung

#### <a name="visual-studio-ide"></a>[Visual Studio-IDE](#tab/visual-studio)

Führen Sie die Anwendung aus, indem Sie oben im IDE-Fenster auf die Schaltfläche **Debuggen** klicken.

#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/cli)

Führen Sie die Anwendung mit dem Befehl `dotnet run` aus dem Anwendungsverzeichnis aus.

```dotnet
dotnet run
```

---

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie erfahren, wie Sie die OCR-Clientbibliothek installieren und die Lese-API verwenden. Informieren Sie sich als Nächstes ausführlicher über die Funktionen der Lese-API.

> [!div class="nextstepaction"]
>[Aufrufen der Lese-API](../../Vision-API-How-to-Topics/call-read-api.md)

* [OCR-Übersicht](../../overview-ocr.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs).
