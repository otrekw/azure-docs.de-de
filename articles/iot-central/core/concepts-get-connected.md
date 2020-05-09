---
title: Gerätekonnektivität in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Artikel werden die wichtigsten Konzepte in Bezug auf die Gerätekonnektivität in Azure IoT Central vorgestellt.
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: b66f5a7d85eb91970d5f551b010dd512b216b9c6
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82509515"
---
# <a name="get-connected-to-azure-iot-central"></a>Herstellen einer Verbindung mit Azure IoT Central

*Dieser Artikel gilt für Operatoren und Geräteentwickler.*

In diesem Artikel werden die Optionen zum Verbinden Ihrer Geräte mit einer Azure IoT Central-Anwendung beschrieben.

In der Regel müssen Sie ein Gerät in Ihrer Anwendung registrieren, bevor eine Verbindung hergestellt werden kann. IoT Central unterstützt jedoch Szenarien, in denen [Geräte eine Verbindung herstellen können, ohne zuvor registriert zu werden](#connect-without-registering-devices).

IoT Central nutzt für die Verwaltung des Verbindungsvorgangs den [Azure IoT Hub Device Provisioning Service (DPS)](../../iot-dps/about-iot-dps.md). Ein Gerät stellt zunächst eine Verbindung mit einem DPS-Endpunkt her, um die Informationen abzurufen, die für die Verbindung mit Ihrer Anwendung erforderlich sind. Intern verarbeitet Ihre IoT Central Anwendung Verbindungen mit Geräten über einen IoT-Hub. DPS ermöglicht Folgendes:

- IoT Central kann Onboarding und das Verbinden von Geräten nach Maß unterstützen.
- Sie können jetzt Geräteanmeldeinformationen generieren und die Geräte offline konfigurieren, ohne sie über IoT Central zu registrieren.
- Sie können Ihre eigenen Geräte-IDs verwenden, um Geräte in IoT Central zu registrieren. Die Verwendung Ihrer eigenen Geräte-IDs vereinfacht die Integration in vorhandene Backofficesysteme.
- Eine einheitliche Möglichkeit, Geräte mit IoT Central zu verbinden.

Zum Absichern der Kommunikation zwischen einem Gerät und Ihrer Anwendung unterstützt IoT Central sowohl SAS (Shared Access Signature) als auch X.509-Zertifikate. In Produktionsumgebungen werden X.509-Zertifikate empfohlen.

In diesem Artikel werden die folgenden Anwendungsfälle beschrieben:

- [Verbinden eines einzelnen Geräts mit SAS](#connect-a-single-device)
- [Verbinden von Geräten nach Maß mit SAS](#connect-devices-at-scale-using-sas)
- [Verbinden von Geräten in jeder Größenordnung mit X.509-Zertifikaten](#connect-devices-using-x509-certificates) – die empfohlene Vorgehensweise für Produktionsumgebungen.
- [Verbinden von Geräten ohne vorherige Registrierung](#connect-without-registering-devices)
- [Verbinden von Geräten, die einzelne DPS-Registrierungen verwenden](#individual-enrollment-based-device-connectivity)
- [Automatisches Zuordnen eines Geräts zu einer Gerätevorlage](#automatically-associate-with-a-device-template)

## <a name="connect-a-single-device"></a>Verbinden eines einzelnen Geräts

Dieser Ansatz ist nützlich, wenn Sie mit IoT Central experimentieren oder Geräte testen. Sie können die SAS-Schlüssel für Geräteverbindungen aus Ihrer IoT Central-Anwendung verwenden, um ein Gerät mit Ihrer IoT Central-Anwendung zu verbinden. Kopieren Sie den _Geräte-SAS-Schlüssel_ aus den Verbindungsinformationen für ein registriertes Gerät:

![SAS-Schlüssel für ein einzelnes Gerät](./media/concepts-get-connected/single-device-sas.png)

Weitere Informationen finden Sie im Tutorial [Erstellen einer Node.js-Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung](./tutorial-connect-device-nodejs.md).

## <a name="connect-devices-at-scale-using-sas"></a>Verbinden von Geräten nach Maß mit SAS

Um mithilfe von SAS-Schlüsseln in jeder Größenordnung Verbindungen von Geräten mit IoT Central herstellen zu können, müssen Sie die Geräte registrieren und dann einrichten:

### <a name="register-devices-in-bulk"></a>Registrieren einer großen Zahl von Geräten

Um eine große Anzahl von Geräten bei Ihrer IoT Central-Anwendung zu registrieren, verwenden Sie eine CSV-Datei zum [Importieren der Geräte-IDs und Gerätenamen](howto-manage-devices.md#import-devices).

Um die Verbindungsinformationen für die importierten Geräte abzurufen, [exportieren Sie eine CSV-Datei aus Ihrer IoT Central-Anwendung](howto-manage-devices.md#export-devices). Die exportierte CSV-Datei enthält die Geräte-IDs und die SAS-Schlüssel.

### <a name="set-up-your-devices"></a>Einrichten Ihrer Geräte

Verwenden Sie die Verbindungsinformationen aus der Exportdatei in Ihrem Gerätecode, um das Herstellen einer Verbindung Ihres Geräts mit Ihrer IoT Central-Anwendung und das Senden von Daten an IoT zu ermöglichen. Außerdem benötigen Sie den DPS-**ID-Bereich** für Ihre Anwendung. Diesen Wert finden Sie unter **Verwaltung > Geräteverbindung**.

> [!NOTE]
> Wie Sie Geräte verbinden können, ohne sie zuerst in IoT Central zu registrieren, erfahren Sie unter [Verbinden ohne vorherige Registrierung der Geräte](#connect-without-registering-devices).

## <a name="connect-devices-using-x509-certificates"></a>Verbinden von Geräten mit X.509-Zertifikaten

In einer Produktionsumgebung ist die Verwendung von X.509-Zertifikaten der empfohlene Mechanismus zur Geräteauthentifizierung für IoT Central. Weitere Informationen finden Sie unter [Geräteauthentifizierung mit X.509-Zertifikaten](../../iot-hub/iot-hub-x509ca-overview.md).

Bevor Sie ein Gerät mit einem X.509-Zertifikat verbinden, müssen Sie ein X.509-Zwischen- oder -Stammzertifikat in der Anwendung hinzufügen und überprüfen. Geräte müssen vom Stamm- oder Zwischenzertifikat generierte X.509-Blattzertifikate verwenden.

### <a name="add-and-verify-a-root-or-intermediate-certificate"></a>Hinzufügen und Überprüfen eines Stamm- oder Zwischenzertifikats

Navigieren Sie zu **Verwaltung > Geräteverbindung > Primäres Zertifikat verwalten**, und fügen Sie das X.509-Stamm- oder Zwischenzertifikat hinzu, das Sie zum Generieren der Gerätezertifikate verwenden.

![Verbindungseinstellungen](media/concepts-get-connected/manage-x509-certificate.png)

Durch das Verifizieren des Zertifikatbesitzes wird sichergestellt, dass die Person, die das Zertifikat hochgeladen hat, im Besitz des privaten Schlüssels für das Zertifikat ist. So verifizieren Sie das Zertifikat:

  1. Wählen Sie die Schaltfläche neben **Überprüfungscode** aus, um einen Code zu generieren.
  1. Erstellen Sie ein X.509-Verifizierungszertifikat mit dem Überprüfungscode, den Sie im vorherigen Schritt generiert haben. Speichern Sie das Zertifikat als CER-Datei.
  1. Laden Sie das signierte Verifizierungszertifikat hoch, und wählen Sie **Überprüfen** aus. Das Zertifikat wird als **Verifiziert** gekennzeichnet, wenn die Überprüfung erfolgreich ist.

Wenn eine Sicherheitsverletzung auftritt oder Ihr primäres Zertifikat abläuft, verwenden Sie das sekundäre Zertifikat, um Downtime zu verringern. Sie können weiterhin Geräte mit dem sekundären Zertifikat bereitstellen, während Sie das primäre Zertifikat aktualisieren.

### <a name="register-and-connect-devices"></a>Registrieren und Verbinden von Geräten

Zum Massenverbinden von Geräten mithilfe von X.509-Zertifikaten registrieren Sie zunächst die Geräte in Ihrer Anwendung, indem Sie mithilfe einer CSV-Datei die [Geräte-IDs und Gerätenamen importieren](howto-manage-devices.md#import-devices). Die Geräte-IDs sollten ausschließlich Kleinbuchstaben aufweisen.

Generieren Sie X.509-Blattzertifikate für Ihre Geräte mit dem hochgeladenen Stamm- oder Zwischenzertifikat. Verwenden Sie die **Geräte-ID** als `CNAME`-Wert in den Blattzertifikaten. Für den Gerätecode werden der Wert **ID-Bereich** für Ihre Anwendung, die **Geräte-ID** und das entsprechende Gerätezertifikat benötigt.

### <a name="for-testing-purposes-only"></a>Nur für Testzwecke

Mit den folgenden Hilfsprogrammen können Sie ausschließlich zu Testzwecken Stamm-, Zwischen- und Gerätezertifikate generieren:

- [Tools for the Azure IoT Device Provisioning Device SDK](https://github.com/Azure/azure-iot-sdk-node/blob/master/provisioning/tools/readme.md) (Tools für das Azure IoT Device Provisioning-Geräte-SDK): Sammlung von Node.js-Tools, mit denen Sie X.509-Zertifikate und -Schlüssel generieren und überprüfen können.
- Wenn Sie ein DevKit-Gerät verwenden, generiert dieses [Befehlszeilentool](https://aka.ms/iotcentral-docs-dicetool) ein Zertifizierungsstellenzertifikat, das Sie Ihrer IoT Central-Anwendung zum Überprüfen der Zertifikate hinzufügen können.
- [Managing test CA certificates for samples and tutorials](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) (Verwalten von Test-Zertifizierungsstellenzertifikaten für Beispiele und Tutorials): Sammlung von PowerShell- und Bash-Skripts für folgende Zwecke:
  - Erstellen einer Zertifikatkette.
  - Speichern Sie die Zertifikate als CER-Dateien zum Hochladen in Ihre IoT Central-Anwendung.
  - Verwenden Sie den Überprüfungscode aus der IoT Central-Anwendung, um das Verifizierungszertifikat zu generieren.
  - Erstellen Sie untergeordnete Zertifikate für Ihre Geräte unter Verwendung Ihrer Geräte-IDs als Parameter für das Tool.

### <a name="further-reference"></a>Weitere Referenz

- [Beispielimplementierung für Raspberry Pi](https://aka.ms/iotcentral-docs-Raspi-releases)
- [Beispiel für einen Geräteclient in C](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)

## <a name="connect-without-registering-devices"></a>Verbinden ohne Registrierung der Geräte

Für alle zuvor beschriebenen Szenarien ist es erforderlich, Geräte vor dem Verbinden in Ihrer Anwendung zu registrieren. IoT Central ermöglicht OEMs außerdem die Massenfertigung von Geräten, die eine Verbindung herstellen können, ohne zuvor registriert zu werden. Ein OEM generiert geeignete Anmeldeinformationen und konfiguriert die Geräte im Werk. Wenn ein Kunde ein Gerät erstmalig einschaltet, stellt es eine Verbindung mit DPS her und wird dadurch automatisch mit der richtigen IoT Central-Anwendung verbunden. Ein IoT Central-Operator muss das Gerät genehmigen, bevor es Daten an die Anwendung sendet.

Der Flow unterscheidet sich geringfügig danach, ob die Geräte SAS-Token oder X.509-Zertifikate verwenden:

### <a name="connect-devices-that-use-sas-tokens-without-registering"></a>Verbinden von Geräten, die SAS-Token verwenden, ohne Registrierung

1. Kopieren Sie den primären Gruppenschlüssel der IoT Central-Anwendung:

    ![Primärer SAS-Schlüssel der Anwendungsgruppe](media/concepts-get-connected/group-sas-keys.png)

1. Verwenden Sie das Tool [dps-keygen](https://www.npmjs.com/package/dps-keygen), um die SAS-Schlüssel für das Gerät zu generieren. Verwenden Sie den primären Gruppenschlüssel aus dem vorherigen Schritt. Die Geräte-ID muss aus Kleinbuchstaben bestehen:

    ```cmd
    dps-keygen -mk:<group primary key> -di:<device ID>
    ```

1. Der OEM schreibt für jedes Gerät eine Geräte-ID, einen generierten SAS-Schlüssel für das Gerät und den Wert **ID-Bereich** der Anwendung in den Flashspeicher.

1. Wenn Sie ein Gerät einschalten, stellt es zuerst eine Verbindung mit DPS her, um seine IoT Central-Registrierungsinformationen abzurufen.

    Das Gerät weist zunächst auf der Seite **Geräte** den Gerätestatus **Nicht zugeordnet** auf und ist keiner Gerätevorlage zugewiesen. **Migrieren** Sie auf der Seite **Geräte** das Gerät zur entsprechenden Gerätevorlage. Die Gerätebereitstellung ist nun abgeschlossen, der Gerätestatus lautet jetzt **Bereitgestellt**, und das Gerät kann mit dem Senden von Daten beginnen.

    Auf der Seite **Verwaltung > Geräteverbindung** steuert die Option **Automatisch genehmigen**, ob Sie das Gerät manuell genehmigen müssen, bevor es mit dem Senden von Daten beginnen kann.

    > [!NOTE]
    > Informationen zum automatischen Zuordnen eines Geräts zu einer Gerätevorlage finden Sie unter [Automatisches Zuordnen eines Geräts zu einer Gerätevorlage](#automatically-associate-with-a-device-template).

### <a name="connect-devices-that-use-x509-certificates-without-registering"></a>Verbinden von Geräten, die X.509-Zertifikate verwenden, ohne Registrierung

1. [Fügen Sie Ihrer IoT Central-Anwendung ein X.509-Stamm- oder -Zwischenzertifikat hinzu](#connect-devices-using-x509-certificates).(#connect-devices-using-x509-certificates)

1. Generieren Sie die untergeordneten Zertifikate für Ihre Geräte, indem Sie das Stamm- bzw. Zwischenzertifikat verwenden, das Sie Ihrer IoT Central-Anwendung hinzugefügt haben. Verwenden Sie in den Blattzertifikaten Geräte-IDs in Kleinbuchstaben als `CNAME`.

1. Der OEM schreibt für jedes Gerät eine Geräte-ID, eine generiertes übriges X.509-Zertifikat und den Wert **ID-Bereich** der Anwendung in den Flashspeicher.

1. Wenn Sie ein Gerät einschalten, stellt es zuerst eine Verbindung mit DPS her, um seine IoT Central-Registrierungsinformationen abzurufen.

    Das Gerät weist zunächst auf der Seite **Geräte** den Gerätestatus **Nicht zugeordnet** auf und ist keiner Gerätevorlage zugewiesen. **Migrieren** Sie auf der Seite **Geräte** das Gerät zur entsprechenden Gerätevorlage. Die Gerätebereitstellung ist nun abgeschlossen, der Gerätestatus lautet jetzt **Bereitgestellt**, und das Gerät kann mit dem Senden von Daten beginnen.

    Auf der Seite **Verwaltung > Geräteverbindung** steuert die Option **Automatisch genehmigen**, ob Sie das Gerät manuell genehmigen müssen, bevor es mit dem Senden von Daten beginnen kann.

    > [!NOTE]
    > Informationen zum automatischen Zuordnen eines Geräts zu einer Gerätevorlage finden Sie unter [Automatisches Zuordnen eines Geräts zu einer Gerätevorlage](#automatically-associate-with-a-device-template).

## <a name="individual-enrollment-based-device-connectivity"></a>Auf individueller Registrierung basierende Gerätekonnektivität

Kunden, die Geräte mit gerätespezifischen Authentifizierungsanmeldeinformationen verbinden, verwenden individuelle Registrierungen. Eine individuelle Registrierung ist ein Eintrag für ein einzelnes Gerät, das verbunden werden darf. Individuelle Registrierungen verwenden als Nachweismechanismen X.509-Blattzertifikate oder SAS-Token (von einem physischen oder virtuellen Trusted Platform Module). Die Geräte-ID (auch als Registrierungs-ID bezeichnet) in einer individuellen Registrierung ist alphanumerisch und besteht aus Kleinbuchstaben und ggf. Bindestrichen. Weitere Informationen finden Sie unter [Individuelle DPS-Registrierung](https://docs.microsoft.com/azure/iot-dps/concepts-service#individual-enrollment).

> [!NOTE]
> Wenn Sie eine individuelle Registrierung für ein Gerät erstellen, hat diese Vorrang vor den Standardoptionen für Gruppenregistrierungen in Ihrer IoT Central-Anwendung.

### <a name="create-individual-enrollments"></a>Erstellen individueller Registrierungen

IoT Central unterstützt die folgenden Nachweismechanismen für individuelle Registrierungen:

- **Nachweis des symmetrischen Schlüssels:** Der Nachweis des symmetrischen Schlüssels ist eine einfache Methode zum Authentifizieren eines Geräts bei einer DPS-Instanz. Öffnen Sie zum Erstellen einer individuellen Registrierung, bei der symmetrische Schlüssel verwendet werden, die Seite **Geräteverbindung**, und wählen Sie **Individuelle Registrierung** als Verbindungsmethode und **Shared Access Signature (SAS)** als Mechanismus aus. Geben Sie Base64-codierte primäre und sekundäre Schlüssel ein, und speichern Sie die Änderungen. Verwenden Sie den **ID-Bereich**, die **Geräte-ID** und den primären oder den sekundären Schlüssel, um Ihr Gerät zu verbinden.

    > [!TIP]
    > Zum Testen können Sie **OpenSSL** verwenden, um Base64-codierte Schlüssel zu generieren: `openssl rand -base64 64`

- **X.509-Zertifikate:** Öffnen Sie zum Erstellen einer individuellen Registrierung mit X.509-Zertifikaten die Seite **Geräteverbindung**, und wählen Sie **Individuelle Registrierung** als Verbindungsmethode und **Zertifikate (X.509)** als Mechanismus aus. Für Gerätezertifikate, die mit einem Eintrag für eine individuelle Registrierung verwendet werden, müssen der Aussteller und der Antragsteller-CN auf die Geräte-ID festgelegt werden.

    > [!TIP]
    > Zum Testen können Sie die [Tools für das Azure IoT Device Provisioning-Geräte-SDK für Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools) verwenden, um ein selbstsigniertes Zertifikat zu generieren: `node create_test_cert.js device "mytestdevice"`

- **Nachweis mit Trusted Platform Module (TPM):** Ein [TPM](https://docs.microsoft.com/azure/iot-dps/concepts-tpm-attestation) ist ein Typ von Hardwaresicherheitsmodulen. Die Verwendung eines TPM stellt eine der sichersten Möglichkeiten dar, ein Gerät zu verbinden. In diesem Artikel wird davon ausgegangen, dass Sie ein diskretes, integriertes oder Firmware-TPM verwenden. Durch Software emulierte TPMs eignen sich gut für die Prototyperstellung oder für Tests, bieten aber nicht das gleiche Maß an Sicherheit wie diskrete, integrierte oder Firmware-TPMs. Verwenden Sie keine Software-TPMs in Produktionsumgebungen. Öffnen Sie zum Erstellen einer individuellen Registrierung mit TPM die Seite **Geräteverbindung**, und wählen Sie **Individuelle Registrierung** als Verbindungsmethode und **TPM** als Mechanismus aus. Geben Sie den TPM-Endorsement Key ein, und speichern Sie die Geräteverbindungsinformationen.

## <a name="automatically-associate-with-a-device-template"></a>Automatisches Zuordnen zu einer Gerätevorlage

Eine der wichtigsten Funktionen von IoT Central ist die Möglichkeit, Gerätevorlagen automatisch für Geräteverbindungen zuzuordnen. Geräte können zusammen mit den Geräteanmeldeinformationen eine **CapabilityModelId** als Teil des Geräteregistrierungsaufrufs senden. Die **CapabilityModelID** ist eine URN, die das vom Gerät implementierte Funktionsmodell identifiziert. Die IoT Central-Anwendung kann die **CapabilityModelID** verwenden, um die zu verwendende Gerätevorlage zu identifizieren und das Gerät dann automatisch der Gerätevorlage zuzuordnen. Der Ermittlungsvorgang funktioniert wie folgt:

1. Wenn die Gerätevorlage bereits in der IoT Central-Anwendung veröffentlicht ist, ist das Gerät der Gerätevorlage zugeordnet.
1. Bei vorab zertifizierten IoT Plug & Play-Geräten wird die Gerätevorlage aus dem öffentlichen Repository abgerufen, wenn die Gerätevorlage nicht bereits in der IoT Central-Anwendung veröffentlicht ist.

Die folgenden Ausschnitte zeigen das Format der zusätzlichen Nutzlast, die das Gerät während des DPS-Registrierungsaufrufs senden muss, damit die automatische Zuordnung funktioniert.

Dies ist das Format für Geräte, die das allgemein verfügbare Geräte-SDK verwenden, das IoT Plug & Play nicht unterstützt:

```javascript
    iotcModelId: '< this is the URN for the capability model>';
```

Dies ist das Format für Geräte, die das Geräte-SDK der öffentlichen Vorschau verwenden, das IoT Plug & Play unterstützt:

```javascript
'__iot:interfaces': {
    CapabilityModelId: <this is the URN for the capability model>
}
```

> [!NOTE]
> Die Option **Automatisch genehmigen** muss unter **Verwaltung > Geräteverbindung** aktiviert sein, damit Geräte automatisch eine Verbindung herstellen, die Gerätevorlage ermitteln und mit dem Senden von Daten beginnen können.

## <a name="device-status-values"></a>Werte für den Gerätestatus

Wenn ein echtes Gerät eine Verbindung mit Ihrer IoT Central-Anwendung herstellt, ändert sich der Gerätestatus wie folgt:

1. Der Gerätestatus lautet zuerst **Registriert**. Dieser Status bedeutet, dass das Gerät in IoT Central erstellt ist und über eine Geräte-ID verfügt. Ein Gerät ist registriert, wenn Folgendes zutrifft:
    - Ein neues echtes Gerät wird auf der Seite **Geräte** hinzugefügt.
    - Eine Gruppe von Geräten wird auf der Seite **Geräte** mithilfe der Option **Importieren** hinzugefügt.

1. Der Gerätestatus ändert sich in **Bereitgestellt**, wenn das Gerät, das mit gültigen Anmeldeinformationen eine Verbindung mit Ihrer IoT Central-Anwendung hergestellt hat, den Bereitstellungsschritt abschließt. In diesem Schritt verwendet das Gerät DPS, um automatisch eine Verbindungszeichenfolge vom IoT-Hub Ihrer IoT Central-Anwendung abzurufen. Das Gerät kann jetzt eine Verbindung mit IoT Central herstellen und Daten senden.

1. Ein Operator kann ein Gerät blockieren. Wenn ein Gerät blockiert wird, kann es keine Daten an Ihre IoT Central-Anwendung senden. Blockierte Geräte weisen den Status **Blockiert** auf. Ein Operator muss das Gerät zurücksetzen, bevor es wieder Daten senden kann. Wenn ein Operator die Blockierung eines Geräts aufhebt, wird der Status auf den vorherigen Wert (**Registriert** oder **Bereitgestellt**) zurückgesetzt.

1. Wenn der Gerätestatus **Warten auf Genehmigung** lautet, ist die Option **Automatische Genehmigung** deaktiviert. Ein Operator muss ein Gerät explizit genehmigen, bevor es mit dem Senden von Daten beginnt. Geräte, die auf der Seite **Geräte** nicht manuell registriert wurden, aber eine Verbindung mit gültigen Anmeldeinformationen hergestellt haben, weisen den Gerätestatus **Warten auf Genehmigung** auf. Diese Geräte können auf der Seite **Geräte** mithilfe der Schaltfläche **Genehmigen** von Operatoren genehmigt werden.

1. Wenn der Gerätestatus **Nicht zugeordnet** lautet, ist den Geräten, die eine Verbindung mit IoT Central herstellen, keine Gerätevorlage zugeordnet. Diese Situation ist in den folgenden Szenarien typisch:

    - Eine Gruppe von Geräten wurde auf der Seite **Geräte** mithilfe der Option **Importieren** hinzugefügt, ohne die Gerätevorlage anzugeben.
    - Ein Geräte wurde auf der Seite **Geräte** manuell hinzugefügt, ohne die Gerätevorlage anzugeben. Das Gerät stellte dann eine Verbindung mit gültigen Anmeldeinformationen her.  

    Der Operator kann auf der Seite **Geräte** mithilfe der Schaltfläche **Migrieren** einem Gerät eine Gerätevorlage zuordnen.

## <a name="best-practices"></a>Bewährte Methoden

Sie sollten die Geräteverbindungszeichenfolge, die von DPS zurückgegeben wird, wenn Sie das Gerät zum ersten Mal verbinden, nicht speichern oder zwischenspeichern. Um eine Verbindung mit einem Gerät wiederherzustellen, durchlaufen Sie den Standardflow für die Geräteregistrierung, um die richtige Geräteverbindungszeichenfolge zu erhalten. Wenn das Gerät die Verbindungszeichenfolge zwischenspeichert, besteht die Gefahr einer veralteten Verbindungszeichenfolge in der Gerätesoftware, wenn IoT Central die zugrunde liegende Azure IoT Hub-Instanz aktualisiert.

## <a name="sdk-support"></a>SDK-Unterstützung

Die Azure-Geräte-SDKs bieten Ihnen die einfachste Möglichkeit, Ihren Gerätecode zu implementieren. Derzeit sind folgende Geräte-SDKs verfügbar:

- [Azure IoT-SDK für C](https://github.com/azure/azure-iot-sdk-c)
- [Azure IoT-SDK für Python](https://github.com/azure/azure-iot-sdk-python)
- [Azure IoT-SDK für Node.js](https://github.com/azure/azure-iot-sdk-node)
- [Azure IoT-SDK für Java](https://github.com/azure/azure-iot-sdk-java)
- [Azure IoT-SDK für .NET](https://github.com/azure/azure-iot-sdk-csharp)

### <a name="sdk-features-and-iot-hub-connectivity"></a>SDK-Features und IoT Hub-Konnektivität

Bei der gesamten Gerätekommunikation mit IoT Hub werden die folgenden IoT Hub-Konnektivitätsoptionen verwendet:

- [Nachrichten, die von Geräten an die Cloud gesendet werden](../../iot-hub/iot-hub-devguide-messages-d2c.md)
- [Gerätezwillinge](../../iot-hub/iot-hub-devguide-device-twins.md)

In der folgenden Tabelle wird die Zuordnung von Azure IoT Central-Gerätefeatures zu IoT Hub-Features zusammengefasst:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Telemetrie | Nachrichten, die von Geräten an die Cloud gesendet werden |
| Eigenschaft | Gemeldete Eigenschaften von Gerätezwillingen |
| Eigenschaft (schreibbar) | Gewünschte und gemeldete Eigenschaften von Gerätezwillingen |
| Get-Help | Direkte Methoden |

Wenn Sie mehr über die Verwendung der Geräte-SDKs erfahren möchten, sehen Sie sich den Beispielcode unter [Verbinden eines DevKit-Geräts mit Ihrer Azure IoT Central-Anwendung](howto-connect-devkit.md) an.

### <a name="protocols"></a>Protokolle

Die Geräte-SDKs unterstützen die folgenden Netzwerkprotokolle zum Herstellen einer Verbindung mit einem IoT Hub:

- MQTT
- AMQP
- HTTPS

Informationen zu diesen verschiedenen Protokollen sowie eine Anleitung zu deren Auswahl finden Sie unter [Auswählen eines Kommunikationsprotokolls](../../iot-hub/iot-hub-devguide-protocols.md).

Wenn Ihr Gerät keines der unterstützten Protokolle verwenden kann, können Sie mithilfe von Azure IoT Edge eine Protokollkonvertierung durchführen. IoT Edge unterstützt weitere Intelligence-on-the-Edge-Szenarien, um die Verarbeitung von der Azure IoT Central-Anwendung auf den Edge auszulagern.

## <a name="security"></a>Sicherheit

Alle Daten, die zwischen Geräten und Azure IoT Central ausgetauscht werden, werden verschlüsselt. IoT Hub authentifiziert jede Anforderung von einem Gerät, das eine Verbindung mit einem der geräteseitigen IoT Hub-Endpunkte herstellt. Um den unverschlüsselten Austausch von Anmeldeinformationen zu vermeiden, verwendet ein Gerät signierte Token zur Authentifizierung. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf IoT Hub](../../iot-hub/iot-hub-devguide-security.md).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie ein Geräteentwickler sind, werden einige der folgenden nächsten Schritte empfohlen:

- Informieren Sie sich über [Überwachen der Gerätekonnektivität per Azure CLI](./howto-monitor-devices-azure-cli.md).
- Informieren Sie sich über [Definieren eines neuen IoT-Gerätetyps in Ihrer Azure IoT Central-Anwendung](./howto-set-up-template.md).
- Informieren Sie sich über [Azure IoT Edge-Geräte und Azure IoT Central](./concepts-iot-edge.md).
