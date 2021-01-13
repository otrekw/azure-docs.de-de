---
title: Bereitstellung mit symmetrischen Schlüsseln – Azure IoT Edge | Microsoft-Dokumentation
description: Stellen Sie nach der Installation mithilfe der Verbindungszeichenfolge ein IoT Edge-Gerät mit symmetrischen Schlüsseln bereit, und führen Sie die Authentifizierung in IoT Hub durch.
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 10/06/2020
ms.author: kgremban
ms.openlocfilehash: f5371539c1b45c14b519729c7c07003bf74847a0
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92043866"
---
# <a name="set-up-an-azure-iot-edge-device-with-symmetric-key-authentication"></a>Einrichten eines Azure IoT Edge-Geräts mit Authentifizierung mit symmetrischem Schlüssel

Dieser Artikel enthält die Schritte zum Registrieren eines neuen IoT Edge-Geräts in IoT Hub und zum Konfigurieren des Geräts für die Authentifizierung mit symmetrischen Schlüsseln.

Die Schritte in diesem Artikel führen Sie durch die sogenannte manuelle Bereitstellung, bei der jedes Gerät manuell mit dem entsprechenden IoT-Hub verbunden wird. Die Alternative ist die automatische Bereitstellung mithilfe von IoT Hub Device Provisioning Service. Diese ist hilfreich, wenn Sie viele Geräte bereitstellen müssen.

<!--TODO: Add auto-provision info/links-->

Bei der manuellen Bereitstellung haben Sie zwei Optionen zum Authentifizieren von IoT Edge-Geräten:

* **Symmetrische Schlüssel**: Wenn Sie in IoT Hub eine neue Geräteidentität erstellen, erstellt der Dienst zwei Schlüssel. Ein Schlüssel befindet sich auf dem Gerät, den es bei der Authentifizierung in IoT Hub vorzeigt.

  Diese Authentifizierungsmethode kann schneller eingerichtet werden, ist dafür aber weniger sicher.

* **X.509, selbstsigniert**: Sie erstellen zwei X. 509-Identitätszertifikate und bewahren diese auf dem Gerät auf. Wenn Sie in IoT Hub eine neue Geräteidentität erstellen, geben Sie Fingerabdrücke aus beiden Zertifikaten an. Wenn sich das Gerät bei IoT Hub authentifiziert, werden die Zertifikate vorgezeigt, und IoT Hub kann überprüfen, ob sie den Fingerabdrücken entsprechen.

  Diese Authentifizierungsmethode ist sicherer und wird in Produktionsszenarios empfohlen.

