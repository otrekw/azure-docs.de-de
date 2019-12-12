---
title: 'Tutorial: Verbinden eines IoT Plug & Play-Geräts (Vorschauversion) mit Azure IoT Central'
description: In diesem Tutorial wird gezeigt, wie Sie ein Gerätefunktionsmodell verwenden, um einen Gerätecode zu generieren. Führen Sie anschließend den Gerätecode aus, und verfolgen Sie, wie das Gerät mit Ihrer IoT Central-Anwendung verbunden wird. Anschließend können Sie die automatisch generierten Ansichten verwenden.
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 2cb07a94b2ae85cc0755e1e7069a76e1ef2a5252
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74977334"
---
# <a name="tutorial-use-a-device-capability-model-to-create-an-iot-plug-and-play-device-and-connect-it-to-your-iot-central-application"></a>Tutorial: Verwenden eines Gerätefunktionsmodells zum Erstellen eines IoT Plug & Play-Geräts und Verbinden mit Ihrer IoT Central-Anwendung

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Mit einem _Gerätefunktionsmodell_ (Device Capability Model, DCM) werden die Funktionen eines [IoT Plug & Play](../../iot-pnp/overview-iot-plug-and-play.md)-Geräts beschrieben. IoT Central kann ein Gerätefunktionsmodell verwenden, um eine Gerätevorlage und Visualisierungen für ein Gerät zu erstellen, wenn das Gerät zum ersten Mal eine Verbindung herstellt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verwenden Sie Visual Studio Code, um mit einem Gerätefunktionsmodell ein IoT Plug & Play-Gerät zu erstellen.
> * Führen Sie den Gerätecode unter Windows aus, und verfolgen Sie, wie die Verbindung mit Ihrer IoT Central-Anwendung hergestellt wird.
> * Zeigen Sie die simulierten Telemetriedaten an, die vom Gerät gesendet werden.

## <a name="prerequisites"></a>Voraussetzungen

Arbeiten Sie die Schnellstartanleitung [Erstellen einer Azure IoT Central-Anwendung (Previewfunktionen)](./quick-deploy-iot-central.md) durch, um mit der Vorlage **Benutzerdefinierte App > Vorschauanwendung** eine IoT Central-Anwendung zu erstellen.

Für dieses Tutorials müssen Sie auf Ihrem lokalen Computer folgende Software installieren:

