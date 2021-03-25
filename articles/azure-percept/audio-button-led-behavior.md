---
title: Tasten- und LED-Verhalten von Azure Percept-Audio
description: Hier finden Sie weitere Informationen zu den Tasten- und LED-Zuständen von Azure Percept-Audio.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: 1d956e33a84b5509c16400c8f5f8e813d116411a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102095747"
---
# <a name="azure-percept-audio-button-and-led-behavior"></a>Tasten- und LED-Verhalten von Azure Percept-Audio

Dieser Leitfaden enthält Informationen zu den Tasten- und LED-Zuständen von Azure Percept-Audio.

## <a name="button-behavior"></a>Tastenverhalten

Sie können die Tasten verwenden, um das Verhalten des Geräts zu steuern.

|Tastenzustand|  Verhalten|
|------------|----------|
|Mute|  Drücken Sie diese Taste, um die Stummschaltung der Mikrofonkomponente zu aktivieren bzw. zu deaktivieren. Das Tastenereignis wird beim Loslassen der Taste ausgelöst.|
|PTT/PTS|   Drücken Sie „PTT“, um die Schlüsselworterkennung zu umgehen und das Lauschen auf Befehle zu aktivieren. Drücken Sie die Taste erneut, um den aktiven Dialog des Agents anzuhalten und zur Schlüsselworterkennung zurückzukehren. Das Tastenereignis wird beim Loslassen der Taste ausgelöst. PTS kann nur verwendet werden, wenn die Schaltfläche gedrückt wird, während der Agent spricht, und nicht, während der Agent lauscht oder nachdenkt.|

## <a name="led-behavior"></a>LED-Verhalten

Anhand von LED-Anzeigen können Sie nachvollziehen, in welchem Zustand sich Ihr Gerät befindet.

|LED|   LED-Zustand|  Status des akustischen SOM|
|---|------------|----------------| 
|L02|   1x Weiß, dauerhaft leuchtend |Einschalten |
|L02|   1x Weiß, mit einer Frequenz von 0,5 Hz blinkend|  Authentifizierung wird durchgeführt |
|L01 und L02 und L03|   3x Blau, dauerhaft leuchtend|     Warten auf Schlüsselwort|
|L01 und L02 und L03|   Blinkendes LED-Array (20 FPS) | Lauschen oder Sprechen|
|L01 und L02 und L03|   Schnell nacheinander aufblinkende LEDs (20 FPS)|    Berechnung|
|L01 und L02 und L03|   3x Rot, dauerhaft leuchtend | Mute|

## <a name="next-steps"></a>Nächste Schritte

Tipps zur Problembehandlung für Ihr Azure Percept-Audiogerät finden Sie in [diesem Handbuch](./troubleshoot-audio-accessory-speech-module.md).