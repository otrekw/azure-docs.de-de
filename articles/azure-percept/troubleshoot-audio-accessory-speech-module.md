---
title: Behandeln von Problemen mit Azure Percept Audio und Speech-Modulen
description: Tipps zur Problembehandlung für Azure Percept Audio und azureearspeechclientmodule
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 03/25/2021
ms.custom: template-how-to
ms.openlocfilehash: c02c93acd23b71f73d5e27d8914d08fb1a1f99f9
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110071396"
---
# <a name="azure-percept-audio-and-speech-module-troubleshooting"></a>Problembehandlung für Azure Percept-Audio und Speech-Module

Beheben Sie Probleme mit dem Sprach-Assistenten mithilfe der folgenden Anweisungen.

## <a name="understanding-ear-som-led-indicators"></a>Grundlegendes zu den LED-Anzeigen für das akustische SOM

Anhand von LED-Anzeigen können Sie nachvollziehen, in welchem Zustand sich Ihr Gerät befindet. Es dauert etwa 4–5 Minuten, bis das Gerät eingeschaltet und das Modul vollständig initialisiert ist. Während der Initialisierungsschritte wird Folgendes angezeigt:

1. Weißes LED in der Mitte eingeschaltet (statisch): Das Gerät ist eingeschaltet.
1. Weißes LED in der Mitte eingeschaltet (blinkt): Die Authentifizierung wird durchgeführt.
1. Mittlere weiße LED ein (statisch): Das Gerät ist authentifiziert, aber das Schlüsselwort ist nicht konfiguriert.
1. Alle drei LEDs beginnen blau zu leuchten, sobald eine Demo bereitgestellt wurde und das Gerät betriebsbereit ist.

Weitere Informationen finden Sie in diesem Artikel über die [Tasten und LED-Verhalten von Azure Percept Audio](./audio-button-led-behavior.md).

### <a name="troubleshooting-led-issues"></a>Behandeln von LED-Problemen
- **Wenn die mittlere LED dauerhaft weiß leuchtet**, versuchen Sie, [eine Vorlage zu verwenden, um einen Sprach-Assistenten zu erstellen](./tutorial-no-code-speech.md).
- **Wenn die mittlere LED dauerhaft blinkt**, deutet dies auf ein Authentifizierungsproblem hin. Probieren Sie diese Problembehandlungsschritte aus:
    - Stellen Sie sicher, dass Ihre USB-A- und Micro-USB-Verbindungen stabil und fest sind. 
    - Überprüfen Sie, ob das [Sprachmodul ausgeführt wird](./troubleshoot-audio-accessory-speech-module.md#checking-runtime-status-of-the-speech-module).
    - Neustart des Geräts
    - [Erfassen Sie Protokolle](./troubleshoot-audio-accessory-speech-module.md#collecting-speech-module-logs), und fügen Sie sie einer Supportanfrage an.
    - Überprüfen Sie, ob auf Ihrem Dev Kit die neueste Software ausgeführt wird, und wenden Sie gegebenenfalls ein verfügbares Update an.

## <a name="checking-runtime-status-of-the-speech-module"></a>Überprüfen des Laufzeitstatus des Speech-Moduls

Überprüfen Sie, ob für **azureearspeechclientmodule** als Laufzeitstatus **Wird ausgeführt** angezeigt wird. Öffnen Sie zum Ermitteln des Laufzeitstatus Ihrer Gerätemodule das [Azure-Portal](https://portal.azure.com/), und navigieren Sie zu **Alle Ressourcen** ->  **[Ihr IoT Hub]**  -> **IoT Edge** ->  **[Ihre Geräte-ID]** . Klicken Sie auf die Registerkarte **Module**, um den Laufzeitstatus aller installierten Module anzuzeigen.

:::image type="content" source="./media/troubleshoot-audio-accessory-speech-module/over-the-air-iot-edge-device-page.png" alt-text="Seite „Edge-Gerät“ im Azure-Portal":::

Wird als Laufzeitstatus von **azureearspeechclientmodule** nicht **Wird ausgeführt** angezeigt, klicken Sie auf **Module festlegen** -> **azureearspeechclientmodule**. Legen Sie auf der Seite **Moduleinstellungen** die Option **Gewünschter Status** auf **Wird ausgeführt** fest, und klicken Sie auf **Aktualisieren**.

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

[local host file path] bezieht sich auf den Speicherort auf Ihrem Host-PC, an den Sie die TXT-Datei kopieren möchten. [remote username] ist der SSH-Benutzername, der während des [Setups](./quickstart-percept-dk-set-up.md) ausgewählt wurde.

## <a name="known-issues"></a>Bekannte Probleme
- Wenn Sie eine kostenlose Testversion verwenden, kann das Sprachmodell den Preisplan der kostenlosen Testversion überschreiten. In diesem Fall stellt das Modell die Funktion ohne Fehlermeldung ein.
- Wenn mehr als 5 IoT Edge-Geräte verbunden sind, wird der Bericht (der per Telemetrie an IoT Hub und Speech Studio gesendete Text) möglicherweise blockiert.
- Wenn sich das Gerät in einer anderen Region als die Ressourcen befindet, wird die Berichtsmeldung möglicherweise verzögert. 

## <a name="useful-links"></a>Nützliche Links
- [Einrichten von Azure Percept-Audio](./quickstart-percept-audio-setup.md)
- [Tasten- und LED-Verhalten von Azure Percept-Audio](./audio-button-led-behavior.md)
- [Erstellen eines Sprach-Assistenten mit Azure Percept DK und Azure Percept-Audio](./tutorial-no-code-speech.md)
- [Allgemeiner Problembehandlungsleitfaden für Azure Percept DK](./troubleshoot-dev-kit.md)
