---
title: Azure Kinect-Firmwaretool
description: Grundlegendes zum Abfragen und Aktualisieren der Gerätefirmeware mithilfe des Azure Kinect-Firmwaretools.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Firmware, Update
ms.openlocfilehash: f8b89751d27fb5a4b18d635f45f63f4f36bd05f6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "85276719"
---
# <a name="azure-kinect-dk-firmware-tool"></a>Azure Kinect DK-Firmwaretool

Das Azure Kinect-Firmwaretool kann verwendet werden, um die Gerätefirmware des Azure Kinect DK abzufragen und zu aktualisieren.

## <a name="list-connected-devices"></a>Auflisten verbundener Geräte

Mithilfe der -l-Option können Sie eine Liste der verbundenen Geräte abrufen.  `AzureKinectFirmwareTool.exe -l`

```console
 == Azure Kinect DK Firmware Tool ==
Found 2 connected devices:
0: Device "000036590812"
1: Device "000274185112"
```

## <a name="check-device-firmware-version"></a>Überprüfen der Gerätefirmware-Version

Sie können die aktuelle Firmwareversion des ersten angeschlossenen Geräts mithilfe der -q-Option überprüfen, beispielsweise `AzureKinectFirmwareTool.exe -q`.

```console
 == Azure Kinect DK Firmware Tool ==
Device Serial Number: 000036590812
Current Firmware Versions:
    RGB camera firmware:      1.5.92
    Depth camera firmware:    1.5.66
    Depth config file:        6109.7
    Audio firmware:           1.5.14
    Build Config:             Production
    Certificate Type:         Microsoft
```

Wenn mehr als ein Gerät angeschlossen ist, können Sie angeben, welches Gerät Sie abfragen möchten, indem Sie dem Befehl die vollständige Seriennummer hinzufügen, wie etwa hier:

`AzureKinectFirmwareTool.exe -q 000036590812`

## <a name="update-device-firmware"></a>Aktualisieren der Gerätefirmware

Das häufigste Einsatzgebiet dieses Tools besteht im Aktualisieren der Gerätefirmware. Führen Sie die Aktualisierung durch Aufrufen des Tools mithilfe der `-u`-Option durch. Ein Firmwareupdate kann einige Minuten in Anspruch nehmen, je nachdem, welche Firmwaredateien aktualisiert werden müssen.

Eine Schritt-für-Schritt-Anleitung zum Firmwareupdate finden Sie unter [Azure Kinect-Firmwareupdate](update-device-firmware.md).  

`AzureKinectFirmwareTool.exe -u firmware\AzureKinectDK_Fw_1.5.926614.bin`

Wenn mehr als ein Gerät angeschlossen ist, können Sie angeben, welches Gerät Sie abfragen möchten, indem Sie dem Befehl die vollständige Seriennummer hinzufügen.

`AzureKinectFirmwareTool.exe -u firmware\AzureKinectDK_Fw_1.5.926614.bin 000036590812`

## <a name="reset-device"></a>Zurücksetzen des Geräts

Ein angeschlossenes Azure Kinect DK kann mithilfe der -r-Option zurückgesetzt werden, falls Sie für das Gerät einen bekannten Zustand herstellen müssen.

Wenn mehr als ein Gerät angeschlossen ist, können Sie angeben, welches Gerät Sie abfragen möchten, indem Sie dem Befehl die vollständige Seriennummer hinzufügen.

`AzureKinectFirmwareTool.exe -r 000036590812`

## <a name="inspect-firmware"></a>Untersuchen der Firmware

Durch Untersuchen der Firmware können Sie die Versionsinformationen aus einer BIN-Firmwaredatei abrufen, bevor Sie ein Update eines realen Geräts durchführen.

`AzureKinectFirmwareTool.exe -i firmware\AzureKinectDK_Fw_1.5.926614.bin`

```console
 == Azure Kinect DK Firmware Tool ==
Loading firmware package ..\tools\updater\firmware\AzureKinectDK_Fw_1.5.926614.bin.
File size: 1228844 bytes
This package contains:
  RGB camera firmware:      1.5.92
  Depth camera firmware:    1.5.66
  Depth config files: 6109.7 5006.27
  Audio firmware:           1.5.14
  Build Config:             Production
  Certificate Type:         Microsoft
  Signature Type:           Microsoft
```

## <a name="firmware-update-tool-options"></a>Optionen für das Firmwareupdatetool

```console
 == Azure Kinect DK Firmware Tool ==
* Usage Info *
    AzureKinectFirmwareTool.exe <Command> <Arguments>

Commands:
    List Devices: -List, -l
    Query Device: -Query, -q
        Arguments: [Serial Number]
    Update Device: -Update, -u
        Arguments: <Firmware Package Path and FileName> [Serial Number]
    Reset Device: -Reset, -r
        Arguments: [Serial Number]
    Inspect Firmware: -Inspect, -i
        Arguments: <Firmware Package Path and FileName>

    If no Serial Number is provided, the tool will just connect to the first device.

Examples:
    AzureKinectFirmwareTool.exe -List
    AzureKinectFirmwareTool.exe -Update c:\data\firmware.bin 0123456
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Schritt-für-Schritt-Anleitung zum Aktualisieren der Gerätefirmware](update-device-firmware.md)
