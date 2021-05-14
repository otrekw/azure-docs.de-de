---
title: 'Schnellstart: Verwenden eines symmetrischen Schlüssels zum Bereitstellen eines Geräts in Azure IoT Hub mithilfe von C#'
description: In dieser Schnellstartanleitung verwenden Sie das C#-Geräte-SDK für Device Provisioning Service (DPS), um ein Gerät mit symmetrischem Schlüssel für einen IoT-Hub bereitzustellen.
author: wesmc7777
ms.author: wesmc
ms.date: 04/23/2021
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: eliotgra
ms.custom: mvc
ms.openlocfilehash: e67616c2c92676c3af79e3040bc09d3b1b87a11b
ms.sourcegitcommit: aba63ab15a1a10f6456c16cd382952df4fd7c3ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107988296"
---
# <a name="quickstart-provision-a-symmetric-key-device-using-c"></a>Schnellstart: Bereitstellen eines Geräts mit symmetrischem Schlüssel mithilfe von C#

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von C# einen Windows-Entwicklungscomputer als Gerät für einen IoT-Hub bereitstellen. Dieses Gerät verwendet einen symmetrischen Schlüssel und eine individuelle Registrierung für die Authentifizierung bei einer Device Provisioning Service-Instanz (DPS-Instanz), damit es einem IoT-Hub zugewiesen werden kann. Zum Bereitstellen des Geräts wird Beispielcode aus den [Azure IoT-Beispielen für C#](https://github.com/Azure-Samples/azure-iot-samples-csharp) verwendet. 

