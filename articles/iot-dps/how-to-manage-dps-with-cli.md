---
title: Verwalten des IoT Hub Device Provisioning Service über die Azure CLI und die IoT-Erweiterung
description: Hier erfahren Sie, wie Sie den IoT Hub Device Provisioning Service (DPS) über die Azure CLI und die IoT-Erweiterung verwalten können.
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: conceptual
ms.service: iot-dps
ms.custom: devx-track-azurecli
services: iot-dps
ms.openlocfilehash: dd0564fbb23a0695d849852fd464308cd1b5fac9
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98678943"
---
# <a name="how-to-use-azure-cli-and-the-iot-extension-to-manage-the-iot-hub-device-provisioning-service"></a>Verwenden von Azure CLI und der IoT-Erweiterung zum Verwalten des IoT Hub Device Provisioning Service

Die [Azure-Befehlszeilenschnittstelle](/cli/azure) ist ein plattformübergreifendes Open-Source-Befehlszeilentool zum Verwalten von Azure-Ressourcen wie IoT Edge. Die Azure-Befehlszeilenschnittstelle steht unter Windows, Linux und macOS zur Verfügung. Mit der Azure-Befehlszeilenschnittstelle können Sie Azure IoT Hub-Ressourcen, Instanzen von Device Provisioning Service und verknüpfte Hubs ohne weitere Tools verwalten.

Mit der IoT-Erweiterung wird die Azure-Befehlszeilenschnittstelle um Features wie die Geräteverwaltung und umfassende IoT Edge-Funktionen erweitert.

In diesem Tutorial führen Sie zuerst die Schritte zum Einrichten der Azure-Befehlszeilenschnittstelle und der IoT-Erweiterung aus. Anschließend wird beschrieben, wie Sie CLI-Befehle zum Durchführen von einfachen Vorgängen von Device Provisioning Service durchführen. 

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="prerequisites"></a>Voraussetzungen

- [Python 2.7x oder Python 3.x](https://www.python.org/downloads/) ist erforderlich.

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- Für diesen Artikel ist mindestens Version 2.0.70 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="basic-device-provisioning-service-operations"></a>Grundlegende Vorgänge des Device Provisioning Service

Im Beispiel wird veranschaulicht, wie Sie sich bei Ihrem Azure-Konto anmelden, eine Azure-Ressourcengruppe erstellen (einen Container, der die Ressourcen für eine Azure-Lösung enthält), einen IoT Hub und einen Device Provisioning-Dienst erstellen, die vorhandenen Device Provisioning-Dienste auflisten und einen verknüpften IoT Hub mit CLI-Befehlen erstellen. 

Führen Sie vor dem Fortfahren die oben beschriebenen Installationsschritte aus. Falls Sie noch kein Azure-Konto besitzen, können Sie sofort ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?v=17.39a). 


### <a name="1-log-in-to-the-azure-account"></a>1. Anmelden am Azure-Konto
  
```azurecli
az login
```

![Der Screenshot zeigt ein Eingabeaufforderungsfenster, in dem der Befehl „az login“ ausgeführt wird.](./media/how-to-manage-dps-with-cli/login.jpg)

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Erstellen der Ressourcengruppe „IoTHubBlogDemo“ für „USA, Osten“ (eastus)

```azurecli
az group create -l eastus -n IoTHubBlogDemo
```

![Ressourcengruppe erstellen](./media/how-to-manage-dps-with-cli/create-resource-group.jpg)


### <a name="3-create-two-device-provisioning-services"></a>3. Erstellen von zwei Device Provisioning-Diensten

```azurecli
az iot dps create --resource-group IoTHubBlogDemo --name demodps
```

![Erstellen des Device Provisioning Service](./media/how-to-manage-dps-with-cli/create-dps.jpg)

```azurecli
az iot dps create --resource-group IoTHubBlogDemo --name demodps2
```

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Auflisten aller vorhandenen Device Provisioning-Dienste unter dieser Ressourcengruppe

```azurecli
az iot dps list --resource-group IoTHubBlogDemo
```

![Auflisten der Device Provisioning-Dienste](./media/how-to-manage-dps-with-cli/list-dps.jpg)


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Erstellen des IoT Hub „blogDemoHub“ unter der neu erstellten Ressourcengruppe

```azurecli
az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo
```

![IoT Hub erstellen](./media/how-to-manage-dps-with-cli/create-hub.jpg)

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Verknüpfen eines vorhandenen IoT Hubs mit einem Device Provisioning-Dienst

```azurecli
az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus
```

![Hub verknüpfen](./media/how-to-manage-dps-with-cli/create-hub.jpg)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Registrieren des Geräts
> * Starten des Geräts
> * Sicherstellen, dass das Gerät registriert ist

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie mehrere Geräte für Hubs mit Lastausgleich bereitgestellt werden. 

> [!div class="nextstepaction"]
> [Bereitstellen von Geräten für IoT Hubs mit Lastausgleich](./tutorial-provision-multiple-hubs.md)