In diesem Artikel wird der Registrierungs- und Bereitstellungsprozess mit der Authentifizierung mit symmetrischen Schlüsseln erläutert. Wenn Sie erfahren möchten, wie Sie ein Gerät mit X.509-Zertifikaten einrichten, können Sie sich in folgendem Artikel informieren: [Einrichten eines Azure IoT Edge-Geräts mit X.509-Zertifikatauthentifizierung](how-to-manual-provision-x509.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die Schritte in diesem Artikel durchführen, sollten Sie über ein Gerät verfügen, auf dem die IoT Edge-Runtime installiert ist. Wenn Sie kein Gerät mit der IoT Edge-Runtime besitzen, führen Sie die in [Installieren oder Deinstallieren der Azure IoT Edge-Runtime](how-to-install-iot-edge.md) beschriebenen Schritte durch.

## <a name="register-a-new-device"></a>Registrieren eines neuen Geräts

Jedes Gerät, das eine Verbindung mit einem IoT-Hub herstellt, verfügt über eine Geräte-ID, die zum Nachverfolgen der Cloud-zu-Gerät- oder Gerät-zu-Cloud-Kommunikation verwendet wird. Sie konfigurieren ein Gerät mit den Verbindungsinformationen, einschließlich des IoT Hub-Hostnamens, der Geräte-ID und der Informationen, die das Gerät zum Authentifizieren bei IoT Hub verwendet.

Bei der Authentifizierung mit einem symmetrischen Schlüssel werden diese Informationen in einer *Verbindungszeichenfolge* erfasst, die Sie aus IoT Hub abrufen und dann auf Ihrem IoT Edge-Gerät aufbewahren können.

Sie können verschiedene Tools verwenden, um ein neues IoT Edge-Gerät in IoT Hub zu registrieren und die Verbindungszeichenfolge abzurufen.

# <a name="portal"></a>[Portal](#tab/azure-portal)

### <a name="prerequisites-for-the-azure-portal"></a>Voraussetzungen für das Azure-Portal

Ein [IoT-Hub](../iot-hub/iot-hub-create-through-portal.md) in Ihrem Azure-Abonnement im Tarif „Free“ oder „Standard“.

### <a name="create-an-iot-edge-device-in-the-azure-portal"></a>Erstellen eines IoT Edge-Geräts im Azure-Portal

In Ihrem IoT Hub im Azure-Portal werden IoT Edge-Geräte separat von IoT-Geräten erstellt und verwaltet, die nicht mit Edge verwendet werden können.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem IoT Hub.

1. Klicken Sie im linken Bereich auf **IoT Edge**, und klicken Sie dann auf **IoT Edge Geräte hinzufügen**.

   ![Hinzufügen eines IoT Edge-Geräts im Azure-Portal](./media/how-to-manual-provision-symmetric-key/portal-add-iot-edge-device.png)

1. Geben Sie auf der Seite **Gerät erstellen** die folgenden Informationen ein:

   * Erstellen Sie eine eindeutige Geräte-ID.
   * Wählen Sie als Authentifizierungstyp **Symmetrischer Schlüssel** aus.
   * Verwenden Sie die Standardeinstellungen, um Authentifizierungsschlüssel automatisch zu generieren und das neue Gerät mit Ihrem Hub zu verbinden.

1. Wählen Sie **Speichern**.

### <a name="view-iot-edge-devices-in-the-azure-portal"></a>Anzeigen von IoT Edge-Geräten im Azure-Portal

Alle auf die Verwendung mit Edge ausgelegten Geräte, die eine Verbindung mit Ihrem IoT-Hub herstellen, sind auf der Seite **IoT Edge** aufgeführt.

![Anzeigen aller IoT Edge-Geräte in Ihrem IoT-Hub mit dem Azure-Portal](./media/how-to-manual-provision-symmetric-key/portal-view-devices.png)

### <a name="retrieve-the-connection-string-in-the-azure-portal"></a>Abrufen der Verbindungszeichenfolge im Azure-Portal

Wenn Sie bereit für die Einrichtung Ihres Geräts sind, benötigen Sie die Verbindungszeichenfolge, die Ihr physisches Gerät mit seiner Identität in IoT Hub verknüpft.

1. Klicken Sie im Portal auf der Seite **IoT Edge** in der Liste der IoT Edge-Geräte auf die Geräte-ID.
2. Kopieren Sie entweder den Wert der **primären Verbindungszeichenfolge** oder der **sekundären Verbindungszeichenfolge**.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

### <a name="prerequisites-for-visual-studio-code"></a>Voraussetzungen für Visual Studio Code

* Ein [IoT-Hub](../iot-hub/iot-hub-create-through-portal.md) in Ihrem Azure-Abonnement im Tarif „Free“ oder „Standard“
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) für Visual Studio Code

### <a name="sign-in-to-access-your-iot-hub"></a>Anmeldung zum Zugreifen auf Ihren IoT Hub

Sie können die Azure IoT-Erweiterungen für Visual Studio Code verwenden, um Vorgänge mit Ihrem IoT Hub auszuführen. Damit diese Vorgänge funktionieren, müssen Sie sich bei Ihrem Azure-Konto anmelden und Ihren IoT Hub auswählen.

