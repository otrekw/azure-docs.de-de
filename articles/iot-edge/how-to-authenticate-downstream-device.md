---
title: Authentifizieren nachgeschalteter Geräte – Azure IoT Edge | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie nachgeschaltete Geräten oder Blattgeräte bei IoT Hub authentifizieren und ihre Verbindung über Azure IoT Edge-Gatewaygeräte weiterleiten.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/13/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 74ef00a1b7310284c5f8c51e3c2c6685ffcd5071
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75434494"
---
# <a name="authenticate-a-downstream-device-to-azure-iot-hub"></a>Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub

In einem Szenario mit transparenten Gateways benötigen nachgeschaltete Geräte (auch als Blattgeräte oder untergeordnete Geräte bezeichnet) wie alle anderen Geräte Identitäten in IoT Hub. In diesem Artikel werden die Optionen für die Authentifizierung von nachgeschalteten Geräten bei IoT Hub beschrieben. Außerdem wird gezeigt, wie Sie die Gatewayverbindung deklarieren.

Es gibt drei allgemeine Schritte zum Einrichten einer erfolgreichen Verbindung mit einem transparenten Gateway. In diesem Artikel wird der zweite Schritt behandelt:

1. Das Gatewaygerät muss eine sichere Verbindung mit nachgeschalteten Geräten herstellen, Nachrichten von nachgeschalteten Geräten empfangen und Nachrichten an das richtige Ziel weiterleiten können. Weitere Informationen finden Sie unter [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md).
2. **Das nachgeschaltete Gerät benötigt eine Geräteidentität, damit es sich bei IoT Hub authentifizieren kann und weiß, dass es über sein Gatewaygerät kommunizieren kann.**
3. Das nachgeschaltete Gerät muss eine sichere Verbindung mit dem Gatewaygerät herstellen. Weitere Informationen finden Sie unter [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md).

Nachgeschaltete Geräte können sich bei IoT Hub mithilfe einer von drei Methoden authentifizieren: symmetrische Schlüssel (auch als SAS-Schlüssel bezeichnet), selbst signierte X.509-Zertifikate oder von einer Zertifizierungsstelle signierte X.509-Zertifikate. Die Authentifizierungsschritte ähneln den Schritten zum Einrichten anderer IoT Edge-Geräte mit IoT Hub. Kleinere Unterschiede bestehen bei der Deklaration der Gatewaybeziehung.

Die Schritte in diesem Artikel zeigen die manuelle Gerätebereitstellung, also die nicht automatisierte Bereitstellung mit dem Azure IoT Hub Device Provisioning Service (DPS). Die Bereitstellung von nachgeschalteten Geräten mit DPS wird nicht unterstützt. 

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte in [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md) aus. Wenn Sie die X.509-Authentifizierung für Ihr nachgeschaltetes Gerät verwenden, müssen Sie dasselbe Skript für die Skripterstellung verwenden, das Sie im Artikel zum transparenten Gateway eingerichtet haben. 

Im Artikel wird an mehreren Stellen auf den *Gatewayhostnamen* verwiesen. Der Gatewayhostname wird im **hostname**-Parameter der Datei „config.yaml“ auf dem IoT Edge-Gatewaygerät deklariert. In der Verbindungszeichenfolge des nachgeschalteten Geräts wird darauf verwiesen. Der Gatewayhostname muss in eine IP-Adresse aufgelöst werden können – entweder mithilfe von DNS oder einem Eintrag in der Hostdatei.

## <a name="register-device-symmetric-key"></a>Registrieren des Geräts (Symmetrischer Schlüssel)

Die Authentifizierung mit symmetrischen Schlüsseln (oder SAS-Schlüsselauthentifizierung) ist die einfachste Möglichkeit für die Authentifizierung bei IoT Hub. Bei der Authentifizierung mit symmetrischen Schlüsseln wird der ID Ihres IoT-Gerätes in IoT Hub ein Base64-Schlüssel zugeordnet. Sie fügen diesen Schlüssel in Ihre IoT-Anwendungen ein, sodass Ihr Gerät diesen beim Herstellen einer Verbindung mit IoT Hub vorweisen kann. 

### <a name="create-the-device-identity"></a>Erstellen der Geräteidentität 

