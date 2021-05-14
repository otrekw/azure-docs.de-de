---
title: Übersicht über das Azure Percept-Audiogerät
description: Weitere Informationen zu Azure Percept-Audio
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: conceptual
ms.date: 03/23/2021
ms.custom: template-concept
ms.openlocfilehash: 23fae249cfceec75af0b7c49a3875ab447ecbafd
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105564261"
---
# <a name="introduction-to-azure-percept-audio"></a>Einführung in Azure Percept-Audio

Azure Percept Audio ist ein Zubehörgerät, das dem [Azure Percept DK](./overview-azure-percept-dk.md) sprachbezogene KI-Funktionen hinzufügt. Es enthält einen vorkonfigurierten Audioprozessor sowie ein lineares 4-Mikrofon-Array und ermöglicht damit die Nutzung von Sprachbefehlen, Schlüsselworterkennung und Fernfeldspracherkennung über Azure Cognitive Services. Es ist standardmäßig in Azure Percept DK, [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819) und anderen Azure-Edgeverwaltungsdiensten integriert. Azure Percept Audio kann im [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270) erworben werden.

> [!div class="nextstepaction"]
> [Jetzt kaufen](https://go.microsoft.com/fwlink/p/?LinkId=2155270)

</br>

> [!VIDEO https://www.youtube.com/embed/Qj8NGn-7s5A]

## <a name="azure-percept-audio-components"></a>Azure Percept-Audiokomponenten

Azure Percept-Audio enthält die folgenden Hauptkomponenten:

- Für die Produktion bereitstehendes Azure Percept Audio-Gerät (SoM) mit linearem 4-Mikrofon-Array und Audioverarbeitung per XMOS-Codec
- Entwicklerplatine (Interposer): zwei Tasten und drei LEDs sowie Micro-USB- und 3,5-mm-Audioanschluss
- Erforderliche Kabel: FPC-Kabel, Micro-USB-Kabel (USB-B auf USB-A)
- Begrüßungskarte
- Mechanische Montageplatte mit integrierter Aufnahme der 80/20 1010-Serie

## <a name="compute-capabilities"></a>Computefunktionen 

Azure Percept Audio übergibt die Audioeingabe über den Spracherkennungsstapel, der auf der CPU der Azure Percept DK-Trägerplatine ausgeführt wird (unter Verwendung einer hybriden Edge-Cloud-Methode). Daher wird für Azure Percept-Audio eine Trägerplatine mit einem Betriebssystem benötigt, das den Spracherkennungsstapel unterstützt. 

Die Audioverarbeitung läuft wie folgt ab: 

- Azure Percept-Audio: Erfasst und konvertiert die Audiodaten und sendet sie an den DK und die Audiobuchse.

- Azure Percept DK: Der Spracherkennungsstapel führt Strahlformung und Echounterdrückung durch und verarbeitet die Audioeingabe zur Optimierung der Spracherkennung. Nach der Verarbeitung wird die Schlüsselworterkennung durchführt.

- Cloud: Verarbeitet Befehle und Ausdrücke in natürlicher Sprache und führt eine Schlüsselwortüberprüfung sowie ein erneutes Training durch. 

- Offline: Wenn das Gerät offline ist, wird das Schlüsselwort erkannt, und es werden Telemetriedaten zum Internetverbindungsstatus erfasst. Da keine Schlüsselwortüberprüfung in der Cloud möglich ist, kann es zu einer erhöhten Falschakzeptanzrate bei der Schlüsselworterkennung kommen. 

## <a name="getting-started"></a>Erste Schritte

- [Zusammensetzen Ihres Azure Percept DK](./quickstart-percept-dk-unboxing.md)
- [Anschließen Ihres Azure Percept-Audiogeräts an Ihr DevKit](./quickstart-percept-audio-setup.md)
- [Abschließen Azure Percept DK-Einrichtungserfahrung](./quickstart-percept-dk-set-up.md)

## <a name="build-a-no-code-prototype"></a>Erstellen eines Prototyps ohne Code

Erstellen Sie eine [Sprachlösung ohne Code](./tutorial-no-code-speech.md) in [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819) mithilfe von Azure Percept-Sprach-Assistenten-Vorlagen für Gast-/Hotelgewerbe, Gesundheitswesen, Inventur- und Automobilszenarien.

### <a name="manage-your-no-code-speech-solution"></a>Verwalten Ihrer Sprachlösung ohne Code

- [Konfigurieren Ihres Sprach-Assistenten in Azure Percept Studio](./how-to-manage-voice-assistant.md)
- [Verwalten Ihres Sprach-Assistenten](./how-to-configure-voice-assistant.md)
- [Problembehandlung für Azure Percept-Audio](./troubleshoot-audio-accessory-speech-module.md)

## <a name="additional-technical-information"></a>Weitere technische Informationen

- [Datenblatt für Azure Percept-Audio](./azure-percept-audio-datasheet.md)
- [Tasten- und LED-Verhalten von Azure Percept-Audio](./audio-button-led-behavior.md)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erwerben eines Azure Percept Audio-Geräts über den Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270)
