---
title: Erstellen eines IoT Plug & Play Preview-Geräts (Vorschauversion) für die Zertifizierung | Microsoft-Dokumentation
description: Als Geräteentwickler erfahren Sie hier mehr darüber, wie Sie ein IoT Plug & Play Preview-Gerät für die Zertifizierung erstellen.
author: tbhagwat3
ms.author: tanmayb
ms.date: 12/28/2019
ms.topic: tutorial
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: ce7d3ee8a0d05d837bc0049cba688cffe14d8a8c
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721641"
---
# <a name="build-an-iot-plug-and-play-preview-device-thats-ready-for-certification"></a>Erstellen eines Azure IoT Plug & Play Preview-Geräts für die Zertifizierung

In diesem Tutorial erfahren Sie, wie Sie als Geräteentwickler ein IoT Plug & Play Preview-Gerät für die Zertifizierung erstellen.

Bei den Zertifizierungstests wird überprüft, ob:

- Ihr IoT Plug & Play-Gerätecode auf Ihrem Gerät installiert ist.
- Ihr IoT Plug & Play-Gerätecode mit dem Azure IoT SDK erstellt wurde.
- Ihr Gerätecode den [Azure IoT Hub Device Provisioning-Dienst](../iot-dps/about-iot-dps.md) unterstützt.
- Ihr Gerätecode die Schnittstelle „Geräteinformationen“ implementiert.
- Das Funktionsmodell und der Gerätecode sind mit IoT Central kompatibel.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- [Visual Studio Code](https://code.visualstudio.com/download)
- Erweiterungspaket [Azure IoT Tools für VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

Außerdem müssen Sie die Schritte unter [Schnellstart: Verwenden eines Gerätefunktionsmodells zum Erstellen eines IoT Plug & Play-Geräts (Vorschauversion, Windows)](quickstart-create-pnp-device-windows.md) ausführen. In der Schnellstartanleitung erfahren Sie, wie Sie Ihre Entwicklungsumgebung mithilfe von Vcpkg einrichten und ein Beispielprojekt erstellen.

## <a name="store-a-capability-model-and-interfaces"></a>Speichern von Gerätefunktionsmodellen und Schnittstellen

Bei IoT Plug & Play-Geräten müssen Sie ein Funktionsmodell und Schnittstellen erstellen, die die Funktionen des Geräts als JSON-Dateien definieren.

Sie können diese JSON-Dateien an drei verschiedenen Speicherorten speichern:

- im öffentlichen Modellrepository
- im Modellrepository Ihres Unternehmens
- auf Ihrem Gerät

Zurzeit müssen die Dateien zur Zertifizierung Ihres Geräts entweder im Repository Ihres Unternehmensmodells oder im öffentlichen Modellrepository gespeichert werden.

## <a name="include-the-required-interfaces"></a>Einbinden der erforderlichen Schnittstellen

Um den Zertifizierungsprozess zu bestehen, müssen Sie die Schnittstelle **Geräteinformationen** in Ihr Funktionsmodell einbinden und implementieren. Diese Schnittstelle weist die folgende Identifikation auf:

```json
"@id": "urn:azureiot:DeviceManagement:DeviceInformation:1"
```

> [!NOTE]
> Wenn Sie den [Schnellstart: Verwenden eines Gerätefunktionsmodells zum Erstellen eines Geräts](quickstart-create-pnp-device-windows.md)abgeschlossen haben, dann ist die Schnittstelle **Geräteinformationen** bereits in Ihr Modell eingebunden.

Fügen Sie zum Einbinden der Schnittstelle **Geräteinformationen** die Schnittstellen-ID der Eigenschaft `implements` des Funktionsmodells hinzu:

```json
{
  "@id": "urn:yourcompanyname:sample:ThermostatDevice:1",
  "@type": "CapabilityModel",
  "displayName": "Thermostat T-1000",
  "implements": [
    "urn:yourcompanyname:sample:Thermostat:1",
    "urn:azureiot:DeviceManagement:DeviceInformation:1"
  ],
  "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
}
```

So zeigen Sie die Schnittstelle **Geräteinformationen** in VS Code an:

1. Öffnen Sie mit **STRG+UMSCHALT+P** die Befehlspalette.

1. Geben Sie **Plug & Play** ein, und wählen Sie **IoT Plug & Play Open Model Repository** (IoT Plug & Play-Modellrepository öffnen) aus. Wählen Sie **Open Public Model Repository** (Öffentliches Modellrepository öffnen) aus. Das öffentliche Modellrepository wird in VS Code geöffnet.

1. Wählen Sie im öffentlichen Modellrepository die Registerkarte **Schnittstellen** aus, und geben Sie **Geräteinformationen** in das Feld „Filter“ ein.

1. Wählen Sie in der gefilterten Liste die Schnittstelle **Geräteinformationen** aus, um eine lokale Kopie davon zu erstellen, und wählen Sie dann **Download** aus. VS Code zeigt die Schnittstellendatei an.

Anzeigen der Schnittstelle **Geräteinformationen** mithilfe der Azure CLI:

1. [Installieren Sie die Erweiterung für die Azure IoT-Befehlszeilenschnittstelle (Azure CLI)](howto-install-pnp-cli.md).

1. Verwenden Sie den folgenden Azure CLI-Befehl, um eine Schnittstelle mit der ID der Schnittstelle „Geräteinformationen“ anzuzeigen:

    ```cmd/sh
    az iot pnp interface show --interface urn:azureiot:DeviceManagement:DeviceInformation:1
    ```

Weitere Informationen finden Sie unter [Installieren und Verwenden der Azure IoT-Erweiterung für Azure CLI](howto-install-pnp-cli.md).

## <a name="update-device-code"></a>Aktualisieren des Gerätecodes

### <a name="enable-device-provisioning-through-the-azure-iot-device-provisioning-service-dps"></a>Aktivieren der Gerätebereitstellung über Azure IoT Device Provisioning Service (DPS)

Zum Zertifizieren des Geräts müssen Sie die Gerätebereitstellung über [Azure IoT Device Provisioning Service (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps) aktivieren. Um die Verwendung von DPS zu aktivieren, können Sie in VS Code den C-Code-Stub generieren. Folgen Sie diesen Schritten:

1. Öffnen Sie den Ordner mit der DCM-Datei, und öffnen Sie mit **STRG+UMSCHALT+P** die Befehlspalette. Geben Sie **IoT Plug & Play** ein, und wählen Sie **Generate Device Code Stub** (Gerätecode-Stub generieren) aus.

1. Wählen Sie die DCM-Datei aus, mit der Sie den Gerätecode-Stub generieren möchten.

1. Geben Sie den Projektnamen ein, etwa **sample_device**. Dies ist der Name Ihrer Geräteanwendung.

1. Wählen Sie **ANSI C** als Sprache aus.

1. Wählen Sie als Verbindungsmethode die Option **Via DPS (Device Provisioning Service) symmetric key** (Mit symmetrischem Schlüssel per Device Provisioning-Dienst) aus.

1. Wählen Sie **CMake Project on Windows** (CMake-Projekt unter Windows) als Projektvorlage aus.

1. Wählen Sie **Via Vcpkg** (Über Vcpkg) als Methode zum Einschließen des Geräte-SDK aus.

1. VS Code öffnet ein neues Fenster mit generierten Gerätecode-Stubdateien.

## <a name="build-and-run-the-code"></a>Erstellen und Ausführen des Codes

Sie verwenden das Vcpkg-Paket, um den generierten Gerätecode-Stub zu erstellen. Die von Ihnen erstellte Anwendung simuliert ein Gerät, mit dem eine Verbindung mit einem IoT-Hub hergestellt wird. Die Anwendung sendet Telemetriedaten und Eigenschaften und empfängt Befehle.

1. Erstellen Sie im Ordner `sample_device` den Unterordner `cmake`, und navigieren Sie zu diesem Ordner:

    ```cmd
    mkdir cmake
    cd cmake
    ```

1. Führen Sie die folgenden Befehle aus, um den generierten Codestub zu erstellen (wobei Sie den Platzhalter durch das Verzeichnis des Vcpkg-Repositorys ersetzen):

    ```cmd
    cmake .. -G "Visual Studio 16 2019" -A Win32 -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"

    cmake --build .
    ```
    
    > [!NOTE]
    > Wenn Sie Visual Studio 2017 oder 2015 verwenden, müssen Sie den CMake-Generator entsprechend den verwendeten Buildtools angeben:
    >```cmd
    ># Either
    >cmake .. -G "Visual Studio 15 2017" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    ># or
    >cmake .. -G "Visual Studio 14 2015" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    >```

    > [!NOTE]
    > Falls CMake Ihren C++-Compiler nicht finden kann, treten beim Ausführen des obigen Befehls Buildfehler auf. Führen Sie den Befehl in diesem Fall an der [Visual Studio-Eingabeaufforderung](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) aus.

1. Geben Sie nach Abschluss des Buildvorgangs die DPS-Anmeldeinformationen (**DPS-ID-Bereich**, **symmetrischer DPS-Schlüssel**, **Geräte-ID**) als Parameter für die Anwendung ein. Informationen zum Ermitteln der Anmeldeinformationen im Zertifizierungsportal finden Sie unter [Verbinden und Testen Ihres IoT Plug & Play-Geräts](tutorial-certification-test.md#connect-and-discover-interfaces).

    ```cmd\sh
    .\Debug\sample_device.exe [Device ID] [DPS ID Scope] [DPS symmetric key]
    ```

### <a name="implement-standard-interfaces"></a>Implementieren von Standardschnittstellen

#### <a name="implement-the-model-information-and-sdk-information-interfaces"></a>Implementieren von Schnittstellen für Modellinformationen und SDK-Informationen

Das Azure IoT-Geräte-SDK implementiert die Schnittstellen für Modellinformationen und SDK-Informationen. Wenn Sie in VS Code die Funktion für die Codegenerierung verwenden, dann verwendet Ihr Gerätecode das IoT Plug & Play Device SDK.

Wenn Sie das Azure IoT-Geräte-SDK nicht verwenden möchten, können Sie den SDK-Quellcode als Referenz für Ihre eigene Implementierung verwenden.

#### <a name="implement-the-device-information-interface"></a>Implementieren der Schnittstelle „Geräteinformationen“

Implementieren Sie die Schnittstelle **Geräteinformationen** auf Ihrem Gerät, und stellen Sie zur Laufzeit gerätespezifische Informationen vom Gerät bereit.

Sie können eine Beispielimplementierung der Schnittstelle **Geräteinformationen** für [Linux](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) als Referenz verwenden.

### <a name="implement-all-the-capabilities-defined-in-your-model"></a>Implementieren aller in Ihrem Modell definierten Funktionen

Während der Zertifizierung wird das Gerät programmgesteuert getestet, um sicherzustellen, dass es die in seinen Schnittstellen definierten Funktionen implementiert. Reagieren Sie mit dem HTTP-Statuscode 501 auf die Lese-/Schreibeigenschaften und Befehlsanforderungen, wenn Ihr Gerät diese nicht implementiert.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie ein IoT Plug & Play-Gerät für die Zertifizierung erstellt haben, können Sie mit dem folgenden Schritt fortfahren:

> [!div class="nextstepaction"]
> [Zertifizieren Ihres Geräts](tutorial-certification-test.md)