1. Öffnen Sie in Visual Studio Code die **Explorer**-Ansicht.
1. Erweitern Sie im unteren Bereich des Explorers den Abschnitt **Azure IoT Hub**.

   ![Erweitern des Abschnitts „Azure IoT Hub Devices“ (Azure IoT Hub-Geräte)](./media/how-to-manual-provision-symmetric-key/azure-iot-hub-devices.png)

1. Klicken Sie auf **...** in der Kopfzeile des Abschnitts **Azure IoT Hub**. Wenn die Ellipse nicht angezeigt wird, zeigen Sie auf den Header, oder klicken Sie darauf.
1. Wählen Sie **Select IoT Hub** (IoT Hub auswählen) aus.
1. Wenn Sie nicht bei Ihrem Azure-Konto angemeldet sind, befolgen Sie die angezeigten Aufforderungen.
1. Wählen Sie Ihr Azure-Abonnement.
1. Wählen Sie Ihren IoT Hub aus.

### <a name="create-an-iot-edge-device-with-visual-studio-code"></a>Erstellen eines IoT Edge-Geräts mit Visual Studio Code

1. Erweitern Sie im Visual Studio Code-Explorer den Abschnitt **Azure IoT Hub Devices** (Azure IoT Hub-Geräte).
1. Klicken Sie auf **...** in der Überschrift des Abschnitts **Azure IoT Hub Devices** (Azure IoT Hub-Geräte). Wenn die Ellipse nicht angezeigt wird, zeigen Sie auf den Header, oder klicken Sie darauf.
1. Wählen Sie **Create IoT Edge Device** (IoT Edge-Gerät erstellen) aus.
1. Geben Sie im geöffneten Textfeld eine ID für Ihr Gerät an.

Auf dem Ausgabebildschirm wird das Ergebnis des Befehls angezeigt. Die Geräteinformationen werden ausgegeben, einschließlich der **deviceID**, die Sie angegeben haben, und der Zeichenfolge **connectionString**, mit der Sie Ihr physisches Gerät mit Ihrem IoT Hub verbinden können.

Auf dem Ausgabebildschirm wird das Ergebnis des Befehls angezeigt. Die Geräteinformationen werden ausgegeben, einschließlich der **deviceID**, die Sie angegeben haben, und der Zeichenfolge **connectionString**, mit der Sie Ihr physisches Gerät mit Ihrem IoT Hub verbinden können.

### <a name="view-iot-edge-devices-with-visual-studio-code"></a>Anzeigen von IoT Edge-Geräten mit Visual Studio Code

Alle Geräte, die Verbindungen mit Ihrem IoT Hub herstellen, werden im Abschnitt **Azure IoT Hub** des Visual Studio Code-Explorers aufgeführt. IoT Edge-Geräte können von Nicht-Edge-Geräten unterschieden werden, da sie ein anderes Symbol aufweisen, und aufgrund der Tatsache, dass die Module **$edgeAgent** und **$edgeHub** auf jedem IoT Edge-Gerät bereitgestellt sind.

![Anzeigen aller IoT Edge-Geräte in Ihrem IoT-Hub mit VS Code](./media/how-to-manual-provision-symmetric-key/view-devices.png)

### <a name="retrieve-the-connection-string-with-visual-studio-code"></a>Abrufen der Verbindungszeichenfolge mit Visual Studio Code

Wenn Sie bereit für die Einrichtung Ihres Geräts sind, benötigen Sie die Verbindungszeichenfolge, die Ihr physisches Gerät mit seiner Identität in IoT Hub verknüpft.

1. Klicken Sie mit der rechten Maustaste auf die ID des Geräts im Abschnitt **Azure IoT Hub**.
1. Wählen Sie **Copy Device Connection String** (Verbindungszeichenfolge des Geräts kopieren) aus.

   Die Verbindungszeichenfolge wird in die Zwischenablage kopiert.

