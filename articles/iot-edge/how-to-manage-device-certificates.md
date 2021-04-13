---
title: Verwalten von Gerätezertifikaten – Azure IoT Edge | Microsoft-Dokumentation
description: Sie können Testzertifikate erstellen und auf einem Azure IoT Edge-Gerät installieren und verwalten, um die Bereitstellung in der Produktion vorzubereiten.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f3b6bd19d47658e5ad079f0b731cbafc866bb333
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105045772"
---
# <a name="manage-certificates-on-an-iot-edge-device"></a>Verwalten von Zertifikaten auf einem IoT Edge-Gerät

[!INCLUDE [iot-edge-version-201806-or-202011](../../includes/iot-edge-version-201806-or-202011.md)]

Alle IoT Edge-Geräte verwenden Zertifikate, um sichere Verbindungen zwischen der Runtime und allen Modulen zu erstellen, die auf dem Gerät ausgeführt werden. IoT Edge-Geräte, die als Gateways fungieren, verwenden dieselben Zertifikate, um auch eine Verbindung mit ihren Downstreamgeräten herzustellen.

## <a name="install-production-certificates"></a>Installieren von Produktionszertifikaten

Wenn Sie IoT Edge installieren und Ihr Gerät bereitstellen, wird das Gerät mit temporären Zertifikaten eingerichtet, sodass Sie den Dienst testen können.
Diese temporären Zertifikate laufen nach 90 Tagen ab oder können durch einen Neustart des Computers zurückgesetzt werden.
Wenn Sie in ein Produktionsszenario wechseln oder ein Gatewaygerät erstellen möchten, müssen Sie Ihre eigenen Zertifikate bereitstellen.
In diesem Artikel werden die Schritte zum Installieren von Zertifikaten auf Ihren IoT Edge-Geräten veranschaulicht.

Weitere Informationen zu den verschiedenen Arten von Zertifikaten und deren Rollen finden Sie unter [Grundlegendes zur Verwendung von Zertifikaten durch Azure IoT Edge](iot-edge-certs.md).

>[!NOTE]
>Der in diesem Artikel verwendete Begriff „Stamm-CA“ bezieht sich auf das öffentliche Zertifikat der obersten Zertifizierungsstelle in der Zertifikatkette für Ihre IoT-Lösung. Sie müssen nicht den Zertifikatstamm einer syndizierten Zertifizierungsstelle oder den Stamm der Zertifizierungsstelle Ihres Unternehmens verwenden. In vielen Fällen ist es tatsächlich nur ein öffentliches Zertifikat einer Zwischenzertifizierungsstelle.

### <a name="prerequisites"></a>Voraussetzungen

* Ein IoT Edge Gerät.

  Wenn Sie kein IoT Edge-Gerät eingerichtet haben, können Sie eines auf einem virtuellen Azure-Computer erstellen. Führen Sie die Schritte in einem der Schnellstartartikel zu [Erstellen eines virtuellen Linux-Geräts](quickstart-linux.md) oder [Erstellen eines virtuellen Windows-Geräts](quickstart.md) aus.

* Ein Zertifikat einer Stamm-CA, das entweder selbstsigniert ist oder über eine vertrauenswürdige kommerzielle Zertifizierungsstelle wie Baltimore, Verisign, DigiCert oder GlobalSign ausgestellt wurde

  Wenn Sie noch keine Stamm-CA besitzen, aber IoT Edge-Features ausprobieren möchten, für die Produktionszertifikate erforderlich sind (z. B. Gatewayszenarios), können Sie [Demozertifikate erstellen, um Features für IoT Edge-Geräte zu testen](how-to-create-test-certificates.md).

### <a name="create-production-certificates"></a>Erstellen von Produktionszertifikaten

Sie sollten Ihre eigene Zertifizierungsstelle verwenden, um die folgenden Dateien zu erstellen:

* Stamm-CA
* Zertifikat der Gerätezertifizierungsstelle
* Privater Schlüssel des Zertifikats der Gerätezertifizierungsstelle

In diesem Artikel ist mit *Stamm-CA* nicht die oberste Zertifizierungsstelle einer Organisation gemeint. Es ist die oberste Zertifizierungsstelle für das IoT Edge-Szenario gemeint, die das IoT Edge-Hub-Modul, die Benutzermodule und alle Downstreamgeräte verwenden, um eine gegenseitige Vertrauensstellung aufzubauen.

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"

