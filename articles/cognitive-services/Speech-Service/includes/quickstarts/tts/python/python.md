---
title: 'Schnellstart: Synthetisieren von Sprache, Python – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Speech SDK Sprache in Python synthetisieren.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: dapine
ms.openlocfilehash: 239bb1e43b6a89ed1e8a18d544cf646b5f2de9b9
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2020
ms.locfileid: "80671126"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)

## <a name="support-and-updates"></a>Support und Updates

Updates für das Speech SDK-Paket für Python werden über PyPI verteilt und in den [Versionshinweisen](~/articles/cognitive-services/Speech-Service/releasenotes.md) angekündigt.
Wenn eine neue Version verfügbar ist, können Sie mit dem Befehl `pip install --upgrade azure-cognitiveservices-speech` auf diese Version aktualisieren.
Die derzeit installierte Version können Sie der Variablen `azure.cognitiveservices.speech.__version__` entnehmen.

Falls Probleme auftreten oder ein Feature fehlt, sehen Sie sich die [Support- und Hilfeoptionen](~/articles/cognitive-services/Speech-Service/support.md) an.

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Erstellen einer Python-Anwendung, die das Speech SDK verwendet

### <a name="run-the-sample"></a>Ausführen des Beispiels

Sie können den [Beispielcode](#sample-code) aus diesem Schnellstart in eine Quelldatei (`quickstart.py`) kopieren und in Ihrer integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) oder in der Konsole ausführen:

```sh
python quickstart.py
```

Alternativ können Sie dieses Schnellstarttutorial als [Jupyter](https://jupyter.org) Notebook aus dem [Beispielrepository für das Speech SDK](https://aka.ms/csspeech/samples) herunterladen und als Notebook ausführen.

### <a name="sample-code"></a>Beispielcode

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/text-to-speech/quickstart.py#code)]

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Installieren und Verwenden des Speech SDK mit Visual Studio Code

1. Laden Sie eine 64-Bit-Version (3.5 bis 3.8) von [Python](https://www.python.org/downloads/) herunter, und installieren Sie sie auf Ihrem Computer.
1. Laden Sie [Visual Studio Code](https://code.visualstudio.com/Download) herunter, und installieren Sie die Software.
1. Öffnen Sie Visual Studio Code, und installieren Sie die Python-Erweiterung. Wählen Sie im Menü **Datei** > **Einstellungen** > **Erweiterungen**. Suchen Sie nach **Python**.

   ![Installieren der Python-Erweiterung](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-extension.png)

1. Erstellen Sie einen Ordner zum Speichern des Projekts. Ein Beispiel ist die Verwendung von Windows-Explorer.
1. Wählen Sie in Visual Studio Code das Symbol **Datei**. Öffnen Sie dann den von Ihnen erstellten Ordner.

   ![Öffnen eines Ordners](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-open-folder.png)

1. Erstellen Sie eine neue Python-Quelldatei (`speechsdk.py`), indem Sie das Symbol „Neue Datei“ auswählen.

   ![Erstellen von Dateien](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-newfile.png)

1. Kopieren Sie den [Python-Code](#sample-code), fügen Sie ihn in die neu erstellte Datei ein, und speichern Sie die Datei.
1. Fügen Sie Ihre Abonnementinformationen für den Speech-Dienst ein.
1. Wenn ein Python-Interpreter ausgewählt wurde, wird er auf der linken Seite der Statusleiste am unteren Rand des Fensters angezeigt.
   Zeigen Sie andernfalls eine Liste der verfügbaren Python-Interpreter an. Öffnen Sie die Befehlspalette (STRG+UMSCHALT+P), und geben Sie **Python: Select Interpreter** ein. Wählen Sie einen passenden Interpreter aus.
1. Sie können das Speech SDK-Paket für Python über Visual Studio Code installieren. Führen Sie diesen Schritt aus, wenn es für den ausgewählten Python-Interpreter noch nicht installiert wurde.
   Öffnen Sie zum Installieren des Speech SDK-Pakets ein Terminal. Rufen Sie erneut die Befehlspalette (STRG+UMSCHALT+P) auf, und geben Sie **Terminal: Neues integriertes Terminal erstellen** eingeben.
   Geben Sie im Terminal, das geöffnet wird, den Befehl `python -m pip install azure-cognitiveservices-speech` bzw. den entsprechenden Befehl für Ihr System ein.
1. Klicken Sie zum Ausführen des Beispielcodes mit der rechten Maustaste an einer beliebigen Stelle im Editor. Wählen Sie **Run Python File in Terminal** (Python-Datei im Terminal ausführen) aus.
   Geben Sie bei entsprechender Aufforderung Text ein. Die synthetisierten Audiodaten werden kurz danach wiedergegeben.

   ![Ausführen eines Beispiels](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-run-tts.png)

Falls Sie Probleme beim Ausführen dieser Anweisungen haben, können Sie das ausführlichere [Visual Studio Code-Tutorial für Python](https://code.visualstudio.com/docs/python/python-tutorial) zurate ziehen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>Weitere Informationen

- [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) (Erstellen einer benutzerdefinierten Stimme)
- [Aufzeichnen benutzerdefinierter Stimmbeispiele](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
