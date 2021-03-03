---
title: Übersicht über das Azure Percept-Audiogerät
description: Weitere Informationen zu Azure Percept-Audio
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: a63f471498667f58fc80f89323ad93b0feb8bc95
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678465"
---
# <a name="introduction-to-azure-percept-audio"></a>Einführung in Azure Percept-Audio

Azure Percept-Audio ist ein Zubehörgerät, das dem Azure Percept DK KI-Funktionen für Sprache hinzufügt. Es enthält einen vorkonfigurierten Sprach-KI-Beschleuniger sowie ein lineares 4-Mikrofonarray, das es Ihnen ermöglicht, benutzerdefinierte Befehle, Schlüsselworterkennung sowie Fernfeldsprach- bis hin zu lokalen Abhörgeräten zu verwenden. Mit Azure Percept-Audio können Gerätehersteller die visuellen Azure Percept DK-Funktionen auf neue, intelligente sprachaktivierte Geräte ausweiten. Es ist standardmäßig in Azure Percept DK, Azure Percept Studio und andere Azure Edge-Verwaltungsdienste integriert. Es kann im [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270) erworben werden.

![Azure Percept-Audiogerät](./media/overview-azure-percept-audio/percept-audio.png)


## <a name="azure-percept-audio-components"></a>Azure Percept-Audiokomponenten

Azure Percept-Audio enthält die folgenden Hauptkomponenten:

- Ein für die Produktion bereites Azure Percept-Audiogerät (SoM) mit einem linearen 4-Mikrofonarray
- Eine Entwicklerplatine (Developer Board; einschließlich 2 Tasten, 3 LEDs, Micro-USB- und 3,5 mm-Audioanschluss)
- Erforderliche Kabel: Flexkabel, Micro-USB, Typ-B auf USB-A
- Begrüßungskarte
- Mechanische Montageplatte mit integrierter Aufnahme der 80/20 1010-Serie


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

### <a name="build-a-no-code-prototype"></a>Erstellen eines Prototyps ohne Code

Erstellen Sie eine [Sprachlösung ohne Code](./tutorial-no-code-speech.md) mithilfe von Azure Percept-Sprach-Assistenten-Vorlagen für Gast-/Hotelgewerbe, Gesundheitswesen, Inventur- und Automobilszenarien.

## <a name="additional-technical-information"></a>Weitere technische Informationen

- [Datenblatt für Azure Percept-Audio](./azure-percept-audio-datasheet.md)

## <a name="next-steps"></a>Nächste Schritte

Bestellen eines Azure Percept-Audiogeräts im [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270).