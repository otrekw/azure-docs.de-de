---
title: 'Anleitung: Verwenden von Sprachgenerierungsvorlagen für Sprachantworten – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie Sprachgenerierungsvorlagen mit benutzerdefinierten Befehlen verwenden können. Sprachgenerierungsvorlagen ermöglichen es Ihnen, die an den Client gesendeten Antworten benutzerdefiniert anzupassen und Varianzen in den Antworten einzuführen.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 0cbc57922b31f1b3879bb2cad8a988a1ba4cc368
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85307241"
---
# <a name="add-language-generation-templates-for-speech-responses"></a>Hinzufügen von Sprachgenerierungsvorlagen für Sprachantworten

In diesem Artikel erfahren Sie, wie Sie Sprachgenerierungsvorlagen mit benutzerdefinierten Befehlen verwenden können. Sprachgenerierungsvorlagen ermöglichen es Ihnen, die an den Client gesendeten Antworten benutzerdefiniert anzupassen und Varianzen in den Antworten einzuführen. Eine benutzerdefinierte Anpassung der Sprachgenerierung kann folgendermaßen erreicht werden:

- Verwenden von Sprachgenerierungsvorlagen
- Verwenden von adaptiven Ausdrücken

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die Schritte in den folgenden Artikeln durchgeführt haben:

> [!div class="checklist"]
> * [Vorgehensweise: Erstellen einer Anwendung mit einfachen Befehlen](./how-to-custom-commands-create-application-with-simple-commands.md)
> * [Vorgehensweise: Hinzufügen von Parametern zu Befehlen](./how-to-custom-commands-add-parameters-to-commands.md)

## <a name="language-generation-templates-overview"></a>Übersicht über Sprachgenerierungsvorlagen

