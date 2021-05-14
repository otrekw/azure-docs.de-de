---
title: Tasten- und LED-Verhalten von Azure Percept-Audio
description: Hier finden Sie weitere Informationen zu den Tasten- und LED-Zuständen von Azure Percept-Audio.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: conceptual
ms.date: 03/25/2021
ms.openlocfilehash: 169c3e7be60d5bf1efb7046aa92316a472fd4518
ms.sourcegitcommit: bd1a4e4df613ff24e954eb3876aebff533b317ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2021
ms.locfileid: "107930855"
---
# <a name="azure-percept-audio-button-and-led-behavior"></a>Tasten- und LED-Verhalten von Azure Percept-Audio

Dieser Leitfaden enthält Informationen zu den Tasten- und LED-Zuständen des Azure Percept-Audiogeräts.

## <a name="button-behavior"></a>Tastenverhalten

Verwenden Sie die Tasten, um das Verhalten des Geräts zu steuern.

|Tastenzustand|Verhalten|
|------------|----------|
|Mute|Drücken Sie diese Taste, um die Stummschaltung der Mikrofonkomponente zu aktivieren bzw. zu deaktivieren. Das Tastenereignis wird beim Loslassen der Taste ausgelöst.|
|PTT/PTS|Drücken Sie „PTT“, um die Schlüsselworterkennung zu umgehen und das Lauschen auf Befehle zu aktivieren. Drücken Sie die Taste erneut, um den aktiven Dialog des Agents anzuhalten und zur Schlüsselworterkennung zurückzukehren. Das Tastenereignis wird beim Loslassen der Taste ausgelöst. PTS kann nur verwendet werden, wenn die Schaltfläche gedrückt wird, während der Agent spricht, und nicht, während der Agent lauscht oder nachdenkt.|

## <a name="led-behavior"></a>LED-Verhalten

Anhand der LED-Anzeigen können Sie nachvollziehen, in welchem Zustand sich Ihr Gerät befindet.

|LED|LED-Zustand|Status des akustischen SOM|
|---|------------|----------------|
|L02|1x Weiß, dauerhaft leuchtend|Einschalten |
|L02|1x Weiß, mit einer Frequenz von 0,5 Hz blinkend|Authentifizierung wird durchgeführt |
|L01 und L02 und L03|3x Blau, dauerhaft leuchtend|Warten auf Schlüsselwort|
|L01 und L02 und L03|Blinkendes LED-Array (20 FPS) |Lauschen oder Sprechen|
|L01 und L02 und L03|Schnell nacheinander aufblinkende LEDs (20 FPS)|Berechnung|
|L01 und L02 und L03|3x Rot, dauerhaft leuchtend |Mute|

## <a name="next-steps"></a>Nächste Schritte

Tipps zur Problembehandlung für Ihr Azure Percept-Audiogerät finden Sie in [diesem Handbuch](./troubleshoot-audio-accessory-speech-module.md).