Sie können auch im Kontextmenü **Get Device Info** (Geräteinformationen abrufen) auswählen, um alle Geräteinformationen einschließlich der Verbindungszeichenfolge im Ausgabefenster anzuzeigen.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

### <a name="prerequisites-for-the-azure-cli"></a>Voraussetzungen für die Azure-Befehlszeilenschnittstelle

* Ein [IoT Hub](../iot-hub/iot-hub-create-using-cli.md) in Ihrem Azure-Abonnement.
* Die [Azure CLI](/cli/azure/install-azure-cli) ist in Ihrer Umgebung vorhanden. Ihre Azure CLI-Version muss mindestens 2.0.70 oder höher lauten. Verwenden Sie `az --version`, um dies zu überprüfen. Diese Version unterstützt az-Erweiterungsbefehle, und das Framework für Knack-Befehle wird eingeführt.
* Die [IoT-Erweiterung für die Azure CLI](https://github.com/Azure/azure-iot-cli-extension) ist vorhanden.

### <a name="create-an-iot-edge-device-with-the-azure-cli"></a>Erstellen eines IoT Edge-Geräts mit der Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl [az iot hub device-identity create](/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-create), um eine neue Geräteidentität in Ihrem IoT-Hub zu erstellen. Beispiel:

   ```azurecli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Dieser Befehl weist drei Parameter auf:

* `--device-id` oder `-d`: Geben Sie einen beschreibenden Namen an, der für Ihren IoT Hub eindeutig ist.
* `hub-name` oder `-n`: Geben Sie den Namen Ihres IoT Hub an.
* `--edge-enabled` oder `--ee`: Deklarieren Sie, dass das Gerät ein IoT Edge-Gerät ist.

   ![az iot hub device-identity create output](./media/how-to-manual-provision-symmetric-key/create-edge-device-cli.png)

### <a name="view-iot-edge-devices-with-the-azure-cli"></a>Anzeigen von IoT Edge-Geräten mit der Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl [az iot hub device-identity list](/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-list), um alle Geräte in Ihrem IoT-Hub anzuzeigen. Beispiel:

   ```azurecli
   az iot hub device-identity list --hub-name [hub name]
   ```

Alle als IoT Edge-Gerät registrierten Geräte weisen die **capabilities.iotEdge**-Eigenschaft mit dem Wert **true** auf.

### <a name="retrieve-the-connection-string-with-the-azure-cli"></a>Abrufen der Verbindungszeichenfolge mit der Azure-Befehlszeilenschnittstelle

Wenn Sie bereit für die Einrichtung Ihres Geräts sind, benötigen Sie die Verbindungszeichenfolge, die Ihr physisches Gerät mit seiner Identität in IoT Hub verknüpft. Verwenden Sie den Befehl [az iot hub device-identity show-connection-string](/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-show-connection-string), um die Verbindungszeichenfolge für ein einzelnes Gerät zurückzugeben:

   ```azurecli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name]
   ```

Beim Wert für den Parameter `device-id` muss die Groß-/Kleinschreibung beachtet werden. Kopieren Sie nicht die Anführungszeichen, in die die Verbindungszeichenfolge eingeschlossen ist.

---

## <a name="provision-an-iot-edge-device"></a>Bereitstellen eines IoT Edge-Geräts

Wenn das IoT Edge-Gerät über eine Identität in IoT Hub und eine Verbindungszeichenfolge verfügt, die für die Authentifizierung verwendet werden kann, müssen Sie das Gerät selbst mit diesen Informationen bereitstellen.

Auf einem Linux-Gerät geben Sie die Verbindungszeichenfolge an, indem Sie eine config.yaml-Datei bearbeiten. Auf einem Windows-Gerät geben Sie die Verbindungszeichenfolge an, indem Sie ein PowerShell-Skript ausführen.

# <a name="linux"></a>[Linux](#tab/linux)

Öffnen Sie die Konfigurationsdatei auf dem IoT Edge-Gerät.

```bash
sudo nano /etc/iotedge/config.yaml
```

Suchen Sie nach den Bereitstellungskonfigurationen in der Datei, und heben Sie die Auskommentierung des Abschnitts **Manual provisioning configuration using a connection string** auf. 

Aktualisieren Sie den Wert von **device_connection_string** durch die Verbindungszeichenfolge Ihres IoT Edge-Geräts. Stellen Sie sicher, dass alle anderen Bereitstellungsabschnitte auskommentiert sind. Stellen Sie sicher, dass der Zeile **provisioning:** kein Leerzeichen vorangestellt ist und dass geschachtelte Elemente jeweils um zwei Leerzeichen eingerückt sind.

```yml
# Manual provisioning configuration using a connection string
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  dynamic_reprovisioning: false
```

Verwenden Sie zum Einfügen des Inhalts der Zwischenablage in Nano `Shift+Right Click`, oder drücken Sie `Shift+Insert`.

Speichern und schließen Sie die Datei.

   `CTRL + X`, `Y`, `Enter`

Starten Sie nach Eingabe der Bereitstellungsinformationen in der Konfigurationsdatei den Daemon neu:

```bash
sudo systemctl restart iotedge
```

# <a name="windows"></a>[Windows](#tab/windows)

1. Führen Sie PowerShell auf dem IoT Edge-Gerät als Administrator aus.

2. Verwenden Sie den Befehl [Initialize-IoTEdge](reference-windows-scripts.md#initialize-iotedge), um die IoT Edge-Runtime auf Ihrem Computer zu konfigurieren. Standardmäßig wird für den Befehl die manuelle Bereitstellung mit Windows-Containern verwendet.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ManualConnectionString -ContainerOs Windows
   ```

   * Wenn Sie Linux-Container verwenden, fügen Sie dem Flag den `-ContainerOs`-Parameter hinzu. Wählen Sie dieselbe Containeroption wie beim Befehl `Deploy-IoTEdge`, den Sie zuvor ausgeführt haben.

      ```powershell
      . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
      Initialize-IoTEdge -ContainerOs Linux
      ```

   * Wenn Sie das Skript „IoTEdgeSecurityDaemon.ps1“ auf Ihr Gerät heruntergeladen haben, um eine Offlineinstallation durchzuführen oder eine bestimmte Version zu installieren, müssen Sie auf die lokale Kopie des Skripts verweisen.

      ```powershell
      . <path>/IoTEdgeSecurityDaemon.ps1
      Initialize-IoTEdge -ManualX509
      ```

