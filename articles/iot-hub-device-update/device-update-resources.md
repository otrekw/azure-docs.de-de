---
title: Grundlegendes zu Device Update for Azure IoT Hub-Ressourcen | Microsoft-Dokumentation
description: Grundlegendes zu Device Update for Azure IoT Hub-Ressourcen
author: vimeht
ms.author: vimeht
ms.date: 2/11/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: ba43889b885252f68bb3b4b158b5626411aac3d5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101678458"
---
# <a name="device-update-resources"></a>Device Update-Ressourcen

Um Device Update for IoT Hub verwenden zu können, müssen Sie ein Konto für die Geräteaktualisierung und eine Instanzressource erstellen. 

## <a name="device-update-account"></a>Device Update-Konto

Ein Device Update-Konto ist eine Ressource, die in Ihrem Azure-Abonnement erstellt wird. Auf der Device Update-Kontoebene können Sie die Region auswählen, in der das Device Update-Konto erstellt wird. Sie können auch Berechtigungen festlegen, um Benutzer zu autorisieren, die Zugriff auf Device Update erhalten sollen.


## <a name="device-update-instance"></a>Device Update-Instanz
Sie müssen eine Device Update-Instanz erstellen, nachdem ein Konto erstellt wurde. Eine Instanz ist ein logischer Container, der Updates und Bereitstellungen enthält, die einem bestimmten IoT Hub zugeordnet sind. Device Update verwendet IoT Hub als Geräteverzeichnis und einen Kommunikationskanal mit Geräten. 

Während der öffentlichen Vorschau können zwei Device Update-Konten pro Abonnement erstellt werden. Außerdem können pro Konto zwei Device Update-Instanzen erstellt werden.

## <a name="configuring-device-update-linked-iot-hub"></a>Konfigurieren von mit IoT Hub verknüpftem Device Update 

Damit Device Update Änderungsbenachrichtigungen von IoT Hub empfangen kann, wird Device Update in den „integrierten“ Event Hub integriert. Durch Klicken auf die Schaltfläche „IoT Hub konfigurieren“ in Ihrer Instanz werden die erforderlichen Nachrichtenrouten und die Zugriffsrichtlinie für die Kommunikation mit IoT-Geräten konfiguriert. 

Die folgenden Nachrichtenrouten werden für Device Update konfiguriert:

|   Routenname    | Routingabfrage  | BESCHREIBUNG  |
| :--------- | :---- |:---- |
|  DeviceUpdate.DigitalTwinChanges | true |Lauscht auf Änderungsereignisse von digitalen Zwillingen.  |
|  DeviceUpdate.DeviceLifeCycle | opType = 'deleteDeviceIdentity'  | Lauscht auf Geräte, die gelöscht wurden. |
|  DeviceUpdate.TelemetryModelInformation | iothub-interface-id = "urn:azureiot:ModelDiscovery:ModelInformation:1 | Lauscht auf neue Gerätetypen. |
|  DeviceUpdate.DeviceTwinEvents| (opType = 'updateTwin' OR opType = 'replaceTwin') AND IS_DEFINED($body.tags.ADUGroup) | Lauscht auf neue Device Update-Gruppen. |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen von Device Update-Ressourcen](./create-device-update-account.md)
