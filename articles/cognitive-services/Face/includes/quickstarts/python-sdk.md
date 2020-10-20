---
title: 'Schnellstart: Clientbibliothek zur Gesichtserkennung für Python'
description: Verwenden Sie die Gesichtserkennungs-Clientbibliothek für Python, um Gesichter zu erkennen, ähnliche Gesichter zu finden (Gesichtssuche anhand von Bildern) und Gesichter zu identifizieren (Gesichtserkennungssuche).
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: include
ms.date: 10/07/2020
ms.author: pafarley
ms.openlocfilehash: 587e702f5c74149542e2fffcf7891b7ea41f4202
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2020
ms.locfileid: "91859724"
---
Erste Schritte mit Gesichtserkennung unter Verwendung der Gesichtserkennungs-Clientbibliothek für Python. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben. Über den Gesichtserkennungsdienst haben Sie Zugriff auf erweiterte Algorithmen für die Erkennung von menschlichen Gesichtern in Bildern.

Verwenden Sie die Clientbibliothek zur Gesichtserkennung für Python für Folgendes:

* Gesichtserkennung in einem Bild
* Suchen ähnlicher Gesichter
* Erstellen und Trainieren einer Personengruppe
* Identifizieren eines Gesichts
* Überprüfen von Gesichtern

[Referenzdokumentation](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/?view=azure-python) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-face) | [Paket (PiPy)](https://pypi.org/project/azure-cognitiveservices-vision-face/) | [Beispiele](https://docs.microsoft.com/samples/browse/?products=azure&term=face)

## <a name="prerequisites"></a>Voraussetzungen

* [Python 3.x](https://www.python.org/)
* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title="Erstellen einer Gesichtserkennungsressource"  target="_blank"> eine Gesichtserkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Gesichtserkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.
* Nachdem Sie einen Schlüssel und einen Endpunkt erhalten haben, [erstellen Sie Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für den Schlüssel und den Endpunkt mit dem Namen `FACE_SUBSCRIPTION_KEY` bzw. `FACE_ENDPOINT`.

## <a name="setting-up"></a>Einrichten
 
### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Erstellen Sie ein neues Python-Skript, etwa *quickstart-file.py*. Öffnen Sie es anschließend in Ihrem bevorzugten Editor bzw. Ihrer bevorzugten IDE, und importieren Sie die folgenden Bibliotheken:

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_imports)]

Erstellen Sie dann Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_subvars)]

> [!NOTE]
> Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellt haben, müssen der Editor, die IDE oder die Shell, in dem bzw. der sie ausgeführt wird, geschlossen und erneut geöffnet werden, damit der Zugriff auf die Variable möglich ist.

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Sie können die Clientbibliothek wie folgt installieren:

```console
pip install --upgrade azure-cognitiveservices-vision-face
```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen verarbeiten einige der Hauptfunktionen der Python-Clientbibliothek für die Gesichtserkennung:

