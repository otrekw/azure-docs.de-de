---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 71e4b0b3b7bf84938123acf70ac18a93c3015a4d
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095314"
---
### <a name="create-a-firebase-project-and-enable-firebase-cloud-messaging-for-android"></a>Erstellen eines Firebase-Projekts und Aktivieren von Firebase Cloud Messaging für Android

1. Melden Sie sich bei der [Firebase-Konsole](https://firebase.google.com/console/) an. Erstellen Sie ein neues Firebase-Projekt, wobei Sie **PushDemo** als **Projektnamen** eingeben.

    > [!NOTE]
    > Es wird ein eindeutiger Name für Sie generiert. Standardmäßig besteht dies aus einer Variante des von Ihnen angegebenen Namens (in Kleinbuchstaben) und einer generierten Zahl, getrennt durch einen Bindestrich. Wenn Sie möchten, können Sie dies ändern, vorausgesetzt, es ist immer noch global eindeutig.

1. Klicken Sie nach der Erstellung Ihres Projekts auf **Add Firebase to your Android app** (Firebase der Android-App hinzufügen).

    ![Hinzufügen von Firebase zu Ihrer Android-App](./media/notification-hubs-common-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)

1. Führen Sie auf der Seite **Hinzufügen von Firebase zu Ihrer Android-App** die folgenden Schritte aus.
    1. Geben Sie in **Android-Paketname** einen Namen für Ihr Paket ein. Beispiel: `com.<organization_identifier>.<package_name>`.

        ![Angeben des Paketnamens](./media/notification-hubs-common-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    1. Wählen Sie **Register app** (App registrieren) aus.  
    1. Klicken Sie auf **google-services.json herunterladen**. Speichern Sie die Datei dann zur späteren Verwendung in einem lokalen Ordner, und wählen Sie **Weiter** aus.  

        ![Herunterladen von „google-services.json“](./media/notification-hubs-common-enable-firebase-cloud-messaging/download-google-service-button.png)
    1. Wählen Sie **Weiter** aus.
    1. Wählen Sie **Weiter zur Konsole** aus.

        > [!NOTE]
        > Wenn die Schaltfläche **Weiter zur Konsole** aufgrund der Aktivierung von *Installation überprüfen* nicht aktiviert ist, wählen Sie **Diesen Schritt überspringen** aus.

1. Klicken Sie in der Firebase-Konsole auf das Zahnrad für Ihr Projekt. Klicken Sie dann auf **Projekteinstellungen**.

    ![Klicken auf „Projekteinstellungen“](./media/notification-hubs-common-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)

    > [!NOTE]
    > Wenn Sie die Datei **google-services.json** nicht heruntergeladen haben, können Sie dies auf dieser Seite tun.

1. Wechseln Sie oben zur Registerkarte **Cloud Messaging**. Kopieren und speichern Sie den **Serverschlüssel** für eine spätere Verwendung. Dieser Wert dient zum Konfigurieren des Notification Hub.

    ![Kopieren des Serverschlüssels](./media/notification-hubs-common-enable-firebase-cloud-messaging/server-key.png)
