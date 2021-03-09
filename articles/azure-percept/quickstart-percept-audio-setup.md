---
title: Erste Schritte mit Azure Percept-Audio
description: Hier erfahren Sie, wie Sie Ihr Azure Percept-Audiogerät mit Azure Percept DK verbinden.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/18/2021
ms.custom: template-quickstart
ms.openlocfilehash: 588ebde85b6012ddbfb88ca8305fc735b7a0ba41
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102097991"
---
# <a name="azure-percept-audio-setup"></a>Einrichten von Azure Percept-Audio

Azure Percept-Audio funktioniert standardmäßig mit Azure Percept DK. Es ist kein spezielles Setup erforderlich.

## <a name="prerequisites"></a>Voraussetzungen

- Azure Percept DK (DevKit)
- Azure Percept-Audio
- [Azure-Abonnement](https://azure.microsoft.com/free/)
- [Azure Percept DK-Setup:](./quickstart-percept-dk-set-up.md) Sie haben Ihr DevKit mit einem WLAN verbunden, eine IoT Hub-Instanz erstellt und das DevKit mit der IoT Hub-Instanz verbunden.
- Lautsprecher oder Kopfhörer mit 3,5-mm-Stecker (optional)

## <a name="connecting-your-devices"></a>Verbinden Ihrer Geräte

1. Stellen Sie mithilfe des beiliegenden USB-Kabels (Micro-USB zu USB-A) eine Verbindung zwischen dem Azure Percept-Audiogerät und der Azure Percept DK-Trägerplatine her. Stecken Sie den Micro-USB-Stecker des Kabels an der Interposer-Platine (Entwicklerplatine) und den USB-A-Stecker an der Percept DK-Trägerplatine ein.
1. (Optional) Schließen Sie Ihre Lautsprecher oder Ihren Kopfhörer am Audioanschluss (Line Out) von Azure Percept-Audio an. Dies ermöglicht es Ihnen, die Audioantworten Ihres Sprach-Assistenten zu hören. Wenn Sie keine Lautsprecher oder Kopfhörer anschließen, können Sie die Antworten aber trotzdem verfolgen, da sie im Demofenster auch in Textform angezeigt werden. 

1. Schalten Sie das DevKit ein. Die LED L02 der Interposer-Platine blinkt weiß, um anzugeben, dass das Gerät eingeschaltet wurde und das Audio-SOM authentifiziert wird.

1. Warten Sie, bis der Authentifizierungsvorgang abgeschlossen ist. Dies kann bis zu drei Minuten dauern.

1. Sie können mit der Erstellung von Prototypen beginnen, wenn einer der folgenden Punkte zutrifft:

    - Die LED L02 leuchtet weiß. Dadurch wird angegeben, dass die Authentifizierung abgeschlossen ist und das DevKit noch nicht mit einem Schlüsselwort konfiguriert wurde.
    - Alle drei LEDs leuchten blau. Dadurch wird angegeben, dass die Authentifizierung abgeschlossen ist und das DevKit mit einem Schlüsselwort konfiguriert wurde.

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine [Sprachlösung ohne Code](./tutorial-no-code-speech.md).
