---
title: Erstellen und Bereitstellen der IoT Plug & Play-Bridge | Microsoft-Dokumentation
description: Identifizieren Sie die IoT Plug & Play-Bridge-Komponenten. Hier erfahren Sie, wie Sie sie auf IoT-Geräten, -Gateways und als IoT Edge-Modul ausführen.
author: usivagna
ms.author: ugans
ms.date: 1/20/2021
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: c89427f83600d3b8091d5293b1757fa6f1a15ef1
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102202483"
---
# <a name="build-and-deploy-the-iot-plug-and-play-bridge"></a>Erstellen und Bereitstellen der IoT Plug & Play-Bridge

Mit der [IoT Plug & Play-Bridge](concepts-iot-pnp-bridge.md#iot-plug-and-play-bridge-architecture) können Sie eine Verbindung der vorhandenen Geräte, die an ein Gateway angeschlossen sind, mit Ihrer IoT-Hub-Instanz herstellen. Mit der Bridge ordnen Sie angeschlossenen Geräten IoT Plug & Play-Schnittstellen zu. Mit einer IoT Plug & Play-Schnittstelle werden die Telemetriedaten, die ein Gerät sendet, die Eigenschaften, die zwischen dem Gerät und der Cloud synchronisiert werden, und die Befehle, auf die das Gerät reagiert, definiert. Die Open-Source-Bridge-Anwendung kann auf Windows- oder Linux-Gateways installiert und konfiguriert werden. Außerdem kann die Bridge als Azure IoT Edge-Runtimemodul ausgeführt werden.

In diesem Artikel wird Folgendes ausführlich erläutert:

- Konfigurieren einer Bridge
- Erstellen und Ausführen der Bridge in verschiedenen Umgebungen

Ein einfaches Beispiel für die Verwendung der Bridge finden Sie unter [Herstellen einer Verbindung eines IoT Plug & Play-Bridge-Beispiels unter Linux oder Windows mit IoT-Hub](howto-use-iot-pnp-bridge.md).

Bei dem Leitfaden und den Beispielen in diesem Artikel werden grundlegende Kenntnisse in [Azure Digital Twins](../digital-twins/overview.md) und [IoT Plug & Play](overview-iot-plug-and-play.md) vorausgesetzt.

## <a name="general-prerequisites"></a>Allgemeine Voraussetzungen

### <a name="supported-os-platforms"></a>Unterstützte Betriebssystemplattformen

Die folgenden Betriebssystemplattformen und Versionen werden unterstützt:

|Plattform  |Unterstützte Versionen  |
|---------|---------|
|Windows 10 |     Alle Windows-SKUs werden unterstützt. Beispiel: IoT Enterprise, Server, Desktop, IoT Core. *Für die Funktionalität zur Überwachung des Kamera-Integritätsstatus wird Build 20H1 oder ein späterer Build empfohlen. Alle anderen Funktionen stehen in allen Windows 10-Builds zur Verfügung.*  |
|Linux     |Getestet unter und unterstützt für Ubuntu 18.04, die Funktionalität anderer Distributionen wurde nicht getestet.         |
||

### <a name="hardware"></a>Hardware

- Alle Hardwareplattformen, die die oben genannten Betriebssystem-SKUs und -Versionen unterstützen.
- Serielle, USB-, Bluetooth- und Kamera-Peripheriegeräte und -Sensoren werden nativ unterstützt. Die IoT Plug & Play-Bridge kann erweitert werden, um beliebige benutzerdefinierte Peripheriegeräte oder Sensoren zu unterstützen.

### <a name="azure-iot-products-and-tools"></a>Azure IoT-Produkte und -Tools

- **Azure IoT Hub**: Sie benötigen einen [Azure IoT-Hub](../iot-hub/index.yml) in Ihrem Azure-Abonnement, mit dem Sie Ihr Gerät verbinden. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Wenn Sie keinen IoT-Hub besitzen, [führen Sie diese Anweisungen aus, um einen zu erstellen](../iot-hub/iot-hub-create-using-cli.md). IoT Plug & Play-Unterstützung ist in IoT-Hubs im Basic-Tarif nicht enthalten.
- **Azure IoT-Explorer**: Für die Interaktion mit Ihrem IoT Plug & Play-Gerät können Sie das Azure IoT-Explorer-Tool verwenden. Dazu müssen Sie [die neueste Version von Azure IoT-Explorer für Ihr Betriebssystem herunterladen und installieren](./howto-use-iot-explorer.md). Konfigurieren Sie das Azure IoT-Explorer-Tool, um mit Ihrem IoT-Hub eine Verbindung herzustellen und im Ordner *pnpbridge\docs\schemas* im geklonten Repository **iot-plug-and-play-bridge** nach Modelldefinitionen zu suchen.

## <a name="configure-a-bridge"></a>Konfigurieren einer Bridge

Es gibt zwei Möglichkeiten, die Bridge zu konfigurieren:

- Verwenden Sie die Konfigurationsdatei, wenn die Bridge nativ auf einem IoT-Gerät oder -Gateway ausgeführt wird. In diesem Szenario kann ein Linux- oder Windows-Computer verwendet werden.
- Verwenden Sie die gewünschte Eigenschaft des Modulzwillings, wenn die Bridge als IoT Edge-Modul in der IoT Edge-Runtime ausgeführt wird. In diesem Szenario wird davon ausgegangen, dass die IoT Edge-Runtime in einer Linux-Umgebung gehostet wird.

### <a name="configuration-file"></a>Konfigurationsdatei

Wenn die Plug & Play-Bridge nativ auf einem IoT-Gerät oder -Gateway ausgeführt wird, wird eine Konfigurationsdatei verwendet.

- Lassen Sie sich die IoT Central- oder IoT Hub-Verbindungsinformationen geben.
- Konfigurieren Sie Geräte, für die IoT Plug & Play-Schnittstellen veröffentlicht wurden.

Verwenden Sie eine der folgenden Optionen, um die Konfigurationsdatei für die Bridge bereitzustellen:

- Übergeben Sie den Pfad zur Konfigurationsdatei als Befehlszeilenparameter an die ausführbare Bridge-Datei.
- Verwenden Sie die im Ordner *pnpbridge\cmake\pnpbridge_x86\src\pnpbridge\samples\console* vorhandene *config.json*-Datei.

Das [Schema der Konfigurationsdatei](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/src/pnpbridge/src/pnpbridge_config_schema.json) ist im GitHub-Repository verfügbar.

> [!TIP]
> Wenn Sie in VS Code eine Bridge-Konfigurationsdatei verwenden, können Sie Ihre Datei mit diesem Schema überprüfen.

### <a name="iot-edge-module-configuration"></a>Konfiguration des IoT Edge-Moduls

Wenn die Bridge als IoT Edge-Modul in einer IoT Edge-Runtime ausgeführt wird, wird die Konfigurationsdatei als Update aus der Cloud an die gewünschte `PnpBridgeConfig`-Eigenschaft gesendet. Die Adapter und Komponenten werden von der Bridge erst nach dieser Eigenschaftsaktualisierung konfiguriert.

## <a name="build-and-run-the-bridge-on-an-iot-device-or-gateway"></a>Erstellen und Ausführen der Bridge auf einem IoT-Gerät oder -Gateway

| Plattform | Unterstützt |
| :-----------: | :-----------: |
| Windows |  Ja |
| Linux | Ja |

### <a name="prerequisites"></a>Voraussetzungen

Für diesen Abschnitt müssen Sie auf Ihrem lokalen Computer folgende Software installieren:

- Eine Entwicklungsumgebung, in der die Kompilierung von C++ unterstützt wird, z. B. [Visual Studio (Community, Professional oder Enterprise)](https://visualstudio.microsoft.com/downloads/): Stellen Sie sicher, dass Sie die Komponente NuGet-Paket-Manager und die Workload **Desktopentwicklung mit C++** aktivieren, wenn Sie Visual Studio installieren.
- [CMake](https://cmake.org/download/): Wählen Sie beim Installieren von CMake die Option **Add CMake to the system PATH** (CMake dem Systempfad hinzufügen).
- Wenn Sie Builds unter Windows erstellen, müssen Sie außerdem das [Windows 17763 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) herunterladen.

### <a name="source-code"></a>Quellcode

Klonen Sie das Repository [IoT Plug & Play-Bridge](https://github.com/Azure/iot-plug-and-play-bridge) auf Ihren lokalen Computer:

```console
git clone https://github.com/Azure/iot-plug-and-play-bridge.git

cd iot-plug-and-play-bridge

git submodule update --init --recursive
```

Rechnen Sie damit, dass die Ausführung dieses Befehls einige Minuten dauern kann.

> [!NOTE]
> Wenn Sie Probleme haben, weil beim Ausführen von `git submodule update` unter Windows ein Fehler auftritt, versuchen Sie das Problem mit folgendem Befehl zu beheben: `git config --system core.longpaths true`.

### <a name="build-the-bridge-on-windows"></a>Erstellen der Bridge unter Windows

Öffnen Sie die **Developer-Eingabeaufforderung für VS 2019**, navigieren Sie zu dem Ordner mit dem geklonten Repository, und führen Sie die folgenden Befehle aus:

```console
cd pnpbridge\scripts\windows

build.cmd
```

Rechnen Sie damit, dass die Ausführung dieses Befehls einige Minuten dauern kann.

Optional können Sie die generierte Projektmappendatei *pnpbridge\cmake\pnpbridge_x86\azure_iot_pnp_bridge.sln* in Visual Studio öffnen, um den Code zu bearbeiten, neu zu erstellen und zu debuggen.

> [!TIP]
> In diesem Projekt werden die Projektdateien mit CMAKE erstellt. Wenn Sie die entsprechenden CMAKE-Dateien nicht aktualisieren, gehen alle Änderungen verloren, die Sie in Visual Studio am Projekt vornehmen.

### <a name="build-the-bridge-on-linux"></a>Erstellen der Bridge unter Linux

Navigieren Sie mithilfe der Bash-Shell zu dem Ordner mit dem geklonten Repository, und führen Sie die folgenden Befehle aus:

```bash
cd scripts/linux

./setup.sh

./build.sh
```

### <a name="configure-the-generic-sensors"></a>Konfigurieren der generischen Sensoren

Ändern Sie die folgenden Parameter unter dem Knoten `pnp_bridge_connection_parameters` in der Datei *config.json* im Ordner *iot-plug-and-play-bridge\pnpbridge\cmake\pnpbridge_x86\src\pnpbridge\samples\console* unter Windows oder im Ordner *iot-plug-and-play-bridge/pnpbridge/cmake/pnpbridge_linux\src/pnpbridge/samples/console* unter Linux:

Wenn Sie zum Herstellen einer Verbindung mit dem IoT-Hub eine Verbindungszeichenfolge verwenden:

```JSON
{
  "connection_parameters": {
    "connection_type" : "connection_string",
    "connection_string" : "dtmi:com:example:RootPnpBridgeSampleDevice;1",
    "root_interface_model_id": "[To fill in]",
    "auth_parameters": {
      "auth_type": "symmetric_key",
      "symmetric_key": "[To fill in]"
    }
  }
}
```

> [!TIP]
> Der Wert `symmetric_key` muss mit dem SAS-Schlüssel in der Verbindungszeichenfolge übereinstimmen.

Wenn Sie zum Herstellen einer Verbindung mit dem IoT-Hub oder der IoT Central-Anwendung DPS verwenden:

```JSON
{
  "connection_parameters": {
    "connection_type" : "dps",
    "root_interface_model_id": "dtmi:com:example:RootPnpBridgeSampleDevice;1",
    "auth_parameters" : {
      "auth_type" : "symmetric_key",
      "symmetric_key" : "[DEVICE KEY]"
    },
    "dps_parameters" : {
      "global_prov_uri" : "[GLOBAL PROVISIONING URI] - typically it is global.azure-devices-provisioning.net",
      "id_scope": "[IoT Central / IoT Hub ID SCOPE]",
      "device_id": "[DEVICE ID]"
    }
  }
}
```

Verschaffen Sie sich einen Überblick über die restlichen Konfigurationsdateien, um zu sehen, welche Schnittstellenkomponenten und globalen Parameter in diesem Beispiel konfiguriert sind.

### <a name="start-the-bridge-in-windows"></a>Starten der Bridge unter Windows

Starten Sie die Bridge, indem Sie sie an der Eingabeaufforderung ausführen:

```console
cd iot-plug-and-play-bridge\pnpbridge\cmake\pnpbridge_x86\src\pnpbridge\samples\console

Debug\pnpbridge_bin.exe
```

> [!TIP]
> Wenn Sie eine Konfigurationsdatei an einem anderen Speicherort verwenden möchten, übergeben Sie den entsprechenden Pfad beim Ausführen der Bridge als Befehlszeilenparameter.
  
> [!TIP]
> Wenn im PC eine Kamera integriert ist oder am PC eine USB-Kamera angeschlossen ist, können Sie eine Anwendung starten, die Kameras verwendet, z. B. die integrierte **Kamera**-App. Während die **Kamera**-App ausgeführt wird, werden in der Ausgabe der Bridge-Konsole die Überwachungsstatistiken angezeigt und die Bildfrequenz wird über die Schnittstelle für den digitalen Zwilling an den IoT-Hub übertragen.

## <a name="build-and-run-the-bridge-as-an-iot-edge-module"></a>Erstellen und Ausführen der Bridge als IoT Edge-Modul

| Plattform | Unterstützt |
| :-----------: | :-----------: |
| Windows |  Nein |
| Linux | Ja |

### <a name="prerequisites"></a>Voraussetzungen

Für diesen Abschnitt benötigen Sie einen IoT-Hub in Azure im Tarif „Free“ oder „Standard“. Informationen zum Erstellen eines IoT-Hubs finden Sie unter [Erstellen eines IoT-Hubs](../iot-hub/iot-hub-create-through-portal.md).

Bei den Schritten in diesem Abschnitt wird davon ausgegangen, dass sich auf einem Windows 10-Computer die folgende Entwicklungsumgebung befindet. Mit den folgenden Tools können Sie ein IoT Edge-Modul erstellen und auf Ihrem IoT Edge-Gerät bereitstellen:

- Windows-Subsystem für Linux (WSL) 2, auf dem Ubuntu 18.04 LTS ausgeführt wird. Weitere Informationen hierzu finden Sie unter [Windows-Subsystem für Linux: Installationsleitfaden für Windows 10](/windows/wsl/install-win10).
- Docker Desktop für Windows, konfiguriert für die Verwendung von WSL 2. Weitere Informationen finden Sie unter [Docker Desktop WSL 2 backend](https://docs.docker.com/docker-for-windows/wsl/) (Docker Desktop WSL 2-Back-End).
- [Visual Studio Code in der Windows-Umgebung](https://code.visualstudio.com/docs/setup/windows) mit den folgenden drei Erweiterungen installiert:

  - [Erweiterungspaket „Remote Development“](https://aka.ms/vscode-remote/download/extension): Mit dieser Erweiterung können Sie in WSL 2 Code erstellen und ausführen.
  - [Docker für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker): Diese Erweiterung muss in der Umgebung **WSL: Ubuntu-18.04** von VS Code aktiviert sein.
  - [Azure IoT Tools für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools): Diese Erweiterung muss in der Umgebung **WSL: Ubuntu-18.04** von VS Code aktiviert sein.

- Führen Sie in der WSL 2-Umgebung die folgenden Befehle aus, um die erforderlichen Buildtools zu installieren:

  ```bash
  sudo apt-get update
  sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
  ```

- Die in der WSL 2-Umgebung installierte [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli-apt) zum Verwalten der Azure-Ressourcen.

  > [!TIP]
  > Wenn Sie möchten, können Sie die `az`-Befehle in der [Azure Cloud Shell](https://shell.azure.com/) ausführen. Dort ist die Befehlszeilenschnittstelle bereits vorinstalliert.

### <a name="create-an-iot-edge-device"></a>Erstellen eines IoT Edge-Geräts

Mit diesen Befehlen können Sie ein IoT Edge-Gerät erstellen, das auf einem virtuellen Azure-Computer ausgeführt wird. Die Ausführung eines IoT Edge-Geräts auf einem virtuellen Computer ist nützlich, wenn Sie Ihre Bereitstellung testen möchten und keinen Zugriff auf ein echtes Gerät haben.

Führen Sie die folgenden Befehle in der WSL 2-Umgebung aus, um eine IoT Edge-Geräteregistrierung zu erstellen. Verwenden Sie den Befehl `az login`, um sich bei Ihrem Azure-Abonnement anzumelden:

```azurecli
az iot hub device-identity create --device-id bridge-edge-device --edge-enabled true --hub-name {your IoT hub name}
```

Führen Sie die folgenden Befehle aus, um einen virtuellen Azure-Computer zu erstellen, auf dem IoT Edge-Runtime installiert ist. Aktualisieren Sie die Platzhalter mit geeigneten Werten:

```azurecli
az group create --name bridge-edge-resources --location eastus
az deployment group create \
--resource-group bridge-edge-resources \
--template-uri "https://aka.ms/iotedge-vm-deploy" \
--parameters dnsLabelPrefix='{unique DNS name for virtual machine}' \
--parameters adminUsername='{admin user name}' \
--parameters deviceConnectionString=$(az iot hub device-identity connection-string show --device-id bridge-edge-device --hub-name {your IoT hub name} -o tsv) \
--parameters authenticationType='password' \
--parameters adminPasswordOrKey="{admin password for virtual machine}"
```

Nun wird IoT Edge-Runtime auf einem virtuellen Computer ausgeführt. Mit dem folgenden Befehl können Sie sich vergewissern, dass **$edgeAgent** und **$edgeHub** auf dem Gerät ausgeführt werden:

```azurecli
az iot hub module-identity list --device-id bridge-edge-device -o table --hub-name {your IoT hub name}
```

> [!CAUTION]
> Für die Ausführung dieses virtuellen Computers fallen Gebühren an. Daher sollten Sie ihn herunterfahren, wenn Sie ihn nicht verwenden, und Sie sollten ihn entfernen, wenn Sie ihn nicht mehr benötigen.

### <a name="download-the-source-code"></a>Herunterladen des Quellcodes

In den folgenden Schritten erstellen Sie in der WSL 2-Umgebung ein Docker-Image. Führen Sie die folgenden Befehle in einer WSL 2-Bash-Shell in einem geeigneten Ordner aus, um das Repository **iot-plug-and-play-bridge** zu klonen:

```bash
git clone https://github.com/Azure/iot-plug-and-play-bridge.git

cd iot-plug-and-play-bridge

git submodule update --init --recursive
```

Rechnen Sie damit, dass die Ausführung dieses Befehls einige Minuten dauern kann.

### <a name="update-the-dockerfile"></a>Aktualisieren der *Dockerfile*

Starten Sie VS Code, öffnen Sie die Befehlspalette, geben Sie *Remote WSL: Open folder in WSL* (Remote WSL: Ordner in WSL öffnen) ein, und öffnen Sie dann den Ordner *iot-plug-and-play-bridge*, in dem sich das geklonte Repository befindet.

Öffnen Sie die Datei *pnpbridge\Dockerfile.amd64*. Bearbeiten Sie die Definitionen der Umgebungsvariablen wie folgt:

```dockerfile
ENV PNP_BRIDGE_ROOT_MODEL_ID="dtmi:com:example:RootPnpBridgeSampleDevice;1"
ENV PNP_BRIDGE_HUB_TRACING_ENABLED="false"
ENV IOTEDGE_WORKLOADURI="something"
```

> [!TIP]
> Sie können den folgenden Befehl verwenden, um die Verbindungszeichenfolge für ein Gerät abzurufen: `az iot hub device-identity connection-string show --device-id bridge-edge-device --hub-name {your IoT hub name}`

Es gibt Beispiele für *Dockerfiles* für andere Architekturen.

### <a name="build-the-iot-plug-and-play-bridge-module-image"></a>Erstellen des IoT Plug & Play-Bridge-Modulimages

Öffnen Sie in VS Code die Befehlspalette, geben Sie *Docker Registries: Log In to Docker CLI* (Docker-Registrierungen: Bei der Docker-Befehlszeilenschnittstelle anmelden) ein, und wählen Sie Ihr Azure-Abonnement und Azure Container Registry aus. Mit diesem Befehl wird zwischen Docker und Ihrer Containerregistrierung eine Verbindung hergestellt. Zudem wird das Modulimage hochgeladen.

Öffnen Sie die Datei *pnpbridge/module.json*. Fügen Sie `{your container registry name}.azurecr.io/iotpnpbridge` als Repository des Containerimages und `v1` als Version hinzu.

Klicken Sie in VS Code mit der rechten Maustaste auf die Datei *pnpbridge/module.json* in der **Explorer**-Ansicht, wählen Sie **Build and Push IoT Edge Module Image** (IoT Edge-Modulimage erstellen und pushen) aus, und wählen Sie **amd64** als Plattform aus.

In VS Code können Sie in der **Docker**-Ansicht den Inhalt Ihrer Containerregistrierung durchsuchen, in der nun das Modulimage **iotpnpbridge:V1-amd64** enthalten ist.

### <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

Auf einem IoT Edge-Gerät wird das entsprechende Modulimage aus einer Containerregistrierung heruntergeladen. In diesem Beispiel wird eine Azure-Containerregistrierung verwendet.

Erstellen Sie zunächst in der Ressourcengruppe **bridge-edge-resources** eine Azure-Containerregistrierung. Aktivieren Sie anschließend den Administratorzugriff auf Ihre Containerregistrierung, und rufen Sie die Anmeldeinformationen ab, die erforderlich sind, damit die Modulimages durch das IoT Edge-Gerät heruntergeladen werden können:

```azurecli
az acr create -g bridge-edge-resources --sku Basic -n {your container registry name}
az acr update --admin-enabled true -n {your container registry name}
az acr credential show -n {your container registry name}
```

### <a name="create-the-deployment-manifest"></a>Erstellen des Bereitstellungsmanifests

In einem IoT Edge-Bereitstellungsmanifest sind die Module und Konfigurationswerte für die Bereitstellung auf einem IoT Edge-Gerät angegeben.

Öffnen Sie die Datei *pnpbridge/deployment.template.json* in VS Code:

- Aktualisieren Sie `registryCredentials` mit den Werten aus dem vorherigen Befehl. Der `address`-Wert lautet `{your container registry name}.azurecr.io`.
- Aktualisieren Sie den `image`-Wert für `ModulePnpBridge`. Das Modulimage sieht wie folgt aus: `{your container registry}.azurecr.io/iotpnpbridge:v1-amd64`.
- Ersetzen Sie `PnPBridgeConfig` durch den folgenden JSON-Code, um den CO2-Erkennungsadapter zu konfigurieren:

  ```json
  "PnpBridgeConfig": {
    "pnp_bridge_interface_components": [
      {
        "_comment": "Component 1 - Modbus Device",
        "pnp_bridge_component_name": "Co2Detector1",
        "pnp_bridge_adapter_id": "modbus-pnp-interface",
        "pnp_bridge_adapter_config": {
          "unit_id": 1,
          "tcp": {
            "host": "10.159.29.2",
            "port": 502
          },
          "modbus_identity": "DL679"
        }
      }
    ],
    "pnp_bridge_adapter_global_configs": {
      "modbus-pnp-interface": {
        "DL679": {
          "telemetry": {
            "co2": {
              "startAddress": "40001",
              "length": 1,
              "dataType": "integer",
              "defaultFrequency": 5000,
              "conversionCoefficient": 1
            },
            "temperature": {
              "startAddress": "40003",
              "length": 1,
              "dataType": "decimal",
              "defaultFrequency": 5000,
              "conversionCoefficient": 0.01
            }
          },
          "properties": {
            "firmwareVersion": {
              "startAddress": "40482",
              "length": 1,
              "dataType": "hexstring",
              "defaultFrequency": 60000,
              "conversionCoefficient": 1,
              "access": 1
            },
            "modelName": {
              "startAddress": "40483",
              "length": 2,
              "dataType": "string",
              "defaultFrequency": 60000,
              "conversionCoefficient": 1,
              "access": 1
            },
            "alarmStatus_co2": {
              "startAddress": "00305",
              "length": 1,
              "dataType": "boolean",
              "defaultFrequency": 1000,
              "conversionCoefficient": 1,
              "access": 1
            },
            "alarmThreshold_co2": {
              "startAddress": "40225",
              "length": 1,
              "dataType": "integer",
              "defaultFrequency": 30000,
              "conversionCoefficient": 1,
              "access": 2
            }
          },
          "commands": {
            "clearAlarm_co2": {
              "startAddress": "00305",
              "length": 1,
              "dataType": "flag",
              "conversionCoefficient": 1
            }
          }
        }
      }
    }
  }
  ```

  Speichern Sie die Änderungen.

Klicken Sie in VS Code mit der rechten Maustaste auf die Datei *pnpbridge/deployment.template.json* in der **Explorer**-Ansicht, und wählen Sie **Generate IoT Edge Deployment Manifest** (IoT Edge-Bereitstellungsmanifest generieren) aus. Mit diesem Befehl wird das Bereitstellungsmanifest *pnpbridge/config/deployment.amd64.json* erstellt.

### <a name="deploy-the-bridge-to-your-iot-edge-device"></a>Bereitstellen der Bridge auf Ihrem IoT Edge-Gerät

Öffnen Sie in VS Code die Befehlspalette, geben Sie *Azure IoT Hub: IoT Hub auswählen* ein, und wählen Sie anschließend Ihr Abonnement und IoT Hub aus.

Klicken Sie in VS Code mit der rechten Maustaste auf die Datei *pnpbridge/config/deployment.amd64.json* in der **Explorer**-Ansicht, wählen Sie **Create Deployment for Single Device** (Bereitstellung für einzelnes Gerät erstellen) und dann **bridge-edge-device** aus.

Führen Sie den folgenden Befehl aus, um auf Ihrem Gerät den Status der Module anzuzeigen:

```azurecli
az iot hub module-identity list --device-id bridge-edge-device -o table --hub-name {your IoT hub name}
```

In der Liste der ausgeführten Module ist nun das Modul **ModulePnpBridge** enthalten, das für die Verwendung des CO2-Erkennungsadapters konfiguriert wurde.

### <a name="tidy-up"></a>Aufräumen

Führen Sie den folgenden Befehl aus, um den virtuellen Computer und die Containerregistrierung aus dem Azure-Abonnement zu entfernen:

```azurecli
az group delete -n bridge-edge-resources
```

## <a name="folder-structure"></a>Ordnerstruktur

*pnpbridge\deps\azure-iot-sdk-c-pnp*: Git-Submodule, die das Azure IoT-Geräte-SDK für C SDK enthalten.

*pnpbridge\scripts*: Buildskripts.

*pnpbridge\src*: Quellcode für den IoT Plug & Play-Bridge-Kern.

*pnpbridge\src\adapters*: Quellcode für verschiedene IoT Plug & Play-Bridge-Adapter.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur IoT Plug & Play-Bridge finden Sie im GitHub-Repository zur [IoT Plug & Play-Bridge](https://github.com/Azure/iot-plug-and-play-bridge).