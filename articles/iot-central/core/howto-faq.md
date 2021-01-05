---
title: Azure IoT Central – häufig gestellte Fragen | Microsoft-Dokumentation
description: Häufig gestellte Fragen (FAQ) zu Azure IoT Central und die zugehörigen Antworten
author: dominicbetts
ms.author: dobett
ms.date: 12/20/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: acabaf843f8acfe7bc0b5e9456dee09bde74bef7
ms.sourcegitcommit: ab829133ee7f024f9364cd731e9b14edbe96b496
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2020
ms.locfileid: "97796039"
---
# <a name="frequently-asked-questions-for-iot-central"></a>Häufig gestellte Fragen zu IoT Central

**Wie überprüfe ich Probleme mit Anmeldeinformationen, wenn ein Gerät keine Verbindung mit meiner IoT Central-Anwendung herstellen kann?**

Das Thema [Behandeln von Problemen, durch die Daten von Ihren Geräten nicht in Azure IoT Central angezeigt werden](troubleshoot-connection.md) enthält Schritte zur Diagnose von Verbindungsproblemen bei Geräten.

**Wie beantrage ich ein Ticket beim Kundensupport?**

Wenn Sie Hilfe benötigen, können Sie ein [Azure-Supportticket](https://portal.azure.com/#create/Microsoft.Support) erstellen.

Weitere Informationen auch zu anderen Supportoptionen finden Sie unter [Support- und Hilfeoptionen für Azure IoT](../../iot-fundamentals/iot-support-help.md).

**Wie entsperre ich ein Gerät?**

Wenn ein Gerät blockiert wird, kann es keine Daten an Ihre IoT Central-Anwendung senden. Blockierte Geräte weisen auf der Seite **Geräte** in Ihrer Anwendung den Status **Blockiert** auf. Ein Operator muss das Gerät entsperren, damit es wieder Daten senden kann:

:::image type="content" source="media/howto-faq/blocked.png" alt-text="Screenshot eines blockierten Geräts":::

Wenn ein Operator die Blockierung eines Geräts aufhebt, wird der Status auf den vorherigen Wert (**Registriert** oder **Bereitgestellt**) zurückgesetzt.

**Wie genehmige ich ein Gerät?**

Wenn auf der Seite **Geräte** der Gerätestatus **Warten auf Genehmigung** angezeigt wird, ist die Option **Automatisch genehmigen** deaktiviert:

:::image type="content" source="media/howto-faq/auto-approve.png" alt-text="Screenshot einer Anwendung mit deaktivierter Option „Automatisch genehmigen“":::

Ein Operator muss ein Gerät explizit genehmigen, bevor es mit dem Senden von Daten beginnt. Geräte, die auf der Seite **Geräte** nicht manuell registriert wurden, aber eine Verbindung mit gültigen Anmeldeinformationen hergestellt haben, weisen den Gerätestatus **Warten auf Genehmigung** auf. Diese Geräte können auf der Seite **Geräte** mithilfe der Schaltfläche **Genehmigen** von Operatoren genehmigt werden:

:::image type="content" source="media/howto-faq/approve-device.png" alt-text="Screenshot der Genehmigung eines Geräts":::

**Wie ordne ich einem Gerät eine Gerätevorlage zu?**

Wenn der Gerätestatus **Nicht zugeordnet** lautet, ist den Geräten, die eine Verbindung mit IoT Central herstellen, keine Gerätevorlage zugeordnet. Diese Situation ist in den folgenden Szenarien typisch:

- Eine Gruppe von Geräten wurde auf der Seite **Geräte** mithilfe der Option **Importieren** hinzugefügt, ohne die Gerätevorlage anzugeben.
- Ein Geräte wurde auf der Seite **Geräte** manuell hinzugefügt, ohne die Gerätevorlage anzugeben. Das Gerät stellte dann eine Verbindung mit gültigen Anmeldeinformationen her.  

Der Operator kann auf der Seite **Geräte** mithilfe der Schaltfläche **Migrieren** einem Gerät eine Gerätevorlage zuordnen. Weitere Informationen finden Sie unter [Verwalten von Geräten in Ihrer Azure IoT Central-Anwendung > Migrieren von Geräten zu einer Vorlage](howto-manage-devices.md).

**Wo kann ich mehr über IoT Hub erfahren?**

Azure IoT Central verwendet Azure IoT Hub als Cloudgateway, das Gerätekonnektivität ermöglicht. IoT Hub ermöglicht Folgendes:

- Datenerfassung im großen Maßstab in der Cloud
- Geräteverwaltung
- Sichere Gerätekonnektivität

Weitere Informationen zu IoT Hub finden Sie unter [Dokumentation zu IoT Hub](../../iot-hub/index.yml).

**Wo kann ich mehr über den Device Provisioning Service (DPS) erfahren?**

Azure IoT Central verwendet DPS, um Geräten das Herstellen einer Verbindung mit Ihrer Anwendung zu ermöglichen. Weitere Informationen zur Rolle von DPS beim Verbinden von Geräten mit IoT Central finden Sie unter [Herstellen einer Verbindung mit Azure IoT Central](concepts-get-connected.md). Weitere Informationen zu DPS finden Sie unter [Bereitstellen von Geräten mit dem Azure IoT Hub Device Provisioning Service](../../iot-dps/about-iot-dps.md).