Vorlagen für benutzerdefinierte Befehle basieren auf den [LG-Vorlagen](https://aka.ms/speech/cc-lg-format) des BotFrameworks. Da benutzerdefinierte Befehle bei Bedarf eine neue LG-Vorlage erstellen (d. h. für Sprachantworten in Parametern oder Aktionen), müssen Sie den Namen der LG-Vorlage nicht angeben. Anstatt also Ihre Vorlage wie folgt zu definieren:

 ```
    # CompletionAction
    - Ok, turning {OnOff} the {SubjectDevice}
    - Done, turning {OnOff} the {SubjectDevice}
    - Proceeding to turn {OnOff} {SubjectDevice}
 ```

Müssen Sie den Text der Vorlage nur wie folgt ohne den Namen definieren.

> [!div class="mx-imgBorder"]
> ![Vorlagen-Editor: Beispiel](./media/custom-commands/template-editor-example.png)


Diese Änderung führt eine Variation in den Sprachantworten ein, die an den Client gesendet werden. Also würde für dieselbe Äußerung die entsprechende Sprachantwort zufällig aus den angebotenen Optionen ausgewählt werden.

Die Nutzung der LG-Vorlagen ermöglicht es Ihnen auch, komplexe Sprachantworten für Befehle mit adaptiven Ausdrücken zu definieren. Weitere Informationen finden Sie unter [LG-Vorlagenformat](https://aka.ms/speech/cc-lg-format). Benutzerdefinierte Befehle unterstützen standardmäßig alle Funktionen mit den folgenden geringfügigen Unterschieden:

* In den LG-Vorlagen werden Entitäten als ${entityName} dargestellt. In den benutzerdefinierten Befehlen verwenden wir keine Entitäten, aber Parameter können als Variablen mit einer der beiden Darstellungen ${parameterName} oder {parameterName} verwendet werden.
* Vorlagenkomposition und -erweiterung werden in benutzerdefinierten Befehlen nicht unterstützt. Der Grund dafür ist, dass Sie nie die `.lg`-Datei direkt bearbeiten, sondern nur die Antworten von automatisch erstellten Vorlagen.
* Von LG eingefügte benutzerdefinierte Funktionen werden in benutzerdefinierten Befehlen nicht unterstützt. Vordefinierte Funktionen werden weiterhin unterstützt.
* Optionen (strict, replaceNull und lineBreakStyle) werden in benutzerdefinierten Befehlen nicht unterstützt.

## <a name="add-template-responses-to-turnonoff-command"></a>Hinzufügen von Vorlagenressourcen zum Befehl „TurnOnOff“

Ändern Sie den Befehl **TurnOnOff**, um einen neuen Parameter mit der folgenden Konfiguration hinzuzufügen:

| Einstellung            | Vorgeschlagener Wert       | 
| ------------------ | --------------------- | 
| Name               | `SubjectContext`         | 
| Is Global          | Deaktiviert             | 
| Erforderlich           | Deaktiviert               | 
| type               | String                |
| Standardwert      | `all` |
| Konfiguration      | Vordefinierte Eingabewerte aus einem internen Katalog akzeptieren | 
| Vordefinierte Eingabewerte | `room`, `bathroom`, `all`|

### <a name="modify-completion-rule"></a>Ändern einer Vervollständigungsregel

Bearbeiten Sie den Abschnitt **Aktionen** der vorhandenen Vervollständigungsregel **ConfirmationResponse**. Wechseln Sie im Popupfenster **Aktion bearbeiten** zu **Vorlagen-Editor**, und ersetzen Sie den Text durch das folgende Beispiel.

```
- IF: @{SubjectContext == "all" && SubjectDevice == "lights"}
    - Ok, turning all the lights {OnOff}
- ELSEIF: @{SubjectDevice == "lights"}
    - Ok, turning {OnOff} the {SubjectContext} {SubjectDevice}
- ELSE:
    - Ok, turning the {SubjectDevice} {OnOff}
    - Done, turning {OnOff} the {SubjectDevice}
```

**Trainieren** und **Testen** Sie Ihre Anwendungen wie folgt. Beachten Sie die Variation der Antworten aufgrund der Verwendung mehrerer Alternativen des Vorlagenwerts und auch die Verwendung adaptiver Ausdrücke.

* Eingabe: Schalte Fernseher ein
* Ausgabe: OK, Fernseher wird eingeschaltet
* Eingabe: Schalte Fernseher ein
* Ausgabe: Erledigt, Fernseher ist eingeschaltet
* Eingabe: Schalte das Licht aus
* Ausgabe: OK, gesamte Beleuchtung wird ausgeschaltet
* Eingabe: Schalte Raumbeleuchtung aus
* Ausgabe: OK, die Raumbeleuchtung wird ausgeschaltet

## <a name="use-custom-voice"></a>Verwenden von Custom Voice

Eine weitere Möglichkeit zum Anpassen von Antworten auf benutzerdefinierte Befehle ist die Auswahl einer benutzerdefinierten Ausgabestimme. Verwenden Sie die folgenden Schritte, um von der Standardstimme zu einer benutzerdefinierten Stimme zu wechseln.

1. Wählen Sie in Ihrer Anwendung für benutzerdefinierte Befehle die Option **Einstellungen** im linken Bereich aus.
1. Wählen Sie im mittleren Bereich **Custom Voice** aus.
1. Wählen Sie die gewünschte benutzerdefinierte oder öffentliche Stimme aus der Tabelle aus.
1. Wählen Sie **Speichern** aus.

> [!div class="mx-imgBorder"]
> ![Beispielsätze mit Parametern](media/custom-commands/select-custom-voice.png)

> [!NOTE]
> - Bei den **öffentlichen Stimmen** sind **neuronale Typen** nur für bestimmte Regionen verfügbar. Informationen zur Verfügbarkeit finden Sie unter [Standardstimmen und neuronale Stimmen nach Region/Endpunkt](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices).
> - Für **benutzerdefinierte Stimmen** können sie auf der Custom Voice-Projektseite erstellt werden. Mehr dazu erfahren Sie unter [Erste Schritte mit Custom Voice](./how-to-custom-voice.md)

Die Anwendung antwortet nun in der ausgewählten Stimme statt in der Standardstimme.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Integrieren Sie Ihre benutzerdefinierten Befehle mit dem Speech SDK](./how-to-custom-commands-setup-speech-sdk.md).