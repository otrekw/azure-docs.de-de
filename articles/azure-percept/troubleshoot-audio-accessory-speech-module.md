---
title: Behandeln von Problemen mit Azure Percept Audio und Speech-Modulen
description: Tipps zur Problembehandlung für Azure Percept Audio und azureearspeechclientmodule
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 03/25/2021
ms.custom: template-how-to
ms.openlocfilehash: c4fc7d7564ecd30326fbec832639b2a81d55e6d5
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105605653"
---
# <a name="azure-percept-audio-and-speech-module-troubleshooting"></a>Problembehandlung für Azure Percept-Audio und Speech-Module

Beheben Sie Probleme mit dem Sprach-Assistenten mithilfe der folgenden Anweisungen.

## <a name="collecting-speech-module-logs"></a>Sammeln von Speech-Modulprotokollen

Um diese Befehle auszuführen, verbinden Sie sich per [SSH mit dem Dev-Kit](./how-to-ssh-into-percept-dk.md) und geben Sie die Befehle in die Eingabeaufforderung des SSH-Clients ein.

Sammeln Sie Speech-Modulprotokolle:

```console
sudo iotedge logs azureearspeechclientmodule
```

Verwenden Sie die folgende Syntax, um Ausgaben zur weiteren Analyse an eine TXT-Datei umzuleiten:

```console
sudo [command] > [file name].txt
```

Ändern Sie die Berechtigungen der txt-Datei, damit Sie kopiert werden kann:

```console
sudo chmod 666 [file name].txt
```

Nach dem Umleiten einer Ausgabe an eine TXT-Datei kopieren Sie die Datei über SCP auf Ihren Host-PC:

```console
scp [remote username]@[IP address]:[remote file path]/[file name].txt [local host file path]
```

[local host file path] bezieht sich auf den Speicherort auf dem Host-PC, an den Sie die TXT-Datei kopieren möchten. [remote username] ist der SSH-Benutzername, der während des [Setups](./quickstart-percept-dk-set-up.md) ausgewählt wurde.

## <a name="checking-runtime-status-of-the-speech-module"></a>Überprüfen des Laufzeitstatus des Speech-Moduls

Überprüfen Sie, ob für **azureearspeechclientmodule** als Laufzeitstatus **Wird ausgeführt** angezeigt wird. Öffnen Sie zum Ermitteln des Laufzeitstatus Ihrer Gerätemodule das [Azure-Portal](https://portal.azure.com/), und navigieren Sie zu **Alle Ressourcen** ->  **[Ihr IoT Hub]**  -> **IoT Edge** ->  **[Ihre Geräte-ID]** . Klicken Sie auf die Registerkarte **Module**, um den Laufzeitstatus aller installierten Module anzuzeigen.

:::image type="content" source="./media/troubleshoot-audio-accessory-speech-module/over-the-air-iot-edge-device-page.png" alt-text="Seite „Edge-Gerät“ im Azure-Portal":::

Wird als Laufzeitstatus von **azureearspeechclientmodule** nicht **Wird ausgeführt** angezeigt, klicken Sie auf **Module festlegen** -> **azureearspeechclientmodule**. Legen Sie auf der Seite **Moduleinstellungen** die Option **Gewünschter Status** auf **Wird ausgeführt** fest, und klicken Sie auf **Aktualisieren**.

## <a name="understanding-ear-som-led-indicators"></a>Grundlegendes zu den LED-Anzeigen für das akustische SOM

Anhand von LED-Anzeigen können Sie nachvollziehen, in welchem Zustand sich Ihr Gerät befindet. Es dauert in der Regel etwa 2 Minuten, bis das Modul nach dem Einschalten vollständig hochgefahren wurde. Während der Initialisierungsschritte wird Folgendes angezeigt:

1. Weißes LED in der Mitte eingeschaltet (statisch): Das Gerät ist eingeschaltet.
2. Weißes LED in der Mitte eingeschaltet (blinkt): Die Authentifizierung wird durchgeführt.
3. Alle drei LEDs leuchten blau, sobald das Gerät authentifiziert wurde und betriebsbereit ist.

|LED|LED-Zustand|Status des akustischen SOM|
|---|---------|--------------|
|L02|1x Weiß, dauerhaft leuchtend|Einschalten |
|L02|1x Weiß, mit einer Frequenz von 0,5 Hz blinkend|Authentifizierung wird durchgeführt |
|L01 und L02 und L03|3x Blau, dauerhaft leuchtend|Warten auf Schlüsselwort|
|L01 und L02 und L03|Blinkendes LED-Array (20 FPS) |Lauschen oder Sprechen|
|L01 und L02 und L03|Schnell nacheinander aufblinkende LEDs (20 FPS)|Berechnung|
|L01 und L02 und L03|3x Rot, dauerhaft leuchtend |Mute|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung von Azure Percept DK finden Sie im [allgemeinen Problembehandlungsleitfaden](./troubleshoot-dev-kit.md).
