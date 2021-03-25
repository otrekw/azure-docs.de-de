---
title: Programmgesteuertes Verwalten von IoT Central | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Ihre IoT Central-Instanz programmgesteuert erstellen und verwalten. Sie können die Anwendung mit SDKs für mehrere Sprachen wie JavaScript, Python, C#, Ruby und Go anzeigen, ändern und entfernen.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 12/23/2020
ms.topic: how-to
ms.openlocfilehash: 2feb4d85e7be4076eef8b2796d15b622f6b17c3a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104658300"
---
# <a name="manage-iot-central-programmatically"></a>Programmgesteuertes Verwalten von IoT Central

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

Statt IoT Central-Anwendungen auf der Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) zu erstellen und zu verwalten, können Sie Ihre Anwendungen auch programmgesteuert über Azure-SDKs verwalten. Zu den unterstützten Sprachen gehören JavaScript, Python, C#, Ruby und Go.

## <a name="install-the-sdk"></a>Installieren des SDKs

In der folgenden Tabelle sind die Repositorys und Paketinstallationsbefehle für die SDKs aufgeführt:

| SDK-Repository | Paketinstallation |
| -------------- | ------------ |
| [Azure IotCentralClient SDK for JavaScript](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/iotcentral/arm-iotcentral) | `npm install @azure/arm-iotcentral` |
| [Microsoft Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/iothub/azure-mgmt-iotcentral/azure/mgmt/iotcentral) | `pip install azure-mgmt-iotcentral` |
| [Azure SDK für .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/iotcentral/Microsoft.Azure.Management.IotCentral) | `dotnet add package Microsoft.Azure.Management.IotCentral` |
| [Microsoft Azure SDK for Ruby – Resource Management (Vorschauversion)](https://github.com/Azure/azure-sdk-for-ruby/tree/master/management/azure_mgmt_iot_central/lib/2018-09-01/generated/azure_mgmt_iot_central) | `gem install azure_mgmt_iot_central` |
| [Azure SDK für Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/iotcentral) | [Maven-Paket](https://search.maven.org/search?q=a:azure-mgmt-iotcentral) |
| [Azure SDK für Go](https://github.com/Azure/azure-sdk-for-go/tree/master/services/iotcentral/mgmt/2018-09-01/iotcentral) | [Paketreleases](https://github.com/Azure/azure-sdk-for-go/releases) |

## <a name="samples"></a>Beispiele

Das Repository [Azure IoT Central ARM SDK samples](/samples/azure-samples/azure-iot-central-arm-sdk-samples/azure-iot-central-arm-sdk-samples/) enthält Codebeispiele für mehrere Programmiersprachen, die zeigen, wie Sie Azure IoT Central-Anwendungen erstellen, aktualisieren, auflisten und löschen.

[!INCLUDE [Warning About Access Required](../../../includes/iot-central-warning-contribitorrequireaccess.md)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie Azure IoT Central-Anwendungen programmgesteuert verwalten, wird als nächster Schritt empfohlen, sich über den [Azure Resource Manager](../../azure-resource-manager/management/overview.md)-Dienst zu informieren.