---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: f26f0ab6da398dcdee307f89b27cca780d08af85
ms.sourcegitcommit: 374d1533ea2f2d9d3f8b6e6a8e65c6a5cd4aea47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85838992"
---
## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Erstellen einer Azure Speech-Ressource<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)
> * Stellen Sie sicher, dass Sie Zugriff auf ein Mikrofon für die Audioaufnahme haben.

## <a name="source-code"></a>Quellcode

Erstellen Sie eine Datei mit dem Namen *quickstart.py*, und kopieren Sie den folgenden Python-Code in diese Datei.

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/from-microphone/quickstart.py#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Erläuterung zum Code

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>Erstellen und Ausführen der App

Sie können nun die Spracherkennung mit dem Speech-Dienst testen. 

Wenn Sie dies unter macOS ausführen und dies die erste von Ihnen erstellte Python-App ist, die ein Mikrofon verwendet, müssen Sie dem Terminal wahrscheinlich Zugriff auf das Mikrofon erteilen. Öffnen Sie **Systemeinstellungen**, und wählen Sie **Sicherheit und Datenschutz** aus. Wählen Sie als nächstes **Datenschutz** aus, und suchen Sie in der Liste **Mikrofon**. Wählen Sie abschließend **Terminal** aus, und speichern Sie. 

1. **Starten der App**: Geben Sie über die Befehlszeile Folgendes ein:
    ```bash
    python quickstart.py
    ```
2. **Starten der Erkennung**: Sie werden aufgefordert, etwas auf Englisch zu sagen. Ihre Spracheingabe wird an den Speech-Dienst gesendet, in Text transkribiert und in der Konsole ausgegeben.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