* [Buildtools für Visual Studio](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16) mit Workloads für **C++-Buildtools** und **NuGet-Paket-Manager-Komponenten**. Oder Sie haben bereits [Visual Studio (Community, Professional oder Enterprise)](https://visualstudio.microsoft.com/downloads/) 2019, 2017 oder 2015 mit denselben Workloads installiert.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/): Wählen Sie beim Installieren von **CMake** die Option **Add CMake to the system PATH** (CMake dem Systempfad hinzufügen).
* [Visual Studio Code](https://code.visualstudio.com/).
* [Node.js](https://nodejs.org/)
* Hilfsprogramm `dps-keygen`:

    ```cmd/sh
    npm i -g dps-keygen
    ```

### <a name="install-azure-iot-tools"></a>Installieren von Azure IoT-Tools

Führen Sie die folgenden Schritte aus, um das Erweiterungspaket „Azure IoT Tools“ in VS Code zu installieren:

1. Wählen Sie in VS Code die Registerkarte **Erweiterungen**.
1. Suchen Sie nach **Azure IoT Tools**.
1. Wählen Sie **Installieren** aus.

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

In diesem Tutorial verwenden Sie den [Vcpkg](https://github.com/microsoft/vcpkg)-Bibliotheks-Manager, um das Azure IoT-C-Geräte-SDK in Ihrer Entwicklungsumgebung zu installieren.

1. Öffnen Sie eine Eingabeaufforderung. Führen Sie den folgenden Befehl aus, um Vcpkg zu installieren:

    ```cmd
    git clone https://github.com/Microsoft/vcpkg.git
    cd vcpkg

    .\bootstrap-vcpkg.bat
    ```

    Führen Sie dann den folgenden Befehl aus, um die benutzerweite [Integration](https://github.com/microsoft/vcpkg/blob/master/docs/users/integration.md) zu verbinden. Wenn Sie diesen Befehl zum ersten Mal ausführen, sind Administratorrechte erforderlich:

    ```cmd
    .\vcpkg.exe integrate install
    ```

1. Installieren Sie Vcpkg für das Azure IoT-C-Geräte-SDK:

    ```cmd
    .\vcpkg.exe install azure-iot-sdk-c[public-preview,use_prov_client]
    ```

## <a name="generate-device-key"></a>Generieren des Geräteschlüssels

Zum Herstellen einer Verbindung mit einer IoT Central-Anwendung benötigen Sie einen Geräteschlüssel. Generieren Sie wie folgt einen Geräteschlüssel:

1. Melden Sie sich bei der IoT Central-Anwendung an, die Sie mithilfe der Vorlage **Benutzerdefinierte App > Vorschauanwendung** in der Schnellstartanleitung [Erstellen einer Azure IoT Central-Anwendung (Previewfunktionen)](./quick-deploy-iot-central.md) erstellt haben.

1. Navigieren Sie zur Seite **Verwaltung**, und wählen Sie **Geräteverbindung**.

1. Notieren Sie sich die Werte für **ID-Bereich** und **Primärschlüssel**, die beim Auswählen von **Schlüssel anzeigen** angezeigt werden. Diese Werte werden im weiteren Verlauf dieses Tutorials benötigt.

    ![Geräteverbindung](./media/tutorial-connect-pnp-device/device-connection.png)

1. Öffnen Sie eine Eingabeaufforderung, und führen Sie den folgenden Befehl aus, um einen Geräteschlüssel zu generieren:

    ```cmd/sh
    dps-keygen -di:mxchip-001 -mk:{Primary Key from previous step}
    ```

    Notieren Sie sich den generierten _Geräteschlüssel_. Sie verwenden diesen Wert in einem späteren Schritt dieses Tutorials.

## <a name="download-your-model"></a>Herunterladen des Modells

In diesem Tutorial verwenden Sie das öffentliche Gerätefunktionsmodell für ein Gerät vom Typ „MxChip IoT DevKit“. Sie benötigen kein echtes DevKit-Gerät, um den Code auszuführen. In diesem Tutorial kompilieren Sie den Code für die Ausführung unter Windows.

1. Erstellen Sie einen Ordner mit dem Namen `central_app`, und öffnen Sie ihn in VS Code.

1. Verwenden Sie **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen, geben Sie **IoT Plug & Play** ein, und wählen Sie **Open Model Repository** (Modellrepository öffnen). Wählen Sie **Public repository** (Öffentliches Repository). VS Code zeigt eine Liste mit den Gerätefunktionsmodellen im öffentlichen Modellrepository an.

1. Wählen Sie das Gerätefunktionsmodell **MXChip IoT DevKit** mit der ID `urn:mxchip:mxchip_iot_devkit:1` aus. Wählen Sie anschließend **Herunterladen**. Sie verfügen nun über eine Kopie des Gerätefunktionsmodells im Ordner `central_app`.

![Modellrepository und Gerätefunktionsmodell](./media/tutorial-connect-pnp-device/public-repository.png)

> [!NOTE]
> Für die Zusammenarbeit mit IoT Central muss das Gerätefunktionsmodell über alle definierten Schnittstellen inline in derselben Datei verfügen.

## <a name="generate-the-c-code-stub"></a>Generieren des C-Code-Stubs

Da Sie jetzt über das Gerätefunktionsmodell **MXChip IoT DevKit** und die zugeordneten Schnittstellen verfügen, können Sie den Gerätecode generieren, mit dem das Modell implementiert wird. Generieren Sie wie folgt den C-Code-Stub in VS Code:

1. Drücken Sie bei geöffnetem Ordner mit den Gerätefunktionsmodell-Dateien die Tastenkombination **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen. Geben Sie **IoT Plug & Play** ein, und wählen Sie **Generate Device Code Stub** (Gerätecode-Stub generieren).

    > [!NOTE]
    > Wenn Sie das Hilfsprogramm „IoT Plug & Play-Codegenerator“ zum ersten Mal verwenden, dauert der Download einige Sekunden.

1. Wählen Sie die Gerätefunktionsmodell-Datei **MXChip IoT DevKit** aus, die Sie gerade heruntergeladen haben.

1. Geben Sie den Projektnamen **devkit_device** ein.

1. Wählen Sie **ANSI C** als Sprache aus.

1. Wählen Sie als Verbindungsmethode die Option **Via DPS (Device Provisioning Service) symmetric key** (Mit symmetrischem Schlüssel per Device Provisioning-Dienst).

1. Wählen Sie als Projekttyp **CMake Project on Windows** (CMake-Projekt unter Windows) aus. Wählen Sie nicht **MXChip IoT DevKit-Projekt**. Diese Option wird verwendet, wenn Sie ein echtes DevKit-Gerät verwenden.

1. Wählen Sie **Via Vcpkg** (Über Vcpkg) als Methode zum Einschließen des SDK aus.

1. VS Code öffnet ein neues Fenster mit generierten Gerätecode-Stubdateien im Ordner `devkit_device`.

![Generierter Gerätecode](./media/tutorial-connect-pnp-device/generated-code.png)

## <a name="build-the-code"></a>Erstellen des Codes

Sie verwenden das Geräte-SDK, um den generierten Gerätecode-Stub zu erstellen. Mit der von Ihnen erstellten Anwendung wird ein Gerät vom Typ **MXChip IoT DevKit** simuliert und eine Verbindung mit Ihrer IoT Central-Anwendung hergestellt. Die Anwendung sendet Telemetriedaten und Eigenschaften und empfängt Befehle.

1. Erstellen Sie an einer Eingabeaufforderung im Ordner `devkit_device` den Unterordner `cmake`, und navigieren Sie zu diesem Ordner:

    ```cmd
    mkdir cmake
    cd cmake
    ```

1. Führen Sie die folgenden Befehle aus, um den generierten Codestub zu erstellen. Ersetzen Sie den Platzhalter `<directory of your Vcpkg repo>` durch den Pfad zu Ihrer Kopie des **Vcpkg**-Repositorys:

    ```cmd
    cmake .. -G "Visual Studio 16 2019" -A Win32 -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"

    cmake --build . -- /p:Configuration=Release
    ```

    Wenn Sie Visual Studio 2017 oder 2015 verwenden, müssen Sie den CMake-Generator entsprechend den verwendeten Buildtools angeben:

    ```cmd
    # Either
    cmake .. -G "Visual Studio 15 2017" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"
    # or
    cmake .. -G "Visual Studio 14 2015" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"
    ```

1. Nachdem der Buildvorgang erfolgreich abgeschlossen wurde, können Sie Ihre Anwendung über die Eingabeaufforderung ausführen. Ersetzen Sie `<scopeid>` und `<devicekey>` durch die zuvor notierten Werte:

    ```cmd
    .\Release\devkit_device.exe mxchip-001 <scopeid> <devicekey>
    ```

1. Die Geräteanwendung beginnt mit dem Senden von Daten an IoT Hub. Manchmal wird der Fehler `Error registering device for DPS` angezeigt, wenn Sie den vorherigen Befehl zum ersten Mal ausführen. Wenn dieser Fehler angezeigt wird, wiederholen Sie den Befehl.

## <a name="view-the-device"></a>Anzeigen des Geräts

Nachdem Ihr Gerätecode eine Verbindung mit Ihrer IoT Central-Instanz hergestellt hat, können Sie die gesendeten Eigenschaften und Telemetriedaten anzeigen:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Geräte**, und wählen Sie das Gerät **mxchip-01** aus. Dieses Gerät wurde automatisch hinzugefügt, als über den Gerätecode die Verbindung hergestellt wurde:

    ![Seite „Übersicht“](./media/tutorial-connect-pnp-device/overview-page.png)

    Nach einigen Minuten werden auf dieser Seite Diagramme mit den Telemetriedaten angezeigt, die vom Gerät gesendet werden.

1. Wählen Sie die Seite **Info**, um die vom Gerät gesendeten Eigenschaftswerte anzuzeigen.

1. Wählen Sie die Seite **Befehle**, um Befehle auf dem Gerät aufzurufen. Sie können über die Eingabeaufforderung verfolgen, wie das Gerät antwortet, auf dem der Gerätecode ausgeführt wird.

1. Navigieren Sie zur Seite **Gerätevorlagen**, um die Vorlage anzuzeigen, die von IoT Central über das Gerätefunktionsmodul im öffentlichen Repository erstellt wurde:

    ![Seite „Gerätevorlagen“](./media/tutorial-connect-pnp-device/device-template.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie eine Verbindung mit einem IoT Plug & Play-Gerät herstellen, das auf der Grundlage eines Gerätefunktionsmodells im öffentlichen Modellrepository generiert wurde.

Weitere Informationen zu Gerätefunktionsmodellen und zur Erstellung Ihrer eigenen Modelle finden Sie in der Schrittanleitung:

> [!div class="nextstepaction"]
> [Erstellen einer Gerätegruppe](./tutorial-use-device-groups.md)
