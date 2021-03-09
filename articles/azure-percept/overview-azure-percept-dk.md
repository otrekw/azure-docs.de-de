---
title: Übersicht über das Azure Percept DK
description: Weitere Informationen zum Azure Percept DK
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: c93ecdc138e757f84e995a69a6c3d7f1b5460abe
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102179373"
---
# <a name="azure-percept-dk-overview"></a>Übersicht über das Azure Percept DK

Azure Percept DK ist ein Development Kit für Edge-KI und IoT, das für die Entwicklung von Proof of Concepts im Zusammenhang mit KI für maschinelles Sehen und Audio konzipiert ist. In Kombination mit [Azure Percept Studio](./overview-azure-percept-studio.md) und [Azure Percept-Audio](./overview-azure-percept-audio.md) wird es zu einer leistungsstarken, aber dennoch einfach zu verwendenden Plattform für die Erstellung von Edge-KI-Lösungen für eine Vielzahl von KI-Anwendungen für maschinelles Sehen oder Audio. Es kann im [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270) erworben werden.

:::image type="content" source="./media/overview-azure-percept-dk/dk-image.png" alt-text="Azure Percept DK-Gerät":::

## <a name="key-features"></a>Wichtige Funktionen

- **Die Möglichkeit, KI im Edgebereich auszuführen**. Durch die integrierte Hardwarebeschleunigung können Vision KI-Modelle ohne eine Cloudverbindung ausgeführt werden.
- **Integrierter Hardwarestamm für Vertrauenssicherheit**. Weitere Details finden Sie in dieser Übersicht über [Azure Percept-Sicherheit](./overview-percept-security.md).
- **Nahtlose Integration in [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819)** und andere Azure-Dienste. Z. B. Azure IoT Hub, Azure Cognitive Services und [Live Video Analytics](https://docs.microsoft.com/azure/media-services/live-video-analytics-edge/overview)
- **Nahtlose Integration mit [Azure Percept-Audio](./overview-azure-percept-audio.md) (optional)**
- **Unterstützung für die wichtigsten KI-Plattformen**. Beispielsweise ONNX und TensorFlow.
- **Integration in das 80/20-Schienensystem**. Dies vereinfacht die Erstellung von Prototypen in Produktionsumgebungen. Weitere Informationen zur 80/20-Integration finden Sie [hier](./overview-8020-integration.md).

## <a name="hardware-components"></a>Hardwarekomponenten

- Die Azure Percept DK-Trägerplatine
    - NXP iMX8m-Prozessor
    - TPM (Trusted Platform Module), Version 2.0
    - WLAN- und Bluetooth-Konnektivität
    - Vollständiges [Datenblatt](./azure-percept-dk-datasheet.md) anzeigen
- Das Azure Percept Vision SoM (System on Module)
    - Intel Movidius Myriad X (MA2085) VPU (Vision Processing Unit)
    - RGB-Kamerasensor mit der Möglichkeit zur Erweiterung um einen zweiten
    - Vollständiges [Datenblatt](./azure-percept-vision-datasheet.md) anzeigen

## <a name="get-started-with-the-azure-percept-dk"></a>Erste Schritte mit dem Azure Percept DK

- Ausführen dieser Schnellstarts
    - [Auspacken und Zusammensetzen des Azure Percept DK](./quickstart-percept-dk-unboxing.md)
    - [Einrichten des Azure Percept DK und Ausführen Ihres ersten Vision-KI-Modells](./quickstart-percept-dk-set-up.md)
- Erstellen erster Proof of Concepts mit diesen Tutorials
    - [Erstellen einer Vision-Lösung ohne Code in Azure Percept Studio](./tutorial-nocode-vision.md)
    - [Erstellen eines Sprach-Assistenten in Azure Percept Studio](./tutorial-no-code-speech.md)

## <a name="next-steps"></a>Nächste Schritte

Bestellen eines Azure Percept DK im [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270).