In diesem Artikel wird die Bereitstellung mit einer individuellen Registrierung erörtert. Sie können jedoch auch Registrierungsgruppen verwenden. Bei der Verwendung von Registrierungsgruppen gibt es einige Unterschiede. Sie müssen beispielsweise einen abgeleiteten Geräteschlüssel mit einer eindeutigen Registrierungs-ID für das Gerät verwenden. Unter [Bereitstellen von Geräten mit symmetrischen Schlüsseln für Registrierungsgruppen](how-to-legacy-device-symm-key.md) finden Sie ein Beispiel für eine Registrierungsgruppe. Weitere Informationen zu Registrierungsgruppen finden Sie unter [Gruppenregistrierungen](concepts-symmetric-key-attestation.md#group-enrollments).

Wenn Sie mit der automatischen Bereitstellung nicht vertraut sind, lesen Sie die Übersicht zur [Bereitstellung](about-iot-dps.md#provisioning-process). 

Vergewissern Sie sich außerdem, dass Sie die Schritte unter [Einrichten des IoT Hub Device Provisioning-Diensts über das Azure-Portal](./quick-setup-auto-provision.md) ausgeführt haben, bevor Sie mit dieser Schnellstartanleitung fortfahren. In dieser Schnellstartanleitung wird vorausgesetzt, dass Sie bereits eine Device Provisioning-Instanz erstellt haben.

In diesem Artikel wird von der Nutzung einer Windows-Arbeitsstation ausgegangen. Allerdings können Sie die Verfahren auch unter Linux ausführen. Ein Beispiel für Linux finden Sie unter [Bereitstellen für Mehrinstanzenfähigkeit](how-to-provision-multitenant.md).


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Voraussetzungen

* Stellen Sie sicher, dass auf Ihrem Windows-basierten Computer mindestens das [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) installiert ist.

* Die neueste Version von [Git](https://git-scm.com/download/) ist installiert.

<a id="setupdevbox"></a>


## <a name="create-a-device-enrollment"></a>Erstellen einer Geräteregistrierung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, wählen Sie im Menü links die Schaltfläche **Alle Ressourcen** aus, und öffnen Sie Ihre Device Provisioning Service-Instanz (DPS).

2. Wählen Sie die Registerkarte **Registrierungen verwalten** und dann oben die Schaltfläche **Individuelle Registrierung hinzufügen** aus. 

3. Geben Sie im Bereich **Registrierung hinzufügen** die folgenden Informationen ein, und wählen Sie die Schaltfläche **Speichern** aus.

   - **Mechanismus**: Wählen Sie **Symmetrischer Schlüssel** als *Mechanismus* für den Nachweis der Identität aus.

   - **Schlüssel automatisch generieren:** Aktivieren Sie dieses Kontrollkästchen.

   - **Registrierungs-ID**: Geben Sie eine Registrierungs-ID ein, um die Registrierung zu identifizieren. Verwenden Sie nur Kleinbuchstaben und Bindestriche („-“). Beispiel: **symm-key-csharp-device-01**

   - **IoT Hub-Geräte-ID**: Geben Sie einen Gerätebezeichner ein. Beispiel: **csharp-device-01**

     ![Hinzufügen einer individuellen Registrierung für den Nachweis des symmetrischen Schlüssels im Portal](./media/quick-create-device-symmetric-key-csharp/create-individual-enrollment-csharp.png)

4. Nachdem Sie Ihre Registrierung gespeichert haben, werden der **Primärschlüssel** und der **Sekundärschlüssel** generiert und dem Registrierungseintrag hinzugefügt. Die Registrierung Ihres Geräts mit symmetrischem Schlüssel wird auf der Registerkarte *Individuelle Registrierungen* in der Spalte *Registrierungs-ID* als **symm-key-csharp-device-01** angezeigt. 

5. Öffnen Sie die Registrierung, und kopieren Sie den Wert des von Ihnen generierten **Primärschlüssels**. Sie verwenden diesen Schlüsselwert und die **Registrierungs-ID** später, wenn Sie den Beispielcode für die Gerätebereitstellung ausführen.



## <a name="prepare-the-c-environment"></a>Vorbereiten der C#-Umgebung 

1. Öffnen Sie eine Git CMD- oder Git Bash-Befehlszeilenumgebung. Klonen Sie mithilfe des folgenden Befehls das GitHub-Repository mit den [Azure IoT-Beispielen für C#](https://github.com/Azure-Samples/azure-iot-samples-csharp):

    ```cmd
    git clone https://github.com/Azure-Samples/azure-iot-samples-csharp.git
    ```



<a id="firstbootsequence"></a>

## <a name="run-the-device-provisioning-code"></a>Ausführen des Gerätebereitstellungscodes

In diesem Abschnitt führen Sie das Beispiel für die Gerätebereitstellung mit drei Parametern aus, mit denen der Gerätebereitstellungs-Beispielcode als Gerät mit symmetrischem Schlüssel für die Registrierung bei Ihrer DPS-Ressource authentifiziert wird. Dabei handelt es sich um folgende drei Parameter:

* ID Scope (ID-Bereich)
* Registrierungs-ID für eine individuelle Registrierung
* Primärer symmetrischer Schlüssel für eine individuelle Registrierung

Der Bereitstellungscode kontaktiert die DPS-Ressource mithilfe dieser Parameter, um Ihr Gerät zu authentifizieren. Anschließend wird das Gerät basierend auf der Konfiguration der individuellen Registrierung einem bereits mit der DPS-Instanz verknüpften IoT-Hub zugewiesen. Nach der Bereitstellung sendet der Beispielcode eine Testtelemetrienachricht an den IoT-Hub.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Device Provisioning Service-Menü die Option **Übersicht** aus, und kopieren Sie den Wert für **ID-Bereich**. Sie verwenden diesen Wert für den `IdScope`-Parameter, wenn Sie den Beispielcode ausführen.

2. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie im geklonten Beispielrepository zu *SymmetricKeySample*:

    ```cmd
    cd azure-iot-samples-csharp\provisioning\Samples\device\SymmetricKeySample
    ```

3. Öffnen Sie im Ordner *SymmetricKeySample* die Datei *Parameters.cs* in einem Text-Editor. Diese Datei enthält die vom Beispiel unterstützten Parameter. Beim Ausführen des Beispiels werden nur die ersten drei erforderlichen Parameter in diesem Artikel verwendet. Sehen Sie sich den Code in dieser Datei an. Es sind keine Änderungen erforderlich.
 
    | Parameter                         | Erforderlich | BESCHREIBUNG     |
    | :-------------------------------- | :------- | :-------------- |
    | `--s` oder `--IdScope`              | Richtig     | Der ID-Bereich der DPS-Instanz |
    | `--i` oder `--Id`                   | Richtig     | Die Registrierungs-ID bei Verwendung der individuellen Registrierung oder die gewünschte Geräte-ID bei Verwendung der Gruppenregistrierung |
    | `--p` oder `--PrimaryKey`           | Richtig     | Der Primärschlüssel der individuellen Registrierung oder Gruppenregistrierung. |
    | `--e` oder `--EnrollmentType`       | Falsch    | Der Typ der Registrierung: `Individual` oder `Group`. Der Standardwert lautet `Individual`. |
    | `--g` oder `--GlobalDeviceEndpoint` | Falsch    | Der globale Endpunkt, mit dem Geräte eine Verbindung herstellen sollen. Der Standardwert lautet `global.azure-devices-provisioning.net`. |
    | `--t` oder `--TransportType`        | Falsch    | Der Transporttyp, der für die Kommunikation mit der Gerätebereitstellungsinstanz verwendet werden soll. Wird standardmäßig auf `Mqtt` festgelegt. Mögliche Werte: `Mqtt`, `Mqtt_WebSocket_Only`, `Mqtt_Tcp_Only`, `Amqp`, `Amqp_WebSocket_Only`, `Amqp_Tcp_only` und `Http1`|
     
4. Öffnen Sie im Ordner *SymmetricKeySample* die Datei *ProvisioningDeviceClientSample.cs* in einem Text-Editor. Diese Datei zeigt, wie die [SecurityProviderSymmetricKey](/dotnet/api/microsoft.azure.devices.shared.securityprovidersymmetrickey?view=azure-dotnet&preserve-view=true)-Klasse zusammen mit der [ProvisioningDeviceClient](/dotnet/api/microsoft.azure.devices.provisioning.client.provisioningdeviceclient?view=azure-dotnet&preserve-view=true)-Klasse verwendet wird, um Ihr Gerät mit symmetrischem Schlüssel bereitzustellen. Sehen Sie sich den Code in dieser Datei an.  Es sind keine Änderungen erforderlich.
 
5. Erstellen Sie den Beispielcode, und führen Sie ihn mit dem folgenden Befehl aus, nachdem Sie die drei Beispielparameter ersetzt haben. Verwenden Sie die richtigen Werte für den ID-Bereich, die Registrierungs-ID und den Primärschlüssel für die Registrierung.
    
    ```console
    dotnet run --s 0ne00000A0A --i symm-key-csharp-device-01 --p sbDDeEzRuEuGKag+kQKV+T1QGakRtHpsERLP0yPjwR93TrpEgEh/Y07CXstfha6dhIPWvdD1nRxK5T0KGKA+nQ==
    ```    


6. Die erwartete Ausgabe sollte in etwa der im folgenden Screenshot dargestellten Ausgabe entsprechen. Darin ist der verknüpfte IoT-Hub enthalten, dem das Gerät basierend auf den Einstellungen der individuellen Registrierung zugewiesen wurde. Die Beispielzeichenfolge „TestMessage“ wird als Test an den Hub gesendet:

    ```output
    D:\azure-iot-samples-csharp\provisioning\Samples\device\SymmetricKeySample>dotnet run --s 0ne00000A0A --i symm-key-csharp-device-01 --p sbDDeEzRuEuGKag+kQKV+T1QGakRtHpsERLP0yPjwR93TrpEgEh/Y07CXstfha6dhIPWvdD1nRxK5T0KGKA+nQ==

    Initializing the device provisioning client...
    Initialized for registration Id symm-key-csharp-device-01.
    Registering with the device provisioning service...
    Registration status: Assigned.
    Device csharp-device-01 registered to ExampleIoTHub.azure-devices.net.
    Creating symmetric key authentication for IoT Hub...
    Testing the provisioned device with IoT Hub...
    Sending a telemetry message...
    Finished.
    Enter any key to exit.
    ```
    
7. Navigieren Sie im Azure-Portal zu dem mit Ihrem Bereitstellungsdienst verknüpften IoT-Hub, und öffnen Sie das Blatt **IoT-Geräte**. Nach der erfolgreichen Bereitstellung des Geräts mit symmetrischen Schlüssel für den Hub wird die Geräte-ID mit dem *STATUS* **Aktiviert** angezeigt. Wenn Sie das Blatt bereits vor dem Ausführen des Gerätebeispielcodes geöffnet haben, müssen Sie möglicherweise im oberen Bereich auf die Schaltfläche **Aktualisieren** klicken. 

    ![Geräteregistrierung bei der IoT Hub-Instanz](./media/quick-create-device-symmetric-key-csharp/hub-registration-csharp.png) 

> [!NOTE]
> Wenn Sie den *anfänglichen Gerätezwillingsstatus* im Registrierungseintrag für Ihr Gerät gegenüber dem Standardwert geändert haben, kann der gewünschte Zwillingsstatus vom Hub abgerufen werden, und es können entsprechende Aktionen durchgeführt werden. Weitere Informationen finden Sie unter [Verstehen und Verwenden von Gerätezwillingen in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).
>


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie das Geräteclientbeispiel weiterhin verwenden und erkunden möchten, überspringen Sie die Bereinigung der in diesem Schnellstart erstellten Ressourcen. Falls Sie nicht fortfahren möchten, führen Sie die folgenden Schritte aus, um alle Ressourcen zu löschen, die im Rahmen dieses Schnellstarts erstellt wurden.

1. Wählen Sie im Azure-Portal im Menü links **Alle Ressourcen** und dann Ihren Gerätebereitstellungsdienst aus. Öffnen Sie **Registrierungen verwalten** für Ihren Dienst, und wählen Sie dann die Registerkarte **Individuelle Registrierungen** aus. Aktivieren Sie das Kontrollkästchen *REGISTRIERUNGS-ID* für das Gerät, das Sie in diesem Schnellstart registriert haben, und wählen Sie oben im Bereich die Schaltfläche **Löschen** aus. 
1. Wählen Sie im linken Menü im Azure-Portal **Alle Ressourcen** und dann Ihren IoT-Hub aus. Öffnen Sie **IoT-Geräte** für Ihren Hub, aktivieren Sie das Kontrollkästchen der *Geräte-ID* des Geräts, das Sie in diesem Schnellstart registriert haben, und wählen Sie dann die Schaltfläche **Löschen** am oberen Rand des Bereichs aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie mithilfe von IoT Hub Device Provisioning Service ein Windows-basiertes Gerät mit symmetrischem Schlüssel für Ihren IoT-Hub bereitgestellt. Wenn Sie erfahren möchten, wie Sie mithilfe von C# Geräte mit X.509-Zertifikat bereitstellen, lesen Sie die folgende Schnellstartanleitung für X.509-Geräte: 

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen und Bereitstellen eines simulierten X.509-Geräts mithilfe des C#-Geräte-SDKs für den IoT Hub Device Provisioning-Dienst](quick-create-simulated-device-x509-csharp.md)
