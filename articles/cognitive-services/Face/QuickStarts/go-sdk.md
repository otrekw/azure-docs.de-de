---
title: 'Schnellstart: Clientbibliothek zur Gesichtserkennung für Go | Microsoft-Dokumentation'
description: Hier erhalten Sie Informationen zu den ersten Schritten mit der Clientbibliothek zur Gesichtserkennung für Go.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: ''
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: 2db40150167a8f16242b2feb15b77820fa1970a9
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76961227"
---
# <a name="quickstart-face-client-library-for-go"></a>Schnellstart: Clientbibliothek zur Gesichtserkennung für Go

Hier erhalten Sie Informationen zu den ersten Schritten mit der Clientbibliothek zur Gesichtserkennung für Go. Führen Sie die nachfolgenden Schritte aus, um die Bibliothek zu installieren und unsere Beispiele für grundlegende Aufgaben zu testen. Über den Gesichtserkennungsdienst haben Sie Zugriff auf erweiterte Algorithmen für die Erkennung von menschlichen Gesichtern in Bildern.

Die Gesichtserkennungsdienst-Clientbibliothek für Go kann für Folgendes verwendet werden:

* [Gesichtserkennung in einem Bild](#detect-faces-in-an-image)
* [Suchen ähnlicher Gesichter](#find-similar-faces)
* [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group)
* [Identifizieren eines Gesichts](#identify-a-face)
* [Erstellen einer Momentaufnahme für die Datenmigration](#take-a-snapshot-for-data-migration)

[Referenzdokumentation](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v1.0/face) | [SDK-Download](https://github.com/Azure/azure-sdk-for-go)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: [Kostenloses Azure-Konto](https://azure.microsoft.com/free/)
* Aktuelle Version von [Go](https://golang.org/dl/)

## <a name="set-up"></a>Einrichten

### <a name="create-a-face-azure-resource"></a>Erstellen einer Azure-Ressource für die Gesichtserkennung 

Wenn Sie den Gesichtserkennungsdienst verwenden möchten, müssen Sie zunächst eine Azure-Ressource erstellen. Wählen Sie einen geeigneten Ressourcentyp aus:

* Eine [Testressource](https://azure.microsoft.com/try/cognitive-services/#decision) (kein Azure-Abonnement erforderlich): 
    * Kostenlos und sieben Tage lang gültig. Nach der Registrierung stehen ein Testschlüssel und ein Endpunkt auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung. 
    * Diese Option eignet sich perfekt, wenn Sie den Gesichtserkennungsdienst testen möchten, aber über kein Azure-Abonnement verfügen.
* Eine [Ressource für den Gesichtserkennungsdienst](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace):
    * Verfügbar über das Azure-Portal, bis Sie die Ressource löschen.
    * Verwenden Sie den Tarif „Free“, um den Dienst zu testen, und führen Sie später für die Produktion ein Upgrade auf einen kostenpflichtigen Tarif durch.
* Eine [Ressource für mehrere Dienste](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne):
    * Verfügbar über das Azure-Portal, bis Sie die Ressource löschen.  
    * Verwenden Sie für Ihre Anwendungen den gleichen Schlüssel und Endpunkt über mehrere Cognitive Services-Instanzen hinweg.

### <a name="create-an-environment-variable"></a>Erstellen einer Umgebungsvariablen

>[!NOTE]
> Nach dem 1. Juli 2019 erstellte Endpunkte für Ressourcen, bei denen es sich nicht um Testressourcen handelt, verwenden das unten gezeigte benutzerdefinierte Format für Subdomänen. Weitere Informationen und eine vollständige Liste mit regionalen Endpunkten finden Sie unter [Benutzerdefinierte Unterdomänennamen für Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Erstellen Sie unter Verwendung des Schlüssels und des Endpunkts der von Ihnen erstellten Ressource zwei Umgebungsvariablen für die Authentifizierung:
* `FACE_SUBSCRIPTION_KEY`: Der Ressourcenschlüssel zum Authentifizieren Ihrer Anforderungen.
* `FACE_ENDPOINT`: Der Ressourcenendpunkt zum Senden von API-Anforderungen. Er sieht wie folgt aus: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

Führen Sie die Schritte für Ihr Betriebssystem aus:
<!-- replace the below endpoint and key examples -->
#### <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
setx FACE_SUBSCRIPTION_KEY <replace-with-your-product-name-key>
setx FACE_ENDPOINT <replace-with-your-product-name-endpoint>
```

Starten Sie das Konsolenfenster neu, nachdem Sie die Umgebungsvariable hinzugefügt haben.

#### <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export FACE_SUBSCRIPTION_KEY=<replace-with-your-product-name-key>
export FACE_ENDPOINT=<replace-with-your-product-name-endpoint>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source ~/.bashrc` aus, damit die Änderungen wirksam werden.

#### <a name="macostabunix"></a>[macOS](#tab/unix)

Bearbeiten Sie `.bash_profile`, und fügen Sie die Umgebungsvariable hinzu:

```bash
export FACE_SUBSCRIPTION_KEY=<replace-with-your-product-name-key>
export FACE_ENDPOINT=<replace-with-your-product-name-endpoint>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source .bash_profile` aus, damit die Änderungen wirksam werden.
***

### <a name="create-a-go-project-directory"></a>Erstellen eines Go-Projektverzeichnisses

Erstellen Sie in einem Konsolenfenster (cmd, PowerShell, Terminal, Bash) einen neuen Arbeitsbereich für Ihr Go-Projekt namens `my-app`, und navigieren Sie dorthin.

```
mkdir -p my-app/{src, bin, pkg}  
cd my-app
```

Ihr Arbeitsbereich enthält drei Ordner:

* **src**: Das Verzeichnis für Quellcode und Pakete. Alle mit dem Befehl `go get` installierten Pakete werden in diesem Ordner platziert.
* **pkg**: Das Verzeichnis für die kompilierten Go-Paketobjekte. Diese Dateien haben alle die Erweiterung `.a`.
* **bin**: Das Verzeichnis für die ausführbaren Binärdateien, die bei der Ausführung von `go install` erstellt werden.

> [!TIP]
> Weitere Informationen zur Struktur eines Go-Arbeitsbereichs finden Sie in der [Dokumentation zur Go-Sprache](https://golang.org/doc/code.html#Workspaces). Dieses Handbuch enthält Informationen zum Festlegen von `$GOPATH` und `$GOROOT`.

### <a name="install-the-client-library-for-go"></a>Installieren der Clientbibliothek für Go

Installieren Sie als Nächstes die Clientbibliothek für Go:

```bash
go get -u https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v1.0/face
```

Bei Verwendung von DEP können Sie alternativ Folgendes in Ihrem Repository ausführen:

```bash
dep ensure -add https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v1.0/face
```

### <a name="create-a-go-application"></a>Erstellen einer Go-Anwendung

Erstellen Sie als Nächstes im Verzeichnis **src** eine Datei mit dem Namen `sample-app.go`:

```bash
cd src
touch sample-app.go
```

Öffnen Sie `sample-app.go` in Ihrer bevorzugten IDE oder in Ihrem bevorzugten Text-Editor. Fügen Sie dann den Paketnamen hinzu, und importieren Sie die folgenden Bibliotheken:

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_imports)]

Beginnen Sie nun damit, Code für verschiedene Vorgänge des Gesichtserkennungsdiensts hinzuzufügen.

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen verarbeiten einige der Hauptfunktionen des Go SDK für den Gesichtserkennungsdienst:

|Name|Beschreibung|
|---|---|
|[BaseClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#BaseClient) | Diese Klasse stellt Ihre Autorisierung für die Verwendung des Gesichtserkennungsdiensts dar. Sie benötigen sie für alle Gesichtserkennungsfunktionen. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Generieren von Instanzen anderer Klassen. |
|[Client](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client)|Diese Klasse behandelt die grundlegenden Erkennungs- und Wiedererkennungsaufgaben, die Sie für menschliche Gesichter ausführen können. |
|[DetectedFace](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#DetectedFace)|Diese Klasse stellt alle Daten dar, die in einem einzelnen Gesicht in einem Bild erkannt wurden. Sie können damit ausführliche Informationen zum Gesicht abrufen.|
|[ListClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#ListClient)|Diese Klasse verwaltet die in der Cloud gespeicherten **FaceList**-Konstrukte, die verschiedene Gesichter speichern. |
|[PersonGroupPersonClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupPersonClient)| Diese Klasse verwaltet die in der Cloud gespeicherten **Person**-Konstrukte, die eine Gruppe von Gesichtern speichern, die zu einer einzelnen Person gehören.|
|[PersonGroupClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupClient)| Diese Klasse verwaltet die in der Cloud gespeicherten **PersonGroup**-Konstrukte, die verschiedene **Person**-Objekte speichern. |
|[SnapshotClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#SnapshotClient)|Mit dieser Klasse wird die Funktion für Momentaufnahmen verwaltet. Sie können mit ihr vorübergehend alle cloudbasierten Gesichtserkennungsdaten speichern und zu einem neuen Azure-Abonnement migrieren. |

## <a name="code-examples"></a>Codebeispiele

In den folgenden Codebeispielen werden einfache Aufgaben mithilfe der Gesichtserkennungsdienst-Clientbibliothek für Go veranschaulicht:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Gesichtserkennung in einem Bild](#detect-faces-in-an-image)
* [Suchen ähnlicher Gesichter](#find-similar-faces)
* [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group)
* [Identifizieren eines Gesichts](#identify-a-face)
* [Erstellen einer Momentaufnahme für die Datenmigration](#take-a-snapshot-for-data-migration)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

> [!NOTE] 
> In dieser Schnellstartanleitung wird davon ausgegangen, dass Sie für den Schlüssel und den Endpunkt der Gesichtserkennung (`FACE_SUBSCRIPTION_KEY` bzw. `FACE_ENDPOINT`) [Umgebungsvariablen erstellt haben](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication).

Erstellen Sie eine Funktion vom Typ **main**, und fügen Sie ihr den folgenden Code hinzu, um einen Client mit Ihrem Endpunkt und Schlüssel zu instanziieren. Sie erstellen ein Objekt vom Typ **[CognitiveServicesAuthorizer](https://godoc.org/github.com/Azure/go-autorest/autorest#CognitiveServicesAuthorizer)** mit Ihrem Schlüssel und verwenden es mit Ihrem Endpunkt, um ein Objekt vom Typ **[Client](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client)** zu erstellen. Durch diesen Code wird auch ein Kontextobjekt instanziiert, das für die Erstellung von Clientobjekten benötigt wird. Darüber hinaus wird ein Remotespeicherort definiert, an dem sich einige der Beispielbilder dieser Schnellstartanleitung befinden.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_main_client)]


## <a name="detect-faces-in-an-image"></a>Gesichtserkennung in einem Bild

Fügen Sie der Methode **main** den folgenden Code hinzu. Dieser Code dient zum Definieren eines Remotebeispielbilds sowie zum Angeben der Gesichtsmerkmale, die aus dem Bild extrahiert werden sollen. Darüber hinaus wird das gewünschte KI-Modell zum Extrahieren von Daten aus den erkannten Gesichtern angegeben. Weitere Informationen zu diesen Optionen finden Sie unter [Angeben eines Gesichtserkennungsmodells](../Face-API-How-to-Topics/specify-recognition-model.md). Abschließend wird mithilfe der Methode **[DetectWithURL](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client.DetectWithURL)** der Gesichtserkennungsvorgang für das Bild ausgeführt, und die Ergebnisse werden im Programmspeicher gespeichert.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_detect)]

### <a name="display-detected-face-data"></a>Anzeigen erkannter Gesichtsdaten

Im nächsten Codeblock werden die Attribute des ersten Elements aus dem Array von Objekten des Typs **[DetectedFace](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#DetectedFace)** in der Konsole ausgegeben. Bei Verwendung eines Bilds mit mehreren Gesichtern sollte stattdessen das Array durchlaufen werden.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_detect_display)]

## <a name="find-similar-faces"></a>Suchen ähnlicher Gesichter

Der folgende Code verwendet ein einzelnes erkanntes Gesicht (Quelle) und durchsucht eine Reihe anderer Gesichter (Ziel) auf Übereinstimmungen. Bei einer Übereinstimmung wird die ID des entsprechenden Gesichts in der Konsole ausgegeben.

### <a name="detect-faces-for-comparison"></a>Erkennen von Gesichtern zum Vergleich

Speichern Sie zunächst einen Verweis auf das Gesicht, das im Abschnitt [Gesichtserkennung in einem Bild](#detect-faces-in-an-image) erkannt wurde. Dieses Gesicht wird als Quelle verwendet.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar_single_ref)]

Geben Sie anschließend den folgenden Code ein, um eine Gruppe von Gesichtern in einem anderen Bild zu erkennen. Diese Gesichter werden als Ziel verwendet.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar_multiple_ref)]

### <a name="find-matches"></a>Suchen von Übereinstimmungen

Im folgenden Code wird die Methode **[FindSimilar](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client.FindSimilar)** verwendet, um nach allen Zielgesichtern zu suchen, die dem Quellgesicht entsprechen.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar)]

### <a name="print-matches"></a>Ausgeben von Übereinstimmungen

Mit dem folgenden Code werden Übereinstimmungsdetails in der Konsole ausgegeben:

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar_print)]


## <a name="create-and-train-a-person-group"></a>Erstellen und Trainieren einer Personengruppe

Für dieses Szenario müssen Sie die folgenden Bilder im Stammverzeichnis Ihres Projekts speichern: https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/images.

Diese Gruppe von Bildern enthält jeweils drei Bilder mit einem einzelnen Gesicht von drei verschiedenen Personen. Im Code werden drei Objekte vom Typ **PersonGroup Person** definiert und Bilddateien zugeordnet, die mit `woman`, `man` und `child` beginnen.

### <a name="create-persongroup"></a>Erstellen einer Personengruppe

Fügen Sie nach dem Herunterladen Ihrer Bilder am Ende der Methode **main** den folgenden Code hinzu. Mit diesem Code wird ein Objekt vom Typ **[PersonGroupClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupClient)** authentifiziert und dann zum Definieren einer neuen Personengruppe (**PersonGroup**) verwendet.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pg_setup)]

### <a name="create-persongroup-persons"></a>Erstellen von Objekten vom Typ „PersonGroup Person“

Im nächsten Codeblock wird ein Objekt vom Typ **[PersonGroupPersonClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupPersonClient)** authentifiziert und zum Definieren von drei neuen Objekten vom Typ **PersonGroup Person** verwendet. Diese Objekte stellen jeweils eine einzelne Person in der Bildergruppe dar.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pgp_setup)]

### <a name="assign-faces-to-persons"></a>Zuweisen von Gesichtern zu Personen

Der folgende Code sortiert die Bilder anhand ihres Präfixes, erkennt Gesichter und weist sie auf der Grundlage des Bilddateinamens dem jeweiligen Objekt vom Typ **PersonGroup Person** zu.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pgp_assign)]

### <a name="train-persongroup"></a>Trainieren von PersonGroup

Nach dem Zuweisen von Gesichtern muss die Personengruppe (**PersonGroup**) trainiert werden, damit die den einzelnen Objekten vom Typ **Person** zugewiesenen visuellen Merkmale erkannt werden können. Der folgende Code ruft die asynchrone **train**-Methode auf, ruft das Ergebnis ab und gibt den Status in der Konsole aus.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pg_train)]

## <a name="identify-a-face"></a>Identifizieren eines Gesichts

Der folgende Code sucht auf einem Bild mit mehreren Gesichtern nach der Identität der einzelnen Personen im Bild. Er vergleicht jedes erkannte Gesicht mit einem **PersonGroup**-Objekt, einer Datenbank mit verschiedenen **Person**-Objekten,deren Gesichtsmerkmale bekannt sind.

> [!IMPORTANT]
> Damit Sie dieses Beispiel ausführen können, müssen Sie zunächst den Code unter [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group) ausführen.

### <a name="get-a-test-image"></a>Abrufen eines Testbilds

Der folgende Code sucht im Stammverzeichnis des Projekts nach dem Bild _test-image-person-group.jpg_ und lädt es in den Programmspeicher. Das Bild befindet sich im gleichen Repository wie die Bilder, die im Abschnitt [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group) verwendet werden: https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/images.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id_source_get)]

