---
title: Einrichten von Azure Percept Audio
description: Hier erfahren Sie, wie Sie Ihr Azure Percept-Audiogerät mit Azure Percept DK verbinden.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: quickstart
ms.date: 03/25/2021
ms.openlocfilehash: fa3dad8cdd38e6db621d8194cc9472430c7c5008
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105605789"
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

1. (Optional) Schließen Sie Ihre Lautsprecher oder Ihren Kopfhörer am Audioanschluss (Line Out) des Azure Percept Audio-Geräts an. Dies ermöglicht es Ihnen, Audioantworten zu hören.

1. Schalten Sie das DevKit ein. Die LED L02 der Audio-Interposer-Platine blinkt weiß, um anzugeben, dass das Gerät eingeschaltet wurde und das Audio-SOM authentifiziert wird.

1. Warten Sie, bis der Authentifizierungsvorgang abgeschlossen ist. Dies kann bis zu drei Minuten dauern.

1. Sie können mit der Erstellung von Prototypen beginnen, wenn einer der folgenden Punkte zutrifft:

    - Die LED L02 leuchtet weiß: Dadurch wird angegeben, dass die Authentifizierung abgeschlossen ist und das DevKit noch nicht mit einem Schlüsselwort konfiguriert wurde.
    - Alle drei LEDs leuchten blau: Dadurch wird angegeben, dass die Authentifizierung abgeschlossen ist und das DevKit mit einem Schlüsselwort konfiguriert wurde.

## <a name="next-steps"></a>Nächste Schritte

Erstellen einer [Sprachlösung ohne Code](./tutorial-no-code-speech.md) in [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819)
