---
title: 'Schnellstart: Synthetisieren von Sprache, C# (Unity) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie mit Unity und dem Speech SDK für Unity eine Sprachsyntheseanwendung erstellen. Mit dieser Anleitung können Sie Sprache aus Text zur Wiedergabe über den Lautsprecher Ihres Geräts in Echtzeit synthetisieren.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: yinhew
ms.openlocfilehash: 0934738c557ac6d26867546783797cf5d2b75056
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81275201"
---
> [!NOTE]
> Unity unterstützt Windows Desktop (x86 und x64) oder die universelle Windows-Plattform (x86, x64, ARM/ARM64) sowie Android (x86, ARM32/64) und iOS (x64-Simulator, ARM32 und ARM64).

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md?tabs=unity&pivots=programming-language-csharp)

## <a name="add-a-ui"></a>Hinzufügen einer Benutzeroberfläche

Wir fügen unserer Szene eine minimalistische Benutzeroberfläche mit einem Eingabefeld zur Eingabe des Texts für die Synthese, einer Schaltfläche zum Starten der Sprachsynthese und einem Textfeld zum Anzeigen des Ergebnisses hinzu.

* Im [Hierarchiefenster](https://docs.unity3d.com/Manual/Hierarchy.html) (standardmäßig auf der linken Seite) ist eine Beispielszene zu sehen, die Unity mit dem neuen Projekt erstellt hat.
* Wählen Sie am oberen Rand des **Hierarchiefensters** die Schaltfläche **Create** (Erstellen) und anschließend **UI** > **Input Field** (Benutzeroberfläche > Eingabefeld) aus.
* Durch diese Option werden drei Spielobjekte erstellt und im **Hierarchiefenster** angezeigt: ein Objekt vom Typ **Input Field** (Eingabefeld) in einem Objekt vom Typ **Canvas** sowie ein Objekt vom Typ **EventSystem** (Ereignissystem).
* [Navigieren Sie durch die Szenenansicht](https://docs.unity3d.com/Manual/SceneViewNavigation.html), um sich die Canvas und das Eingabefeld in der [Szenenansicht](https://docs.unity3d.com/Manual/UsingTheSceneView.html) genauer anzusehen.
* Wählen Sie im **Hierarchiefenster** das Objekt **Input Field** (Eingabefeld) aus, um dessen Einstellungen im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) anzuzeigen.
* Legen Sie die Eigenschaften **Pos X** und **Pos Y** auf **0** fest, um das Eingabefeld in der Mitte der Canvas zu positionieren.
* Wählen Sie am oberen Rand des **Hierarchiefensters** die Schaltfläche **Create** (Erstellen) aus. Wählen Sie **UI** > **Button** (Benutzeroberfläche > Schaltfläche) aus, um eine Schaltfläche zu erstellen.
* Wählen Sie im **Hierarchiefenster** das Objekt **Button** (Schaltfläche) aus, um dessen Einstellungen im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) anzuzeigen.
* Legen Sie die Eigenschaften **Pos X** und **Pos Y** auf **0** und **-48** fest. Legen Sie die Eigenschaften **Width** (Breite) und **Height** (Höhe) auf **160** und **30** fest, um sicherzustellen, dass sich die Schaltfläche und das Eingabefeld nicht überlappen.
* Wählen Sie am oberen Rand des **Hierarchiefensters** die Schaltfläche **Create** (Erstellen) aus. Wählen Sie **UI** > **Text** (Benutzeroberfläche > Text) aus, um ein Textfeld zu erstellen.
* Wählen Sie im **Hierarchiefenster** das Objekt **Text** aus, um dessen Einstellungen im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) anzuzeigen.
* Legen Sie die Eigenschaften **Pos X** und **Pos Y** auf **0** und **80** fest. Legen Sie die Eigenschaften **Width** (Breite) und **Height** (Höhe) auf **320** und **80** fest, um sicherzustellen, dass sich die Schaltfläche und das Eingabefeld nicht überlappen.
* Wählen Sie am oberen Rand des **Hierarchiefensters** die Schaltfläche **Create** (Erstellen) aus. Wählen Sie **Audio** > **Audio Source** (Audio > Audioquelle) aus, um eine Audioquelle zu erstellen.

Die Benutzeroberfläche sollte dann in etwa wie folgt aussehen:

