---
title: Verwalten von Gerätezertifikaten – Azure IoT Edge | Microsoft-Dokumentation
description: Sie können Testzertifikate erstellen und auf einem Azure IoT Edge-Gerät installieren und verwalten, um die Bereitstellung in der Produktion vorzubereiten.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/02/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9e3925d2c14d51785ed4fe00a508ea353490e1cd
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89669029"
---
# <a name="manage-certificates-on-an-iot-edge-device"></a>Verwalten von Zertifikaten auf einem IoT Edge-Gerät

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

* Ein IoT Edge-Gerät, das entweder unter [Windows](how-to-install-iot-edge-windows.md) oder [Linux](how-to-install-iot-edge-linux.md) ausgeführt wird
* Ein Zertifikat einer Stamm-CA, das entweder selbstsigniert ist oder über eine vertrauenswürdige kommerzielle Zertifizierungsstelle wie Baltimore, Verisign, DigiCert oder GlobalSign ausgestellt wurde

Wenn Sie noch keine Stamm-CA besitzen, aber IoT Edge-Features ausprobieren möchten, für die Produktionszertifikate erforderlich sind (z. B. Gatewayszenarios), können Sie [Demozertifikate erstellen, um Features für IoT Edge-Geräte zu testen](how-to-create-test-certificates.md).

### <a name="create-production-certificates"></a>Erstellen von Produktionszertifikaten

Sie sollten Ihre eigene Zertifizierungsstelle verwenden, um die folgenden Dateien zu erstellen:

* Stamm-CA
* Zertifikat der Gerätezertifizierungsstelle
* Privater Schlüssel des Zertifikats der Gerätezertifizierungsstelle

In diesem Artikel ist mit *Stamm-CA* nicht die oberste Zertifizierungsstelle einer Organisation gemeint. Es ist die oberste Zertifizierungsstelle für das IoT Edge-Szenario gemeint, die das IoT Edge-Hub-Modul, die Benutzermodule und alle Downstreamgeräte verwenden, um eine gegenseitige Vertrauensstellung aufzubauen.

> [!NOTE]
> Derzeit verhindert eine Einschränkung in libiothsm die Verwendung von Zertifikaten, die am bzw. nach dem 1. Januar 2038 ablaufen.

