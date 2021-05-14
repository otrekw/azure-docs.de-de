---
title: Aktualisieren Ihres Azure Percept DK-Geräts über eine USB-Verbindung
description: Hier erfahren Sie, wie Sie Ihr Azure Percept DK-Gerät über eine USB-Verbindung aktualisieren.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 03/18/2021
ms.custom: template-how-to
ms.openlocfilehash: e8ab657ec7b4ef9a413993c064c931b4fc1523c5
ms.sourcegitcommit: bd1a4e4df613ff24e954eb3876aebff533b317ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2021
ms.locfileid: "107929451"
---
# <a name="how-to-update-azure-percept-dk-over-a-usb-connection"></a>Aktualisieren Ihres Azure Percept DK-Geräts über eine USB-Verbindung

Obwohl die Verwendung von OTA-Updates (Over-the-Air) die beste Methode darstellt, das Betriebssystem und die Firmware Ihres Development Kits auf dem neuesten Stand zu halten, gibt es Szenarios, in denen das Aktualisieren (oder Flashen) des Development Kits über eine USB-Verbindung erforderlich ist:

- Ein OTA-Update ist aufgrund von Konnektivitätsproblemen oder anderen technischen Problemen nicht möglich.
- Das Gerät muss auf die Werkseinstellungen zurückgesetzt werden.

In dieser Anleitung wird beschrieben, wie Sie das Betriebssystem und die Firmware Ihres Development Kits erfolgreich über eine USB-Verbindung aktualisieren.

> [!WARNING]
> Wenn Sie Ihr Development Kit über eine USB-Verbindung aktualisieren, werden alle vorhandenen Daten auf dem Gerät gelöscht (einschließlich der KI-Modelle und Container).
>
> Befolgen Sie alle Anweisungen in der angegebenen Reihenfolge. Wenn Sie Schritte überspringen, kann Ihr Development Kit möglicherweise nicht mehr verwendet werden.

## <a name="prerequisites"></a>Voraussetzungen

- Azure Percept DK
- Ein auf Windows, Linux oder Mac OS X basierender Hostcomputer mit WLAN-Funktion und einem verfügbaren USB-C- oder USB-A-Anschluss
- Ein USB-C-zu-USB-A-Kabel (optional, separat erhältlich)
- SSH-Anmeldung, die während des [Azure Percept DK-Setups](./quickstart-percept-dk-set-up.md) erstellt wurde

## <a name="download-software-tools-and-update-files"></a>Herunterladen von Softwaretools und Updatedateien

1. [NXP UUU-Tool:](https://github.com/NXPmicro/mfgtools/releases) Laden Sie das **neueste Release** der Datei „uuu.exe“ (Windows) bzw. der UUU-Datei (Linux) herunter. Diese finden Sie auf der Registerkarte **Ressourcen**.

1. [7-Zip:](https://www.7-zip.org/) Diese Software wird zum Extrahieren der Rohimagedatei aus der komprimierten XZ-Datei verwendet. Laden Sie die entsprechende EXE-Datei herunter, und installieren Sie sie.

1. [Laden Sie die Updatedateien herunter.](https://go.microsoft.com/fwlink/?linkid=2155734) Sie befinden sich alle in einer ZIP-Datei, die Sie im nächsten Abschnitt extrahieren werden.

1. Stellen Sie sicher, dass alle drei Buildartefakte vorhanden sind:
    - Azure-Percept-DK- *&lt;Versionsnummer&gt;* .raw.xz
    - fast-hab-fw.raw
    - emmc_full.txt

## <a name="set-up-your-environment"></a>Einrichten der Umgebung

1. Erstellen Sie einen Ordner bzw. ein Verzeichnis auf dem Hostcomputer an einem Speicherort, der über die Befehlszeile leicht zugänglich ist.

1. Kopieren Sie das UUU-Tool (**uuu.exe** oder **uuu**) in den neuen Ordner.

1. Extrahieren Sie die zuvor heruntergeladenen Updatedateien in den neuen Ordner, der das UUU-Tool enthält.

## <a name="update-your-device"></a>Aktualisieren Ihres Geräts

1. [Stellen Sie eine SSH-Verbindung mit Ihrem Development Kit her.](./how-to-ssh-into-percept-dk.md)

1. Öffnen Sie als Nächstes eine Windows-Eingabeaufforderung (**Start** > **CMD**) oder ein Linux-Terminal, und navigieren Sie zum Ordner mit den Updatedateien und dem UUU-Tool. Geben Sie den folgenden Befehl in der Eingabeaufforderung oder im Terminal ein, um Ihren Computer für das Empfangen eines flashbaren Geräts vorzubereiten:

    - Windows:

        ```console
        uuu -b emmc_full.txt fast-hab-fw.raw Azure-Percept-DK-<version number>.raw 
        ```

    - Linux:

        ```bash
        sudo ./uuu -b emmc_full.txt fast-hab-fw.raw Azure-Percept-DK-<version number>.raw
        ```

1. Trennen Sie das Azure Percept Vision-Gerät vom USB-C-Anschluss der Trägerplatine.

1. Stecken Sie das mitgelieferte USB-C-Kabel in den USB-C-Anschluss der Trägerplatine und in den USB-C-Anschluss des Hostcomputers. Wenn Ihr Computer nur über einen USB-A-Anschluss verfügt, verbinden Sie ein USB-C-zu-USB-A-Kabel (wird separat verkauft) mit der Trägerplatine und dem Hostcomputer.

1. Geben Sie in der SSH-Clienteingabeaufforderung die folgenden Befehle ein:

    1. Legen Sie für das Gerät den USB-Updatemodus fest:

        ```bash
        sudo flagutil    -wBfRequestUsbFlash    -v1
        ```

    1. Starten Sie das Gerät neu. Die Updateinstallation wird gestartet.

        ```bash
        sudo reboot -f
        ```

1. Navigieren Sie zurück zur anderen Eingabeaufforderung oder zum anderen Terminal. Wenn das Update abgeschlossen ist, wird eine Meldung mit den Informationen ```Success 1    Failure 0``` angezeigt:

    > [!NOTE]
    > Nach der Aktualisierung wird Ihr Gerät auf die Werkseinstellungen zurückgesetzt, und die WLAN-Verbindung und die SSH-Anmeldung werden gelöscht.

1. Schalten Sie nach Abschluss des Updates die Trägerplatine aus. Trennen Sie das USB-Kabel vom PC.  

## <a name="next-steps"></a>Nächste Schritte

Nutzen Sie die [Azure Percept DK-Setupfunktion](./quickstart-percept-dk-set-up.md), um Ihr Gerät neu zu konfigurieren.
