---
title: Trainieren eines Modells – Custom Translator
titleSuffix: Azure Cognitive Services
description: Das Trainieren eines Modells ist ein wichtiger Schritt beim Erstellen eines Übersetzungsmodells. Das Training erfolgt basierend auf Dokumenten, die Sie für diese Trainingsvorgänge auswählen.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: lajanuar
ms.topic: conceptual
ms.openlocfilehash: 5c17f842b93fb99a6a06b94f84ae26126794b776
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98896425"
---
# <a name="train-a-model"></a>Trainieren eines Modells

Das Trainieren ist der erste und wichtigste Schritt beim Erstellen eines Übersetzungsmodells, da andernfalls die Erstellung des Modells nicht möglich ist. Das Trainieren erfolgt basierend auf Dokumenten, die Sie für die Trainingsvorgänge auswählen. Beachten Sie bei der Auswahl von Dokumenten für den Dokumenttyp „Training“ die Mindestanforderung für 10.000 parallele Sätze. Bei der Auswahl von Dokumenten wird die Gesamtanzahl der Trainingssätze als Hinweis für Sie angezeigt. Diese Anforderung gilt nicht, wenn Sie zum Trainieren eines Modells nur Dokumente des Dokumenttyps „Wörterbuch“ auswählen.

Trainieren Sie ein Modell wie folgt:

1. Wählen Sie das Projekt aus, für das Sie ein Modell erstellen möchten.

2. Auf der Registerkarte „Daten“ für das Projekt werden alle relevanten Dokumente für das Sprachpaar des Projekts angezeigt. Wählen Sie die Dokumente, die Sie zum Trainieren Ihres Modells verwenden möchten, manuell aus. Sie können auf diesem Bildschirm Dokumente für das Trainieren, Optimieren und Testen auswählen. Außerdem können Sie einfach den Trainingssatz auswählen und Custom Translator die Erstellung des Optimierungs- und Testsatzes überlassen.

    - Dokumentname: Name des Dokuments.

    - Kopplung: Gibt an, ob es sich um ein paralleles oder einsprachiges Dokument handelt. Einsprachige Dokumente werden für das Training derzeit nicht verwendet.

    - Dokumenttyp: Hier kann Trainieren, Optimieren, Testen oder Wörterbuch als Typ angegeben werden.

    - Sprachpaar: Gibt die Ausgangs- und Zielsprache für das Projekt an.

    - Ausgangssätze: Gibt die Anzahl von Sätzen an, die aus der Quelldatei extrahiert werden.

    - Zielsätze: Gibt die Anzahl von Sätzen an, die aus der Zieldatei extrahiert werden.

    ![Trainieren des Modells](media/how-to/how-to-train-model.png)

3. Klicken Sie auf die Schaltfläche „Modell erstellen“.

4. Geben Sie im Dialogfeld den Namen für Ihr Modell ein. Standardmäßig ist „Sofort trainieren“ ausgewählt, um die Trainingspipeline zu starten, wenn Sie auf die Schaltfläche „Modell erstellen“ klicken. Sie können „Als Entwurf speichern“ auswählen, um die Modellmetadaten zu erstellen und das Modell in einen Entwurfszustand zu versetzen, aber das Modelltraining würde nicht gestartet. Zu einem späteren Zeitpunkt müssen Sie Modelle im Entwurfsstatus manuell auswählen, um sie zu trainieren.

5. Klicken Sie auf die Schaltfläche „Modell erstellen“.

    ![Dialogfeld „Modell trainieren“](media/how-to/how-to-train-model-2.png)

6. Benutzerdefinierter Translator übermittelt das Training und zeigt den zugehörigen Status auf der Registerkarte „Modelle“ an.

    ![Seite „Modell trainieren“](media/how-to/how-to-train-model-3.png)

>[!Note]
>Der benutzerdefinierte Translator unterstützt zehn gleichzeitige Trainings innerhalb eines Arbeitsbereichs zu einem beliebigen Zeitpunkt.

## <a name="modify-a-model-name"></a>Ändern eines Modellnamens

Sie können einen Modellnamen auf der Seite „Modelldetails“ ändern.

1. Klicken Sie auf der Seite „Projekte“ auf den Projektnamen, unter dem das Modell gespeichert ist.
2. Klicken Sie auf die Registerkarte „Modell“.
3. Klicken Sie auf den Modellnamen, um die Modelldetails anzuzeigen.
4. Klicken Sie auf das Stiftsymbol.

    ![Bearbeiten des Modells](media/how-to/how-to-edit-model.png)

5. Ändern Sie im Dialogfeld den Modellnamen, und benennen Sie Ihr Modell mit einem aussagekräftigen Namen.

    ![Dialogfeld für Bearbeitung](media/how-to/how-to-edit-model-dialog.png)

6. Klicken Sie auf Speichern.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich, [wie Sie Modelldetails anzeigen](how-to-view-model-details.md).
