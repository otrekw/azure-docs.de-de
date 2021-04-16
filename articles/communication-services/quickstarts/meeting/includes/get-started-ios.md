---
title: Schnellstart – Hinzufügen von Beitreten zu einer Teams-Besprechung zu einer iOS-App mithilfe von Azure Communication Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie die Azure Communication Services-Bibliothek zum Einbetten von Teams für iOS verwenden.
author: palatter
ms.author: palatter
ms.date: 01/25/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 4d28864d41d6540afc87126daf589ed2929f891d
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104803067"
---
In dieser Schnellstartanleitung erfahren Sie, wie Sie mit der Azure Communication Services-Bibliothek zum Einbetten von Teams für iOS einer Teams-Besprechung beitreten.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser Schnellstartanleitung benötigen Sie Folgendes:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Einen Mac mit [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532) zusammen mit einem gültigen in Ihrer Keychain installierten Entwicklerzertifikat.
- Eine bereitgestellte Communication Services-Ressource. [Erstellen einer Communication Services-Ressource](../../create-communication-resource.md)
- Ein [Benutzerzugriffstoken](../../access-tokens.md) für Ihren Azure Communication Service
- Eine Installation von [Cocoapods](https://cocoapods.org/) für Ihre Buildumgebung.

## <a name="setting-up"></a>Einrichten

### <a name="creating-the-xcode-project"></a>Erstellen des Xcode-Projekts

Erstellen Sie in Xcode ein neues iOS-Projekt, und wählen Sie die Vorlage **App** aus. Wir verwenden UIKit-Storyboards. Im Rahmen dieser Schnellstartanleitung werden keine Tests erstellt. Sie können die Option **Tests einschließen** daher deaktivieren.

:::image type="content" source="../media/ios/xcode-new-project-template-select.png" alt-text="Screenshot: ausgewählte Vorlage„Neues Projekt“ in Xcode.":::

Benennen Sie das Projekt mit `TeamsEmbedGettingStarted`.

:::image type="content" source="../media/ios/xcode-new-project-details.png" alt-text="Screenshot: Details des neuen Projekts in Xcode.":::

### <a name="install-the-package-and-dependencies-with-cocoapods"></a>Installieren des Pakets und der Abhängigkeiten mit CocoaPods

1. Erstellen Sie eine Podfile für die Anwendung:

```
platform :ios, '12.0'
use_frameworks!

target 'TeamsEmbedGettingStarted' do
    pod 'AzureCommunication', '~> 1.0.0-beta.8'
end

azure_libs = [
'AzureCommunication',
'AzureCore']

post_install do |installer|
    installer.pods_project.targets.each do |target|
    if azure_libs.include?(target.name)
        puts "Adding BUILD_LIBRARY_FOR_DISTRIBUTION to #{target.name}"
        target.build_configurations.each do |config|
        xcconfig_path = config.base_configuration_reference.real_path
        File.open(xcconfig_path, "a") {|file| file.puts "BUILD_LIBRARY_FOR_DISTRIBUTION = YES"}
        end
    end
    end
end
```

2. Führen Sie aus `pod install`.
3. Öffnen Sie die generierte `.xcworkspace` in Xcode.

### <a name="request-access-to-the-microphone-camera-etc"></a>Fordern Sie Zugriff auf das Mikrofon, die Kamera usw. an.

Aktualisieren Sie die Eigenschaftenliste der APP-Informationen, um auf die Hardware des Geräts zuzugreifen. Legen Sie den zugehörigen Wert auf eine Zeichenfolge (`string`) fest. Diese wird in den Dialog aufgenommen, mit dem das System um den Zugriff beim Benutzer anfordert.

Klicken Sie mit der rechten Maustaste auf den Eintrag `Info.plist` der Projektstruktur, und wählen Sie anschließend **Open As** (Öffnen als)  > **Source Code** (Quellcode) aus. Fügen Sie die folgenden Zeilen im Abschnitt `<dict>` der obersten Ebene hinzu, und speichern anschließend Sie die Datei.

```xml
<key>NSBluetoothAlwaysUsageDescription</key>
<string></string>
<key>NSBluetoothPeripheralUsageDescription</key>
<string></string>
<key>NSCameraUsageDescription</key>
<string></string>
<key>NSContactsUsageDescription</key>
<string></string>
<key>NSMicrophoneUsageDescription</key>
<string></string>
```

### <a name="add-the-teams-embed-framework"></a>Hinzufügen des Frameworks zum Einbetten von Teams

1. Laden Sie das Framework herunter.
2. Erstellen Sie im Stammverzeichnis des Projekts einen Ordner `Frameworks`. Ex. `\TeamsEmbedGettingStarted\Frameworks\`
3. Kopieren Sie die heruntergeladenen Frameworks `TeamsAppSDK.framework` und `MeetingUIClient.framework` in diesen Ordner.
4. Fügen Sie `TeamsAppSDK.framework` und `MeetingUIClient.framework` zum Projektziel auf der Registerkarte „Allgemein“ hinzu. Verwenden Sie `Add Other`  ->  `Add Files...`, um zu den Framework-Dateien zu navigieren und sie hinzuzufügen.

:::image type="content" source="../media/ios/xcode-add-frameworks.png" alt-text="Screenshot: hinzugefügte Frameworks in Xcode.":::

5. Wenn es noch nicht geschehen ist, fügen Sie auf der Registerkarte mit den Buildeinstellungen des Projektziels `$(PROJECT_DIR)/Frameworks` und `Framework Search Paths` hinzu. Um die Einstellung zu finden, müssen Sie den Filter von `basic` in `all` ändern. Sie können auch die Suchleiste auf der rechten Seite verwenden.

:::image type="content" source="../media/ios/xcode-add-framework-search-path.png" alt-text="Screenshot: Suchpfad zum Framework in Xcode.":::

### <a name="turn-off-bitcode"></a>Deaktivieren von Bitcode

Legen Sie die Option `Enable Bitcode` bei den Projektbuildeinstellungen auf `No` fest. Um die Einstellung zu finden, müssen Sie den Filter von `basic` in `all` ändern. Sie können auch die Suchleiste auf der rechten Seite verwenden.

:::image type="content" source="../media/ios/xcode-bitcode-option.png" alt-text="Screenshot: Bitcode-Option in Xcode.":::

### <a name="add-framework-signing-script"></a>Signaturskript für das Framework hinzufügen

Wählen Sie das App-Ziel aus, und wählen Sie die Registerkarte `Build Phases`. Klicken Sie dann auf `+` und anschließend auf `New Run Script Phase`. Stellen Sie sicher, dass diese neue Phase nach den `Embed Frameworks`-Phasen liegt.



:::image type="content" source="../media/ios/xcode-build-script.png" alt-text="Screenshot: Hinzufügen des Buildskripts in Xcode.":::

```bash
#!/bin/sh
if [ -d "${TARGET_BUILD_DIR}"/"${PRODUCT_NAME}".app/Frameworks/TeamsAppSDK.framework/Frameworks ]; then
    pushd "${TARGET_BUILD_DIR}"/"${PRODUCT_NAME}".app/Frameworks/TeamsAppSDK.framework/Frameworks
    for EACH in *.framework; do
        echo "-- signing ${EACH}"
        /usr/bin/codesign --force --deep --sign "${EXPANDED_CODE_SIGN_IDENTITY}" --entitlements "${TARGET_TEMP_DIR}/${PRODUCT_NAME}.app.xcent" --timestamp=none $EACH
        echo "-- moving ${EACH}"
        mv -nv ${EACH} ../../
    done
    rm -rf "${TARGET_BUILD_DIR}"/"${PRODUCT_NAME}".app/Frameworks/TeamsAppSDK.framework/Frameworks
    popd
    echo "BUILD DIR ${TARGET_BUILD_DIR}"
fi
```

### <a name="turn-on-voice-over-ip-background-mode"></a>Schalten Sie den Hintergrundmodus von Voice over IP ein.

Wählen Sie Ihr App-Ziel aus, und klicken Sie auf die Registerkarte „Funktionen“.

Aktivieren Sie `Background Modes`. Hierzu klicken Sie oben auf `+ Capabilities` und wählen dann `Background Modes` aus.

Aktivieren Sie das Kontrollkästchen für `Voice over IP`.

:::image type="content" source="../media/ios/xcode-background-voip.png" alt-text="Screenshot: aktiviertes VoIP in Xcode.":::

### <a name="add-a-window-reference-to-appdelegate"></a>Hinzufügen eines Fensterverweises auf AppDelegate

Öffnen Sie die Datei **AppDelegat.swift** Ihres Projekts, und fügen Sie einen Verweis auf „Fenster“ hinzu.

```swift
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?
```

### <a name="add-a-button-to-the-viewcontroller"></a>Hinzufügen einer Schaltfläche zum ViewController

Erstellen Sie in **ViewController.swift** eine Schaltfläche im `viewDidLoad`-Rückruf.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    let button = UIButton(frame: CGRect(x: 100, y: 100, width: 200, height: 50))
    button.backgroundColor = .black
    button.setTitle("Join Meeting", for: .normal)
    button.addTarget(self, action: #selector(joinMeetingTapped), for: .touchUpInside)
    
    button.translatesAutoresizingMaskIntoConstraints = false
    self.view.addSubview(button)
    button.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    button.centerYAnchor.constraint(equalTo: view.centerYAnchor).isActive = true
```

Erstellen Sie ein Outlet für die Schaltfläche in **ViewController.swift**.

```swift
@IBAction func joinMeetingTapped(_ sender: UIButton) {

}
```

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Öffnen Sie die Datei **ViewController.swift** Ihres Projekts, und fügen Sie am Anfang der Datei eine `import`-Deklaration hinzu, um die `AzureCommunication library` und die `MeetingUIClient` zu importieren. 

```swift
import UIKit
import AzureCommunication
import MeetingUIClient
```

Ersetzen Sie die Implementierung der `ViewController`-Klasse durch eine einfache Schaltfläche, mit der der Benutzer einer Besprechung beitreten kann. In dieser Schnellstartanleitung fügen wir an diese Schaltfläche eine Geschäftslogik an.

```swift
class ViewController: UIViewController {

    private var meetingUIClient: MeetingUIClient?
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Initialize meetingClient
    }

    @IBAction func joinMeetingTapped(_ sender: UIButton) {
        joinMeeting()
    }
    
    private func joinMeeting() {
        // Add join meeting logic
    }
}
```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen dienen zur Behandlung einiger der wichtigsten Features der Azure Communication Services-Bibliothek zum Einbetten von Teams:

| name                                  | BESCHREIBUNG                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| MeetingUIClient | Der MeetingUIClient ist der Haupteinstiegspunkt der Bibliothek zum Einbetten von Teams. |
| MeetingUIClientDelegate | Der MeetingUIClientDelegate wird zum Empfangen von Ereignissen verwendet, z. B. von Veränderungen im Aufrufstatus. |
| MeetingJoinOptions | MeetingJoinOptions wird für konfigurierbare Optionen wie den Anzeigenamen verwendet. | 
| CallState | Der CallState wird zum Melden von Veränderungen im Anrufstatus verwendet. Die Optionen lauten wie folgt: connecting (Verbinden), waitingInlobby (Warten), connected (Verbunden) und ended (Beendet). |

## <a name="create-and-authenticate-the-client"></a>Erstellen und Authentifizieren des Clients

Initialisieren Sie eine `MeetingUIClient`-Instanz mit einem Benutzerzugriffstoken, das es uns ermöglicht, Besprechungen beizutreten. Fügen Sie den folgenden Code zum `viewDidLoad`-Rückruf in **ViewController.swift** hinzu:

```swift
do {
    let communicationTokenRefreshOptions = CommunicationTokenRefreshOptions(initialToken: "<USER_ACCESS_TOKEN>", refreshProactively: true, tokenRefresher: fetchTokenAsync(completionHandler:))
    let credential = try CommunicationTokenCredential(with: communicationTokenRefreshOptions)
    meetingUIClient = MeetingUIClient(with: credential)
}
catch {
    print("Failed to create communication token credential")
}
```

Ersetzen Sie `<USER_ACCESS_TOKEN>` durch ein gültiges Benutzerzugriffstoken für Ihre Ressource. Wenn Sie noch über kein Token verfügen, finden Sie unter [Benutzerzugriffstoken](../../access-tokens.md) weitere Informationen.

### <a name="setup-token-refreshing"></a>Das Einrichtungstoken wird aktualisiert

Erstellen Sie eine `fetchTokenAsync`-Methode. Fügen Sie dann Ihre `fetchToken`-Logik hinzu, um das Benutzertoken zu erhalten.

```swift
private func fetchTokenAsync(completionHandler: @escaping TokenRefreshOnCompletion) {
    func getTokenFromServer(completionHandler: @escaping (String) -> Void) {
        completionHandler("<USER_ACCESS_TOKEN>")
    }
    getTokenFromServer { newToken in
        completionHandler(newToken, nil)
    }
}
```

Ersetzen Sie `<USER_ACCESS_TOKEN>` durch ein gültiges Benutzerzugriffstoken für Ihre Ressource.

## <a name="join-a-meeting"></a>Beitreten zu einer Besprechung

Die Methode `joinMeeting` wird als die Aktion festgelegt, die ausgeführt wird, wenn auf die Schaltfläche *Besprechung beitreten* getippt wird. Aktualisieren Sie die Implementierung, um mit `MeetingUIClient` einer Besprechung beizutreten:

```swift
private func joinMeeting() {
    let meetingJoinOptions = MeetingJoinOptions(displayName: "John Smith")
        
    meetingUIClient?.join(meetingUrl: "<MEETING_URL>", meetingJoinOptions: meetingJoinOptions, completionHandler: { (error: Error?) in
        if (error != nil) {
            print("Join meeting failed: \(error!)")
        }
    })
}
```

Ersetzen Sie `<MEETING URL>` durch einen Teams-Besprechungslink.

### <a name="get-a-teams-meeting-link"></a>Erhalten des Teams-Besprechungslinks

Ein Teams-Besprechungslink kann mithilfe der Graph-APIs abgerufen werden. Dies wird in der [Graph-Dokumentation](/graph/api/onlinemeeting-createorget?tabs=http&view=graph-rest-beta&preserve-view=true) erläutert.
Das Communication Services-SDK für Telefonie akzeptiert einen vollständigen Teams-Besprechungslink. Dieser Link wird als Teil der `onlineMeeting`-Ressource zurückgegeben, auf die Sie über die [`joinWebUrl`-Eigenschaft](/graph/api/resources/onlinemeeting?view=graph-rest-beta&preserve-view=true) zugreifen können. Sie können die erforderlichen Besprechungsinformationen auch aus der URL **Besprechung beitreten** in der Teams-Besprechungseinladung selbst abrufen.

## <a name="run-the-code"></a>Ausführen des Codes

Sie können Ihre App auf dem iOS-Simulator erstellen und ausführen, indem Sie **Product** (Produkt)  > **Run** (Ausführen) auswählen oder die Tastenkombination (&#8984;-R) verwenden.

:::image type="content" source="../media/ios/quick-start-join-meeting.png" alt-text="Ergebnis der Schnellstart-App":::

:::image type="content" source="../media/ios/quick-start-meeting-joined.png" alt-text="Endgültiges Erscheinungsbild, nachdem der Beitritt zur Besprechung erfolgt ist":::

> [!NOTE]
> Wenn Sie zum ersten Mal einer Besprechung beitreten, fordert das System Sie auf, den Zugriff auf das Mikrofon zuzulassen. Verwenden Sie in einer Produktionsanwendung die `AVAudioSession`-API[, um den Berechtigungsstatus zu überprüfen](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/requesting_access_to_protected_resources) und das Verhalten Ihrer Anwendung entsprechend zu aktualisieren, wenn die Berechtigung nicht erteilt wird.

## <a name="add-localization-support-based-on-your-app"></a>Hinzufügen unterstützter Lokalisierungen ausgehend von Ihrer App

Das Microsoft Teams-SDK unterstützt mehr als 100 Zeichenfolgen und Ressourcen. Das Framework-Paket enthält die Basissprachen und Englisch. Die restlichen Sprachen sind in der Datei aus dem `Localizations.zip`-Paket enthalten.

#### <a name="add-localizations-to-the-sdk-based-on-what-your-app-supports"></a>Fügen Sie basierend auf den von Ihrer App unterstützten Funktionen Lokalisierungen zum SDK hinzu

1. Ermitteln Sie im Xcode-Projekt der App > Info > Lokalisierungsliste, welche Art von Lokalisierungen Ihre Anwendung unterstützt
2. Entzippen Sie die im Paket enthaltene Datei Localizations.zip
3. Kopieren Sie, entsprechend den von Ihrer App unterstützten Lokalisierungen, Lokalisierungsordner aus dem entzippten Ordner in das Stammverzeichnis von TeamsAppSDK.framework

## <a name="preparation-for-app-store-upload"></a>Vorbereitung für den Upload in den App Store

Entfernen Sie im Fall der Archivierung die i386- und x86_64-Architekturen aus den Frameworks.

Wenn Sie Ihre Anwendung archivieren möchten, fügen Sie vor der Phase der Codesignierung die Architekturen `i386` und `x86_64` zum Entfernen von Skripten zu den Buildphasen hinzu.

Wählen Sie Ihr Projekt im Projektvavigator aus. Wechseln Sie im Editor-Bereich zu Buildphasen → klicken Sie auf das +-Zeichen → Neue Run Script-Phase erstellen.

```bash
echo "Target architectures: $ARCHS"
APP_PATH="${TARGET_BUILD_DIR}/${WRAPPER_NAME}"
find "$APP_PATH" -name '*.framework' -type d | while read -r FRAMEWORK
do
FRAMEWORK_EXECUTABLE_NAME=$(defaults read "$FRAMEWORK/Info.plist" CFBundleExecutable)
FRAMEWORK_EXECUTABLE_PATH="$FRAMEWORK/$FRAMEWORK_EXECUTABLE_NAME"
echo "Executable is $FRAMEWORK_EXECUTABLE_PATH"
echo $(lipo -info "$FRAMEWORK_EXECUTABLE_PATH")
FRAMEWORK_TMP_PATH="$FRAMEWORK_EXECUTABLE_PATH-tmp"
# remove simulator's archs if location is not simulator's directory
case "${TARGET_BUILD_DIR}" in
*"iphonesimulator")
    echo "No need to remove archs"
    ;;
*)
    if $(lipo "$FRAMEWORK_EXECUTABLE_PATH" -verify_arch "i386") ; then
    lipo -output "$FRAMEWORK_TMP_PATH" -remove "i386" "$FRAMEWORK_EXECUTABLE_PATH"
    echo "i386 architecture removed"
    rm "$FRAMEWORK_EXECUTABLE_PATH"
    mv "$FRAMEWORK_TMP_PATH" "$FRAMEWORK_EXECUTABLE_PATH"
    fi
    if $(lipo "$FRAMEWORK_EXECUTABLE_PATH" -verify_arch "x86_64") ; then
    lipo -output "$FRAMEWORK_TMP_PATH" -remove "x86_64" "$FRAMEWORK_EXECUTABLE_PATH"
    echo "x86_64 architecture removed"
    rm "$FRAMEWORK_EXECUTABLE_PATH"
    mv "$FRAMEWORK_TMP_PATH" "$FRAMEWORK_EXECUTABLE_PATH"
    fi
    ;;
esac
echo "Completed for executable $FRAMEWORK_EXECUTABLE_PATH"
echo $(lipo -info "$FRAMEWORK_EXECUTABLE_PATH")
done
```

## <a name="sample-code"></a>Beispielcode

Sie können die Beispiel-App von [GitHub](https://github.com/Azure-Samples/teams-embed-ios-getting-started) herunterladen.
