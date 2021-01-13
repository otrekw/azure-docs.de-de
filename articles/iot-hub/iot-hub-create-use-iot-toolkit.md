---
title: Erstellen einer Azure IoT Hub-Instanz mit den Azure IoT-Tools für VS Code | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mit den Azure IoT-Tools für Visual Studio Code eine Azure IoT Hub-Instanz in einer Ressourcengruppe erstellen.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: junhan
ms.openlocfilehash: 668087ae596688e86b7b84a16bc5c0fd0f9fcef6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "75912243"
---
# <a name="create-an-iot-hub-using-the-azure-iot-tools-for-visual-studio-code"></a>Erstellen einer IoT Hub-Instanz mithilfe der Azure IoT-Tools für Visual Studio Code

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

In diesem Artikel erfahren Sie, wie Sie die [Azure IoT-Tools für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) zum Erstellen einer Azure IoT Hub-Instanz verwenden. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

- ein Azure-Abonnement Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.

- [Visual Studio Code](https://code.visualstudio.com/)

- [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) für Visual Studio Code.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

1. Öffnen Sie in Visual Studio Code die **Explorer**-Ansicht.

2. Erweitern Sie im unteren Bereich des Explorers den Abschnitt **Azure IoT Hub Devices** (Azure IoT Hub-Geräte). 

   ![Erweitern von „Azure IoT Hub Devices“ (Azure IoT Hub-Geräte)](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Klicken Sie auf **...** in der Überschrift des Abschnitts **Azure IoT Hub Devices** (Azure IoT Hub-Geräte). Wenn die Ellipse nicht angezeigt wird, zeigen Sie auf den Header. 

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