### <a name="detect-source-faces-in-test-image"></a>Erkennen von Quellgesichtern im Testbild

Der nächste Codeblock dient zum Ausführen einer gewöhnlichen Gesichtserkennung für das Testbild, um alle Gesichter abzurufen und sie in einem Array zu speichern.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id_source_detect)]

### <a name="identify-faces"></a>Identifizieren von Gesichtern

Die Methode **[Identify](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client.Identify)** vergleicht das Array der erkannten Gesichter mit der angegebenen Personengruppe (**PersonGroup**), die in einem früheren Abschnitt definiert und trainiert wurde. Falls eine Übereinstimmung zwischen einem erkannten Gesicht und einer **Person** in der Gruppe erkannt wird, wird das Ergebnis gespeichert.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id)]

Anschließend werden detaillierte Übereinstimmungsergebnisse in der Konsole ausgegeben.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id_print)]


## <a name="verify-faces"></a>Überprüfen von Gesichtern

Der Überprüfungsvorgang (Verify) akzeptiert eine Gesichtserkennungs-ID und entweder eine andere Gesichtserkennungs-ID oder ein **Person**-Objekt und ermittelt, ob sie zu derselben Person gehören.

Mit dem folgenden Code werden Gesichter in zwei Quellbildern erkannt und dann jeweils anhand eines Gesichts überprüft, das in einem Zielbild erkannt wurde.