Fügen Sie Ihrem IoT-Hub ein neues IoT-Gerät hinzu. Verwenden Sie dazu entweder das Azure-Portal, die Azure-Befehlszeilenschnittstelle oder die IoT-Erweiterung für Visual Studio Code. Denken Sie daran, dass nachgeschaltete Geräte in IoT Hub als reguläre IoT-Geräte – nicht als IoT Edge-Geräte – angegeben werden müssen. 

Geben Sie beim Erstellen der neuen Geräteidentität die folgenden Informationen an: 

* Erstellen Sie eine ID für Ihr Gerät.

* Wählen Sie als Authentifizierungstyp **Symmetrischer Schlüssel** aus. 

* Wählen Sie optional **Übergeordnetes Gerät festlegen** und dann das IoT Edge-Gatewaygerät aus, über das dieses nachgeschaltete Gerät verbunden wird. Dieser Schritt ist für die Authentifizierung mit symmetrischen Schlüsseln optional. Er wird aber empfohlen, da die Festlegung eines übergeordneten Geräts [Offlinefunktionen](offline-capabilities.md) für das nachgeschaltete Gerät aktiviert. Sie können das Gerät jederzeit aktualisieren, um das übergeordnete Gerät später hinzuzufügen oder zu ändern. 

   ![Erstellen der Geräte-ID bei der Authentifizierung mit symmetrischem Schlüssel im Portal](./media/how-to-authenticate-downstream-device/symmetric-key-portal.png)

Sie können die [IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-iot-cli-extension) verwenden, um diesen Vorgang abzuschließen. Das folgende Beispiel erstellt ein neues IoT-Gerät mit Authentifizierung mit symmetrischen Schlüsseln und weist ein übergeordnetes Gerät zu: 

```cli
az iot hub device-identity create -n {iothub name} -d {new device ID} --pd {existing gateway device ID}
```

Weitere Informationen zu Befehlen für die Azure-Befehlszeilenschnittstelle für die Geräteerstellung und die Verwaltung von über-und untergeordneten Geräten finden Sie in der Referenz zu den [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest)-Befehlen.


