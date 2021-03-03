---
title: Erste Schritte mit Azure Percept-Audio
description: Hier erfahren Sie, wie Sie Ihr Azure Percept-Audiogerät mit Azure Percept DK verbinden.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/18/2021
ms.custom: template-quickstart
ms.openlocfilehash: 575107859f56df742ab41a299269c250511022b3
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101664300"
---
# <a name="azure-percept-audio-setup"></a>Einrichten von Azure Percept-Audio

Azure Percept-Audio funktioniert standardmäßig mit Azure Percept DK. Es ist kein spezielles Setup erforderlich.

## <a name="prerequisites"></a>Voraussetzungen

- Azure Percept DK (DevKit)
- Azure Percept-Audio
- [Azure-Abonnement](https://azure.microsoft.com/free/)
- [Azure Percept DK-Setup:](./quickstart-percept-dk-set-up.md) Sie haben Ihr DevKit mit einem WLAN verbunden, eine IoT Hub-Instanz erstellt und das DevKit mit der IoT Hub-Instanz verbunden.

## <a name="connecting-your-devices"></a>Verbinden Ihrer Geräte

1. Stellen Sie eine Verbindung zwischen dem Azure Percept-Audiogerät und der Azure Percept DK-Trägerplatine her. Stecken Sie den USB-Micro-B-Stecker des Kabels am Audio-SOM und den USB-A-Stecker an der Percept DK-Trägerplatine ein.

1. Schalten Sie das DevKit ein.

    - Die LED L01 des Audio-SoM leuchtet nun grün und gibt damit an, dass das Gerät eingeschaltet wurde.
    - Die LED L02 blinkt grün und gibt damit an, dass das Audio-SoM authentifiziert wird.

1. Warten Sie, bis der Authentifizierungsvorgang abgeschlossen ist. Dies kann bis zu drei Minuten dauern.

1. Sie können mit der Erstellung von Prototypen beginnen, wenn einer der folgenden Punkte zutrifft:

    - LED L01 ist aus, und LED L02 leuchtet weiß. Dadurch wird angegeben, dass die Authentifizierung abgeschlossen ist und das DevKit noch nicht mit einem Schlüsselwort konfiguriert wurde.
    - Alle drei LEDs leuchten blau. Dadurch wird angegeben, dass die Authentifizierung abgeschlossen ist und das DevKit mit einem Schlüsselwort konfiguriert wurde.

    > [!NOTE]
    > Wenden Sie sich an den Support, wenn das DevKit nicht authentifiziert wird.

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine [Sprachlösung ohne Code](./tutorial-no-code-speech.md).