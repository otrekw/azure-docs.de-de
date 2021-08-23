---
title: Schnellstart – Hinzufügen von Beitreten zu einer Teams-Besprechung zu einer Android-App mithilfe von Azure Communication Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie die Azure Communication Services-Bibliothek zum Einbetten von Teams für Android verwenden.
author: palatter
ms.author: palatter
ms.date: 06/30/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 0340135bf17f31ccc1dd00507a1c57426a3c641d
ms.sourcegitcommit: 6bd31ec35ac44d79debfe98a3ef32fb3522e3934
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113218150"
---
In dieser Schnellstartanleitung erfahren Sie, wie Sie mit der Azure Communication Services-Bibliothek zum Einbetten von Teams für Android einer Microsoft Teams-Besprechung beitreten.

## <a name="sample-code"></a>Beispielcode

Sie können die Beispiel-App von [GitHub](https://github.com/Azure-Samples/teams-embed-android-getting-started) herunterladen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Android Studio](https://developer.android.com/studio) zum Erstellen Ihrer Android-Anwendung
- Eine bereitgestellte Communication Services-Ressource. [Erstellen einer Communication Services-Ressource](../../create-communication-resource.md)
- Ein [Benutzerzugriffstoken](../../access-tokens.md) für Ihren Azure Communication Service

## <a name="setting-up"></a>Einrichten

### <a name="create-an-android-app-with-an-empty-activity"></a>Erstellen Sie eine Android-App mit einer leeren Aktivität.

Wählen Sie in Android Studio „Start a new Android Studio project“ (Neues Android Studio-Projekt starten) aus.

:::image type="content" source="../media/android/studio-new-project.png" alt-text="Ein Screenshot, der die Schaltfläche „Start a new Android Studio project“ (Neues Android Studio-Projekt starten) zeigt, die in Android Studio ausgewählt ist":::

Wählen Sie unter „Phone and Tablet“ (Telefon und Tablet) die Projektvorlage „Empty Activity“ (Leere Aktivität) aus.

:::image type="content" source="../media/android/studio-blank-activity.png" alt-text="Screenshot, der die Option „Empty Activity“ (Leere Aktivität) zeigt, die auf dem Bildschirm für die Projektvorlage ausgewählt ist":::

Geben Sie dem Projekt den Namen `TeamsEmbedAndroidGettingStarted`, legen Sie Sprache auf Java fest, und wählen Sie mindestens das SDK „API 21: Android 5,0 (Lollipop)“ oder höher aus.

:::image type="content" source="../media/android/studio-calling-min-api.png" alt-text="Screenshot, der die Option „Empty Activity“ (Leere Aktivität) zeigt, die auf dem Bildschirm für die Projektvorlage 2 ausgewählt ist":::


### <a name="install-the-azure-package"></a>Installieren des Azure-Pakets

Fügen Sie in der Datei `build.gradle` auf App-Ebene (**App-Ordner**) die folgenden Zeilen zu den Abschnitten „dependencies“ und „android“ hinzu:

```groovy
android {
    ...
    packagingOptions {
        pickFirst  'META-INF/*'
    }
}
```

```groovy
dependencies {
    ...
    implementation 'com.azure.android:azure-communication-common:1.0.0'
    ...
}
```

Aktualisieren Sie die Werte in der Datei `build.gradle`:

```groovy
 buildTypes {
        release {
        ...
            minifyEnabled true
            shrinkResources true
        ...
    }
}
```


### <a name="install-the-teams-embed-package"></a>Installieren des Teams Embed-Pakets

Laden Sie das Paket herunter `MicrosoftTeamsSDK`.

Entpacken Sie anschließend den Ordner `MicrosoftTeamsSDK` in Ihren Projektordner für Apps. Ex. `TeamsEmbedAndroidGettingStarted/app/MicrosoftTeamsSDK`.

### <a name="add-teams-embed-package-to-your-buildgradle"></a>Hinzufügen des Teams Embed-Pakets zu build.gradle

Fügen Sie in `build.gradle` auf App-Ebene am Ende der Datei die folgende Zeile hinzu:

```groovy
apply from: 'MicrosoftTeamsSDK/MicrosoftTeamsSDK.gradle'
 ```

Synchronisieren Sie das Projekt mit gradle-Dateien.

### <a name="create-application-class"></a>Erstellen der Anwendungsklasse

Erstellen Sie eine neue Java-Klassendatei mit dem Namen `TeamsEmbedAndroidGettingStarted`. Bei dieser Klasse handelt es sich um die Anwendungsklasse, die `TeamsSDKApplication` erweitern muss. [Android-Dokumentation](https://developer.android.com/reference/android/app/Application)

:::image type="content" source="../media/android/application-class-location.png" alt-text="Screenshot, der zeigt, an welcher Stelle die Anwendungsklasse in Android Studio erstellt wird":::

```java
package com.microsoft.teamsembedandroidgettingstarted;

import com.microsoft.teamssdk.app.TeamsSDKApplication;

public class TeamsEmbedAndroidGettingStarted extends TeamsSDKApplication {
}
```

### <a name="update-themes"></a>Aktualisieren von Designs

Benennen Sie die Formatvorlage in Ihren beiden Dateien `theme.xml` und `theme.xml (night)` als `AppTheme`.

:::image type="content" source="../media/android/theme-settings.png" alt-text="Screenshot: theme.xml-Dateien in Android Studio":::

```xml
<style name="AppTheme" parent="Theme.AppCompat.DayNight.DarkActionBar">
```

```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
```

### <a name="add-permissions-to-application-manifest"></a>Hinzufügen von Berechtigungen zum Anwendungsmanifest

Fügen Sie zum Anwendungsmanifest (`app/src/main/AndroidManifest.xml`) die Berechtigung `RECORD_AUDIO` hinzu:  


```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.yourcompany.TeamsEmbedAndroidGettingStarted">
    <uses-permission android:name="android.permission.RECORD_AUDIO"/>
    <application
```

### <a name="add-app-name-and-theme-to-application-manifest"></a>Hinzufügen des App-Namens und -Designs zum Anwendungsmanifest

Fügen Sie zum Manifest „xmlns:tools ="http://schemas.android.com/tools" hinzu.

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.yourcompany.TeamsEmbedAndroidGettingStarted">
```

Fügen Sie `.TeamsEmbedAndroidGettingStarted` zu `android:name` und `android:name` zu `tools:replace` hinzu, und ändern Sie `android:theme` in `@style/AppTheme`

```xml
<application
    android:name=".TeamsEmbedAndroidGettingStarted"
    tools:replace="android:name"
    android:theme="@style/AppTheme"
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true">
```

### <a name="set-up-the-layout-for-the-app"></a>Festlegen des Layouts für die App

Erstellen Sie eine Schaltfläche mit der ID `join_meeting`. Navigieren Sie zur Layoutdatei (`app/src/main/res/layout/activity_main.xml`), und ersetzen Sie den Inhalt der Datei durch den folgenden Code:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/join_meeting"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Join Meeting"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

### <a name="create-the-main-activity-scaffolding-and-bindings"></a>Erstellen des Hauptaktivitätsgerüsts und der Bindungen

Nachdem Sie das Layout erstellt haben, können Sie das Grundgerüst der Aktivität zusammen mit den Bindungen hinzufügen. Die Aktivität verarbeitet die Anforderung von Runtimeberechtigungen, erstellt den Besprechungs-Agent und tritt einer Besprechung bei, wenn die Schaltfläche ausgewählt wird. Jede dieser Optionen wird in einem eigenen Abschnitt behandelt. 

Die Methode `onCreate` wird überschrieben, um die Bindungen für die Schaltfläche `Join Meeting` hinzuzufügen. Dies ist nur einmal der Fall, wenn die Aktivität erstellt wird. Weitere Informationen zu `onCreate` finden Sie im Handbuch mit [grundlegenden Informationen zum Aktivitätslebenszyklus](https://developer.android.com/guide/components/activities/activity-lifecycle).

Navigieren Sie zu **MainActivity.java**, und ersetzen Sie den Inhalt durch den folgenden Code:

```java
package com.yourcompany.teamsembedandroidgettingstarted;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;

import android.Manifest;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.widget.Button;
import android.widget.Toast;

import com.azure.android.communication.common.CommunicationTokenCredential;
import com.azure.android.communication.common.CommunicationTokenRefreshOptions;
import com.azure.android.communication.ui.meetings.MeetingUIClientJoinOptions;
import com.azure.android.communication.ui.meetings.MeetingUIClient;
import com.azure.android.communication.ui.meetings.MeetingUIClientTeamsMeetingLinkLocator;

import java.util.ArrayList;
import java.util.concurrent.Callable;

public class MainActivity extends AppCompatActivity {

    private final String displayName = "John Smith";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        Button joinMeetingButton = findViewById(R.id.join_meeting);
        joinMeetingButton.setOnClickListener(l -> joinMeeting());
    }
    
    private void joinMeeting() {
    // See section on joining a meeting
    }

    private MeetingUIClient createMeetingUIClient() {
        // See section on creating meeting ui client
    }

    private void getAllPermissions() {
        // See section on getting permissions
    }
}
```

### <a name="request-permissions-at-runtime"></a>Anfordern von Berechtigungen zur Runtime

Für Android 6.0 und höher (API-Ebene 23) und `targetSdkVersion` 23 oder höher werden Berechtigungen zur Runtime gewährt und nicht bei der Installation der App. Zur Anforderung von Berechtigungen kann `getAllPermissions` implementiert werden, um für jede erforderliche Berechtigung `ActivityCompat.checkSelfPermission` und `ActivityCompat.requestPermissions` aufzurufen.

```java
/**
 * Request each required permission if the app doesn't already have it.
 */
private void getAllPermissions() {
    String[] requiredPermissions = new String[]{Manifest.permission.RECORD_AUDIO};
    ArrayList<String> permissionsToAskFor = new ArrayList<>();
    for (String permission : requiredPermissions) {
        if (ActivityCompat.checkSelfPermission(this, permission) != PackageManager.PERMISSION_GRANTED) {
            permissionsToAskFor.add(permission);
        }
    }
    if (!permissionsToAskFor.isEmpty()) {
        ActivityCompat.requestPermissions(this, permissionsToAskFor.toArray(new String[0]), 202);
    }
}
```

> [!NOTE]
> Berücksichtigen Sie beim Entwerfen der App, wann diese Berechtigungen angefordert werden sollen. Berechtigungen müssen angefordert werden, wenn sie benötigt werden, und nicht vorher. Weitere Informationen finden Sie in der [Android-Dokumentation zu Berechtigungen](https://developer.android.com/training/permissions/requesting).

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen dienen zur Behandlung einiger der wichtigsten Features der Azure Communication Services-Bibliothek zum Einbetten von Teams:

| name                                    | BESCHREIBUNG                                                                                                                                                       |
| --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MeetingUIClient                         | Der MeetingUIClient ist der Haupteinstiegspunkt der Bibliothek zum Einbetten von Teams.                                                                                           |
| MeetingUIClientJoinOptions              | MeetingUIClientJoinOptions wird für konfigurierbare Optionen wie den Anzeigenamen verwendet.                                                                                |
| MeetingUIClientTeamsMeetingLinkLocator  | MeetingUIClientTeamsMeetingLinkLocator wird zum Festlegen der Besprechungs-URL für den Beitritt zu einer Besprechung verwendet.                                                                      |
| MeetingUIClientGroupCallLocator         | MeetingUIClientGroupCallLocator wird zum Festlegen der Gruppen-ID für den Beitritt verwendet.                                                                                         |
| MeetingUIClientIconType                 | MeetingUIClientIconType wird verwendet, um anzugeben, welche Symbole durch ein App-spezifisches Symbol ersetzt werden können.                                                                  |
| MeetingUIClientCall                     | MeetingUIClientCall beschreibt den Anruf und stellt die APIs bereit, um ihn zu steuern.                                                                                           |
| MeetingUIClientCallState                | MeetingUIClientCallState wird zum Melden von Veränderungen im Anrufstatus verwendet. Die folgenden Optionen sind verfügbar: `CONNECTING`,`WAITING_IN_LOBBY`,`CONNECTED` und `ENDED`. |
| MeetingUIClientAudioRoute               | MeetingUIClientAudioRoute wird für lokale Audiorouten wie `Earpiece` oder `SpeakerOn` verwendet.                                                                          |
| MeetingUIClientLayoutMode               | MeetingUIClientLayoutMode wird verwendet, um die Auswahl unterschiedlicher anrufinterner Benutzeroberflächenmodi zu ermöglichen.                                                                              |
| MeetingUIClientAvatarSize               | MeetingUIClientAvatarSize ist eine Enumeration zur Bezeichnung verschiedener Avatargrößen, die von MeetingUIClientCallIdentityProvider angefordert werden können.                                |
| MeetingUIClientCallEventListener        | MeetingUIClientCallEventListener wird zum Empfangen von Ereignissen verwendet, z. B. von Veränderungen im Aufrufstatus.                                                                    |
| MeetingUIClientCallIdentityProvider     | MeetingUIClientCallIdentityProvider wird verwendet, um den Benutzern in einer Besprechung Benutzerdetails zuzuordnen.                                                                    |
| MeetingUIClientCallUserEventListener    | MeetingUIClientCallUserEventListener stellt Informationen zu Benutzeraktionen auf der Benutzeroberfläche bereit.                                                                       |

## <a name="create-a-meetingclient-from-the-user-access-token"></a>Erstellen eines Besprechungs-Agent aus dem Benutzerzugriffstoken

Ein authentifizierter Besprechungs-Client kann mit einem Benutzerzugriffstoken instanziiert werden. Dieses Token wird von einem Dienst generiert, dessen Authentifizierung für die Anwendung spezifisch ist. Weitere Informationen zu Benutzerzugriffstoken finden Sie im Handbuch zu [Benutzerzugriffstoken](../../access-tokens.md). Ersetzen Sie für den Schnellstart `<USER_ACCESS_TOKEN>` durch ein Benutzerzugriffstoken, das für Ihre Azure Communication Service-Ressource generiert wurde.

```java
private MeetingUIClient createMeetingUIClient() { 
    try {
        CommunicationTokenRefreshOptions refreshOptions = new CommunicationTokenRefreshOptions(tokenRefresher, true, "<USER_ACCESS_TOKEN>");
        CommunicationTokenCredential credential = new CommunicationTokenCredential(refreshOptions);
        return new MeetingUIClient(credential);
    } catch (Exception ex) {
        Toast.makeText(getApplicationContext(), "Failed to create meeting ui client: " + ex.getMessage(), Toast.LENGTH_SHORT).show();
    }
}
```

### <a name="setup-token-refreshing"></a>Das Einrichtungstoken wird aktualisiert

Erstellen Sie eine aufrufbare `tokenRefresher`-Methode. Erstellen Sie dann eine `fetchToken`-Methode, um das Benutzertoken zu erhalten. [Eine Anleitung dazu finden Sie hier](../../access-tokens.md?pivots=programming-language-java)

```java
Callable<String> tokenRefresher = () -> {
    return fetchToken();
};

public String fetchToken() {
    // Get token
    return USER_ACCESS_TOKEN;
}
```

## <a name="start-a-meeting-using-the-meeting-client"></a>Starten einer Besprechung mithilfe des Besprechungs-Clients

Die Methode `joinMeeting` wird als die Aktion festgelegt, die ausgeführt wird, wenn auf die Schaltfläche *Besprechung beitreten* getippt wird. Der Beitritt zu einer Besprechung kann über `MeetingUIClient` erfolgen, und es sind nur `MeetingUIClientTeamsMeetingLinkLocator` und `MeetingUIClientJoinOptions` erforderlich.
Hinweis: Ersetzen Sie `<MEETING_URL>` durch einen Microsoft Teams-Besprechungslink.

```java
/**
 * Join a meeting with a meetingURL.
 */
private void joinMeeting() {
    getAllPermissions();
    MeetingUIClient meetingUIClient = createMeetingUIClient();
    
    MeetingUIClientTeamsMeetingLinkLocator meetingUIClientTeamsMeetingLinkLocator = new MeetingUIClientTeamsMeetingLinkLocator(<MEETING_URL>);
    
    MeetingUIClientJoinOptions meetingJoinOptions = new MeetingUIClientJoinOptions(displayName, false);
    
    try {
        meetingUIClient.join(meetingUIClientTeamsMeetingLinkLocator, meetingJoinOptions);
    } catch (Exception ex) {
        Toast.makeText(getApplicationContext(), "Failed to join meeting: " + ex.getMessage(), Toast.LENGTH_SHORT).show();
    }
}
```

### <a name="get-a-microsoft-teams-meeting-link"></a>Abrufen eines Microsoft Teams-Besprechungslinks

Ein Microsoft Teams-Besprechungslink kann mithilfe der Graph-APIs abgerufen werden. Die einzelnen Schritte zum Abrufen eines Besprechungslinks werden in der [Graph-Dokumentation](/graph/api/onlinemeeting-createorget?tabs=http&view=graph-rest-beta&preserve-view=true) beschrieben.
Das Communication Services-SDK für Telefonie akzeptiert einen vollständigen Teams-Besprechungslink. Dieser Link wird als Teil der `onlineMeeting`-Ressource zurückgegeben, auf die Sie über die [`joinWebUrl`-Eigenschaft](/graph/api/resources/onlinemeeting?view=graph-rest-beta&preserve-view=true) zugreifen können. Sie können die erforderlichen Besprechungsinformationen auch aus der URL **Besprechung beitreten** in der Teams-Besprechungseinladung selbst abrufen.

## <a name="launch-the-app-and-join-a-meeting"></a>Starten der App und Beitreten zu einer Teams-Besprechung

Die App kann jetzt mithilfe der Schaltfläche „Run app“ (App ausführen) auf der Symbolleiste gestartet werden (UMSCHALT + F10). 

:::image type="content" source="../media/android/quickstart-android-join-meeting.png" alt-text="Screenshot, der die fertige Anwendung zeigt":::

:::image type="content" source="../media/android/quickstart-android-joined-meeting.png" alt-text="Screenshot: Fertige Anwendung, nach dem Beitritt zur Besprechung.":::

## <a name="add-localization-support-based-on-your-app"></a>Hinzufügen unterstützter Lokalisierungen ausgehend von Ihrer App

Das Microsoft Teams-SDK unterstützt mehr als 100 Zeichenfolgen in über 50 Sprachen. Standardmäßig ist nur Englisch aktiviert. Die übrigen Sprachen können in der Gradle-Datei aktiviert werden.

#### <a name="add-localizations-to-the-sdk-based-on-what-your-app-supports"></a>Fügen Sie basierend auf den von Ihrer App unterstützten Funktionen Lokalisierungen zum SDK hinzu

1. Bestimmen Sie die Liste der Sprachen, die Ihre App unterstützt.
2. Öffnen Sie die Datei MicrosoftTeamsSDK.gradle.
3. Im defaultConfig-Block ist für die resConfigs-Eigenschaft standardmäßig „en“ festgelegt. Fügen Sie die Sprachen hinzu, die Ihre App benötigt. Verweis: [Android-Dokumentation](https://developer.android.com/studio/build/shrink-code#unused-alt-resources)
