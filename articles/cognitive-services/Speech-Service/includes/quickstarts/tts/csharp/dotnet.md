---
title: 'Schnellstart: Synthetisieren von Sprache, C# (Windows) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie mit .NET Framework für Windows und dem Speech SDK eine Konsolenanwendung für die Sprachsynthese erstellen. Anschließend können Sie Sprache aus Text synthetisieren und in Echtzeit über Ihren Lautsprecher ausgeben.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/27/2019
ms.author: yinhew
ms.openlocfilehash: b615ba8085650e9aa686fb4a229d9752c4f6e2ce
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78924850"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md?tabs=dotnet)

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie **Program.cs**, und ersetzen Sie den automatisch generierten Code durch dieses Beispiel:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/text-to-speech/helloworld/Program.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel für den Speech-Dienst.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](~/articles/cognitive-services/Speech-Service/regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung des kostenlosen Testabonnements ist die Region beispielsweise `westus`.

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus.

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung **helloworld** zu starten.

1. Geben Sie einen englischen Ausdruck oder Satz ein. Die Anwendung überträgt Ihren Text an den Speech-Dienst, der die synthetisierte Sprache zur Wiedergabe über den Lautsprecher an die Anwendung sendet.

   ![Benutzeroberfläche für die Sprachsynthese](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-dotnet-windows-console-output.png)

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>Weitere Informationen

- [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) (Erstellen einer benutzerdefinierten Stimme)
- [Aufzeichnen benutzerdefinierter Stimmbeispiele](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
