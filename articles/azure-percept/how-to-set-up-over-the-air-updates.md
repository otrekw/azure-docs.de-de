---
title: Einrichten von Azure IoT Hub für die Bereitstellung von OTA-Updates
description: Hie erfahren Sie, wie Sie Azure IoT Hub für die Bereitstellung von OTA-Updates (Over the Air; per Funk) für Azure Percept DK konfigurieren.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: 80f30ae818b9e74ff97d2bc26552ed7d7124b499
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660367"
---
# <a name="how-to-set-up-azure-iot-hub-to-deploy-over-the-air-updates-to-your-azure-percept-dk"></a>Einrichten von Azure IoT Hub für die Bereitstellung von OTA-Updates für Ihr Azure Percept DK-Gerät
Nutzen Sie Over-the-Air-Updates, um Ihr Azure Percept SK-Gerät zu schützen und auf dem neuesten Stand zu halten. Sie können Ihre Azure-Umgebung in wenigen einfachen Schritten mit Device Update für IoT Hub einrichten und die neuesten Updates für Ihr Azure Percept DK-Gerät bereitstellen.

## <a name="create-a-device-update-account"></a>Erstellen eines Device Update-Kontos

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit dem Azure-Konto an, das Sie für Azure Percept verwenden. 

1. Beginnen Sie im Suchfenster am oberen Rand der Seite mit der Eingabe von „Device Update für IoT Hub“.

1. Wählen Sie **Device Update für IoT Hub** aus, wenn diese Option im Suchfenster angezeigt wird.

1. Klicken Sie links oben auf der Seite auf die Schaltfläche **+ Hinzufügen**.

1. Wählen Sie das Azure-Abonnement und die Ressourcengruppe aus, die Ihrem Azure Percept-Gerät zugeordnet sind. (Dort befindet sich die IoT Hub-Instanz aus dem Onboarding.)

1. Geben Sie einen Namen und einen Speicherort für Ihr Device Update-Konto an.

1. Überprüfen Sie die Angaben, und wählen Sie anschließend **Überprüfen und erstellen** aus.
 
1. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
 
## <a name="create-a-device-update-instance"></a>Erstellen einer Device Update-Instanz
Erstellen Sie nun innerhalb Ihres Kontos für „Device Update für IoT Hub“ eine Instanz.

1. Klicken Sie in Ihrer Ressource vom Typ „Device Update für IoT Hub“ unter **Instanzverwaltung** auf **Instanzen**.
 
1. Klicken Sie auf **+ Erstellen**, geben Sie einen Instanznamen an, und wählen Sie die IoT Hub-Instanz aus, die Ihrem Azure Percept-Gerät zugeordnet ist (also die Instanz, die im Rahmen des Onboardings erstellt wurde). Dies kann einige Minuten in Anspruch nehmen.
 
1. Klicken Sie auf **Erstellen**

## <a name="configure-iot-hub"></a>Konfigurieren von IoT Hub

1. Warten Sie in der Instanzverwaltung auf der Seite **Instanzen**, bis die Device Update-Instanz den Status **Erfolgreich** hat. Klicken Sie neben **Löschen** auf das Symbol **Aktualisieren**, um den Status zu aktualisieren.
 
1. Wählen Sie die für Sie erstellte Instanz aus, und klicken Sie anschließend auf **IoT-Hub konfigurieren**. Wählen Sie im linken Bereich **Ich bin mit der Durchführung dieser Änderungen einverstanden.** aus, und klicken Sie auf **Aktualisieren**.
 
1. Warten Sie, bis der Prozess erfolgreich beendet wurde.
 
## <a name="configure-access-control-roles"></a>Konfigurieren von Zugriffssteuerungsrollen
Der letzte Schritt ermöglicht es Ihnen, Benutzern Berechtigungen zum Veröffentlichen und Bereitstellen von Updates zu erteilen.

1. Klicken Sie in Ihrer Ressource vom Typ „Device Update für IoT Hub“ auf **Zugriffssteuerung (IAM)** .
 
2. Klicken Sie auf **+Hinzufügen**, und wählen Sie anschließend **Rollenzuweisung hinzufügen** aus.
 
3. Wählen Sie unter **Rolle** die Option **Administrator für Geräteupdates** aus. Wählen Sie unter **Zugriff zuweisen zu** die Option **Benutzer, Gruppe oder Dienstprinzipal** aus. Wählen Sie unter **Auswählen** Ihr Konto oder das Konto der Person aus, von der Updates bereitgestellt werden. Klicken Sie dann auf **Speichern**. 

    > [!TIP]
    > Wenn Sie mehreren Personen in Ihrer Organisation Zugriff gewähren möchten, können Sie diesen Schritt wiederholen und jeden der gewünschten Benutzer als **Administrator für Geräteupdates** festlegen.

## <a name="next-steps"></a>Nächste Schritte

Nun ist alles eingerichtet, und Sie können mit Device Update für IoT Hub [Over-the-Air-Updates für Ihr Azure Percept Development Kit durchführen](./how-to-update-over-the-air.md). Navigieren Sie zu der Azure IoT Hub-Instanz, die Sie für Ihr Azure Percept-Gerät verwenden.