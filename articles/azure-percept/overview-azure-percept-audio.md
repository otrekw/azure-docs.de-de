---
title: Übersicht über das Azure Percept-Audiogerät
description: Weitere Informationen zu Azure Percept-Audio
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: 8884663b3f0e861e62f48c3aab680f0f31e74428
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102098382"
---
# <a name="introduction-to-azure-percept-audio"></a>Einführung in Azure Percept-Audio

Azure Percept-Audio ist ein Zubehörgerät, das dem Azure Percept DK KI-Funktionen für Sprache hinzufügt. Es enthält einen vorkonfigurierten Audioprozessor sowie ein lineares 4-Mikrofonarray, was die Nutzung von Sprachbefehlen, Schlüsselworterkennung und Fernfeldspracherkennung über lokale Geräte mit Azure Cognitive Services ermöglicht. Mit Azure Percept-Audio können Gerätehersteller die Azure Percept DK-Funktionen über maschinelles Sehen hinaus auf neue, intelligente sprachaktivierte Geräte ausweiten. Es ist standardmäßig in Azure Percept DK, Azure Percept Studio und andere Azure Edge-Verwaltungsdienste integriert. Es kann im [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270) erworben werden.

:::image type="content" source="./media/overview-azure-percept-audio/percept-audio.png" alt-text="Azure Percept-Audiogerät":::

## <a name="azure-percept-audio-components"></a>Azure Percept-Audiokomponenten

Azure Percept-Audio enthält die folgenden Hauptkomponenten:

- Für die Produktion bereites Azure Percept-Audiogerät (SOM) mit linearem 4-Mikrofonarray und Audioverarbeitung per XMOS-Codec
- Eine Entwicklerplatine (Interposer) mit zwei Tasten und drei LEDs sowie mit Micro-USB- und 3,5-mm-Audioanschluss
- Erforderliche Kabel: FPC-Kabel, Micro-USB-Kabel (USB-B auf USB-A)
- Begrüßungskarte
- Mechanische Montageplatte mit integrierter Aufnahme der 80/20 1010-Serie

## <a name="compute-capabilities"></a>Computefunktionen 

Azure Percept-Audio übergibt die Audioeingabe über den Spracherkennungsstapel, der auf der CPU der Trägerplatine von Azure Percept DK ausgeführt wird (unter Verwendung einer hybriden Edge-Cloud-Methode). Daher wird für Azure Percept-Audio eine Trägerplatine mit einem Betriebssystem benötigt, das den Spracherkennungsstapel unterstützt. 

Die Verarbeitung läuft wie folgt ab: 

- Azure Percept-Audio: Führt die Strahlformung und Echounterdrückung durch und verarbeitet die Audioeingabe, um sie für die Spracherkennung und die Übermittlung an das DK zu optimieren.  

- Azure Percept DK: Der Spracherkennungsstapel führt die Schlüsselworterkennung durch.  

- Cloud: Verarbeitet Befehle und Ausdrücke in natürlicher Sprache und führt eine Schlüsselwortüberprüfung sowie ein erneutes Training durch. 

- Offline: Wenn das Gerät offline ist, wird das Schlüsselwort erkannt, und es werden Telemetriedaten zum Internetverbindungsstatus erfasst. Da keine Schlüsselwortüberprüfung in der Cloud möglich ist, kann es zu einer erhöhten Falschakzeptanzrate bei der Schlüsselworterkennung kommen. 

<!---

## How it works

Azure Percept Audio passes the audio input to the Azure Percept DK carrier board in a hybrid edge-cloud manner. Specifically,

- The Azure Percept Audio device: processes the incoming speech input to the clearest format by executing beam forming and echo cancellation befor sending the input to the Azure Percept DK. 
- The Azure Percept DK uses edge processing to perform keyword spotting and then sends the relevant inputs to Azure speech services.
- Cloud: Processing of natural language commands and phrases, in addition to keyword verification and retraining.
- Offline: If the device is offline it will detect the keyword and capture telemetry that there is no internet connection at the time of the command. It will not be able to weed out false accepts since it cannot perform keyword verification.

-->

## <a name="getting-started"></a>Erste Schritte

- [Zusammensetzen Ihres Azure Percept DK](./quickstart-percept-dk-unboxing.md)
- [Abschließen Azure Percept DK-Einrichtungserfahrung](./quickstart-percept-dk-set-up.md)
- [Anschließen Ihres Azure Percept-Audiogeräts an Ihr DevKit](./quickstart-percept-audio-setup.md)

## <a name="build-a-no-code-prototype"></a>Erstellen eines Prototyps ohne Code

Erstellen Sie eine [Sprachlösung ohne Code](./tutorial-no-code-speech.md) mithilfe von Azure Percept-Sprach-Assistenten-Vorlagen für Gast-/Hotelgewerbe, Gesundheitswesen, Inventur- und Automobilszenarien.

### <a name="manage-your-no-code-speech-solution"></a>Verwalten Ihrer Sprachlösung ohne Code

- [Verwalten Ihres Sprach-Assistenten](./how-to-manage-voice-assistant.md)
- [Konfigurieren Ihres Sprach-Assistenten in Azure Percept Studio](./how-to-configure-voice-assistant.md)
- [Problembehandlung für Azure Percept-Audio](./troubleshoot-audio-accessory-speech-module.md)

## <a name="additional-technical-information"></a>Weitere technische Informationen

- [Datenblatt für Azure Percept-Audio](./azure-percept-audio-datasheet.md)
- [Tasten- und LED-Verhalten von Azure Percept-Audio](./audio-button-led-behavior.md)

## <a name="next-steps"></a>Nächste Schritte

Bestellen eines Azure Percept-Audiogeräts im [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270).
