---
title: Erstellen eines Geräteupdatekontos in Device Update for Azure IoT Hub | Microsoft-Dokumentation
description: Erstellen Sie ein Geräteupdatekonto in Device Update for Azure IoT Hub.
author: vimeht
ms.author: vimeht
ms.date: 2/11/2021
ms.topic: how-to
ms.service: iot-hub-device-update
ms.openlocfilehash: a0201882b74d2acf57c246d2dc63530ca8b1f010
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101692605"
---
# <a name="device-update-for-iot-hub-resource-management"></a>Device Update for IoT Hub: Ressourcenverwaltung

Für die ersten Schritte mit Device Update müssen Sie ein Device Update-Konto und eine Instanz erstellen sowie Rollen für die Zugriffssteuerung festlegen. 

## <a name="prerequisites"></a>Voraussetzungen

* Zugriff auf einen IoT Hub. Es wird empfohlen, einen S1-Tarif (Standard) oder höher zu verwenden. 
* Unterstützte Browser:
  * [Microsoft Edge](https://www.microsoft.com/edge)
  * Google Chrome

## <a name="create-a-device-update-account"></a>Erstellen eines Geräteupdatekontos

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).

2. Klicken Sie auf „Ressource erstellen“, und suchen Sie nach „Device Update for IoT Hub“.

   :::image type="content" source="media/create-device-update-account/device-update-marketplace.png" alt-text="Screenshot der Device Update for IoT Hub-Ressource" lightbox="media/create-device-update-account/device-update-marketplace.png":::

3. Klicken Sie auf „Erstellen -> Device Update for IoT Hub“.

4. Geben Sie das Azure-Abonnement an, das Ihrem Device Update-Konto und der Ressourcengruppe zugeordnet werden soll.

5. Geben Sie einen Namen und einen Speicherort für Ihr Device Update-Konto an.

   :::image type="content" source="media/create-device-update-account/account-details.png" alt-text="Screenshot der Kontodetails" lightbox="media/create-device-update-account/account-details.png":::

 > [!NOTE]
 > Sie können zur Seite [Azure-Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=iot-hub) wechseln, um die Regionen zu ermitteln, in denen Device Update for IoT Hub verfügbar ist. Wenn Device Update for IoT Hub in Ihrer Region nicht verfügbar ist, können Sie ein Konto in einer verfügbaren Region erstellen, die in Ihrer Nähe liegt. 

6. Klicken Sie auf „Weiter: Überprüfen + erstellen>“.

   :::image type="content" source="media/create-device-update-account/account-review.png" alt-text="Screenshot der Überprüfung der Kontodetails" lightbox="media/create-device-update-account/account-review.png":::

7. Überprüfen Sie die Angaben, und wählen Sie dann „Erstellen“ aus. Sie sehen, dass Ihre Bereitstellung in Bearbeitung ist. 

   :::image type="content" source="media/create-device-update-account/account-deployment-inprogress.png" alt-text="Screenshot der laufenden Kontobereitstellung" lightbox="media/create-device-update-account/account-deployment-inprogress.png":::

8. Der Status der Bereitstellung wird in wenigen Minuten in „Abgeschlossen“ geändert. Klicken Sie auf „Zu Ressource wechseln“.

   :::image type="content" source="media/create-device-update-account/account-complete.png" alt-text="Screenshot der abgeschlossenen Kontobereitstellung" lightbox="media/create-device-update-account/account-complete.png":::

## <a name="create-a-device-update-instance"></a>Erstellen einer Device Update-Instanz 

Eine Device Update-Instanz ist einem einzelnen IoT Hub zugeordnet. Wählen Sie den IoT Hub aus, der mit Device Update verwendet werden soll. In diesem Schritt wird eine neue SAS-Richtlinie erstellt, um sicherzustellen, dass Device Update nur die erforderlichen Berechtigungen für die Arbeit mit IoT Hub verwendet (Schreibvorgänge in Registrierung und Herstellen der Verbindung mit dem Dienst). Diese Richtlinie stellt sicher, dass der Zugriff nur auf Device Update beschränkt ist.

So erstellen Sie eine Device Update-Instanz, nachdem ein Konto erstellt wurde

1. Sobald Sie sich in Ihrer neu erstellten Kontoressource befinden, wechseln Sie zum Blatt „Instanzen“ der Instanzverwaltung.

   :::image type="content" source="media/create-device-update-account/instance-blade.png" alt-text="Screenshot der Instanzverwaltung innerhalb des Kontos" lightbox="media/create-device-update-account/instance-blade.png":::

