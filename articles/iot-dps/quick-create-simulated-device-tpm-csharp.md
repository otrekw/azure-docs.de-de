---
title: 'Schnellstart: Bereitstellen eines simulierten TPM-Geräts für Azure IoT Hub mithilfe von C#'
description: 'Schnellstart: Erstellen und Bereitstellen eines simulierten TPM-Geräts mithilfe des C#-Geräte-SDK für Azure IoT Hub Device Provisioning Service (DPS) In dieser Schnellstartanleitung werden individuelle Registrierungen verwendet.'
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: 82bd284ede23e8880f79c614f4a6e2f588a4293c
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74976994"
---
# <a name="quickstart-create-and-provision-a-simulated-tpm-device-using-c-device-sdk-for-iot-hub-device-provisioning-service"></a>Schnellstart: Erstellen und Bereitstellen eines simulierten TPM-Geräts mithilfe des C#-Geräte-SDKs für den IoT Hub Device Provisioning-Dienst

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-tpm](../../includes/iot-dps-selector-quick-create-simulated-device-tpm.md)]

Diese Schritte zeigen Ihnen, wie Sie mit den [Azure IoT Samples for C# (Azure IoT-Codebeispiele für C#)](https://github.com/Azure-Samples/azure-iot-samples-csharp) ein TPM-Gerät auf einem Entwicklungscomputer mit Windows-Betriebssystem simulieren. Das Beispiel stellt auch eine Verbindung des simulierten Geräts mit einem IoT Hub über den Device Provisioning Service her. 

In dem Beispielcode wird der Windows-TPM-Simulator als [Hardwaresicherheitsmodul (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) des Geräts verwendet. 

Sollten Sie mit der automatischen Bereitstellung nicht vertraut sein, lesen Sie auch die Informationen unter [Konzepte für die automatische Bereitstellung](concepts-auto-provisioning.md). Vergewissern Sie sich außerdem, dass Sie die Schritte unter [Einrichten des IoT Hub Device Provisioning-Diensts über das Azure-Portal](./quick-setup-auto-provision.md) ausgeführt haben, bevor Sie fortfahren. 

In Azure IoT Device Provisioning Service werden zwei Registrierungsarten unterstützt:

- [Registrierungsgruppen:](concepts-service.md#enrollment-group) Werden zum Registrieren mehrerer verwandter Geräte verwendet.
- [Individuelle Registrierungen](concepts-service.md#individual-enrollment): Werden zum Registrieren eines einzelnen Geräts verwendet.

In diesem Artikel werden individuelle Registrierungen veranschaulicht.

[!INCLUDE [IoT Device Provisioning Service basic](../../includes/iot-dps-basic.md)]

<a id="setupdevbox"></a>
## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung 

1. Vergewissern Sie sich, dass auf Ihrem Computer das [.NET Core 2.1 SDK oder höher](https://www.microsoft.com/net/download/windows) installiert ist. 

1. Vergewissern Sie sich, dass `git` auf Ihrem Computer installiert ist und den Umgebungsvariablen hinzugefügt wurde, auf die das Befehlsfenster Zugriff hat. Unter [Git-Clienttools von Software Freedom Conservancy](https://git-scm.com/download/) finden Sie die neueste Version der zu installierenden `git`-Tools. Hierzu zählt auch die Befehlszeilen-App **Git Bash**, über die Sie mit Ihrem lokalen Git-Repository interagieren können. 

1. Öffnen Sie eine Eingabeaufforderung oder Git Bash. Klonen Sie die Azure IoT-Codebeispiele für das C#-GitHub-Repository:

    ```cmd
    git clone https://github.com/Azure-Samples/azure-iot-samples-csharp.git
    ```

## <a name="provision-the-simulated-device"></a>Bereitstellen des simulierten Geräts

1. Melden Sie sich beim Azure-Portal an. Wählen Sie im Menü links die Schaltfläche **Alle Ressourcen** aus, und öffnen Sie Ihren Gerätebereitstellungsdienst. Notieren Sie auf dem Blatt **Übersicht** den Wert **_ID-Bereich_** .

    ![Kopieren des ID-Bereichs für den Bereitstellungsdienst aus dem Portalblatt](./media/quick-create-simulated-device-tpm-csharp/copy-scope.png) 

1. Wechseln Sie an einer Eingabeaufforderung zum Projektverzeichnis für das TPM-Gerätebereitstellungsbeispiel.

    ```cmd
    cd .\azure-iot-samples-csharp\provisioning\Samples\device\TpmSample
    ```

1. Geben Sie den folgenden Befehl ein, um das TPM-Gerätebereitstellungsbeispiel zu erstellen und auszuführen. Ersetzen Sie den Wert `<IDScope>` durch den ID-Bereich für Ihren Bereitstellungsdienst. 

    ```cmd
    dotnet run <IDScope>
    ```

    Mit diesem Befehl wird der TPM-Chipsimulator in einer separaten Eingabeaufforderung gestartet. Unter Windows wird möglicherweise eine Windows-Sicherheitswarnung angezeigt, in der Sie gefragt werden, ob Sie die Kommunikation von „Simulator.exe“ in öffentlichen Netzwerken zulassen möchten. Für dieses Beispiel können Sie die Anforderung abbrechen.

1. Im ursprünglichen Befehlsfenster werden der **_Endorsement Key_** , die **_Registrierungs-ID_** und eine vorgeschlagene **_Geräte-ID_** für die Geräteregistrierung angezeigt. Notieren Sie sich diese Werte. Sie werden mit diesem Wert eine individuelle Registrierung in Ihrer Device Provisioning Service-Instanz erstellen. 
   > [!NOTE]
   > Verwechseln das Fenster mit der Befehlsausgabe nicht mit dem Fenster, das die Ausgabe des TPM-Simulators enthält. Möglicherweise müssen Sie das ursprüngliche Befehlsfenster auswählen, um es im Vordergrund anzuzeigen.

    ![Befehlsfensterausgabe](./media/quick-create-simulated-device-tpm-csharp/output1.png) 

1. Wählen Sie im Azure-Portal im Menü von Device Provisioning Service die Option **Registrierungen verwalten** aus. Wählen Sie die Registerkarte **Individuelle Registrierungen** und dann oben die Schaltfläche **Individuelle Registrierung hinzufügen** aus. 

1. Geben Sie im Bereich **Registrierung hinzufügen** die folgenden Informationen ein:
   - Wählen Sie **TPM** als *Mechanismus* für den Nachweis der Identität.
   - Geben Sie die zuvor notierte *Registrierungs-ID* und den *Endorsement Key* für Ihr TPM-Gerät entsprechend den zuvor notierten Werten ein.
   - Wählen Sie einen IoT Hub aus, der mit Ihrem Bereitstellungsdienst verknüpft ist.
   - Optional können Sie die folgenden Informationen angeben:
       - Geben Sie eine eindeutige *Geräte-ID* ein. (Sie können den Vorschlag verwenden oder eine eigene angeben.) Achten Sie darauf, dass Sie beim Benennen Ihres Geräts keine sensiblen Daten angeben. Wenn Sie keine Angabe machen, wird das Gerät stattdessen über die Registrierungs-ID identifiziert.
       - Aktualisieren Sie **Initial device twin state** (Anfänglicher Gerätezwillingsstatus) mit der gewünschten Anfangskonfiguration für das Gerät.
   - Klicken Sie abschließend auf die Schaltfläche **Speichern**. 

     ![Eingeben von Geräteregistrierungsinformationen auf dem Portalblatt](./media/quick-create-simulated-device-tpm-csharp/enterdevice-enrollment.png)  

   Nach erfolgreicher Registrierung wird die *Registrierungs-ID* Ihres Geräts in der Liste auf der Registerkarte *Individual Enrollments* (Individuelle Registrierungen) angezeigt. 

1. Drücken Sie im Befehlsfenster (in dem der **_Endorsement Key_** , die **_Registrierungs-ID_** und eine vorgeschlagene **_Geräte-ID_** angezeigt wurden) die *EINGABETASTE*, um das simulierte Gerät zu registrieren. Beachten Sie die Nachrichten, die den Start und die Verbindungsherstellung des Geräts mit dem Device Provisioning-Dienst simulieren, um Ihre IoT Hub-Informationen abzurufen. 

1. Vergewissern Sie sich, dass das Gerät bereitgestellt wurde. Nachdem das simulierte Gerät erfolgreich für die mit Ihrem Bereitstellungsdienst verknüpfte IoT Hub-Instanz bereitgestellt wurde, wird die Geräte-ID auf dem Blatt **IoT-Geräte** des Hubs angezeigt. 

    ![Geräteregistrierung bei der IoT Hub-Instanz](./media/quick-create-simulated-device-tpm-csharp/hub_registration.png) 

    Wenn Sie den *anfänglichen Gerätezwillingsstatus* im Registrierungseintrag für Ihr Gerät gegenüber dem Standardwert geändert haben, kann der gewünschte Zwillingsstatus vom Hub abgerufen werden, und es können entsprechende Aktionen durchgeführt werden. Weitere Informationen finden Sie unter [Verstehen und Verwenden von Gerätezwillingen in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie das Geräteclientbeispiel weiterhin verwenden und erkunden möchten, überspringen Sie die Bereinigung der in diesem Schnellstart erstellten Ressourcen. Falls Sie nicht fortfahren möchten, führen Sie die folgenden Schritte aus, um alle Ressourcen zu löschen, die im Rahmen dieses Schnellstarts erstellt wurden.

1. Schließen Sie auf Ihrem Computer das Ausgabefenster des Geräteclientbeispiels.
1. Schließen Sie auf Ihrem Computer das TPM-Simulatorfenster.
1. Wählen Sie im Azure-Portal im Menü links **Alle Ressourcen** und dann Ihren Gerätebereitstellungsdienst aus. Klicken Sie auf dem Blatt **Übersicht** am oberen Rand des Bereichs auf **Löschen**.  
1. Wählen Sie im linken Menü im Azure-Portal **Alle Ressourcen** und dann Ihren IoT-Hub aus. Klicken Sie auf dem Blatt **Übersicht** am oberen Rand des Bereichs auf **Löschen**.  

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie auf Ihrem Computer ein simuliertes TPM-Gerät erstellt und mithilfe von IoT Hub Device Provisioning Service für Ihren IoT-Hub bereitgestellt. Informationen zum programmgesteuerten Registrieren Ihres TPM-Geräts finden Sie im Schnellstart für die programmgesteuerte Registrierung von TPM-Geräten. 

> [!div class="nextstepaction"]
> [Azure-Schnellstart: Registrieren eines TPM-Geräts bei Azure IoT Hub Device Provisioning Service](quick-enroll-device-tpm-csharp.md)
