---
title: Übersicht über das Azure Percept-Audiogerät
description: Weitere Informationen zu Azure Percept-Audio
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: 85eb4ed0832c9384fcd05154833bc21f83f4adf2
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102426347"
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

- Azure Percept-Audio: Erfasst und konvertiert die Audiodaten und sendet sie an den DK und die Audiobuchse.

- Azure Percept DK: Der Spracherkennungsstapel führt Strahlformung und Echounterdrückung durch und verarbeitet die Audioeingabe zur Optimierung der Spracherkennung. Anschließend wird die Schlüsselworterkennung durchführt.

- Cloud: Verarbeitet Befehle und Ausdrücke in natürlicher Sprache und führt eine Schlüsselwortüberprüfung sowie ein erneutes Training durch. 

- Offline: Wenn das Gerät offline ist, wird das Schlüsselwort erkannt, und es werden Telemetriedaten zum Internetverbindungsstatus erfasst. Da keine Schlüsselwortüberprüfung in der Cloud möglich ist, kann es zu einer erhöhten Falschakzeptanzrate bei der Schlüsselworterkennung kommen. 

## <a name="getting-started"></a>Erste Schritte

- [Zusammensetzen Ihres Azure Percept DK](./quickstart-percept-dk-unboxing.md)
- [Abschließen Azure Percept DK-Einrichtungserfahrung](./quickstart-percept-dk-set-up.md)
- [Anschließen Ihres Azure Percept-Audiogeräts an Ihr DevKit](./quickstart-percept-audio-setup.md)

## <a name="build-a-no-code-prototype"></a>Erstellen eines Prototyps ohne Code

Erstellen Sie eine [Sprachlösung ohne Code](./tutorial-no-code-speech.md) in [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819) mithilfe von Azure Percept-Sprach-Assistenten-Vorlagen für Gast-/Hotelgewerbe, Gesundheitswesen, Inventur- und Automobilszenarien.

### <a name="manage-your-no-code-speech-solution"></a>Verwalten Ihrer Sprachlösung ohne Code

- [Verwalten Ihres Sprach-Assistenten](./how-to-manage-voice-assistant.md)
- [Konfigurieren Ihres Sprach-Assistenten in Azure Percept Studio](./how-to-configure-voice-assistant.md)
- [Problembehandlung für Azure Percept-Audio](./troubleshoot-audio-accessory-speech-module.md)

## <a name="additional-technical-information"></a>Weitere technische Informationen

- [Datenblatt für Azure Percept-Audio](./azure-percept-audio-datasheet.md)
- [Tasten- und LED-Verhalten von Azure Percept-Audio](./audio-button-led-behavior.md)

## <a name="next-steps"></a>Nächste Schritte

Bestellen eines Azure Percept-Audiogeräts im [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270).