Als Nächstes müssen Sie [die Verbindungszeichenfolge abrufen und ändern](#retrieve-and-modify-connection-string), damit Ihr Gerät weiß, dass es eine Verbindung über sein Gateway herstellen soll. 

## <a name="register-device-x509-self-signed"></a>Registrieren des Geräts (X.509, selbstsigniert) 

Für die Authentifizierung mit selbstsignierten X.509-Zertifikaten (auch als Fingerabdruck-Authentifizierung bezeichnet) müssen Sie neue Zertifikate erstellen und auf Ihrem IoT-Gerät speichern. Diese Zertifikate verfügen über einen Fingerabdruck, den Sie für die Authentifizierung an IoT Hub übergeben. 

Wenn Sie über keine Zertifizierungsstelle zum Erstellen von X.509-Zertifikaten verfügen, können Sie [Demozertifikate zum Testen von IoT Edge-Gerätefeatures erstellen](how-to-create-test-certificates.md). Verwenden Sie beim Generieren von Testzertifikaten für Ihr nachgeschaltetes Gerät dasselbe Stammzertifizierungsstellenzertifikat, mit dem die Zertifikate für Ihr Gatewaygerät generiert wurden. 

1. Erstellen Sie mithilfe Ihres Zertifizierungsstellenzertifikats zwei Gerätezertifikate (primär und sekundär) für das nachgeschaltete Gerät. 

   Beim Gerätezertifikat muss der Antragstellername auf die Geräte-ID festgelegt werden, die Sie beim Registrieren des IoT-Geräts im Azure IoT Hub verwenden werden. Diese Einstellung ist für die Authentifizierung erforderlich.

2. Rufen Sie den SHA1-Fingerabdruck aller Zertifikate ab. Es handelt sich dabei um eine hexadezimale Zeichenfolge mit 40 Zeichen. Verwenden Sie den folgenden OpenSSL-Befehl, um das Zertifikat anzuzeigen und den Fingerabdruck zu suchen:

   ```PowerShell/bash
   openssl x509 -in <primary device certificate>.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

   Führen Sie diesen Befehl zweimal aus – einmal für das primäre Zertifikat und einmal für das sekundäre Zertifikat. Sie geben Fingerabdrücke für beide Zertifikate an, wenn Sie ein neues IoT-Gerät mithilfe von selbst signierten X.509-Zertifikaten registrieren. 

3. Navigieren Sie im Azure-Portal zu Ihrem IoT-Hub, und erstellen Sie eine neue IoT-Geräteidentität mit den folgenden Werten: 

   * Geben Sie die **Geräte-ID** an, die mit dem Antragstellernamen Ihrer Gerätezertifikate übereinstimmt. 
   * Wählen Sie als Authentifizierungstyp **X.509, selbstsigniert** aus.
   * Fügen Sie die hexadezimalen Zeichenfolgen ein, die Sie aus den primären und sekundären Zertifikaten Ihres Geräts kopiert haben.
   * Wählen Sie **Übergeordnetes Gerät festlegen** und dann das IoT Edge-Gatewaygerät aus, über das dieses nachgeschaltete Gerät verbunden wird. Für die X.509-Authentifizierung ist für ein nachgeschaltetes Gerät ein übergeordnetes Gerät erforderlich. 

   ![Erstellen der Geräte-ID bei der Authentifizierung mit selbstsignierten X.509-Zertifikaten im Portal](./media/how-to-authenticate-downstream-device/x509-self-signed-portal.png)

4. Kopieren Sie das Gerätezertifikat und die Schlüssel an einen beliebigen Speicherort auf dem nachgeschalteten Gerät. Verschieben Sie außerdem eine Kopie des freigegebenen Stammzertifizierungsstellenzertifikats, mit dem sowohl das Gatewaygerätezertikat als auch die Zertifikate für das nachgeschaltete Gerät generiert wurden. 

   Sie verweisen auf diese Dateien in den Blattgeräteanwendungen, die eine Verbindung mit IoT Hub herstellen. Sie können einen Dienst wie [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) oder eine Funktion wie [Secure Copy Protocol](https://www.ssh.com/ssh/scp/) zum Verschieben der Zertifikatsdateien verwenden.

5. Sehen Sie sich je nach Ihrer bevorzugten Sprache Beispiele dafür an, wie in IoT-Anwendungen auf X.509-Zertifikate verwiesen werden kann: 

   * C#: [Einrichten der X.509-Sicherheit in Ihrem Azure IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates)
   * C: [iotedge_downstream_device_sample](https://github.com/Azure/azure-iot-sdk-c/tree/x509_edge_bugbash/iothub_client/samples/iotedge_downstream_device_sample)
   * Node.js: [simple_sample_device_x509.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js)
   * Java: [SendEventX509.java](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py)
   * Python: [send_message_x509.py](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/advanced-hub-scenarios/send_message_x509.py)

Sie können die [IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-iot-cli-extension) verwenden, um den Vorgang zur Geräteerstellung abzuschließen. Das folgende Beispiel erstellt ein neues IoT-Gerät mit Authentifizierung mit selbstsignierten X.509-Zertifikaten und weist ein übergeordnetes Gerät zu: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_thumbprint --ptp {primary thumbprint} --stp {secondary thumbprint}
```

Weitere Informationen zu den Befehlen für die Azure-Befehlszeilenschnittstelle für die Geräte- und Zertifikaterstellung sowie die Verwaltung von über-und untergeordneten Geräten finden Sie in der Referenz zu den [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest)-Befehlen.


Als Nächstes müssen Sie [die Verbindungszeichenfolge abrufen und ändern](#retrieve-and-modify-connection-string), damit Ihr Gerät weiß, dass es eine Verbindung über sein Gateway herstellen soll. 


## <a name="register-device-x509-ca-signed"></a>Registrieren des Geräts (X.509, durch Zertifizierungsstelle signiert)

Für die Authentifizierung mit X.509-Zertifikaten, die von einer Zertifizierungsstelle (ZS) signiert wurden, muss in IoT Hub ein Zertifikat einer Stammzertifizierungsstelle registriert sein, mit dem Sie die Zertifikate für Ihre IoT-Geräte signieren. Jedes Gerät mit einem Zertifikat, das durch das Zertifikat der Stammzertifizierungsstelle oder einer seiner Zwischenzertifizierungsstellen ausgegeben wurde, wird zur Authentifizierung zugelassen. 

Dieser Abschnitt baut auf den Anweisungen im IoT Hub-Artikel [Einrichten der X.509-Sicherheit in Ihrem Azure IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md) auf. Befolgen Sie die Schritte in diesem Abschnitt, damit Sie die Werte kennen, die Sie für das Einrichten eines nachgeschalteten Geräts benötigen, das eine Verbindung über ein Gateway herstellt. 

Wenn Sie über keine Zertifizierungsstelle zum Erstellen von X.509-Zertifikaten verfügen, können Sie [Demozertifikate zum Testen von IoT Edge-Gerätefeatures erstellen](how-to-create-test-certificates.md). Verwenden Sie beim Generieren von Testzertifikaten für Ihr nachgeschaltetes Gerät dasselbe Stammzertifizierungsstellenzertifikat, mit dem die Zertifikate für Ihr Gatewaygerät generiert wurden. 

1. Befolgen Sie die Anweisungen im Abschnitt [Registrieren der X.509-Zertifizierungsstellenzertifikate bei IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md#register-x509-ca-certificates-to-your-iot-hub) unter *Einrichten der X.509-Sicherheit in Ihrem Azure IoT Hub*. In diesem Abschnitt führen Sie die folgenden Schritte aus: 

   1. Laden Sie ein Zertifikat der Stammzertifizierungsstelle hoch. Wenn Sie die Demozertifikate verwenden, lautet die Stammzertifizierungsstelle **\<pfad>/certs/azure-iot-test-only.root.ca.cert.pem**. 

   2. Stellen Sie sicher, dass Sie Besitzer des Zertifikats der Stammzertifizierungsstelle sind.

2. Befolgen Sie die Anweisungen im Abschnitt [Erstellen eines X.509-Geräts für Ihren IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md#create-an-x509-device-for-your-iot-hub) unter *Einrichten der X.509-Sicherheit in Ihrem Azure IoT Hub*. In diesem Abschnitt führen Sie die folgenden Schritte aus: 

   1. Fügen Sie ein neues Gerät hinzu. Geben Sie in **Geräte-ID** einen Namen in Kleinbuchstaben an, und wählen Sie als Authentifizierungstyp **X.509, durch Zertifizierungsstelle signiert** aus. 
   2. Legen Sie ein übergeordnetes Gerät fest. Wählen Sie für nachgeschaltete Geräte **Übergeordnetes Gerät festlegen** und dann das IoT Edge-Gatewaygerät aus, das die Verbindung mit IoT Hub bereitstellt. 

3. Erstellen Sie eine Vertrauenskette für das nachgeschaltete Gerät. Verwenden Sie dasselbe Zertifikat der Stammzertifizierungsstelle, das Sie zum Erstellen dieser Kette in IoT Hub hochgeladen haben. Verwenden Sie dieselbe Geräte-ID in Kleinbuchstaben, die Sie der Geräteidentität im Portal zugewiesen haben.

4. Kopieren Sie das Gerätezertifikat und die Schlüssel an einen beliebigen Speicherort auf dem nachgeschalteten Gerät. Verschieben Sie außerdem eine Kopie des freigegebenen Stammzertifizierungsstellenzertifikats, mit dem sowohl das Gatewaygerätezertikat als auch die Zertifikate für das nachgeschaltete Gerät generiert wurden. 

   Sie verweisen auf diese Dateien in den Blattgeräteanwendungen, die eine Verbindung mit IoT Hub herstellen. Sie können einen Dienst wie [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) oder eine Funktion wie [Secure Copy Protocol](https://www.ssh.com/ssh/scp/) zum Verschieben der Zertifikatsdateien verwenden.

5. Sehen Sie sich je nach Ihrer bevorzugten Sprache Beispiele dafür an, wie in IoT-Anwendungen auf X.509-Zertifikate verwiesen werden kann: 

   * C#: [Einrichten der X.509-Sicherheit in Ihrem Azure IoT Hub](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates)
   * C: [iotedge_downstream_device_sample](https://github.com/Azure/azure-iot-sdk-c/tree/x509_edge_bugbash/iothub_client/samples/iotedge_downstream_device_sample)
   * Node.js: [simple_sample_device_x509.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js)
   * Java: [SendEventX509.java](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py)
   * Python: [send_message_x509.py](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/advanced-hub-scenarios/send_message_x509.py)

Sie können die [IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-iot-cli-extension) verwenden, um den Vorgang zur Geräteerstellung abzuschließen. Das folgende Beispiel erstellt ein neues IoT-Gerät mit Authentifizierung mit X.509-Zertifikaten, die durch eine Zertifizierungsstelle signiert wurden, und weist ein übergeordnetes Gerät zu: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_ca
```

Weitere Informationen zu Befehlen für die Azure-Befehlszeilenschnittstelle für die Geräteerstellung und die Verwaltung von über-und untergeordneten Geräten finden Sie in der Referenz zu den [az iot hub device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest)-Befehlen.


Als Nächstes müssen Sie [die Verbindungszeichenfolge abrufen und ändern](#retrieve-and-modify-connection-string), damit Ihr Gerät weiß, dass es eine Verbindung über sein Gateway herstellen soll. 


## <a name="retrieve-and-modify-connection-string"></a>Abrufen und Ändern der Verbindungszeichenfolge

Nachdem Sie im Portal eine IoT-Geräteidentität erstellt haben, können Sie die zugehörigen primären oder sekundären Schlüssel abrufen. Einer dieser Schlüssel muss in der Verbindungszeichenfolge enthalten sein, die Sie in Anwendungen einfügen, die mit IoT Hub kommunizieren. IoT Hub stellt der Einfachheit halber für die Authentifizierung mit symmetrischen Schlüsseln in den Gerätedetails eine vollständige Verbindungszeichenfolge bereit. Sie müssen der Verbindungszeichenfolge zusätzliche Informationen über das Gatewaygerät hinzufügen. 

Verbindungszeichenfolgen für nachgeschaltete Geräte müssen folgende Komponenten enthalten: 

* Den IoT-Hub, mit dem das Gerät eine Verbindung herstellt: `Hostname={iothub name}.azure-devices.net`
* Die beim Hub registrierte Geräte-ID: `DeviceID={device ID}`
* Entweder den primären oder den sekundären Schlüssel: `SharedAccessKey={key}`
* Das Gatewaygerät, über das das Gerät die Verbindung herstellt. Sie finden den Wert für **hostname** in der Datei „config.yaml“ des IoT Edge-Gatewaygeräts: `GatewayHostName={gateway hostname}`

Insgesamt sieht eine vollständige Verbindungszeichenfolge wie folgt aus:

``` 
HostName=myiothub.azure-devices.net;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz;GatewayHostName=myGatewayDevice
```

Wenn Sie für dieses nachgeschaltete Gerät eine Beziehung zwischen über- und untergeordneten Elementen eingerichtet haben (erforderlich für die X.509-Authentifizierung, doch optional für die Authentifizierung mit symmetrischem Schlüssel), können Sie die Verbindungszeichenfolge vereinfachen, indem Sie das Gateway direkt als Verbindungshost aufrufen. Beispiel: 

```
HostName=myGatewayDevice;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz
```

An diesem Punkt sollten Sie ein IoT Edge-Gerät registriert und als Gateway konfiguriert haben. Außerdem haben Sie ein nachgeschaltetes IoT-Gerät registriert, das auf sein Gatewaygerät verweist. Als letzten Schritt müssen Sie Zertifikate auf Ihrem nachgeschalteten Gerät platzieren, damit es eine sichere Verbindung mit dem Gateway herstellen kann. 

Lesen Sie dann den nächsten Artikel in der Gateway-Serie, [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md).


## <a name="next-steps"></a>Nächste Schritte

Sie haben beim Durcharbeiten dieses Artikels ein IoT Edge-Geräts erstellt, das als transparentes Gateway fungiert, sowie ein nachgeschaltetes Gerät, das bei einem IoT-Hub registriert ist. Als Nächstes müssen Sie Ihre nachgeschalteten Geräte so konfigurieren, dass sie dem Gatewaygerät vertrauen und eine sichere Verbindung damit herstellen. Weitere Informationen finden Sie unter [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md).
