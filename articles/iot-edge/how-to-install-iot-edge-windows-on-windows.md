---
title: Installieren von Azure IoT Edge für Windows | Microsoft-Dokumentation
description: Installieren von Azure IoT Edge für Windows-Container auf Windows-Geräten
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: kgremban
monikerRange: iotedge-2018-06
ms.openlocfilehash: bb87d09b67658f9a3d7c68f635bfcd9a29de675c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103201626"
---
# <a name="install-and-manage-azure-iot-edge-with-windows-containers"></a>Installieren und Verwalten von Azure IoT Edge mit Windows-Containern

[!INCLUDE [iot-edge-version-201806](../../includes/iot-edge-version-201806.md)]

Die Azure IoT Edge-Runtime verwandelt ein Gerät in ein IoT Edge-Gerät. Wenn ein Gerät mit der IoT Edge-Runtime konfiguriert wurde, können Sie darauf Geschäftslogik aus der Cloud bereitstellen. Weitere Informationen finden Sie unter [Grundlegendes zur Azure IoT Edge-Runtime und ihrer Architektur](iot-edge-runtime.md).

Das Setup eines IoT Edge-Geräts erfolgt in zwei Schritten. Der erste Schritt ist das Installieren der Runtime und deren Abhängigkeiten. Der zweite Schritt besteht darin, das Gerät mit seiner Identität in der Cloud zu verbinden und die Authentifizierung bei IoT Hub einzurichten.

In diesem Artikel sind die Schritte zum Installieren der Azure IoT Edge-Runtime mit Windows-Containern aufgeführt. Wenn Sie Linux-Container auf einem Windows-Gerät verwenden möchten, finden Sie weitere Informationen im Artikel [Azure IoT Edge für Linux unter Windows](how-to-install-iot-edge-on-windows.md).

