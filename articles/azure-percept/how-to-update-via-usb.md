---
title: Aktualisieren Ihres Azure Percept DK-Geräts über eine USB-C-Kabelverbindung
description: Hier erfahren Sie, wie Sie Ihr Azure Percept DK-Gerät über eine USB-C-Kabelverbindung aktualisieren.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 03/18/2021
ms.custom: template-how-to
ms.openlocfilehash: 39889455cd41883f5782f9fb140b400d6ed09ed7
ms.sourcegitcommit: 32ee8da1440a2d81c49ff25c5922f786e85109b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109786733"
---
# <a name="how-to-update-azure-percept-dk-over-a-usb-c-cable-connection"></a>Aktualisieren eines Azure Percept DK-Geräts über eine USB-C-Kabelverbindung

In dieser Anleitung wird beschrieben, wie Sie das Betriebssystem und die Firmware Ihres Development Kits erfolgreich über eine USB-Verbindung aktualisieren. Hier finden Sie eine Übersicht darüber, welche Schritte Sie im Rahmen dieses Verfahrens ausführen.
1. Herunterladen des Updatepakets auf einen Hostcomputer
1. Ausführen des Befehls, mit dem das Updatepaket an das Entwicklerkit übertragen wird
1. Versetzen des Entwicklerkits in den USB-Modus (mithilfe von SSH), sodass es vom Hostcomputer erkannt werden und das Updatepaket empfangen kann
1. Verbinden des Entwicklerkits mit dem Hostcomputer per USB-C-Kabel
1. Warten, bis das Update abgeschlossen ist

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

1. [NXP UUU-Tool:](https://github.com/NXPmicro/mfgtools/releases) Laden Sie das **neueste Release** der Datei „uuu.exe“ (Windows) bzw. der UUU-Datei (Linux) herunter. Diese finden Sie auf der Registerkarte **Ressourcen**. UUU ist ein Tool, das von NXP entwickelt wurde und zum Aktualisieren von NXP-Entwicklungsboards verwendet wird.

1. [Laden Sie die Updatedateien herunter.](https://go.microsoft.com/fwlink/?linkid=2155734) Sie befinden sich alle in einer ZIP-Datei, die Sie im nächsten Abschnitt extrahieren werden.

1. Stellen Sie sicher, dass alle drei Buildartefakte vorhanden sind:
    - Azure-Percept-DK- *&lt;Versionsnummer&gt;* .raw
    - fast-hab-fw.raw
    - emmc_full.txt

## <a name="set-up-your-environment"></a>Einrichten der Umgebung

1. Erstellen Sie einen Ordner bzw. ein Verzeichnis auf dem Hostcomputer an einem Speicherort, der über die Befehlszeile leicht zugänglich ist.

1. Kopieren Sie das UUU-Tool (**uuu.exe** oder **uuu**) in den neuen Ordner.

1. Extrahieren Sie die zuvor heruntergeladenen Updatedateien in den neuen Ordner, der das UUU-Tool enthält.

## <a name="update-your-device"></a>Aktualisieren Ihres Geräts

Bei diesem Verfahren wird der einzelne USB-C-Anschluss des Entwicklerkits für die Aktualisierung verwendet.  Verfügt Ihr Computer über einen USB-C-Anschluss, können Sie das Azure Percept Vision-Gerät trennen und dieses Kabel verwenden.  Wenn Ihr Computer nur über einen USB-A-Anschluss verfügt, trennen Sie das Azure Percept Vision-Gerät vom USB-C-Anschluss des Entwicklerkits, und schließen Sie ein USB-C-zu-USB-A-Kabel (separat erhältlich) am Entwicklerkit und am Hostcomputer an.

1. Öffnen Sie eine Windows-Eingabeaufforderung (Start > cmd) oder ein Linux-Terminal, und **navigieren Sie zum Ordner mit den Updatedateien und dem UUU-Tool**. 

1. Geben Sie in der Befehlszeile oder im Terminal den folgenden Befehl ein:

    - Windows:

        ```console
        uuu -b emmc_full.txt fast-hab-fw.raw Azure-Percept-DK-<version number>.raw 
        ```

    - Linux:

        ```bash
        sudo ./uuu -b emmc_full.txt fast-hab-fw.raw Azure-Percept-DK-<version number>.raw
        ```

1. Im Eingabeaufforderungsfenster wird die folgende Meldung angezeigt: **Waiting for Known USB Device to Appear...** (Warten auf die Anzeige eines bekannten USB-Geräts...). Das UUU-Tool wartet nun, dass das Entwicklerkit vom Hostcomputer erkannt wird. Sie können jetzt mit den nächsten Schritten fortfahren.

1. Stecken Sie das mitgelieferte USB-C-Kabel in den USB-C-Anschluss des Entwicklerkits und in den USB-C-Anschluss des Hostcomputers. Wenn Ihr Computer nur über einen USB-A-Anschluss verfügt, verbinden Sie ein USB-C-zu-USB-A-Kabel (separat erhältlich) mit dem Entwicklerkit und dem Hostcomputer.

1. Stellen Sie eine SSH-Verbindung mit Ihrem Entwicklerkit her. Wenn Sie Hilfe zu SSH benötigen, befolgen Sie [diese Anweisungen](./how-to-ssh-into-percept-dk.md).

1. Geben Sie im SSH-Terminal die folgenden Befehle ein:

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

1. Schalten Sie nach Abschluss des Updates das Entwicklerkit aus. Trennen Sie das USB-Kabel vom PC.  

## <a name="next-steps"></a>Nächste Schritte

Nutzen Sie die [Azure Percept DK-Setupfunktion](./quickstart-percept-dk-set-up.md), um Ihr Gerät neu zu konfigurieren.