2. Klicken Sie auf „Erstellen“, und geben Sie einen Instanznamen an. Anschließend wählen Sie Ihren IoT Hub aus.

   :::image type="content" source="media/create-device-update-account/instance-details.png" alt-text="Screenshot: Details zur Instanz" lightbox="media/create-device-update-account/instance-details.png":::

   > [!NOTE] 
   > Der IoT Hub, den Sie mit Ihrer Device Update-Ressource verknüpfen, muss sich nicht in derselben Region befinden wie Ihr Device Update-Konto. Für eine bessere Leistung wird jedoch empfohlen, dass sich Ihr IoT Hub in einer Region befindet, die mit der Region Ihres Device Update-Kontos übereinstimmt oder in deren Nähe liegt. 

3. Klicken Sie auf „Erstellen“. Die Instanz wird mit dem Status „Wird erstellt“ angezeigt. 

   :::image type="content" source="media/create-device-update-account/instance-creating.png" alt-text="Screenshot zur Erstellung der Instanz" lightbox="media/create-device-update-account/instance-creating.png":::

4. Warten Sie 5-10 Minuten, bis die Bereitstellung der Instanz abgeschlossen ist. Aktualisieren Sie den Status, bis der „Bereitstellungsstatus“ zu „Erfolgreich“ wechselt.

   :::image type="content" source="media/create-device-update-account/instance-succeeded.png" alt-text="Screenshot der erfolgreichen Instanzerstellung" lightbox="media/create-device-update-account/instance-succeeded.png":::

## <a name="configure-iot-hub"></a>Konfigurieren von IoT Hub 

Damit Device Update Änderungsbenachrichtigungen von IoT Hub empfangen kann, wird Device Update in den „integrierten“ Event Hub integriert. Durch Klicken auf die Schaltfläche „IoT Hub konfigurieren“ werden die erforderlichen Nachrichtenrouten und die Zugriffsrichtlinie für die Kommunikation mit IoT-Geräten konfiguriert. 

So konfigurieren Sie IoT Hub

1. Sobald der „Bereitstellungsstatus“ der Instanz zu „Erfolgreich“ wechselt, wählen Sie die Instanz auf dem Blatt der Instanzverwaltung aus. Klicken Sie auf „IoT Hub konfigurieren“.

   :::image type="content" source="media/create-device-update-account/instance-configure.png" alt-text="Screenshot der Konfiguration von IoT Hub für eine Instanz" lightbox="media/create-device-update-account/instance-configure.png":::

2. Wählen Sie „Ich bin mit der Durchführung dieser Änderungen einverstanden“ aus.

   :::image type="content" source="media/create-device-update-account/instance-configure-selected.png" alt-text="Screenshot der Zustimmung zur Konfiguration von IoT Hub für eine Instanz" lightbox="media/create-device-update-account/instance-configure-selected.png":::

3. Klicken Sie auf „Aktualisieren“.

[Erfahren Sie mehr über die konfigurierten Nachrichtenrouten.](device-update-resources.md) 


## <a name="configure-access-control-roles"></a>Konfigurieren von Zugriffssteuerungsrollen

Damit andere Benutzer auf Device Update zugreifen können, muss den Benutzern der Zugriff auf diese Ressource gewährt werden. 

1. Wechseln Sie zur Zugriffssteuerung (IAM) innerhalb des Device Update-Kontos.

   :::image type="content" source="media/create-device-update-account/account-access-control.png" alt-text="Screenshot der Zugriffssteuerung innerhalb des Device Update-Kontos" lightbox="media/create-device-update-account/account-access-control.png":::

2. Klicken Sie auf „Rollenzuweisung hinzufügen“.

3. Wählen Sie unter „Rolle auswählen“ eine Device Update-Rolle aus den angegebenen Optionen aus.
     - Device Update-Administrator
     - Device Update-Leser
     - Device Update-Inhaltsadministrator
     - Device Update-Inhaltsleser
     - Device Update-Bereitstellungsadministrator
     - Device Update-Bereitstellungsleser
     
   :::image type="content" source="media/create-device-update-account/role-assignment.png" alt-text="Screenshot der Rollenzuweisungen für die Zugriffssteuerung innerhalb des Device Update-Kontos" lightbox="media/create-device-update-account/role-assignment.png":::
    
    [Erfahren Sie mehr über die rollenbasierte Zugriffssteuerung in Device Update for IoT Hub](device-update-control-access.md) 
    
4. Zuweisen des Zugriffs zu einem Benutzer oder einer Azure AD-Gruppe
5. Klicken Sie auf Speichern.
6. Sie sind jetzt bereit, die Device Update-Umgebung von Ihrem IoT Hub aus zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

[Importieren Sie ein Update mithilfe von Device Update for IoT Hub.](import-update.md)

[Erfahren Sie mehr über das Device Update-Konto und die Instanz.](device-update-resources.md) 

[Erfahren Sie mehr über die Device Update-Zugriffssteuerungsrollen.](device-update-control-access.md) 