### <a name="get-test-images"></a>Abrufen von Testbildern

In den folgenden Codeblöcke werden Variablen deklariert, die auf die Ziel- und Quellbilder für den Überprüfungsvorgang verweisen.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver_images)]

### <a name="detect-faces-for-verification"></a>Erkennen von Gesichtern für die Überprüfung

Mit dem folgenden Code werden Gesichter in den Quell- und Zielbildern erkannt und in Variablen gespeichert.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver_detect_source)]

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver_detect_target)]

### <a name="get-verification-results"></a>Abrufen der Überprüfungsergebnisse

Der folgende Code vergleicht jedes Quellbild mit dem Zielbild und gibt eine Meldung mit dem Hinweis aus, ob sie zur gleichen Person gehören.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver)]


## <a name="take-a-snapshot-for-data-migration"></a>Erstellen einer Momentaufnahme für die Datenmigration

Mit dem Momentaufnahmefeature können Sie Ihre gespeicherten Gesichtserkennungsdaten (etwa ein trainiertes **PersonGroup**-Objekt) in ein anderes Abonnement für die Gesichtserkennung von Azure Cognitive Services verschieben. Sie können dieses Feature beispielsweise verwenden, wenn Sie ein Objekt vom Typ **PersonGroup** mit einem kostenlosen Testabonnement erstellt haben und dieses nun zu Ihrem kostenpflichtigen Abonnement migrieren möchten. Eine umfassende Übersicht über das Feature für Momentaufnahmen finden Sie unter [Migrieren Ihrer Gesichtserkennungsdaten in ein anderes Abonnement für die Gesichtserkennung](../Face-API-How-to-Topics/how-to-migrate-face-data.md).

