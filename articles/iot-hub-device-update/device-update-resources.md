---
title: Grundlegendes zu Device Update for Azure IoT Hub-Ressourcen | Microsoft-Dokumentation
description: Grundlegendes zu Device Update for Azure IoT Hub-Ressourcen
author: vimeht
ms.author: vimeht
ms.date: 2/11/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: e8194a269aec09f087632d2f6069d0624d7a835b
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111410655"
---
# <a name="device-update-resources"></a>Device Update-Ressourcen

Um Device Update for IoT Hub verwenden zu können, müssen Sie ein Konto für die Geräteaktualisierung und eine Instanzressource erstellen. 

## <a name="device-update-account"></a>Device Update-Konto

Ein Device Update-Konto ist eine Ressource, die in Ihrem Azure-Abonnement erstellt wird. Auf der Device Update-Kontoebene können Sie die Region auswählen, in der das Device Update-Konto erstellt wird. Sie können auch Berechtigungen festlegen, um Benutzer zu autorisieren, die Zugriff auf Device Update erhalten sollen.


## <a name="device-update-instance"></a>Device Update-Instanz
Sie müssen eine Device Update-Instanz erstellen, nachdem ein Konto erstellt wurde. Eine Instanz ist ein logischer Container, der Updates und Bereitstellungen enthält, die einem bestimmten IoT Hub zugeordnet sind. Device Update verwendet IoT Hub als Geräteverzeichnis und einen Kommunikationskanal mit Geräten. 

Während der öffentlichen Vorschau können zwei Device Update-Konten pro Abonnement erstellt werden. Außerdem können pro Konto zwei Device Update-Instanzen erstellt werden.

## <a name="configuring-device-update-linked-iot-hub"></a>Konfigurieren von mit IoT Hub verknüpftem Device Update 

Damit Device Update Änderungsbenachrichtigungen von IoT Hub empfangen kann, wird Device Update in den „integrierten“ Event Hub integriert. Durch Klicken auf die Schaltfläche „IoT Hub konfigurieren“ in Ihrer Instanz werden die erforderlichen Nachrichtenrouten, die Consumergruppen und die Zugriffsrichtlinie für die Kommunikation mit IoT-Geräten konfiguriert. 

### <a name="message-routing"></a>Nachrichtenrouting

Die folgenden Nachrichtenrouten werden für Device Update konfiguriert:

|   Routenname    | Datenquelle | Routingabfrage  | Endpunkt | BESCHREIBUNG  |
| :--------- | :---- |:---- |:---- |:---- |
|  DeviceUpdate.DigitalTwinChanges | DigitalTwinChangeEvents | true | events | Lauscht auf Änderungsereignisse von digitalen Zwillingen.  |
|  DeviceUpdate.DeviceLifecycle | DeviceLifecycleEvents | opType = 'deleteDeviceIdentity' OR opType = 'deleteModuleIdentity'  | events | Lauscht auf Geräte, die gelöscht wurden. |
|  DeviceUpdate.DeviceTwinEvents| TwinChangeEvents | (opType = 'updateTwin' OR opType = 'replaceTwin') AND IS_DEFINED($body.tags.ADUGroup) | events | Lauscht auf neue Device Update-Gruppen. |

> [!NOTE]
> Routennamen spielen bei der Konfiguration dieser Routen keine Rolle. DeviceUpdate wird als Präfix hinzugefügt, um die Namen konsistent und leicht identifizierbar zu machen, sodass sie für Device Update verwendet werden. Die restlichen Routeneigenschaften sollten so konfiguriert werden, wie sie in der folgenden Tabelle aufgeführt sind, damit Device Update ordnungsgemäß funktioniert. 

### <a name="consumer-group"></a>Consumergruppe

Beim Konfigurieren des IoT Hub wird auch eine Event Hub-Consumergruppe erstellt, die für die Device Update-Verwaltungsdienste erforderlich ist. 

:::image type="content" source="media/device-update-resources/consumer-group.png" alt-text="Screenshot: Consumergruppen" lightbox="media/device-update-resources/consumer-group.png":::

### <a name="access-policy"></a>Zugriffsrichtlinie

Eine Richtlinie für den gemeinsamen Zugriff (SAS-Richtlinie) mit dem Namen "deviceupdateservice" ist für die Device Update-Verwaltungsdienste erforderlich, um updatefähige Geräte abzufragen. Die Richtlinie "deviceupdateservice" wird erstellt und erhält im Rahmen der Konfiguration des IoT Hub die folgenden Berechtigungen:
- Lesevorgänge in Registrierung
- Dienstverbindung
- Geräteverbindung

:::image type="content" source="media/device-update-resources/access-policy.png" alt-text="Screenshot: Zugriffsrichtlinie" lightbox="media/device-update-resources/access-policy.png":::

## <a name="next-steps"></a>Nächste Schritte

[Erstellen von Device Update-Ressourcen](./create-device-update-account.md)