> [!NOTE]
> Derzeit verhindert eine Einschränkung in libiothsm die Verwendung von Zertifikaten, die am bzw. nach dem 1. Januar 2038 ablaufen.

:::moniker-end

Ein Beispiel für diese Zertifikate finden Sie in den Skripts zum Erstellen von Demozertifikaten in [Verwalten von Zertifikaten von Testzertifizierungsstellen für Beispiele und Tutorials](https://github.com/Azure/iotedge/tree/master/tools/CACertificates).

### <a name="install-certificates-on-the-device"></a>Installieren von Zertifikaten auf dem Gerät

Installieren Sie Ihre Zertifikatkette auf dem IoT Edge-Gerät, und konfigurieren Sie die IoT Edge-Runtime so, dass sie auf die neuen Zertifikate verweist.

Kopieren Sie diese drei Zertifikat und Schlüsseldateien auf Ihr IoT Edge-Gerät. Sie können einen Dienst wie [Azure Key Vault](../key-vault/index.yml) oder eine Funktion wie [Secure Copy Protocol](https://www.ssh.com/ssh/scp/) zum Verschieben der Zertifikatsdateien verwenden. Wenn Sie die Zertifikate auf dem IoT Edge-Gerät selbst erstellt haben, können Sie diesen Schritt überspringen und den Pfad für das Arbeitsverzeichnis verwenden.

Wenn Sie IoT Edge für Linux unter Windows verwenden, müssen Sie Dateiübertragungen zwischen dem Host-Betriebssystem und dem virtuellen Linux-Computer mithilfe des in der Azure IoT Edge-Datei `id_rsa` gespeicherten SSH-Schlüssels authentifizieren. Sie können einen authentifizierten SCP mit dem folgenden Befehl ausführen:

   ```powershell-interactive
   C:\WINDOWS\System32\OpenSSH\scp.exe -i 'C:\Program Files\Azure IoT Edge\id_rsa' <PATH_TO_SOURCE_FILE> iotedge-user@<VM_IP>:<PATH_TO_FILE_DESTINATION>
   ```

   >[!NOTE]
   >Die IP-Adresse des virtuellen Linux-Computers kann über den Befehl `Get-EflowVmAddr` abgefragt werden.

Wenn Sie die Beispielskripts zum [Erstellen von Demozertifikaten](how-to-create-test-certificates.md) verwendet haben, kopieren Sie die folgenden Dateien auf Ihr IoT Edge-Gerät:

* Zertifikat der Gerätezertifizierungsstelle: `<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
* Privater Schlüssel des Zertifikats der Gerätezertifizierungsstelle: `<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`
* Stamm-CA: `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"

1. Öffnen Sie die Konfigurationsdatei des IoT Edge-Sicherheitsdaemons.

   * Linux und IoT Edge für Linux unter Windows: `/etc/iotedge/config.yaml`

   * Windows mit Windows-Containern: `C:\ProgramData\iotedge\config.yaml`

1. Legen Sie die Eigenschaften für **Zertifikat** in „config.yaml“ auf den URI-Pfad zu den Zertifikat- und Schlüsseldateien auf dem IoT Edge-Gerät fest. Entfernen Sie das `#`-Zeichen vor den Zertifikateigenschaften, um die Auskommentierung der vier Zeilen aufzuheben. Stellen Sie sicher, dass der Zeile **certificates:** kein Leerzeichen vorangestellt ist und dass die geschachtelten Zertifikate jeweils um zwei Leerzeichen eingerückt sind. Beispiel:

   * Linux und IoT Edge für Linux unter Windows:

      ```yaml
      certificates:
        device_ca_cert: "file:///<path>/<device CA cert>"
        device_ca_pk: "file:///<path>/<device CA key>"
        trusted_ca_certs: "file:///<path>/<root CA cert>"
      ```

   * Windows mit Windows-Containern:

      ```yaml
      certificates:
        device_ca_cert: "file:///C:/<path>/<device CA cert>"
        device_ca_pk: "file:///C:/<path>/<device CA key>"
        trusted_ca_certs: "file:///C:/<path>/<root CA cert>"
      ```

1. Stellen Sie auf Linux-Geräten sicher, dass der Benutzer **iotedge** Leseberechtigungen für das Verzeichnis mit den Zertifikaten hat.

1. Wenn Sie zuvor bereits andere Zertifikate für IoT Edge auf dem Gerät verwendet haben, löschen Sie die Dateien in den folgenden beiden Verzeichnissen, bevor Sie IoT Edge starten oder neu starten:

   * Linux und IoT Edge für Linux unter Windows: `/var/lib/iotedge/hsm/certs` und `/var/lib/iotedge/hsm/cert_keys`

   * Windows mit Windows-Containern: `C:\ProgramData\iotedge\hsm\certs` und `C:\ProgramData\iotedge\hsm\cert_keys`

:::moniker-end
<!-- end 1.1 -->

<!-- 1.2 -->
:::moniker range=">=iotedge-2020-11"

1. Öffnen Sie die Konfigurationsdatei des IoT Edge-Sicherheitsdaemons: `/etc/aziot/config.toml`

1. Suchen Sie am Anfang der Datei den Parameter `trust_bundle_cert`. Heben Sie die Auskommentierung dieser Zeile auf, und geben Sie den Datei-URI zum Zertifikat der Stammzertifizierungsstelle auf Ihrem Gerät an.

   ```toml
   trust_bundle_cert = "file:///<path>/<root CA cert>"
   ```

1. Suchen Sie in der Datei „config.toml“ nach dem Abschnitt `[edge_ca]`. Heben Sie die Auskommentierung der Zeilen in diesem Abschnitt auf, und geben Sie die Datei-URI-Pfade für die Zertifikat- und Schlüsseldateien auf dem IoT Edge Gerät an.

   ```toml
   [edge_ca]
   cert = "file:///<path>/<device CA cert>"
   pk = "file:///<path>/<device CA key>"
   ```

1. Stellen Sie sicher, dass der Benutzer **iotedge** Leseberechtigungen für das Verzeichnis mit den Zertifikaten hat.

1. Wenn Sie zuvor bereits andere Zertifikate für IoT Edge auf dem Gerät verwendet haben, löschen Sie die Dateien in den folgenden beiden Verzeichnissen, bevor Sie IoT Edge starten oder neu starten:

   * `/var/lib/aziot/certd/certs`
   * `/var/lib/aziot/keyd/keys`

:::moniker-end
<!-- end 1.2 -->

<!-- 1.1. -->
<!-- Temporarily, customizable certificate lifetime not available in 1.2. Update before GA. -->
:::moniker range="iotedge-2018-06"

## <a name="customize-certificate-lifetime"></a>Anpassen der Gültigkeitsdauer von Zertifikaten

IoT Edge generiert in mehreren Fällen automatisch Zertifikate auf dem Gerät:

* Wenn Sie bei der Installation und Bereitstellung von IoT Edge keine eigenen Produktionszertifikate bereitstellen, generiert der IoT Edge-Sicherheits-Manager automatisch ein **gerätebezogenes Zertifizierungsstellenzertifikat**. Dieses selbstsignierte Zertifikat ist nur für Entwicklungs- und Testszenarien und nicht für die Produktion gedacht. Es läuft nach 90 Tagen ab.
* Der IoT Edge-Sicherheits-Manager generiert auch ein **workloadbezogenes Zertifizierungsstellenzertifikat**, das vom gerätebezogenen Zertifizierungsstellenzertifikat signiert ist.

Weitere Informationen zur Aufgabe der verschiedenen Zertifikate auf einem IoT Edge-Gerät finden Sie unter [Grundlegendes zur Verwendung von Zertifikaten durch Azure IoT Edge](iot-edge-certs.md).

Bei diesen beiden automatisch generierten Zertifikaten haben Sie die Möglichkeit, das Flag **auto_generated_ca_lifetime_days** in der Konfigurationsdatei festzulegen, um die Anzahl der Tage für die Gültigkeitsdauer der Zertifikate zu konfigurieren.

>[!NOTE]
>Es gibt ein drittes automatisch generiertes Zertifikat, das vom IoT Edge-Sicherheits-Manager erstellt wird, das **IoT Edge-Hubserverzertifikat**. Dieses Zertifikat hat stets eine Gültigkeitsdauer von 90 Tagen, wird aber vor Ablauf automatisch verlängert. Der Wert für **auto_generated_ca_lifetime_days** wirkt sich nicht auf dieses Zertifikat aus.

Bei Ablauf nach der angegebenen Anzahl von Tagen muss IoT Edge neu gestartet werden, um das Zertifikat der Gerätezertifizierungsstelle erneut zu generieren. Das Zertifikat der Gerätezertifizierungsstelle wird nicht automatisch erneuert.

1. Wenn Sie für das Ablaufdatum des Zertifikats einen Wert konfigurieren möchten, der nicht dem Standardwert von 90 Tagen entspricht, fügen Sie im Abschnitt **certificates** der Konfigurationsdatei den Wert in Tagen hinzu.

   ```yaml
   certificates:
     device_ca_cert: "<ADD URI TO DEVICE CA CERTIFICATE HERE>"
     device_ca_pk: "<ADD URI TO DEVICE CA PRIVATE KEY HERE>"
     trusted_ca_certs: "<ADD URI TO TRUSTED CA CERTIFICATES HERE>"
     auto_generated_ca_lifetime_days: <value>
   ```

   > [!NOTE]
   > Derzeit verhindert eine Einschränkung in libiothsm die Verwendung von Zertifikaten, die am oder nach dem 1. Januar 2038 ablaufen.

1. Löschen Sie den Inhalt des Ordners `hsm`, um alle zuvor generierten Zertifikate zu entfernen.

   * Linux und IoT Edge für Linux unter Windows: `/var/lib/iotedge/hsm/certs` und `/var/lib/iotedge/hsm/cert_keys`

   * Windows mit Windows-Containern: `C:\ProgramData\iotedge\hsm\certs` und `C:\ProgramData\iotedge\hsm\cert_keys`

1. Starten Sie den IoT Edge-Dienst neu.

   * Linux und IoT Edge für Linux unter Windows:

   ```bash
   sudo systemctl restart iotedge
   ```

   * Windows mit Windows-Containern:

   ```powershell
   Restart-Service iotedge
   ```

1. Bestätigen Sie die Einstellung der Gültigkeitsdauer.

   * Linux und IoT Edge für Linux unter Windows:

   ```bash
   sudo iotedge check --verbose
   ```

   * Windows mit Windows-Containern:

   ```powershell
   iotedge check --verbose
   ```

   Prüfen Sie die Ausgabe der Prüfung **Produktionsbereitschaft: Zertifikate**, in der die Anzahl der Tage bis zum Ablauf der automatisch generierten gerätebezogenen Zertifizierungsstellenzertifikate aufgelistet ist.

:::moniker-end
<!-- end 1.1 -->

<!-- 
<!-- 1.2 --
:::moniker range=">=iotedge-2020-11"

1. To configure the certificate expiration to something other than the default 90 days, add the value in days to the **certificates** section of the config file.

   ```toml
   [certificates]
   device_ca_cert = "<ADD URI TO DEVICE CA CERTIFICATE HERE>"
   device_ca_pk = "<ADD URI TO DEVICE CA PRIVATE KEY HERE>"
   trusted_ca_certs = "<ADD URI TO TRUSTED CA CERTIFICATES HERE>"
   auto_generated_ca_lifetime_days = <value>
   ```

1. Delete the contents of the `certd` and `keyd` folders to remove any previously generated certificates: `/var/lib/aziot/certd/certs` `/var/lib/aziot/keyd/keys`

1. Restart IoT Edge.

   ```bash
   sudo iotedge system restart
   ```

1. Confirm the new lifetime setting.

   ```bash
   sudo iotedge check --verbose
   ```

   Check the output of the **production readiness: certificates** check, which lists the number of days until the automatically generated device CA certificates expire.
:::moniker-end
<!-- end 1.2 --
-->

## <a name="next-steps"></a>Nächste Schritte

Das Installieren von Zertifikaten auf einem IoT Edge-Gerät ist erforderlich, bevor Sie die Lösung in der Produktionsumgebung bereitstellen. Unter [Vorbereiten der Bereitstellung einer IoT Edge-Lösung für die Produktion](production-checklist.md) finden Sie weitere Informationen.