In diesem Beispiel wird das Objekt vom Typ **PersonGroup** migriert, das Sie unter [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group) erstellt haben. Sie können entweder zuerst diesen Abschnitt abschließen oder Ihre eigenen Konstrukte mit Gesichtserkennungsdaten verwenden.

### <a name="set-up-target-subscription"></a>Einrichten des Zielabonnements

Sie benötigen zunächst ein zweites Azure-Abonnement mit einer Gesichtserkennungsressource. Führen Sie dazu erneut die Schritte im Abschnitt [Einrichten](#set-up) aus. 

Erstellen Sie anschließend im oberen Bereich der Methode **main** die folgenden Variablen. Darüber hinaus müssen Sie neue Umgebungsvariablen für die Abonnement-ID Ihres Azure-Kontos sowie den Schlüssel, den Endpunkt und die Abonnement-ID Ihres neuen Kontos (Zielkonto) erstellen.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_target_client)]

Fügen Sie für die nächsten Schritte den Wert Ihrer Abonnement-ID in ein Array ein.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_target_id)]

### <a name="authenticate-target-client"></a>Authentifizieren des Zielclients

Speichern Sie weiter unten in Ihrem Skript Ihr ursprüngliches Clientobjekt als Quellclient, und authentifizieren Sie anschließend ein neues Clientobjekt für Ihr Zielabonnement. 

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_target_auth)]

