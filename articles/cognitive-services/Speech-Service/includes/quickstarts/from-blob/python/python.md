---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: 3ca50a9bad36e0174dc4ee0059c9d01fcc18a5f1
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400975"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit dem Azure-Portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

## <a name="download-and-install-the-api-client-library"></a>Herunterladen und Installieren der API-Clientbibliothek

Um das Beispiel auszuführen, müssen Sie die Python-Bibliothek für die REST-API generieren, wozu Sie [Swagger](https://swagger.io) verwenden.

Führen Sie die folgenden Schritte für die Installation aus:

1. Gehe zu https://editor.swagger.io.
1. Klicken Sie auf **Datei** und dann auf **URL importieren**.
1. Geben Sie die Swagger-URL ein, einschließlich der Region Ihres Abonnements für den Speech-Dienst: `https://<your-region>.cris.ai/docs/v2.0/swagger`.
1. Klicken Sie auf **Generate Client** (Client generieren), und wählen Sie **Python** aus.
1. Speichern Sie die Clientbibliothek.
1. Extrahieren Sie die heruntergeladene Datei „python-client-generated.zip“ an einer beliebigen Stelle im Dateisystem.
1. Installieren Sie das extrahierte Python-Clientmodul mithilfe von PIP in ihrer Python-Umgebung: `pip install path/to/package/python-client`.
1. Das installierte Paket hat den Namen `swagger_client`. Sie können mithilfe des Befehls `python -c "import swagger_client"` überprüfen, ob die Installation funktioniert hat.

> [!NOTE]
> Aufgrund eines [bekannten Fehlers in der automatischen Swagger-Generierung](https://github.com/swagger-api/swagger-codegen/issues/7541) könnten beim Importieren des `swagger_client`-Pakets Fehler auftreten.
> Diese können Sie beheben, indem Sie die Zeile mit dem Inhalt
> ```py
> from swagger_client.models.model import Model  # noqa: F401,E501
> ```
> aus der Datei `swagger_client/models/model.py` und die Zeile mit dem Inhalt
> ```py
> from swagger_client.models.inner_error import InnerError  # noqa: F401,E501
> ```
> aus der Datei `swagger_client/models/inner_error.py` innerhalb des installierten Pakets löschen. Die Fehlermeldung informiert Sie darüber, wo sich diese Dateien für Ihre Installation befinden.

## <a name="install-other-dependencies"></a>Installieren anderer Abhängigkeiten

Im Beispiel wird die `requests`-Bibliothek verwendet. Sie können sie installieren mithilfe des Befehls

```bash
pip install requests
```

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=1-2,7-34,115-119)]

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="create-and-configure-an-http-client"></a>Erstellen und Konfigurieren eines HTTP-Clients
Als Erstes benötigen wir einen HTTP-Client, der über eine korrekte Basis-URL und einen Authentifizierungssatz verfügt.
Fügen Sie diesen Code in `transcribe` [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=37-45)] ein.

## <a name="generate-a-transcription-request"></a>Generieren einer Transkriptionsanforderung
Als Nächstes generieren wir die Transkriptionsanforderung. Fügen Sie diesen Code zu `transcribe` [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=52-54)] hinzu.

## <a name="send-the-request-and-check-its-status"></a>Senden der Anforderung und Überprüfen ihres Status
Nun senden wir die Anforderung an den Speech-Dienst und überprüfen den ersten Antwortcode. Mit diesem Antwortcode wird lediglich angegeben, ob der Dienst die Anforderung erhalten hat. Der Dienst gibt in den Antwortheadern eine URL zurück, die für den Speicherort steht, an dem der Transkriptionsstatus gespeichert wird.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=65-73)]

## <a name="wait-for-the-transcription-to-complete"></a>Warten auf den Abschluss der Transkription
Da die Transkription vom Dienst asynchron verarbeitet wird, müssen wir regelmäßig den Status abrufen. Wir führen alle fünf Sekunden eine Überprüfung durch.

Wir werden alle Transkriptionen auflisten, die von dieser Speech-Dienstressource verarbeitet werden, und nach der von uns erstellten suchen.

Hier ist der Abrufcode mit der Statusanzeige für alle Vorgänge mit Ausnahme des erfolgreichen Abschlusses angegeben. Dies führen wir als Nächstes durch.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=75-94,99-112)]

## <a name="display-the-transcription-results"></a>Anzeigen der Transkriptionsergebnisse
Nachdem der Dienst die Transkription erfolgreich abgeschlossen hat, werden die Ergebnisse unter einer anderen URL gespeichert, die wir in der Statusantwort erhalten.

Hier erhalten wir den JSON-Ergebniscode und zeigen ihn an.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=95-98)]

## <a name="check-your-code"></a>Überprüfen des Codes
Ihr Code sollte nun wie folgt aussehen: (Wir haben dieser Version einige Kommentare hinzugefügt.) [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=1-118)]

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen der App

Sie können nun Ihre App erstellen und die Spracherkennung mit dem Speech-Dienst testen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
