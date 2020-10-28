---
title: Anzeigen und Verwalten von integrierten Sensoren in Azure Defender für IoT
description: In diesem Artikel wird beschrieben, wie Sie integrierte Sensoren anzeigen und löschen sowie neue Aktivierungsdateien für integrierte Sensoren in Azure Defender für IoT herunterladen können.
author: rkarlin
ms.author: rkarlin
ms.date: 10/10/2020
ms.topic: article
ms.service: azure
ms.openlocfilehash: 6aec19d413e1730bc1599f6cbac1c62e9b304ecd
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92094151"
---
# <a name="view-and-manage-onboarded-sensors"></a>Anzeigen und Verwalten von integrierten Sensoren

In diesem Artikel wird beschrieben, wie Sie integrierte Sensoren anzeigen und löschen sowie neue Aktivierungsdateien für integrierte Sensoren herunterladen können.

## <a name="update-sensor-management-mode"></a>Aktualisieren des Sensorverwaltungsmodus

Möglicherweise möchten Sie den Modus aktualisieren, in dem Ihr Sensor verwaltet wird. In diesem Fall müssen Sie den aktuellen Sensor von der Seite zur Sensorverwaltung entfernen und eine neue Aktivierungsdatei hochladen.

- **Arbeiten im über die Cloud verwalteten Modus statt im lokal verwalteten Modus** : Aktualisieren Sie die Aktivierungsdatei für Ihren lokal verwalteten Sensor mit einer Aktivierungsdatei für einen über die Cloud verwalteten Sensor. Nach der Reaktivierung werden Sensorerkennungen sowohl im Sensor als auch im Azure Defender für IoT-Portal angezeigt. Nachdem die Reaktivierungsdatei erfolgreich hochgeladen wurde, werden neu erkannte Warnungsinformationen an Azure gesendet.

- **Arbeiten im lokal verwalteten Modus statt im über die Cloud verwalteten Modus** : Aktualisieren Sie die Aktivierungsdatei für einen über die Cloud verwalteten Sensor mit einer Aktivierungsdatei für einen lokal verwalteten Sensor. Nach der Reaktivierung werden Informationen zur Sensorerkennung nur im Sensor angezeigt.

- **Zuordnen des Sensors zu einem neuen IoT Hub** : Möglicherweise möchten Sie Ihren Sensor einem neuen IoT Hub zuordnen. Registrieren Sie dazu den Sensor erneut, und laden Sie eine neue Aktivierungsdatei hoch.

**So reaktivieren Sie den Sensor:**

1. Navigieren Sie zur Azure Defender für IoT-Seite **Sensor Management** (Sensorverwaltung).

2. Wählen Sie den Sensor aus, für den Sie eine neue Aktivierungsdatei hochladen möchten.

3. Löschen Sie es.

4. Integrieren Sie den Sensor wieder über die Seite **Onboarding** im neuen Modus oder in einen neuen IoT Hub.

5. Laden Sie die Aktivierungsdatei von der Seite **Download activation file** (Aktivierungsdatei herunterladen) herunter.

6. Melden Sie sich bei der Sensorkonsole von Azure Defender für IoT an.

7. Wählen Sie in der Sensorkonsole **Systemeinstellungen** und dann **Reaktivierung** aus.

   ![Screenshot der Ansicht „Reaktivierung“](media/updates/image14.png)

8. Wählen Sie **Hochladen** und dann die gespeicherte Datei aus.

9. Wählen Sie **Aktivieren** aus.
 
## <a name="delete-sensors"></a>Löschen von Sensoren

Wenn Sie einen von der Cloud verwalteten Sensor löschen, werden keine Informationen an den IoT Hub gesendet.

Löschen Sie lokal verwaltete Sensoren, wenn Sie nicht mehr damit arbeiten.

**So löschen Sie Sensoren:**

1. Navigieren Sie zur Azure Defender für IoT-Seite **Sensor Management** (Sensorverwaltung).

2. Wählen Sie die Sensoren aus, die Sie löschen möchten.

3. Wählen Sie **Sensor löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Konfigurationsoptionen finden Sie in der Schrittanleitung für die Modulkonfiguration.
> [!div class="nextstepaction"]
> [Schrittanleitung für die Modulkonfiguration](./how-to-agent-configuration.md)