### <a name="take-a-snapshot"></a>Erstellen einer Momentaufnahme

Im nächsten Schritt wird mithilfe von **[Take](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#SnapshotClient.Take)** die Momentaufnahme erstellt. Dadurch werden die Gesichtserkennungsdaten Ihres ursprünglichen Abonnements an einem temporären Cloudspeicherort gespeichert. Diese Methode gibt eine ID zurück, mit der Sie den Status des Vorgangs abfragen.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_take)]

Fragen Sie als Nächstes die ID ab, bis der Vorgang abgeschlossen ist.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_query)]

### <a name="apply-the-snapshot"></a>Anwenden der Momentaufnahme

Schreiben Sie Ihre neu hochgeladenen Gesichtserkennungsdaten mithilfe des Vorgangs **[Apply](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#SnapshotClient.Apply)** in Ihr Zielabonnement. Mit dieser Methode wird ebenfalls eine ID zurückgegeben.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_apply)]

Fragen Sie erneut die ID ab, bis der Vorgang abgeschlossen ist.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_apply_query)]

Nachdem Sie diese Schritte ausgeführt haben, können Sie über Ihr neues (Ziel-)Abonnement auf Ihre Konstrukte mit Gesichtserkennungsdaten zugreifen.

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Go-Anwendung mit dem Befehl `go run [arguments]` aus dem Anwendungsverzeichnis aus.

```bash
go run sample-app.go
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Falls Sie in dieser Schnellstartanleitung ein Objekt vom Typ **PersonGroup** erstellt haben und dieses löschen möchten, rufen Sie die Methode **[Delete](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupClient.Delete)** auf. Wenn Sie in dieser Schnellstartanleitung Daten mithilfe des Momentaufnahmefeatures migriert haben, müssen Sie auch das im Zielabonnement gespeicherte **PersonGroup**-Objekt löschen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie die Gesichtserkennungsbibliothek für Go für einfache Aufgaben verwenden. In der Referenzdokumentation finden Sie weitere Informationen zur Bibliothek.

> [!div class="nextstepaction"]
> [Referenz zur Gesichtserkennungs-API (Go)](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face)

* [Was ist der Gesichtserkennungsdienst?](../overview.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/Face/FaceQuickstart.go).
