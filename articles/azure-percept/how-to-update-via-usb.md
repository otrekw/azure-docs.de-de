---
title: Aktualisieren Ihres Azure Percept DK-Geräts über eine USB-Verbindung
description: Hier erfahren Sie, wie Sie Ihr Azure Percept DK-Gerät über eine USB-Verbindung aktualisieren.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: 7f5e5e4da9fea671fc85a55fc8cc191fa14b720f
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660385"
---
# <a name="how-to-update-azure-percept-dk-over-a-usb-connection"></a>Aktualisieren Ihres Azure Percept DK-Geräts über eine USB-Verbindung

In dieser Anleitung erfahren Sie, wie Sie ein USB-Update für die Trägerplatine Ihres Azure Percept DK-Geräts durchführen.

## <a name="prerequisites"></a>Voraussetzungen
- Hostcomputer mit verfügbarem USB-C- oder USB-A-Anschluss.
- Azure Percept DK-Trägerplatine (Development Kit) und mitgeliefertes USB-Kabel (USB-C zu USB-C). Sollte Ihr Hostcomputer über keinen USB-C-Anschluss, sondern über einen USB-A-Anschluss verfügen, können Sie ein (separat erhältliches) USB-C-zu-USB-A-Kabel verwenden.
- Installieren Sie [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) (Administratorzugriff erforderlich).
- Installieren Sie das NXP-UUU-Tool. Laden Sie [hier](https://github.com/NXPmicro/mfgtools/releases) die neueste Version der Datei „uuu.exe“ (Windows) bzw. der UUU-Datei (Linux) herunter. Diese finden Sie auf der Registerkarte „Assets“ (Ressourcen).
- [Installieren Sie 7-Zip.](https://www.7-zip.org/) Diese Software wird zum Extrahieren der Rohimagedatei aus der komprimierten XZ-Datei verwendet. Laden Sie die entsprechende EXE-Datei herunter, und installieren Sie sie.

## <a name="steps"></a>Schritte
1.  Laden Sie die folgenden [drei USB-Updatedateien](https://go.microsoft.com/fwlink/?linkid=2155734) herunter:
    - pe101-uefi-***&lt;Versionsnummer&gt;***.raw.xz
    - emmc_full.txt
    - fast-hab-fw.raw
 
1. Extrahieren Sie die komprimierte Datei „pe101-uefi  **&lt;Versionsnummer&gt;** _**.raw.xz“ zu „pe101-uefi-*_ * _&lt;Versionsnummer.raw&gt;_ “. Benötigen Sie Hilfe beim Extrahieren? Laden Sie 7-Zip herunter, und installieren Sie es. Klicken Sie anschließend mit der rechten Maustaste auf die Imagedatei mit der Erweiterung **.xz**, und wählen Sie „7-Zip“ &gt; „Hier entpacken“ aus.

1. Kopieren Sie die folgenden drei Dateien in den Ordner, der das UUU-Tool enthält:
    - Extrahierte Datei „pe101-uefi-***&lt;Versionsnummer&gt;***.raw“ (aus Schritt 2)
    - „emmc_full.txt“ (aus Schritt 1)
    - „fast-hab-fw.raw“ (aus Schritt 1)
 
1. Schalten Sie das Development Kit ein.
1. [Stellen Sie eine SSH-Verbindung mit dem Development Kit her.](./how-to-ssh-into-percept-dk.md)
1. Öffnen Sie eine Windows-Eingabeaufforderung („Start“ &gt; „cmd“) oder ein Linux-Terminal, und navigieren Sie zum Ordner mit den Updatedateien. Führen Sie den folgenden Befehl aus, um das Update zu initiieren:
    - Windows: ```uuu -b emmc_full.txt fast-hab-fw.raw pe101-uefi-<version number>.raw```
    - Linux: ```sudo ./uuu -b emmc_full.txt fast-hab-fw.raw pe101-uefi-<version number>.raw```
    
Nach Ausführung dieser Befehle wird in der Eingabeaufforderung unter Umständen eine Meldung mit dem Hinweis angezeigt, dass auf das Gerät gewartet wird. Das ist normal, und Sie können mit dem nächsten Schritt fortfahren.
    
1. Verbinden Sie die Trägerplatine des Development Kits per USB-Kabel mit dem Hostcomputer. Verbinden Sie das Kabel immer zuerst mit dem USB-C-Anschluss der Trägerplatine und dann mit dem USB-C- oder USB-A-Anschluss des Hostcomputers (USB-C-zu-USB-A-Kabel separat erhältlich) – je nachdem, welche Anschlüsse verfügbar sind. 
 
1. Geben Sie im SSH-/PuTTY-Terminal die folgenden Befehle ein, um das Development Kit in den USB-Modus zu versetzen und es anschließend neu zu starten.
    - ```flagutil    -wBfRequestUsbFlash    -v1```
    - ```reboot -f```
 
1. Möglicherweise wird angezeigt, dass das Gerät vom Hostcomputer erkannt wurde, und der Updateprozess wird automatisch gestartet. Kehren Sie zur Eingabeaufforderung zurück, um den Status anzuzeigen. Der Prozess dauert bis zu zehn Minuten. War das Update erfolgreich, wird eine Meldung mit der Angabe angezeigt, dass ein erfolgreicher Prozess durchgeführt wurde und keine Fehler aufgetreten sind.
 
1. Schalten Sie nach Abschluss des Updates die Trägerplatine aus. Trennen Sie das USB-Kabel vom PC.  Verbinden Sie das Azure Percept-Vision-Modul wieder mit der Trägerplatine.

1. Schalten Sie die Trägerplatine wieder ein.

## <a name="next-steps"></a>Nächste Schritte

Ihr Development Kit wurde erfolgreich aktualisiert. Sie können die Entwicklung und den Betrieb mit Ihrem Development Kit fortsetzen.