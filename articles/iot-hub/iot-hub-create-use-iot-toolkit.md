---
title: Erstellen einer Azure IoT Hub-Instanz mit den Azure IoT-Tools für VS Code | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mit den Azure IoT-Tools für Visual Studio Code eine Azure IoT Hub-Instanz in einer Ressourcengruppe erstellen.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: junhan
ms.openlocfilehash: e8d6bebffc46b0b158aeb975ae1e32a863b956d2
ms.sourcegitcommit: 3de22db010c5efa9e11cffd44a3715723c36696a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109655850"
---
# <a name="create-an-iot-hub-using-the-azure-iot-tools-for-visual-studio-code"></a>Erstellen einer IoT Hub-Instanz mithilfe der Azure IoT-Tools für Visual Studio Code

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

In diesem Artikel erfahren Sie, wie Sie die [Azure IoT-Tools für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) zum Erstellen einer Azure IoT Hub-Instanz verwenden. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- [Visual Studio Code](https://code.visualstudio.com/)

- [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) für Visual Studio Code


## <a name="create-an-iot-hub-and-device-in-an-iot-project"></a>Erstellen einer IoT Hub-Instanz und eines Geräts in einem IoT-Projekt

Die folgenden Schritte zeigen, wie Sie eine IoT Hub-Instanz erstellen und ein Gerät beim Hub in einem IoT-Projekt in Visual Studio Code registrieren.

Anstatt Azure IoT Hub und das Gerät über das Azure-Portal bereitzustellen, können Sie dies auch in Visual Studio Code erledigen, ohne die Entwicklungsumgebung verlassen zu müssen. Führen Sie dazu die in diesem Abschnitt beschriebenen Schritte aus.

1. Klicken Sie im neu geöffneten Projektfenster auf `F1`, um die Befehlspalette zu öffnen, geben Sie **Azure IoT Workbench: Provision Azure Services...** (Azure IoT Workbench: Azure-Dienste bereitstellen) ein, und wählen Sie den Eintrag aus. Befolgen Sie die Anleitung, um die Bereitstellung Ihrer Azure IoT Hub-Instanz abzuschließen und das IoT Hub-Gerät zu erstellen.

    ![Bereitstellungsbefehl](media/iot-hub-create-use-iot-toolkit/provision.png)

    > [!NOTE]
    > Wenn Sie sich nicht bei Azure angemeldet sind. Befolgen Sie die Popupbenachrichtigung zur Anmeldung.

1. Wählen Sie das Abonnement aus, das Sie verwenden möchten.

    ![Abonnement auswählen](media/iot-hub-create-use-iot-toolkit/select-subscription.png)

1. Wählen Sie dann eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue [Ressourcengruppe](../azure-resource-manager/management/overview.md#terminology).

    ![Ressourcengruppe auswählen](media/iot-hub-create-use-iot-toolkit/select-resource-group.png)

1. Folgen Sie in der von Ihnen angegebenen Ressourcengruppe den Eingabeaufforderungen, um eine vorhandene IoT Hub-Instanz auszuwählen oder eine neue Azure IoT Hub-Instanz zu erstellen.

    ![IoT Hub-Schritte auswählen](media/iot-hub-create-use-iot-toolkit/iot-hub-provision.png)

    ![IoT Hub auswählen](media/iot-hub-create-use-iot-toolkit/select-iot-hub.png)

    ![Ausgewählter IoT Hub](media/iot-hub-create-use-iot-toolkit/iot-hub-selected.png)

1. Im Ausgabefenster wird der bereitgestellte Azure IoT Hub angezeigt.

    ![Bereitgestellter IoT Hub](media/iot-hub-create-use-iot-toolkit/iot-hub-provisioned.png)

1. Wählen Sie ein IoT Hub-Gerät in der von Ihnen bereitgestellten Azure IoT Hub-Instanz aus, oder erstellen Sie ein neues.

    ![IoT-Geräteschritte auswählen](media/iot-hub-create-use-iot-toolkit/iot-device-provision.png)

    ![Bereitgestelltes IoT-Gerät auswählen](media/iot-hub-create-use-iot-toolkit/select-iot-device.png)

1. Jetzt haben Sie Azure IoT Hub bereitgestellt und das Gerät darin erstellt. Außerdem wird die Geräteverbindungszeichenfolge in Visual Studio Code gespeichert.

    ![Bereitstellung abgeschlossen](media/iot-hub-create-use-iot-toolkit/provision-done.png)



## <a name="create-an-iot-hub-without-an-iot-project"></a>Erstellen einer IoT Hub-Instanz ohne IoT-Projekt

In den folgenden Schritten wird gezeigt, wie Sie eine IoT Hub-Instanz ohne IoT-Projekt in Visual Studio Code erstellen.

1. Öffnen Sie in Visual Studio Code die **Explorer**-Ansicht.

2. Erweitern Sie im unteren Bereich des Explorers den Abschnitt **Azure IoT Hub**. 

   ![Erweitern von „Azure IoT Hub Devices“ (Azure IoT Hub-Geräte)](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Klicken Sie auf **...** in der Kopfzeile des Abschnitts **Azure IoT Hub**. Wenn die Ellipse nicht angezeigt wird, zeigen Sie auf den Header. 

4. Klicken Sie auf **IoT Hub erstellen**.

5. In der unteren rechten Ecke wird ein Popupfenster angezeigt, über das Sie sich zum ersten Mal bei Azure anmelden können.

6. Wählen Sie ein Azure-Abonnement aus. 

7. Wählen Sie eine Ressourcengruppe aus.

8. Wählen Sie einen Standort aus.

9. Wählen Sie einen Tarif aus.

10. Geben Sie einen global eindeutigen Namen für Ihre IoT Hub-Instanz ein.

11. Warten Sie einige Minuten, bis die IoT Hub-Instanz erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun eine IoT Hub-Instanz mithilfe der Azure IoT-Tools für Visual Studio Code bereitgestellt. Schauen Sie sich für weiterführende Informationen die folgenden Artikel an:

* [Senden und Empfangen von Nachrichten zwischen Ihrem Gerät und IoT Hub mithilfe der Azure IoT-Tools für Visual Studio Code](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

* [Verwenden der Azure IoT-Tools für Visual Studio Code für die Geräteverwaltung mit Azure IoT Hub](iot-hub-device-management-iot-toolkit.md)

* [Weitere Informationen finden Sie auf der Wiki-Seite zu Azure IoT Hub für VS Code](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki).
