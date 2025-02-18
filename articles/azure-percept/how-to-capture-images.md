---
title: Erstellen von Bildern für eine codelose Lösung für maschinelles Sehen in Azure Percept Studio
description: Erfassen von Bildern mit Ihrem Azure Percept DK-Gerät in Azure Percept Studio
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/12/2021
ms.custom: template-how-to
ms.openlocfilehash: 2c7116d7e62afe6a59ad48bbb28dabf3c05bc311
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108754395"
---
# <a name="capture-images-for-a-vision-project-in-azure-percept-studio"></a>Erstellen von Bildern für ein Projekt für maschinelles Sehen in Azure Percept Studio

In dieser Anleitung erfahren Sie, wie Sie Bilder mit Azure Percept DK für ein bereits vorhandenes Vision-Projekt erfassen. Falls Sie noch kein Vision-Projekt erstellt haben, sehen Sie sich das [Vision-Tutorial ohne Code](./tutorial-nocode-vision.md) an.

## <a name="prerequisites"></a>Voraussetzungen

- Azure Percept DK (DevKit)
- [Azure-Abonnement](https://azure.microsoft.com/free/)
- [Azure Percept DK-Setup:](./quickstart-percept-dk-set-up.md) Sie haben Ihr DevKit mit einem WLAN verbunden, eine IoT Hub-Instanz erstellt und das DevKit mit der IoT Hub-Instanz verbunden.
- [Vision-Projekt ohne Code](./tutorial-nocode-vision.md)

## <a name="capture-images"></a>Erfassen von Bildern

1. Schalten Sie Ihr DevKit ein.

1. Navigieren Sie zu [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819).

1. Wählen Sie auf der linken Seite der Übersicht **Geräte** aus.

    :::image type="content" source="./media/how-to-capture-images/overview-devices-inline.png" alt-text="Übersichtsbildschirm von Azure Percept Studio" lightbox="./media/how-to-capture-images/overview-devices.png":::

1. Wählen Sie in der Liste Ihr DevKit aus.

    :::image type="content" source="./media/how-to-capture-images/select-device.png" alt-text="Liste der Percept-Geräte":::

1. Wählen Sie auf der Seite Ihres Geräts **Bilder für ein Projekt erfassen** aus.

    :::image type="content" source="./media/how-to-capture-images/capture-images.png" alt-text="Seite mit Percept-Geräten und den verfügbaren Aktionen":::

1. Gehen Sie im Fenster **Bilderfassung** wie folgt vor:

    1. Wählen Sie im Dropdownmenü **Projekt** das Vision-Projekt aus, für das Sie Bilder erfassen möchten.

    1. Wählen Sie **Gerätestream anzeigen** aus, und vergewissern Sie sich, dass die Kamera des Vision-SOM korrekt platziert ist.

    1. Wählen Sie **Foto aufnehmen** aus, um ein Bild aufzunehmen.

    1. Aktivieren Sie stattdessen das Kontrollkästchen neben **Automatische Bilderfassung**, um einen Selbstauslöser für die Bilderfassung einzurichten:

        1. Wählen Sie unter **Capture rate** (Erfassungsrate) die gewünschte Rate für die Erfassung aus.
        1. Wählen Sie unter **Ziel** aus, wie viele Bilder insgesamt erfasst werden sollen.

    :::image type="content" source="./media/how-to-capture-images/take-photo.png" alt-text="Bildschirm für Bilderfassung":::

Alle Bilder stehen in [Custom Vision](https://www.customvision.ai/) zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte

[Testen Sie Ihr Vision-Modell ohne Code, und trainieren Sie es neu.](../cognitive-services/custom-vision-service/test-your-model.md)