3. Geben Sie bei Aufforderung die Geräteverbindungszeichenfolge ein, die Sie im vorherigen Abschnitt abgerufen haben. Die Verbindungszeichenfolge des Geräts ordnet das physische Gerät in IoT Hub einer Geräte-ID zu und bietet Authentifizierungsinformationen.

   Die Verbindungszeichenfolge hat das folgende Format und darf keine Anführungszeichen enthalten: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

Wenn Sie ein Gerät manuell bereitstellen, können Sie zusätzliche Parameter verwenden, um den Prozess folgendermaßen anzupassen:

* Sie können direkten Datenverkehr über einen Proxyserver leiten.
* Sie können ein bestimmtes edgeAgent-Containerimage deklarieren und Anmeldeinformationen bereitstellen, wenn dieses sich in einer privaten Registrierung befindet.

Weitere Informationen zu diesen zusätzlichen Parametern finden Sie unter [PowerShell-Skripts für IoT Edge unter Windows](reference-windows-scripts.md).

---

[!INCLUDE [Verify and troubleshoot installation](../../includes/iot-edge-verify-troubleshoot-install.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können nun mit dem Artikel zum [Bereitstellen von IoT Edge-Modulen](how-to-deploy-modules-portal.md) fortfahren, um zu erfahren, wie Sie Module auf Ihrem Gerät bereitstellen.