|Name|BESCHREIBUNG|
|---|---|
|[FaceClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.faceclient?view=azure-python) | Diese Klasse stellt Ihre Autorisierung für die Verwendung des Gesichtserkennungsdiensts dar. Sie benötigen sie für alle Gesichtserkennungsfunktionen. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Generieren von Instanzen anderer Klassen. |
|[FaceOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python)|Diese Klasse behandelt die grundlegenden Erkennungs- und Wiedererkennungsaufgaben, die Sie für menschliche Gesichter ausführen können. |
|[DetectedFace](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.models.detectedface?view=azure-python)|Diese Klasse stellt alle Daten dar, die in einem einzelnen Gesicht in einem Bild erkannt wurden. Sie können damit ausführliche Informationen zum Gesicht abrufen.|
|[FaceListOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.facelistoperations?view=azure-python)|Diese Klasse verwaltet die in der Cloud gespeicherten **FaceList**-Konstrukte, die verschiedene Gesichter speichern. |
|[PersonGroupPersonOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.persongrouppersonoperations?view=azure-python)| Diese Klasse verwaltet die in der Cloud gespeicherten **Person**-Konstrukte, die eine Gruppe von Gesichtern speichern, die zu einer einzelnen Person gehören.|
|[PersonGroupOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.persongroupoperations?view=azure-python)| Diese Klasse verwaltet die in der Cloud gespeicherten **PersonGroup**-Konstrukte, die verschiedene **Person**-Objekte speichern. |
|[ShapshotOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.snapshotoperations?view=azure-python)|Mit dieser Klasse wird die Funktionalität für Momentaufnahmen verwaltet. Sie können mit ihr vorübergehend alle cloudbasierten Gesichtserkennungsdaten speichern und zu einem neuen Azure-Abonnement migrieren. |

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der Clientbibliothek zur Gesichtserkennung für Python ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Gesichtserkennung in einem Bild](#detect-faces-in-an-image)
* [Suchen ähnlicher Gesichter](#find-similar-faces)
* [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group)
* [Identifizieren eines Gesichts](#identify-a-face)
* [Überprüfen von Gesichtern](#verify-faces)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

> [!NOTE]
> In dieser Schnellstartanleitung wird vorausgesetzt, dass Sie für den Schlüssel für die Gesichtserkennung mit dem Namen `FACE_SUBSCRIPTION_KEY`[eine Umgebungsvariable erstellt haben](../../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication).

Instanziieren Sie einen Client mit Ihrem Endpunkt und Schlüssel. Erstellen Sie ein [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python)-Objekt mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein [FaceClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.faceclient?view=azure-python)-Objekt zu erstellen.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_auth)]

## <a name="detect-faces-in-an-image"></a>Gesichtserkennung in einem Bild

Der folgende Code erkennt ein Gesicht in einem Remotebild. Die ID des erkannten Gesichts wird in der Konsole ausgegeben und darüber hinaus im Programmspeicher gespeichert. Anschließend werden die Gesichter in einem Bild mit mehreren Personen erkannt und ihre IDs ebenfalls in der Konsole ausgegeben. Indem Sie die Parameter in der Methode [detect_with_url](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python#detect-with-url-url--return-face-id-true--return-face-landmarks-false--return-face-attributes-none--recognition-model--recognition-01---return-recognition-model-false--detection-model--detection-01---custom-headers-none--raw-false----operation-config-) ändern, können Sie mit jedem [DetectedFace](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.models.detectedface?view=azure-python)-Objekt andere Informationen zurückgeben.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_detect)]

Weitere Szenarien zur Gesichtserkennung finden Sie im Beispielcode auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/Face/FaceQuickstart.py).

### <a name="display-and-frame-faces"></a>Anzeigen und Umranden von Gesichtern

Der folgende Code gibt ein bestimmtes Bild an die Anzeige aus und zeichnet unter Verwendung der Eigenschaft „DetectedFace.faceRectangle“ Rechtecke um die Gesichter.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_frame)]

![Eine junge Frau mit einem roten Rechteck um das Gesicht](../../images/face-rectangle-result.png)

## <a name="find-similar-faces"></a>Suchen ähnlicher Gesichter

Der folgende Code verwendet ein einzelnes erkanntes Gesicht (Quelle) und durchsucht eine Reihe anderer Gesichter (Ziel) auf Übereinstimmungen (Gesichtssuche anhand von Bildern). Bei einer Übereinstimmung wird die ID des entsprechenden Gesichts in der Konsole ausgegeben.

### <a name="find-matches"></a>Suchen von Übereinstimmungen

Führen Sie zuerst den Code im obigen Abschnitt ([Gesichtserkennung in einem Bild](#detect-faces-in-an-image)) aus, um einen Verweis auf ein einzelnes Gesicht zu speichern. Führen Sie dann den folgenden Code aus, um Verweise auf mehrere Gesichter in einem Gruppenbild zu erhalten:

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_detectgroup)]

Fügen Sie anschließend den folgenden Codeblock hinzu, um nach Instanzen des ersten Gesichts in der Gruppe zu suchen. Informationen zum Ändern dieses Verhaltens finden Sie im Artikel zur Methode [find_similar](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python#find-similar-face-id--face-list-id-none--large-face-list-id-none--face-ids-none--max-num-of-candidates-returned-20--mode--matchperson---custom-headers-none--raw-false----operation-config-).

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_findsimilar)]

### <a name="print-matches"></a>Ausgeben von Übereinstimmungen

Verwenden Sie den folgenden Code, um die Übereinstimmungsdetails in der Konsole auszugeben:

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_findsimilar_print)]

## <a name="create-and-train-a-person-group"></a>Erstellen und Trainieren einer Personengruppe

Mit dem folgenden Code wird eine Personengruppe (**PersonGroup**) mit drei verschiedenen Personen (**Person**) erstellt. Er ordnet jeder **Person** verschiedene Beispielbilder zu. Danach folgt ein Trainingsschritt, um die einzelnen Personen zu erkennen. 

### <a name="create-persongroup"></a>Erstellen einer Personengruppe

Für dieses Szenario müssen Sie die folgenden Bilder im Stammverzeichnis Ihres Projekts speichern: https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/images.

Diese Gruppe von Bildern enthält drei verschiedene Bilder des Gesichts, die den drei Personen entsprechen. Der Code definiert drei **Person**-Objekte und ordnet sie Bilddateien zu, die mit `woman`, `man` und `child` beginnen.

