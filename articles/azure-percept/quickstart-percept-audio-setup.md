---
title: Einrichten von Azure Percept Audio
description: Hier erfahren Sie, wie Sie Ihr Azure Percept-Audiogerät mit Azure Percept DK verbinden.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: quickstart
ms.date: 03/25/2021
ms.openlocfilehash: 03cab112681536708496c35e4440e463d1522406
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113108187"
---
# <a name="azure-percept-audio-setup"></a>Einrichten von Azure Percept-Audio

Azure Percept-Audio funktioniert standardmäßig mit Azure Percept DK. Es ist kein spezielles Setup erforderlich.

## <a name="prerequisites"></a>Voraussetzungen

- Azure Percept DK (DevKit)
- Azure Percept-Audio
- [Azure-Abonnement](https://azure.microsoft.com/free/)
- [Azure Percept DK-Setup](./quickstart-percept-dk-set-up.md): Sie haben Ihr DevKit mit einem WLAN verbunden, eine IoT Hub-Instanz erstellt und das DevKit mit der IoT Hub-Instanz verbunden.
- Lautsprecher oder Kopfhörer mit 3,5-mm-Stecker (optional)

## <a name="connecting-your-devices"></a>Verbinden Ihrer Geräte

1. Stellen Sie mithilfe des beiliegenden USB-Kabels (Micro-USB zu USB-A) eine Verbindung zwischen dem Azure Percept-Audiogerät und der Azure Percept DK-Trägerplatine her. Stecken Sie den Micro-USB-Stecker des Kabels an der Interposer-Platine (Entwicklerplatine) und den USB-A-Stecker an der Percept DK-Trägerplatine ein.

1. (Optional) Schließen Sie Ihren Lautsprecher oder Kopfhörer am Audioanschluss (Line Out) des Azure Percept Audio-Geräts an. Dies ermöglicht es Ihnen, Audioantworten zu hören.

1. Schalten Sie das Dev Kit ein, indem Sie es mit dem Netzadapter verbinden. Die LED L02 blinkt weiß, um anzugeben, dass das Gerät eingeschaltet wurde und authentifiziert wird.

1. Warten Sie, bis der Authentifizierungsvorgang abgeschlossen ist, was bis zu 5 Minuten dauern kann.

1. Sie können mit der Erstellung von Prototypen beginnen, wenn einer der folgenden LED-Zustände sichtbar ist:

    - Die LED L02 leuchtet weiß: bedeutet, dass die Authentifizierung abgeschlossen ist und das DevKit ohne Schlüsselwort konfiguriert wurde.
    - Alle drei LEDs leuchten blau: bedeutet, dass die Authentifizierung abgeschlossen ist und das DevKit mit einem Schlüsselwort konfiguriert wurde.

## <a name="next-steps"></a>Nächste Schritte

Erstellen einer [Sprachlösung ohne Code](./tutorial-no-code-speech.md) in [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819)