>[!NOTE]
>Azure IoT Edge mit Windows-Containern wird ab Version 1.2 von Azure IoT Edge nicht mehr unterstützt.
>
>Ziehen Sie die neue Methode [Azure IoT Edge für Linux unter Windows](iot-edge-for-linux-on-windows.md) zum Ausführen von IoT Edge auf Windows-Geräten in Betracht.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Windows-Gerät

  IoT Edge erfordert in Kombination mit Windows-Containern die Windows-Version 1809/Build 17763. Dies ist der neueste [Windows Long Term Support-Build](/windows/release-information/). Sehen Sie unbedingt die [Liste der unterstützten Systeme](support.md#operating-systems) mit der Liste der unterstützten SKUs durch.

* Eine [registrierte Geräte-ID](how-to-register-device.md)

  Wenn Sie Ihr Gerät mithilfe einer Authentifizierung mit symmetrischen Schlüsseln registriert haben, halten Sie die Verbindungszeichenfolge für das Gerät bereit.

  Wenn Sie Ihr Gerät mithilfe einer Authentifizierung mit selbst signiertem X.509-Zertifikat registriert haben, muss mindestens eines der Zertifikate, das Sie zum Registrieren des Geräts verwendet haben, und dessen übereinstimmender privater Schlüssel auf Ihrem Gerät zur Verfügung stehen.

## <a name="install-a-container-engine"></a>Installieren einer Containerengine

Azure IoT Edge basiert auf einer OCI-kompatiblen Containerruntime wie [Moby](https://github.com/moby/moby). Eine Moby-basierte Engine ist im Installationsskript enthalten. Es sind keine zusätzlichen Schritte erforderlich, um die-Engine zu installieren.

## <a name="install-the-iot-edge-security-daemon"></a>Installieren des Daemons für IoT Edge-Sicherheit

1. Führen Sie PowerShell als Administrator aus.

   Verwenden Sie eine AMD64-Sitzung von PowerShell, nicht PowerShell(x86). Wenn Sie nicht sicher sind, welchen Sitzungstyp Sie verwenden, führen Sie den folgenden Befehl aus:

   ```powershell
   (Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   ```

2. Führen Sie den Befehl [Deploy-IoTEdge](reference-windows-scripts.md#deploy-iotedge) aus, der die folgenden Aufgaben ausführt:

   * Überprüft, ob Ihr Windows-Computer mit einer unterstützten Version läuft.
   * Aktiviert das Containerfeature.
   * Lädt die Moby-Engine und die IoT Edge-Runtime herunter.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

3. Starten Sie das Gerät neu, wenn Sie dazu aufgefordert werden.

Wenn Sie IoT Edge auf einem Gerät installieren, können Sie zusätzliche Parameter verwenden, um den Prozess folgendermaßen anzupassen:

* Sie können direkten Datenverkehr über einen Proxyserver leiten.
* Legen Sie im Installationsprogramm ein lokales Verzeichnis für die Offlineinstallation fest.

Weitere Informationen zu diesen zusätzlichen Parametern finden Sie unter [PowerShell-Skripts für IoT Edge mit Windows-Containern](reference-windows-scripts.md).

## <a name="provision-the-device-with-its-cloud-identity"></a>Bereitstellen des Geräts mit seiner Cloud-Identität

Nachdem die Containerengine und die IoT Edge-Runtime auf Ihrem Gerät installiert wurden, sind Sie nun für den nächsten Schritt bereit, nämlich das Einrichten des Geräts mit seinen Cloudidentitäts- und Authentifizierungsinformationen.

Wählen Sie den nächsten Abschnitt auf der Grundlage des gewünschten Authentifizierungstyps aus:

* [Option 1: Authentifizieren mit symmetrischen Schlüsseln](#option-1-authenticate-with-symmetric-keys)
* [Option 2: Authentifizieren mit X.509-Zertifikaten](#option-2-authenticate-with-x509-certificates)

### <a name="option-1-authenticate-with-symmetric-keys"></a>Option 1: Authentifizieren mit symmetrischen Schlüsseln

Zu diesem Zeitpunkt wird die IoT Edge-Runtime auf Ihrem Windows-Gerät installiert, und Sie müssen das Gerät mit seiner Cloud-Identität und seinen Authentifizierungsinformationen bereitstellen.

In diesem Abschnitt werden die Schritte zum Bereitstellen eines Geräts mit einer Authentifizierung mit symmetrischem Schlüssel erläutert. Sie sollten Ihr Gerät in IoT Hub registriert und die Verbindungszeichenfolge aus den Geräteinformationen abgerufen haben. Führen Sie andernfalls die Schritte in [Registrieren eines IoT Edge Geräts in IoT Hub](how-to-register-device.md) aus.

1. Führen Sie PowerShell auf dem IoT Edge-Gerät als Administrator aus.

2. Verwenden Sie den Befehl [Initialize-IoTEdge](reference-windows-scripts.md#initialize-iotedge), um die IoT Edge-Runtime auf Ihrem Computer zu konfigurieren. Standardmäßig wird für den Befehl die manuelle Bereitstellung mit Windows-Containern verwendet.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ManualConnectionString -ContainerOs Windows
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

Weitere Informationen zu diesen zusätzlichen Parametern finden Sie unter [PowerShell-Skripts für IoT Edge mit Windows-Containern](reference-windows-scripts.md).

### <a name="option-2-authenticate-with-x509-certificates"></a>Option 2: Authentifizieren mit X.509-Zertifikaten

Zu diesem Zeitpunkt wird die IoT Edge-Runtime auf Ihrem Windows-Gerät installiert, und Sie müssen das Gerät mit seiner Cloud-Identität und seinen Authentifizierungsinformationen bereitstellen.

In diesem Abschnitt werden die Schritte zum Bereitstellen eines Geräts mit einer Authentifizierung mit X.509-Zertifikat erläutert. Sie sollten Ihr Gerät in IoT Hub registriert und dabei Fingerabdrücke bereitgestellt haben, die mit dem Zertifikat und dem privaten Schlüssel auf Ihrem IoT Edge-Gerät übereinstimmen. Führen Sie andernfalls die Schritte in [Registrieren eines IoT Edge Geräts in IoT Hub](how-to-register-device.md) aus.

1. Führen Sie PowerShell auf dem IoT Edge-Gerät als Administrator aus.

2. Verwenden Sie den Befehl [Initialize-IoTEdge](reference-windows-scripts.md#initialize-iotedge), um die IoT Edge-Runtime auf Ihrem Computer zu konfigurieren.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ManualX509
   ```

   * Wenn Sie das Skript „IoTEdgeSecurityDaemon.ps1“ auf Ihr Gerät heruntergeladen haben, um eine Offlineinstallation durchzuführen oder eine bestimmte Version zu installieren, müssen Sie auf die lokale Kopie des Skripts verweisen.

      ```powershell
      . <path>/IoTEdgeSecurityDaemon.ps1
      Initialize-IoTEdge -ManualX509
      ```

3. Geben Sie bei entsprechender Aufforderung die folgenden Informationen ein:

   * **IotHubHostName**: Dies ist der Hostname des IoT-Hubs, mit dem das Gerät eine Verbindung herstellt. Beispiel: `{IoT hub name}.azure-devices.net`.
   * **DeviceId**: Dies ist die ID, die Sie bei der Registrierung des Geräts angegeben haben.
   * **X509IdentityCertificate**: Dies ist der absolute Pfad zu einem Identitätszertifikat auf dem Gerät. Beispiel: `C:\path\identity_certificate.pem`.
   * **X509IdentityPrivateKey**: Dies ist der absolute Pfad zu der Datei mit dem privaten Schlüssel für das bereitgestellte Identitätszertifikat. Beispiel: `C:\path\identity_key.pem`.

Wenn Sie ein Gerät manuell bereitstellen, können Sie zusätzliche Parameter verwenden, um den Prozess folgendermaßen anzupassen:

* Sie können direkten Datenverkehr über einen Proxyserver leiten.
* Sie können ein bestimmtes edgeAgent-Containerimage deklarieren und Anmeldeinformationen bereitstellen, wenn dieses sich in einer privaten Registrierung befindet.

Weitere Informationen zu diesen zusätzlichen Parametern finden Sie unter [PowerShell-Skripts für IoT Edge mit Windows-Containern](reference-windows-scripts.md).

## <a name="offline-or-specific-version-installation-optional"></a>Offlineinstallation oder Installation einer bestimmten Version (optional)

Die in diesem Abschnitt aufgeführten Schritte sind für Szenarien vorgesehen, die von den Schritten der Standardinstallation nicht abgedeckt werden. Das können beispielsweise sein:

* Die Offlineinstallation von IoT Edge
* Die Installation einer Release Candidate-Version
* Installation einer anderen als der aktuellsten Version

Während der Installation werden drei Dateien heruntergeladen:

* Ein PowerShell-Skript, das die Installationsanweisungen enthält
* Eine Microsoft Azure IoT Edge-CAB-Datei, die IoT Edge-Sicherheitsdaemon (iotedged), Moby-Container-Engine und Moby-CLI enthält
* Ein Installationsprogramm für das Visual C++ Redistributable Package (VC-Runtime)

Wenn Ihr Gerät während der Installation offline ist oder wenn Sie eine bestimmte Version von IoT Edge installieren möchten, können Sie diese Dateien im Voraus auf das Gerät herunterladen. Wenn Sie die Installation durchführen möchten, verweisen Sie im Installationsskript auf das Verzeichnis, in dem die heruntergeladenen Dateien gespeichert sind. Das Installationsprogramm überprüft dieses Verzeichnis zuerst und lädt dann nur Komponenten herunter, die nicht gefunden werden konnten. Wenn alle Dateien offline verfügbar sind, können Sie die Installation ohne Internetverbindung durchführen.

1. Die aktuellen IoT Edge-Installationsdateien und die früheren Versionen finden Sie auf der Seite für [Azure IoT Edge-Releases](https://github.com/Azure/azure-iotedge/releases).

2. Suchen Sie die Version, die Sie installieren möchten, und laden Sie die folgenden Dateien aus dem Abschnitt **Assets** der Versionshinweise auf Ihr IoT-Gerät herunter:

   * IoTEdgeSecurityDaemon.ps1
   * Microsoft-Azure-IoTEdge-amd64.cab aus dem Releasekanal 1.1.

   Es ist wichtig, das PowerShell-Skript für dasselbe Release zu verwenden, zu dem die CAB-Datei gehört, weil die Unterstützung für Features sich in jedem Release ändert.

3. Wenn die von Ihnen heruntergeladene CAB-Datei ein Suffix der Architektur enthält, benennen Sie die Datei in **Microsoft-Azure-IoTEdge.cab** um.

4. Optional können Sie auch ein Installationsprogramm für Visual C++ Redistributable herunterladen. Das PowerShell-Skript verwendet beispielsweise diese Version: [vc_redist.x64.exe](https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe). Speichern Sie das Installationsprogramm auf Ihrem IoT-Gerät im selben Ordner, in dem sich die IoT Edge-Dateien befinden.

5. Für eine Installation mit Offlinekomponenten führen Sie eine [DOT-Quellentnahme](/powershell/module/microsoft.powershell.core/about/about_scripts#script-scope-and-dot-sourcing) in der lokalen Kopie des PowerShell-Skripts durch.

6. Führen Sie den Befehl [Deploy-IoTEdge](reference-windows-scripts.md#deploy-iotedge) mit dem Parameter `-OfflineInstallationPath` aus. Geben Sie den absoluten Pfad zum Dateiverzeichnis an. Beispiel:

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Deploy-IoTEdge -OfflineInstallationPath <path>
   ```

   Der Bereitstellungsbefehl verwendet alle Komponenten, die im angegebenen lokalen Dateiverzeichnis gefunden werden. Wenn die CAB-Datei oder der Visual C++-Installer fehlt, versucht der Befehl, diese Komponenten herunterzuladen.

## <a name="update-iot-edge"></a>Aktualisieren von IoT Edge

Verwenden Sie den Befehl `Update-IoTEdge` zum Aktualisieren des Sicherheitsdaemons. Das Skript lädt per Pull automatisch die neueste Version des Sicherheitsdaemons herunter.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge
```

Wenn Sie den Update-IoTEdge-Befehl ausführen, wird der Sicherheitsdaemon zusammen mit den beiden Runtimecontainerimages von Ihrem Gerät entfernt und aktualisiert. Die Datei „config.yaml“ verbleibt auf dem Gerät, ebenso wie Daten der Moby-Containerengine. Durch die Beibehaltung der Konfigurationsinformationen müssen Sie während des Aktualisierungsvorgangs die Verbindungszeichenfolge oder die Device Provisioning Service-Informationen für Ihr Gerät nicht erneut angeben.

Wenn Sie ein Update auf eine bestimmte Version des Sicherheitsdaemons durchführen möchten, suchen Sie die gewünschte Zielversion aus dem Releasekanal 1.1 in [IoT Edge-Releases](https://github.com/Azure/azure-iotedge/releases). Laden Sie in dieser Version die Datei **Microsoft-Azure-IoTEdge.cab** herunter. Verwenden Sie anschließend den Parameter `-OfflineInstallationPath`, um auf den lokalen Speicherort der heruntergeladenen Datei zu verweisen. Beispiel:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -OfflineInstallationPath <absolute path to directory>
```

>[!NOTE]
>Der Parameter `-OfflineInstallationPath` sucht im angegebenen Verzeichnis nach der Datei **Microsoft-Azure-IoTEdge.cab**. Benennen Sie die Datei um, um das Architektursuffix zu entfernen, sofern vorhanden.

Wenn Sie ein Gerät offline aktualisieren möchten, finden Sie die gewünschte Version unter [Azure IoT Edge-Releases](https://github.com/Azure/azure-iotedge/releases). Laden Sie in dieser Version die Dateien *IoTEdgeSecurityDaemon.ps1* und *Microsoft-Azure-IoTEdge.cab* herunter. Es ist wichtig, das PowerShell-Skript für dasselbe Release zu verwenden, zu dem die CAB-Datei gehört, weil die Unterstützung für Features sich in jedem Release ändert.

Wenn die von Ihnen heruntergeladene CAB-Datei ein Suffix der Architektur enthält, benennen Sie die Datei in **Microsoft-Azure-IoTEdge.cab** um.

Für ein Update mit Offlinekomponenten führen Sie eine [DOT-Quellentnahme](/powershell/module/microsoft.powershell.core/about/about_scripts#script-scope-and-dot-sourcing) in der lokalen Kopie des PowerShell-Skripts durch. Verwenden Sie dann den `-OfflineInstallationPath`-Parameter als Teil des `Update-IoTEdge`-Befehls, und geben Sie den absoluten Pfad zum Dateiverzeichnis an. Beispiel:

```powershell
. <path>\IoTEdgeSecurityDaemon.ps1
Update-IoTEdge -OfflineInstallationPath <path>
```

Weitere Informationen zu Updateoptionen erhalten Sie mit dem Befehl `Get-Help Update-IoTEdge -full` oder in [PowerShell-Skripts für IoT Edge mit Windows-Containern](reference-windows-scripts.md).

## <a name="uninstall-iot-edge"></a>Deinstallieren von IoT Edge

Verwenden Sie die folgenden Befehle, wenn Sie die IoT Edge-Installationen von Ihrem Gerät entfernen möchten.

Wenn Sie IoT Edge von Ihrem Windows-Gerät entfernen möchten, sollten Sie den Befehl [Uninstall-IoTEdge](reference-windows-scripts.md#uninstall-iotedge) in einem PowerShell-Fenster ausführen, das mit Administratorrechten geöffnet wurde. Dieser Befehl entfernt die IoT Edge-Runtime, die vorhandenen Konfigurationen und die Daten der Moby-Engine.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-IoTEdge
```

Um weitere Informationen zu den Deinstallationsoptionen zu erhalten, verwenden Sie den Befehl `Get-Help Uninstall-IoTEdge -full`.

## <a name="next-steps"></a>Nächste Schritte

Sie können nun mit dem Artikel zum [Bereitstellen von IoT Edge-Modulen](how-to-deploy-modules-portal.md) fortfahren, um zu erfahren, wie Sie Module auf Ihrem Gerät bereitstellen.