Definieren Sie nach der Einrichtung Ihrer Bilder am Anfang Ihres Skripts eine Bezeichnung für das **PersonGroup**-Objekt, das Sie erstellen.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroupvars)]

Fügen Sie dann am Ende des Skripts den folgenden Code hinzu. Mit diesem Code werden ein **PersonGroup**-Objekt und drei **Person**-Objekte erstellt.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroup_create)]

### <a name="assign-faces-to-persons"></a>Zuweisen von Gesichtern zu Personen

Der folgende Code sortiert Ihre Bilder anhand ihres Präfixes, erkennt Gesichter und weist die Gesichter den einzelnen **Person**-Objekten zu.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroup_assign)]

### <a name="train-persongroup"></a>Trainieren von PersonGroup

Nach dem Zuweisen von Gesichtern müssen Sie das **PersonGroup**-Objekt trainieren, sodass es die den einzelnen **Person**-Objekten zugewiesenen visuellen Merkmale erkennen kann. Der folgende Code ruft die asynchrone **train**-Methode auf, ruft das Ergebnis ab und gibt den Status in der Konsole aus.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroup_train)]

## <a name="identify-a-face"></a>Identifizieren eines Gesichts

Der Identifizierungsvorgang verwendet ein Bild einer Person (oder mehrerer Personen) und sucht im Bild nach der Identität der einzelnen Gesichter (Gesichtserkennungssuche). Er vergleicht jedes erkannte Gesicht mit einem **PersonGroup**-Objekt, einer Datenbank mit verschiedenen **Person**-Objekten,deren Gesichtsmerkmale bekannt sind.

> [!IMPORTANT]
> Damit Sie dieses Beispiel ausführen können, müssen Sie zunächst den Code unter [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group) ausführen.

### <a name="get-a-test-image"></a>Abrufen eines Testbilds

Der folgende Code sucht im Stammverzeichnis des Projekts nach dem Bild _test-image-person-group.jpg_ und erkennt die Gesichter im Bild. Dieses Bild befindet sich am gleichen Speicherort wie die für die Verwaltung von **PersonGroup** verwendeten Bilder: https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/images.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_identify_testimage)]

### <a name="identify-faces"></a>Identifizieren von Gesichtern

Die Methode **identify** verwendet ein Array erkannter Gesichter und vergleicht diese mit einem **PersonGroup**-Objekt. Stimmt ein erkanntes Gesicht mit einem **Person**-Objekt überein, wird das Ergebnis gespeichert. Dieser Code gibt detaillierte Übereinstimmungsergebnisse in der Konsole aus.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_identify)]

## <a name="verify-faces"></a>Überprüfen von Gesichtern

Der Überprüfungsvorgang (Verify) akzeptiert eine Gesichtserkennungs-ID und entweder eine andere Gesichtserkennungs-ID oder ein **Person**-Objekt und ermittelt, ob sie zu derselben Person gehören.

Mit dem folgenden Code werden Gesichter in zwei Quellbildern erkannt und dann anhand eines Gesichts überprüft, das in einem Zielbild erkannt wurde.

### <a name="get-test-images"></a>Abrufen von Testbildern

Die folgenden Codeblöcke deklarieren Variablen, die für den Überprüfungsvorgang auf die Quell- und Zielbilder verweisen.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify_baseurl)]

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify_photos)]

### <a name="detect-faces-for-verification"></a>Erkennen von Gesichtern für die Überprüfung

Mit dem folgenden Code werden Gesichter in den Quell- und Zielbildern erkannt und in Variablen gespeichert.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify_detect)]

### <a name="get-verification-results"></a>Abrufen der Überprüfungsergebnisse

Der folgende Code vergleicht jedes Quellbild mit dem Zielbild und gibt eine Meldung mit dem Hinweis aus, ob sie zur gleichen Person gehören.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie Ihre Anwendung zur Gesichtserkennung aus dem Anwendungsverzeichnis mit dem `python`-Befehl aus.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli#clean-up-resources)

Falls Sie in dieser Schnellstartanleitung ein **PersonGroup**-Objekt erstellt haben und dieses löschen möchten, führen Sie in Ihrem Skript den folgenden Code aus:

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_deletegroup)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie die Gesichtserkennungs-Clientbibliothek für Python für einfache Gesichtserkennungsaufgaben verwenden. In der Referenzdokumentation finden Sie weitere Informationen zur Bibliothek.

> [!div class="nextstepaction"]
> [Referenz zur Gesichtserkennungs-API (Python)](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/?view=azure-python)

* [Was ist der Gesichtserkennungsdienst?](../../overview.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/Face/FaceQuickstart.py).
