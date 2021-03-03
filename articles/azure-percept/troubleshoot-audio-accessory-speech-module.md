---
title: Behandeln von Problemen mit Azure Percept-Audio und Speech-Modulen
description: Hier finden Sie Tipps zur Problembehandlung für einige der häufigsten Probleme während des Onboardings.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: 1e2ad920afb55066f07430568f976154f6d7cae1
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678511"
---
# <a name="azure-percept-audio-and-speech-module-troubleshooting"></a>Problembehandlung für Azure Percept-Audio und Speech-Module

Beheben Sie Probleme mit dem Sprach-Assistenten mithilfe der folgenden Anweisungen.

## <a name="collecting-speech-module-logs"></a>Sammeln von Speech-Modulprotokollen

Zum Ausführen dieser Befehle [stellen Sie eine Verbindung mit dem Azure Percept DK-WLAN-Zugriffspunkt sowie eine SSH-Verbindung mit dem DevKit her](./how-to-ssh-into-percept-dk.md) und geben die Befehle im SSH-Terminal ein.

```console
 iotedge logs azureearspeechclientmodule
```

Verwenden Sie die folgende Syntax, um Ausgaben zur weiteren Analyse an eine TXT-Datei umzuleiten:

```console
[command] > [file name].txt
```

Nach dem Umleiten einer Ausgabe an eine TXT-Datei kopieren Sie die Datei über SCP auf Ihren Host-PC:

```console
scp [remote username]@[IP address]:[remote file path]/[file name].txt [local host file path]
```

[local host file path] bezieht sich auf den Speicherort auf dem Host-PC, an den Sie die TXT-Datei kopieren möchten. [remote username] ist der SSH-Benutzername, der während des [Setups](./quickstart-percept-dk-set-up.md) ausgewählt wurde. Wenn Sie auf der Windows-Willkommensseite keine SSH-Anmeldung eingerichtet haben, lautet der Remotebenutzername „root“.

## <a name="checking-runtime-status-of-the-speech-module"></a>Überprüfen des Laufzeitstatus des Speech-Moduls

Überprüfen Sie, ob für **azureearspeechclientmodule** als Laufzeitstatus **Wird ausgeführt** angezeigt wird. Öffnen Sie zum Ermitteln des Laufzeitstatus Ihrer Gerätemodule das [Azure-Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home), und navigieren Sie zu **Alle Ressourcen** ->  **\<your IoT hub>**  -> **IoT Edge** ->  **\<your device ID>** . Klicken Sie auf die Registerkarte **Module**, um den Laufzeitstatus aller installierten Module anzuzeigen.

:::image type="content" source="./media/troubleshoot-audio-accessory-speech-module/over-the-air-iot-edge-device-page.png" alt-text="Seite „Edge-Gerät“ im Azure-Portal":::

Wird als Laufzeitstatus von **azureearspeechclientmodule** nicht **Wird ausgeführt** angezeigt, klicken Sie auf **Module festlegen** -> **azureearspeechclientmodule**. Legen Sie auf der Seite **Moduleinstellungen** die Option **Gewünschter Status** auf **Wird ausgeführt** fest, und klicken Sie auf **Aktualisieren**.

:::image type="content" source="./media/troubleshoot-audio-accessory-speech-module/firmware-desired-status-stopped.png" alt-text="Bildschirm „Module festlegen“ im Azure-Portal":::

## <a name="understanding-ear-som-led-indicators"></a>Grundlegendes zu den LED-Anzeigen für das akustische SOM

Anhand von LED-Anzeigen können Sie nachvollziehen, in welchem Zustand sich Ihr Gerät befindet. Es dauert in der Regel etwa zwei Minuten, bis das Modul nach dem *Einschalten* vollständig initialisiert wurde. Im Rahmen der Initialisierungsschritte wird Folgendes angezeigt:

1. 1 grüne LED links: Das Gerät wird eingeschaltet. 
2. 1 grüne LED links und grün blinkende LED in der Mitte: Die Authentifizierung wird ausgeführt. 
3. Alle drei LEDs leuchten blau, sobald das Gerät authentifiziert wurde und betriebsbereit ist.

|LED-Zustand                  |Status des akustischen SOM            |
|----------------------------|---------------------------|
|1x grün (linke LED)         |Einschalten |
|1x grün (linke LED) <br> 1x grün blinkend (mittlere LED) |Authentifizierung wird durchgeführt. |
|3x aus                      |Initialisierung abgeschlossen |
|3x blau                     |Betriebsbereit |
|3x blau blinkend            |Schlüsselwort erkannt |
|3x schnell nacheinander aufblinkend              |Verarbeiten |
|3x rot                      |Stummschalten |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung von Azure Percept DK finden Sie im [allgemeinen Problembehandlungsleitfaden](./troubleshoot-dev-kit.md).