[![Screenshot der Schnellstart-Benutzeroberfläche im Unity-Editor](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-unity-ui-inline.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-unity-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Wählen Sie im [Projektfenster](https://docs.unity3d.com/Manual/ProjectView.html) (standardmäßig links unten) die Schaltfläche **Create** (Erstellen) und anschließend **C# script** (C#-Skript) aus. Nennen Sie das Skript `HelloWorld`.

1. Doppelklicken Sie auf das Skript, um es zu bearbeiten.

   > [!NOTE]
   > Sie können konfigurieren, welcher Code-Editor gestartet wird, indem Sie **Edit** > **Preferences** (Bearbeiten > Einstellungen) auswählen. Weitere Informationen finden Sie im [Unity-Benutzerhandbuch](https://docs.unity3d.com/Manual/Preferences.html).

1. Ersetzen Sie den gesamten Code durch Folgendes:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/unity/text-to-speech/Assets/Scripts/HelloWorld.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel für den Speech-Dienst.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](~/articles/cognitive-services/Speech-Service/regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung der kostenlosen Testversion ist die Region beispielsweise `westus`.

1. Speichern Sie die vorgenommenen Skriptänderungen.

1. Kehren Sie zum Unity-Editor zurück, und fügen Sie das Skript als Komponente einem Ihrer Spielobjekte hinzu.

   * Wählen Sie im **Hierarchiefenster** das Objekt **Canvas** aus, um dessen Einstellungen im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) zu öffnen.
   * Wählen Sie im **Inspektorfenster** die Schaltfläche **Add Component** (Komponente hinzufügen) aus. Suchen Sie anschließend nach dem Skript `HelloWorld`, das Sie zuvor erstellt haben, und fügen Sie es hinzu.
   * Die Hello World-Komponente besitzt vier nicht initialisierte Eigenschaften: **Output Text** (Ausgabetext), **Input Field** (Eingabefeld), **Speak Button** (Schaltfläche zum Starten des Sprechens) und **Audio Source** (Audioquelle). Diese entsprechen öffentlichen Eigenschaften der `HelloWorld`-Klasse.
     Klicken Sie zum Verknüpfen dieser Eigenschaften auf die Objektauswahl (kleines Kreissymbol rechts neben der Eigenschaft). Wählen Sie die zuvor erstellten Text- und Schaltflächenobjekte aus.

     > [!NOTE]
     > Das Eingabefeld und die Schaltfläche haben auch ein geschachteltes Textobjekt. Achten Sie darauf, dass Sie nicht versehentlich dieses Objekt für die Textausgabe auswählen. Alternativ können Sie im **Inspektorfenster** die Textobjekte umbenennen, die das Feld **Name** verwenden, um sie nicht zu verwechseln.

## <a name="run-the-application-in-the-unity-editor"></a>Ausführen der Anwendung im Unity-Editor

* Wählen Sie auf der Symbolleiste des Unity-Editors (unterhalb der Menüleiste) auf die Schaltfläche **Play** (Spielen).
* Wenn die App gestartet wird, geben Sie Text in das Eingabefeld ein, und wählen Sie die Schaltfläche aus. Ihr Text wird an den Speech-Dienst übermittelt und in Sprache synthetisiert, die dann über Ihren Lautsprecher ausgegeben wird.

  [![Screenshot der ausgeführten Schnellstart-App im Unity-Spielefenster](~/articles/cognitive-services/speech-service/media/sdk/qs-tts-csharp-unity-output-inline.png)](~/articles/cognitive-services/speech-service/media/sdk/qs-tts-csharp-unity-output-expanded.png#lightbox)

* Überprüfen Sie das [Konsolenfenster](https://docs.unity3d.com/Manual/Console.html) auf Debugmeldungen.

## <a name="additional-options-to-run-this-application"></a>Zusätzliche Optionen für die Anwendungsausführung

Diese Anwendung kann auch für Android, als eigenständige Windows-App oder als UWP-Anwendung bereitgestellt werden.
Die Konfiguration für diese zusätzlichen Ziele wird in unserem [Beispielrepository](https://aka.ms/csspeech/samples) im Ordner „quickstart/csharp-unity“ beschrieben.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Weitere Informationen

- [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) (Erstellen einer benutzerdefinierten Stimme)
- [Aufzeichnen benutzerdefinierter Stimmbeispiele](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