Ein Beispiel für diese Zertifikate finden Sie in den Skripts zum Erstellen von Demozertifikaten in [Verwalten von Zertifikaten von Testzertifizierungsstellen für Beispiele und Tutorials](https://github.com/Azure/iotedge/tree/master/tools/CACertificates).

### <a name="install-certificates-on-the-device"></a>Installieren von Zertifikaten auf dem Gerät

Installieren Sie Ihre Zertifikatkette auf dem IoT Edge-Gerät, und konfigurieren Sie die IoT Edge-Runtime so, dass sie auf die neuen Zertifikate verweist.

Wenn Sie die Beispielskripts verwendet haben, um [Demozertifikate zu erstellen](how-to-create-test-certificates.md), kopieren Sie die folgenden Dateien auf Ihr IoT Edge-Gerät:

* Zertifikat der Gerätezertifizierungsstelle: `<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
* Privater Schlüssel des Zertifikats der Gerätezertifizierungsstelle: `<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`
* Stamm-CA: `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

1. Kopieren Sie diese drei Zertifikat und Schlüsseldateien auf Ihr IoT Edge-Gerät.

   Sie können einen Dienst wie [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) oder eine Funktion wie [Secure Copy Protocol](https://www.ssh.com/ssh/scp/) zum Verschieben der Zertifikatsdateien verwenden.  Wenn Sie die Zertifikate auf dem IoT Edge-Gerät selbst erstellt haben, können Sie diesen Schritt überspringen und den Pfad für das Arbeitsverzeichnis verwenden.

1. Öffnen Sie die Konfigurationsdatei des IoT Edge-Sicherheitsdaemons.

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

1. Legen Sie die Eigenschaften für **Zertifikat** in „config.yaml“ auf den URI-Pfad zu den Zertifikat- und Schlüsseldateien auf dem IoT Edge-Gerät fest. Entfernen Sie das `#`-Zeichen vor den Zertifikateigenschaften, um die Auskommentierung der vier Zeilen aufzuheben. Stellen Sie sicher, dass der Zeile **certificates:** kein Leerzeichen vorangestellt ist und dass die geschachtelten Zertifikate jeweils um zwei Leerzeichen eingerückt sind. Beispiel:

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "file:///C:/<path>/<device CA cert>"
        device_ca_pk: "file:///C:/<path>/<device CA key>"
        trusted_ca_certs: "file:///C:/<path>/<root CA cert>"
      ```

   * Linux:

      ```yaml
      certificates:
        device_ca_cert: "file:///<path>/<device CA cert>"
        device_ca_pk: "file:///<path>/<device CA key>"
        trusted_ca_certs: "file:///<path>/<root CA cert>"
      ```

1. Stellen Sie auf Linux-Geräten sicher, dass der Benutzer **iotedge** Leseberechtigungen für das Verzeichnis mit den Zertifikaten hat.

1. Wenn Sie zuvor bereits andere Zertifikate für IoT Edge auf dem Gerät verwendet haben, löschen Sie die Dateien in den folgenden beiden Verzeichnissen, bevor Sie IoT Edge starten oder neu starten:

   * Windows: `C:\ProgramData\iotedge\hsm\certs` und `C:\ProgramData\iotedge\hsm\cert_keys`

   * Linux: `/var/lib/iotedge/hsm/certs` und `/var/lib/iotedge/hsm/cert_keys`

## <a name="customize-certificate-lifetime"></a>Anpassen der Gültigkeitsdauer von Zertifikaten

IoT Edge generiert in mehreren Fällen automatisch Zertifikate auf dem Gerät:

* Wenn Sie bei der Installation und Bereitstellung von IoT Edge keine eigenen Produktionszertifikate bereitstellen, generiert der IoT Edge-Sicherheits-Manager automatisch ein **gerätebezogenes Zertifizierungsstellenzertifikat**. Dieses selbstsignierte Zertifikat ist nur für Entwicklungs- und Testszenarien und nicht für die Produktion gedacht. Es läuft nach 90 Tagen ab.
* Der IoT Edge-Sicherheits-Manager generiert auch ein **workloadbezogenes Zertifizierungsstellenzertifikat**, das vom gerätebezogenen Zertifizierungsstellenzertifikat signiert ist.

Weitere Informationen zur Aufgabe der verschiedenen Zertifikate auf einem IoT Edge-Gerät finden Sie unter [Grundlegendes zur Verwendung von Zertifikaten durch Azure IoT Edge](iot-edge-certs.md).

Bei diesen beiden automatisch generierten Zertifikaten haben Sie die Möglichkeit, das Flag **auto_generated_ca_lifetime_days** in der Datei „config.yaml“ festzulegen, um die Anzahl der Tage für die Gültigkeitsdauer der Zertifikate zu konfigurieren.

>[!NOTE]
>Es gibt ein drittes automatisch generiertes Zertifikat, das vom IoT Edge-Sicherheits-Manager erstellt wird, das **IoT Edge-Hubserverzertifikat**. Dieses Zertifikat hat stets eine Gültigkeitsdauer von 90 Tagen, wird aber vor Ablauf automatisch verlängert. Der Wert für **auto_generated_ca_lifetime_days** wirkt sich nicht auf dieses Zertifikat aus.

Um für das Ablaufdatum des Zertifikats einen Wert zu konfigurieren, der nicht dem Standardwert von 90 Tagen entspricht, fügen Sie der Datei **config.yaml** den Wert im Abschnitt **certificates** in Tagen hinzu.

Beim Ablauf nach der angegebenen Anzahl von Tagen muss der IoT Edge-Sicherheits-Daemon neu gestartet werden, um das Zertifikat der Gerätezertifizierungsstelle erneut zu generieren. Es wird nicht automatisch verlängert.

```yaml
certificates:
  device_ca_cert: "<ADD URI TO DEVICE CA CERTIFICATE HERE>"
  device_ca_pk: "<ADD URI TO DEVICE CA PRIVATE KEY HERE>"
  trusted_ca_certs: "<ADD URI TO TRUSTED CA CERTIFICATES HERE>"
  auto_generated_ca_lifetime_days: <value>
```

> [!NOTE]
> Derzeit verhindert eine Einschränkung in libiothsm die Verwendung von Zertifikaten, die am bzw. nach dem 1. Januar 2038 ablaufen.

Führen Sie die folgenden Schritte aus, nachdem Sie den Wert in der Datei „config.yaml“ angegeben haben:

1. Löschen Sie den Inhalt des Ordners `hsm`.

   Windows: `C:\ProgramData\iotedge\hsm\certs and C:\ProgramData\iotedge\hsm\cert_keys` Linux: `/var/lib/iotedge/hsm/certs and /var/lib/iotedge/hsm/cert_keys`

1. Starten Sie den IoT Edge-Dienst neu.

   Windows:

   ```powershell
   Restart-Service iotedge
   ```

   Linux:

   ```bash
   sudo systemctl restart iotedge
   ```

1. Bestätigen Sie die Einstellung der Gültigkeitsdauer.

   Windows:

   ```powershell
   iotedge check --verbose
   ```

   Linux:

   ```bash
   sudo iotedge check --verbose
   ```

   Prüfen Sie die Ausgabe der Prüfung **Produktionsbereitschaft: Zertifikate**, in der die Anzahl der Tage bis zum Ablauf der automatisch generierten gerätebezogenen Zertifizierungsstellenzertifikate aufgelistet ist.

## <a name="next-steps"></a>Nächste Schritte

Das Installieren von Zertifikaten auf einem IoT Edge-Gerät ist erforderlich, bevor Sie die Lösung in der Produktionsumgebung bereitstellen. Unter [Vorbereiten der Bereitstellung einer IoT Edge-Lösung für die Produktion](production-checklist.md) finden Sie weitere Informationen.
