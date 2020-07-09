---
title: Senden von Pushbenachrichtigungen an Android mit Azure Notification Hubs und Firebase SDK-Version 1.0.0-preview1
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Notification Hubs und Google Firebase Cloud Messaging Pushbenachrichtigungen an Android-Geräte senden.
author: sethmanheim
ms.author: sethm
ms.date: 5/28/2020
ms.topic: tutorial
ms.service: notification-hubs
ms.reviewer: thsomasu
ms.lastreviewed: 05/27/2020
ms.openlocfilehash: 292f7b65528c13d23d4f2b72467538c43c6ad75e
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84172038"
---
# <a name="tutorial-send-push-notifications-to-android-devices-using-firebase-sdk-version-100-preview1"></a>Tutorial: Senden von Pushbenachrichtigungen an Android-Geräte mit Firebase SDK-Version 1.0.0-preview1

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Notification Hubs und der aktualisierten Version von Firebase Cloud Messaging (FCM) SDK (Version 1.0.0-preview1) Pushbenachrichtigungen an eine Android-Anwendung senden. In diesem Tutorial erstellen Sie eine leere Android-App, die Pushbenachrichtigungen mithilfe von Firebase Cloud Messaging (FCM) empfängt.

Den vollständigen Code für dieses Tutorial können Sie [von GitHub](https://github.com/Azure/azure-notificationhubs-android/tree/v1-preview/notification-hubs-test-app-refresh) herunterladen.

Dieses Tutorial enthält die folgenden Schritte:

- Erstellen eines Android Studio-Projekts.
- Erstellen eines Firebase-Projekts, das Firebase Cloud Messaging unterstützt.
- Erstellen eines Notification Hubs.
- Verbinden Sie Ihre App mit dem Hub.
- Testen der App.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/free/).

Sie benötigen außerdem folgende Elemente:

