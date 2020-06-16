---
title: Überprüfen der Datenqualität für Custom Speech – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Custom Speech bietet Tools zur visuellen Überprüfung der Erkennungsqualität eines Modells durch Vergleichen von Audiodaten mit dem entsprechenden Erkennungsergebnis. Sie können hochgeladene Audiodaten wiedergeben und bestimmen, ob das angegebene Erkennungsergebnis korrekt ist.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: erhopf
ms.openlocfilehash: e871d2c8e0fe00fa7db3144a787447163c82e62d
ms.sourcegitcommit: d7fba095266e2fb5ad8776bffe97921a57832e23
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84629038"
---
# <a name="inspect-custom-speech-data"></a>Überprüfen von Custom Speech-Daten

> [!NOTE]
> Auf dieser Seite wird vorausgesetzt, dass Sie [Prepare data for Custom Speech](how-to-custom-speech-test-data.md) (Vorbereiten von Testdaten für Custom Speech) gelesen und ein Dataset für die Überprüfung hochgeladen haben.

Custom Speech bietet Tools zur visuellen Überprüfung der Erkennungsqualität eines Modells durch Vergleichen von Audiodaten mit dem entsprechenden Erkennungsergebnis. Über das [Custom Speech-Portal](https://speech.microsoft.com/customspeech) können Sie hochgeladene Audiodaten wiedergeben und bestimmen, ob das angegebene Erkennungsergebnis korrekt ist. Mit diesem Tool können Sie die Qualität des Baselinemodells von Microsoft für die Spracherkennung überprüfen, ein trainiertes benutzerdefiniertes Modell untersuchen oder die Aufzeichnungen von zwei Modellen vergleichen.

In diesem Dokument erfahren Sie, wie Sie die Qualität eines Modells unter Verwendung der zuvor hochgeladenen Trainingsdaten visuell überprüfen.

Auf dieser Seite erfahren Sie, wie Sie die Qualität des Baseline-Spracherkennungsmodells von Microsoft und/oder eines trainierten benutzerdefinierten Modells visuell überprüfen. Dafür verwenden Sie die Daten, die Sie zu Testzwecken über die Registerkarte **Daten** hochgeladen haben.

## <a name="create-a-test"></a>Erstellen eines Tests

Gehen Sie wie folgt vor, um einen Test zu erstellen:

1. Melden Sie sich beim [Custom Speech-Portal](https://speech.microsoft.com/customspeech) an.
2. Navigieren Sie zu **Spracherkennung > Custom Speech > [Projektname] > Testen**.
3. Klicken Sie auf **Test hinzufügen**.
4. Wählen Sie **Inspect quality (Audio-only data)** (Qualität überprüfen (nur Audiodaten)) aus. Geben Sie einen Namen und eine Beschreibung für den Test ein, und wählen Sie Ihr Audiodataset aus.
5. Wählen Sie bis zu zwei Modelle aus, die Sie testen möchten.
6. Klicken Sie auf **Erstellen**.

Nachdem ein Test erfolgreich erstellt wurde, können Sie sehen, wie ein Modell das angegebene Audiodataset transkribiert, oder die Ergebnisse von zwei Modellen direkt miteinander vergleichen.

[!INCLUDE [service-pricing-advisory](includes/service-pricing-advisory.md)]

## <a name="side-by-side-model-comparisons"></a>Gegenüberstellung der Modelle

Wenn der Teststatus _Erfolgreich_ lautet, klicken Sie auf den Namen des Testelements, um Testdetails anzuzeigen. Auf dieser Detailseite werden alle Äußerungen in Ihrem Dataset aufgelistet. Außerdem werden die Erkennungsergebnisse der beiden Modelle gezeigt, die Sie vergleichen.

Zur einfacheren Untersuchung der Gegenüberstellung können Sie verschiedene Fehlertypen wie Einfügungen, Löschungen und Ersetzungen aktivieren oder deaktivieren. Indem Sie sich die Audiodaten anhören und mit den Erkennungsergebnissen in den einzelnen Spalten vergleichen (die die menschenmarkierte Transkription sowie die Ergebnisse zweier Spracherkennungsmodelle enthalten), können Sie entscheiden, welches Modell Ihre Anforderungen erfüllt und wo Verbesserungen erforderlich sind.

Parallele Modelltests sind nützlich, um zu überprüfen, welches Spracherkennungsmodell für eine Anwendung am besten geeignet ist. Für eine objektive Genauigkeitsmessung, für die transkribiertes Audio benötigt wird, befolgen Sie die Anweisungen unter [Bewerten der Genauigkeit](how-to-custom-speech-evaluate-data.md).

## <a name="next-steps"></a>Nächste Schritte

- [Bewerten Ihrer Daten](how-to-custom-speech-evaluate-data.md)
- [Trainieren Ihres Modells](how-to-custom-speech-train-model.md)
- [Verbessern Ihres Modells](how-to-custom-speech-improve-accuracy.md)
- [Bereitstellen Ihres Modells](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Prepare data for Custom Speech](how-to-custom-speech-test-data.md) (Vorbereiten von Daten für Custom Speech)
