---
title: 'Installieren von Zertifikaten auf Geräten: Azure IoT Edge | Microsoft-Dokumentation'
description: Erstellen Sie Testzertifikate, und erfahren Sie, wie Sie diese zur Vorbereitung der Produktionsbereitstellung auf einem Azure IoT Edge-Gerät installieren.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/03/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: cf073572cd5b371ec484c99f14cbefb4cba75ce7
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76509902"
---
# <a name="install-production-certificates-on-an-iot-edge-device"></a>Installieren von Produktionszertifikaten auf einem IoT Edge-Gerät

Alle IoT Edge-Geräte verwenden Zertifikate, um sichere Verbindungen zwischen der Runtime und allen Modulen zu erstellen, die auf dem Gerät ausgeführt werden.
IoT Edge-Geräte, die als Gateways fungieren, verwenden dieselben Zertifikate, um auch eine Verbindung mit ihren Downstreamgeräten herzustellen.

Wenn Sie IoT Edge installieren und Ihr Gerät bereitstellen, wird das Gerät mit temporären Zertifikaten eingerichtet, sodass Sie den Dienst testen können.
Diese temporären Zertifikate laufen nach 90 Tagen ab oder können durch einen Neustart des Computers zurückgesetzt werden.
Wenn Sie bereit sind, Ihre Geräte in ein Produktionsszenario zu verschieben, oder wenn Sie ein Gatewayszenario einrichten möchten, müssen Sie Ihre eigenen Zertifikate bereitstellen.
In diesem Artikel werden die Schritte zum Installieren von Zertifikaten auf Ihren IoT Edge-Geräten veranschaulicht.

Weitere Informationen zu den verschiedenen Arten von Zertifikaten und deren Rollen in einem IoT Edge-Szenario finden Sie unter [Details zur Verwendung von Azure IoT Edge-Zertifikaten](iot-edge-certs.md).

>[!NOTE]
>Der in diesem Artikel verwendete Begriff „Stamm-CA“ bezieht sich auf das öffentliche Zertifikat der obersten Zertifizierungsstelle in der Zertifikatkette für Ihre IoT-Lösung. Sie müssen nicht den Zertifikatstamm einer syndizierten Zertifizierungsstelle oder den Stamm der Zertifizierungsstelle Ihres Unternehmens verwenden. In vielen Fällen ist es tatsächlich nur ein öffentliches Zertifikat einer Zwischenzertifizierungsstelle.

## <a name="prerequisites"></a>Voraussetzungen

* Ein IoT Edge-Gerät, das entweder unter [Windows](how-to-install-iot-edge-windows.md) oder [Linux](how-to-install-iot-edge-linux.md) ausgeführt wird
* Ein Zertifikat einer Stamm-CA, das entweder selbstsigniert ist oder über eine vertrauenswürdige kommerzielle Zertifizierungsstelle wie Baltimore, Verisign, DigiCert oder GlobalSign ausgestellt wurde

Wenn Sie noch keine Stamm-CA besitzen, aber IoT Edge-Features ausprobieren möchten, für die Produktionszertifikate erforderlich sind (z. B. Gatewayszenarios), können Sie [Demozertifikate erstellen, um Features für IoT Edge-Geräte zu testen](how-to-create-test-certificates.md).

## <a name="create-production-certificates"></a>Erstellen von Produktionszertifikaten

Sie sollten Ihre eigene Zertifizierungsstelle verwenden, um die folgenden Dateien zu erstellen:

* Stamm-CA
* Zertifikat der Gerätezertifizierungsstelle
* Privater Schlüssel des Zertifikats der Gerätezertifizierungsstelle

In diesem Artikel ist mit *Stamm-CA* nicht die oberste Zertifizierungsstelle einer Organisation gemeint. Es ist die oberste Zertifizierungsstelle für das IoT Edge-Szenario gemeint, die das IoT Edge-Hub-Modul, die Benutzermodule und alle Downstreamgeräte verwenden, um eine gegenseitige Vertrauensstellung aufzubauen.

Ein Beispiel für diese Zertifikate finden Sie in den Skripts zum Erstellen von Demozertifikaten in [Verwalten von Zertifikaten von Testzertifizierungsstellen für Beispiele und Tutorials](https://github.com/Azure/iotedge/tree/master/tools/CACertificates).

## <a name="install-certificates-on-the-device"></a>Installieren von Zertifikaten auf dem Gerät

Installieren Sie Ihre Zertifikatkette auf dem IoT Edge-Gerät, und konfigurieren Sie die IoT Edge-Runtime so, dass sie auf die neuen Zertifikate verweist.

Wenn Sie die Beispielskripts verwendet haben, um [Demozertifikate zu erstellen](how-to-create-test-certificates.md), müssen Sie folgende drei Dateien auf Ihr IoT Edge-Gerät kopieren:

* Zertifikat der Gerätezertifizierungsstelle: `<WRKDIR>\certs\iot-edge-device-MyEdgeDeviceCA-full-chain.cert.pem`
* Privater Schlüssel des Zertifikats der Gerätezertifizierungsstelle: `<WRKDIR>\private\iot-edge-device-MyEdgeDeviceCA.key.pem`
* Stamm-CA: `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

1. Kopieren Sie diese drei Zertifikat und Schlüsseldateien auf Ihr IoT Edge-Gerät.

   Sie können einen Dienst wie [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) oder eine Funktion wie [Secure Copy Protocol](https://www.ssh.com/ssh/scp/) zum Verschieben der Zertifikatsdateien verwenden.  Wenn Sie die Zertifikate auf dem IoT Edge-Gerät selbst erstellt haben, können Sie diesen Schritt überspringen und den Pfad für das Arbeitsverzeichnis verwenden.

2. Öffnen Sie die Konfigurationsdatei des IoT Edge-Sicherheitsdaemons.

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Legen Sie die Eigenschaften des **Zertifikats** in der Datei „config.yaml“ auf den Datei-URI der Zertifikat- und Schlüsseldateien auf dem IoT Edge-Gerät fest. Entfernen Sie das `#`-Zeichen vor den Zertifikateigenschaften, um die Auskommentierung der vier Zeilen aufzuheben. Stellen Sie sicher, dass der Zeile **certificates:** kein Leerzeichen vorangestellt ist und dass die geschachtelten Zertifikate jeweils um zwei Leerzeichen eingerückt sind. Beispiel:

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "file:///c:/path/device-ca.cert.pem"
        device_ca_pk: "file:///c:/path/device-ca.key.pem"
        trusted_ca_certs: "file:///c:/path/root-ca.root.ca.cert.pem"
      ```

   * Linux:

      ```yaml
      certificates:
        device_ca_cert: "file:///path/device-ca.cert.pem"
        device_ca_pk: "file:///path/device-ca.key.pem"
        trusted_ca_certs: "file:///path/root-ca.root.ca.cert.pem"
      ```

4. Stellen Sie auf Linux-Geräten sicher, dass der Benutzer **iotedge** Leseberechtigungen für das Verzeichnis mit den Zertifikaten hat.

## <a name="next-steps"></a>Nächste Schritte

Das Installieren von Zertifikaten auf einem IoT Edge-Gerät ist erforderlich, bevor Sie die Lösung in der Produktionsumgebung bereitstellen. Unter [Vorbereiten der Bereitstellung einer IoT Edge-Lösung für die Produktion](production-checklist.md) finden Sie weitere Informationen.