- Die aktuelle Version von  [Android Studio](https://go.microsoft.com/fwlink/?LinkId=389797) wird empfohlen.
- Minimale Unterstützung ist API-Ebene 16.

## <a name="create-an-android-studio-project"></a>Erstellen eines Android Studio-Projekts

Der erste Schritt zum Erstellen eines Projekts in Android Studio:

1. Starten Sie Android Studio.

2. Wählen Sie  **Datei** aus, und wählen Sie dann **Neu** und schließlich **Neues Projekt** aus.

3. Wählen Sie auf der Seite  **Projekt auswählen** die Option  **Leere Aktivität ** und dann  **Weiter** aus.

4. Führen Sie auf der Seite  **Projekt konfigurieren** die folgenden Schritte aus:
   1. Geben Sie einen Namen für die Anwendung ein.
   2. Geben Sie einen Speicherort an, in dem die Projektdateien gespeichert werden.
   3. Wählen Sie  **Fertig stellen** aus.

   :::image type="content" source="media/android-sdk/configure-project.png" alt-text="Projekt konfigurieren":::

## <a name="create-a-firebase-project-that-supports-fcm"></a>Erstellen eines Firebase-Projekts, das FCM unterstützt

1. Melden Sie sich bei der  [Firebase-Konsole](https://firebase.google.com/console/) an. Erstellen Sie ein neues Firebase-Projekt, falls Sie noch keins besitzen.

2. Klicken Sie nach der Erstellung Ihres Projekts auf  **Add Firebase to your Android app** (Firebase der Android-App hinzufügen).

   :::image type="content" source="media/android-sdk/get-started.png" alt-text="Firebase hinzufügen":::

3. Führen Sie auf der Seite  **Add Firebase to your Android app** (Firebase der Android-App hinzufügen) Folgendes aus:

   1. Kopieren Sie für  **Android package name** (Name des Android-Pakets) den Wert der **applicationId** in die Datei **build.gradle** Ihrer Anwendung. In diesem Beispiel lautet er  `com.fabrikam.fcmtutorial1app`.

      :::image type="content" source="media/android-sdk/specify-package-name-fcm-settings.png" alt-text="Angeben des Paketnamens":::

   2. Wählen Sie  **App registrieren** aus.

4. Wählen Sie  **Download google-services.json** (google-services.json herunterladen) aus, speichern Sie die Datei im Ordner **app** des Projekts, und wählen Sie dann  **Next** (Weiter) aus.

   :::image type="content" source="media/android-sdk/download-google-service-button.png" alt-text="Google-Dienst herunterladen":::

5. Klicken Sie in der Firebase-Konsole auf das Zahnrad für Ihr Projekt. Wählen Sie dann  **Projekteinstellungen** aus.

   :::image type="content" source="media/android-sdk/notification-hubs-firebase-console-project-settings.png" alt-text="Projekteinstellungen":::

6. Wenn Sie die Datei **google-services.json** nicht in den Ordner **app** Ihres Android Studio-Projekts heruntergeladen haben, können Sie dies auf dieser Seite tun.

7. Wechseln Sie zur Registerkarte **Cloud Messaging**.

8. Kopieren und speichern Sie den  **Serverschlüssel** zur späteren Verwendung. Verwenden Sie diesen Wert zum Konfigurieren Ihres Hubs.

## <a name="configure-a-notification-hub"></a>Konfigurieren eines Notification Hubs

1. Melden Sie sich beim  [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie im Menü auf der linken Seite **Alle Dienste** und dann im Abschnitt **Mobil** die Option **Notification Hubs** aus. Wählen Sie das Sternsymbol neben dem Dienstnamen aus, um den Dienst im linken Menü zum Abschnitt  **FAVORITEN** hinzuzufügen. Nachdem **Notification Hubs** unter **FAVORITEN** hinzugefügt wurde, können Sie diesen Eintrag im Menü auf der linken Seite auswählen.

3. Wählen Sie auf der Seite **Notification Hubs** auf der Symbolleiste **Hinzufügen** aus.

   :::image type="content" source="media/android-sdk/add-hub.png" alt-text="Hub hinzufügen":::

4. Gehen Sie auf der Seite **Notification Hubs** folgendermaßen vor:

   1. Geben Sie in **Notification Hub** einen Namen ein.

   2. Geben Sie in **Neuen Namespace erstellen** einen Namen ein. Ein Namespace enthält mindestens einen Hub.

   3. Wählen Sie im Dropdown **Standort** einen Wert aus. Dieser Wert gibt den Standort an, an dem der Hub erstellt werden soll.

   4. Wählen Sie eine vorhandene Ressourcengruppe in **Ressourcengruppe** aus, oder erstellen Sie eine neue.

   5. Wählen Sie  **Erstellen** aus.

      :::image type="content" source="media/android-sdk/create-hub.png" alt-text="Erstellen eines Hubs":::

5. Wählen Sie **Benachrichtigungen** (Glockensymbol) und dann **Zu Ressource wechseln** aus. Sie können auch die Liste auf der Seite **Notification Hubs** aktualisieren und Ihren Hub auswählen.

   :::image type="content" source="media/android-sdk/notification-hubs.png" alt-text="Auswählen des Hubs":::

6. Wählen Sie in der Liste **Zugriffsrichtlinien** aus. Beachten Sie, dass zwei Verbindungszeichenfolgen verfügbar sind. Sie werden später für die Behandlung von Pushbenachrichtigungen benötigt.

   :::image type="content" source="media/android-sdk/access-policies.png" alt-text="Zugriffsrichtlinien":::

   > [!IMPORTANT]
   > Verwenden Sie in Ihrer Anwendung nicht die Richtlinie  **DefaultFullSharedAccessSignature**. Diese Richtlinie darf nur im App-Back-End verwendet werden.

## <a name="configure-firebase-cloud-messaging-settings-for-the-hub"></a>Konfigurieren von Firebase Cloud Messaging-Einstellungen für den Hub

1. Wählen Sie im linken Bereich unter **Einstellungen** die Option **Google (GCM/FCM)** aus.

2. Geben Sie den **Serverschlüssel** für das FCM-Projekt ein, das Sie zuvor gespeichert haben.

3. Wählen Sie auf der Symbolleiste **Speichern** aus.

   :::image type="content" source="media/android-sdk/fcm-server-key.png" alt-text="Serverschlüssel":::

4. Im Azure-Portal wird in einer Meldung angezeigt, dass der Hub aktualisiert wurde. Die Schaltfläche **Speichern** ist deaktiviert.

Ihr Benachrichtigungshub ist jetzt für die Verwendung mit Firebase Cloud Messaging konfiguriert. Sie verfügen außerdem über die Verbindungszeichenfolgen, die erforderlich sind, um Benachrichtigungen an ein Gerät zu senden und eine App für den Empfang von Benachrichtigungen zu registrieren.

## <a name="connect-your-app-to-the-notification-hub"></a>Verbinden Ihrer App mit dem Notification Hub

### <a name="add-google-play-services-to-the-project"></a>Hinzufügen von Google Play Services zum Projekt

1. Wählen Sie in Android Studio im Menü **Tools** und dann **SDK-Manager** aus.

2. Wählen Sie die Zielversion des Android SDK aus, das in Ihrem Projekt verwendet wird. Wählen Sie dann **Paketdetails anzeigen** aus.

   :::image type="content" source="media/android-sdk/notification-hubs-android-studio-sdk-manager.png" alt-text="SDK-Manager":::

3. Wählen Sie **Google-APIs** aus, falls diese noch nicht installiert wurden.

   :::image type="content" source="media/android-sdk/google-apis-selected.png" alt-text="APIs":::

4. Wechseln Sie zur Registerkarte **SDK Tools**. Wählen Sie Google Play Services noch nicht installiert haben, wählen Sie **Google Play Services**, wie in der folgenden Abbildung gezeigt, aus. Wählen Sie dann **Anwenden** aus, um die Installation auszuführen. Notieren Sie den SDK-Pfad, den Sie in einem späteren Schritt angeben müssen.

   :::image type="content" source="media/android-sdk/google-play-services-selected.png" alt-text="Play Services":::

5. Wählen Sie **OK** aus, wenn das Dialogfeld **Änderung bestätigen** angezeigt wird. Die gewünschten Komponenten werden mit dem entsprechenden Installationsprogramm installiert. Wählen Sie **Fertig stellen** aus, wenn die Installation der Komponenten abgeschlossen ist.

6. Wählen Sie **OK** aus, um das Dialogfeld **Settings for New Projects** (Einstellungen für neue Projekte) zu schließen.

### <a name="add-azure-notification-hubs-libraries"></a>Hinzufügen von Azure Notification Hubs-Bibliotheken

1. Fügen Sie in der Datei **uild.gradle** der App im Abschnitt „dependencies“ die folgenden Zeilen hinzu:

   ```gradle
   implementation 'com.microsoft.azure:notification-hubs-android-sdk:1.0.0-preview1@aar'
   implementation 'androidx.appcompat:appcompat:1.0.0'

   implementation 'com.google.firebase:firebase-messaging:20.1.5'

   implementation 'com.android.volley:volley:1.1.1'
   ```

2. Fügen Sie hinter dem Abschnitt „dependencies“ das folgende Repository hinzu:

   ```gradle
   repositories {
      maven {
         url "https://dl.bintray.com/microsoftazuremobile/SDK"
      }
   }
   ```

### <a name="add-google-firebase-support"></a>Hinzufügen von Unterstützung für Google Firebase

1. Fügen Sie am Ende der Datei das folgende Plug-In hinzu, sofern es noch nicht vorhanden ist.

   ```gradle
   apply plugin: 'com.google.gms.google-services'
   ```

2. Wählen Sie auf der Symbolleiste **Jetzt synchronisieren** aus.

### <a name="add-code"></a>Code hinzufügen

1. Erstellen Sie ein **NotificationHubListener**-Objekt, das das Abfangen der Nachrichten von Azure Notification Hubs durchführt.

   ```csharp
   public class CustomNotificationListener implements NotificationHubListener {

      @override

      public void onNotificationReceived(Context context, NotificationMessage message) {

      /* The following notification properties are available. */

      String title = message.getTitle();
      String message = message.getMessage();
      Map<String, String> data = message.getData();

      if (message != null) {
         Log.d(TAG, "Message Notification Title: " + title);
         Log.d(TAG, "Message Notification Body: " + message);
         }
      }
   }
   ```

2. Fügen Sie in der `OnCreate`-Methode der -Klasse den folgenden Code hinzu, um den Initialisierungsprozess von Notification Hubs zu starten, wenn die Aktivität erstellt wird:

   ```java
   @Override
   protected void onCreate(Bundle savedInstanceState) {

      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity\_main);
      NotificationHub.setListener(new CustomNotificationListener());
      NotificationHub.initialize(this.getApplication(), “Connection-String”, "Hub Name");

   }
   ```

3. Wählen Sie in Android Studio auf der Menüleiste **Build** **(Erstellen) und dann**Rebuild Project  (Projekt neu erstellen) aus, um sicherzustellen, dass im Code keine Fehler enthalten sind. Wenn eine Fehlermeldung zum **ic_launcher**-Symbol angezeigt wird, entfernen Sie die folgende Anweisung aus der Datei „AndroidManifest.xml“:

   ```xml
   android:icon="@mipmap/ic_launcher"
   ```

4. Stellen Sie sicher, dass Sie über ein virtuelles Gerät zum Ausführen der App verfügen. Wenn Sie keines haben, fügen Sie wie folgt eines hinzu:

   1. :::image type="content" source="media/android-sdk/open-device-manager.png" alt-text="Geräte-Manager":::
   2. :::image type="content" source="media/android-sdk/your-virtual-devices.png" alt-text="Virtuelle Geräte":::
   3. Führen Sie die App auf Ihrem ausgewählten Gerät aus, und vergewissern Sie sich, dass sie richtig beim Hub registriert wird.

      :::image type="content" source="media/android-sdk/device-registration.png" alt-text="Geräteregistrierung":::

      > [!NOTE]
      > Bei der Registrierung treten beim ersten Starten unter Umständen Fehler auf, bis die `onTokenRefresh()`-Methode des Instanz-ID-Diensts aufgerufen wird. Durch eine Aktualisierung sollte eine erfolgreiche Registrierung beim Notification Hub initiiert werden.

## <a name="send-a-test-notification"></a>Senden einer Testbenachrichtigung

Sie können Pushbenachrichtigungen aus dem [Azure-Portal](https://portal.azure.com/) wie folgt an Ihren Notification Hub senden:

1. Wählen Sie im Azure-Portal auf der Seite „Notification Hub“ für Ihren Hub im Abschnitt  **Problembehandlung** die Option **Testsendevorgang** aus.

2. Wählen Sie in **Plattformen** die Option **Android** aus.

3. Wählen Sie **Senden** aus. Es wird noch keine Benachrichtigung auf dem Android-Gerät angezeigt, da Sie die mobile App nicht darauf ausgeführt haben. Wählen Sie nach dem Ausführen der mobilen App erneut die Schaltfläche **Senden** aus, um die Benachrichtigung anzuzeigen.

4. Sie sehen das Ergebnis des Vorgangs in der Liste am unteren Rand der Portalseite.

   :::image type="content" source="media/android-sdk/notification-hubs-test-send.png" alt-text="Testbenachrichtigung senden":::

5. Die Benachrichtigung wird auf Ihrem Gerät angezeigt.

Pushbenachrichtigungen werden normalerweise mithilfe einer kompatiblen Bibliothek über einen Back-End-Dienst wie Mobile Apps oder ASP.NET gesendet. Falls für Ihr Back-End keine Bibliothek verfügbar ist, können Sie Benachrichtigungen auch direkt über die REST-API senden.

## <a name="run-the-mobile-app-on-emulator"></a>Ausführen der mobilen App im Emulator

Vor dem Testen von Pushbenachrichtigungen in einem Emulator müssen Sie sicherstellen, dass das Emulatorimage die Google-API-Ebene unterstützt, die Sie für die App ausgewählt haben. Wenn Ihr Image keine nativen Google-APIs unterstützt, wird unter Umständen die Ausnahme **SERVICE_NOT_AVAILABEL** angezeigt.

Stellen Sie zusätzlich sicher, dass Ihr Google-Konto dem ausgeführten Emulator unter **Einstellungen** > **Konten** hinzugefügt wurde. Andernfalls führt die Registrierung bei FCM möglicherweise zur Ausnahme **AUTHENTICATION_FAILED**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie mit Firebase Cloud Messaging Benachrichtigungen an alle Android-Geräte gesendet, die bei dem Dienst registriert wurden. Um zu erfahren, wie Sie Pushbenachrichtigungen an bestimmte Geräte senden, fahren Sie mit dem folgenden Tutorial fort:

> [!div class="nextstepaction"]
>[Tutorial: Senden von Benachrichtigungen an bestimmte Benutzer](push-notifications-android-specific-users-firebase-cloud-messaging.md).

Im Folgenden finden Sie eine Liste einiger anderer Tutorials zum Senden von Benachrichtigungen:

- Azure Mobile Apps: Ein Beispiel für das Senden von Benachrichtigungen von einem mit Notification Hubs integrierten Mobile Apps-Back-End finden Sie unter [Hinzufügen von Pushbenachrichtigungen zu Ihrer iOS-App](/azure/app-service-mobile/app-service-mobile-ios-get-started-push).

- ASP.NET: [Verwenden von Notification Hubs zum Senden von Pushbenachrichtigungen an Benutzer](notification-hubs-aspnet-backend-ios-apple-apns-notification.md).

- Azure Notification Hubs Java SDK: Informationen zum Senden von Benachrichtigungen über Java finden Sie unter [Verwenden von Notification Hubs von Java aus](notification-hubs-java-push-notification-tutorial.md). Dies wurde für die Android-Entwicklung in Eclipse getestet.

- PHP: [Verwenden von Notification Hubs von PHP aus](notification-hubs-php-push-notification-tutorial.md).
