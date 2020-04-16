---
title: 'Tutorial: MSAL für iOS und macOS – Microsoft Identity Platform | Azure'
description: Hier erfahren Sie, wie iOS- und macOS-Apps (Swift) über Microsoft Identity Platform eine API aufrufen, für die Zugriffstoken erforderlich sind
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 08/30/2019
ms.author: jmprieur
ms.reviewer: oldalton
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 0c807390f043527aa721e9c1c2a9269ff0dee6b9
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80990972"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-or-macos-app"></a>Anmelden von Benutzern und Aufrufen von Microsoft Graph aus einer iOS- oder macOS-App

In diesem Tutorial wird die Integration einer iOS- oder macOS-App in Microsoft Identity Platform beschrieben. Die App meldet einen Benutzer an, ruft ein Zugriffstoken zum Aufrufen der Microsoft Graph-API ab und sendet eine Anforderung an die Microsoft Graph-API.  

Am Ende dieses Leitfadens akzeptiert Ihre Anwendung Anmeldungen von persönlichen Microsoft-Konten (z.B. outlook.com, live.com u.a.) sowie von Geschäfts,- Schul- oder Unikonten von allen Unternehmen oder Organisationen, die Azure Active Directory nutzen.

>[!NOTE]
> Wenn Sie mit Microsoft Identity Platform noch nicht vertraut sind, wird empfohlen, mit dem Schnellstart [Anmelden von Benutzern und Aufrufen der Microsoft Graph-API aus einer iOS- oder macOS-App](quickstart-v2-ios.md) zu beginnen.

## <a name="how-this-tutorial-works"></a>So funktioniert dieses Tutorial

![Zeigt, wie die in diesem Tutorial generierte Beispiel-App funktioniert](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

Mit der App in diesem Tutorial werden Benutzer angemeldet und Daten in ihrem Namen abgerufen.  Auf diese Daten wird über eine geschützte API (hier: Microsoft Graph-API) zugegriffen, für die eine Autorisierung erforderlich ist und die über Microsoft Identity Platform geschützt ist.

Dies gilt insbesondere in folgenden Fällen:

* Die App meldet den Benutzer entweder über einen Browser oder über Microsoft Authenticator an.
* Der Endbenutzer akzeptiert die von Ihrer Anwendung angeforderten Berechtigungen.
* Ihre App stellt ein Zugriffstoken für die Microsoft Graph-API aus.
* Das Zugriffstoken ist in der HTTP-Anforderung an die Web-API enthalten.
* Die Microsoft Graph-Antwort wird verarbeitet.

Dieses Beispiel verwendet die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) zum Implementieren der Authentifizierung. MSAL erneuert Token automatisch, ermöglicht das einmalige Anmelden zwischen anderen Apps auf dem Gerät und verwaltet die Konten.

Dieses Tutorial gilt für iOS- und macOS-Apps. Bei diesen beiden Plattformen unterscheiden sich einige Schritte voneinander. 

## <a name="prerequisites"></a>Voraussetzungen

- Zum Erstellen der App in diesem Leitfaden ist mindestens Version 11.x von Xcode erforderlich. Sie können XCode von der [iTunes-Website](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode-URL zum Herunterladen") herunterladen.
- Microsoft Authentication Library ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). Sie können einen Abhängigkeitsmanager verwenden oder die Bibliothek manuell hinzufügen. In den nachstehenden Anweisungen wird die Vorgehensweise erläutert.

In diesem Tutorial wird ein neues Projekt erstellt. Wenn Sie stattdessen das abgeschlossene Tutorial herunterladen möchten, laden Sie den Code herunter:
- [iOS-Beispielcode](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)
- [macOS-Beispielcode](https://github.com/Azure-Samples/active-directory-macOS-swift-native-v2/archive/master.zip)

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts

1. Starten Sie Xcode, und wählen Sie **Create a new Xcode project** (Neues Xcode-Projekt erstellen) aus.
2. Für iOS-Apps: Wählen Sie **iOS** > **Single view App** (Einzelansicht-App) und dann **Next** (Weiter) aus.
3. Für macOS-Aps: Wählen Sie **macOS** > **Cocoa-App** und anschließend **Next** (Weiter) aus.
4. Geben Sie einen Produktnamen ein.
5. Legen Sie die **Sprache** auf **Swift** fest, und wählen Sie **Next** (Weiter) aus.
6. Wählen Sie einen Ordner für das Erstellen Ihrer App aus, und klicken Sie auf **Erstellen**.

## <a name="register-your-application"></a>Anwendung registrieren

1. [Navigieren Sie zum Azure-Portal](https://aka.ms/MobileAppReg).
2. Öffnen Sie das Blatt „App-Registrierungen“, und klicken Sie auf **+ Neue Registrierung**.
3. Geben Sie unter **Name** einen Namen für Ihre App ein, und legen Sie keinen Umleitungs-URI fest.
4. Wählen Sie unter **Unterstützte Kontotypen** die Option **Accounts in any organizational directory (Any Azure AD directory - Multitenant) and personal Microsoft accounts (e.g. Skype, Xbox)** (Konten in allen Organisationsverzeichnissen (beliebiges Azure AD-Verzeichnis: mehrere Mandanten) und persönliche Microsoft-Konten (z. B. Skype, Xbox)) aus.
5. Klicken Sie auf **Registrieren**.
6. Wählen Sie im Abschnitt **Verwalten** des angezeigten Bereichs die Option **Authentifizierung** aus.

7. Klicken Sie im oberen Bildschirmbereich auf **Neue Benutzeroberfläche ausprobieren**, um die neue Benutzeroberfläche für die App-Registrierung zu öffnen, und klicken Sie dann auf **+Neue Registrierung** >  **+ Plattform hinzufügen** > **iOS/macOS**.
    - Geben Sie die Bündel-ID Ihres Projekts ein. Wenn Sie den Code heruntergeladen haben, lautet diese `com.microsoft.identitysample.MSALiOS`. Wählen Sie bei der Erstellung eines eigenen Projekts Ihr Projekt in Xcode aus, und öffnen Sie die Registerkarte **General** (Allgemein). Die Paket-ID wird im Abschnitt **Identity** (Identität) angezeigt.
8. Klicken Sie auf `Configure`, und speichern Sie die **MSAL-Konfiguration**, die auf der Seite **MSAL-Konfiguration** angezeigt wird, damit Sie diese später beim Konfigurieren Ihrer App eingeben können.  Klicken Sie auf **Fertig**.

## <a name="add-msal"></a>Hinzufügen von MSAL

Installieren Sie MSAL mit einer der folgenden Optionen in Ihrer App:

### <a name="cocoapods"></a>CocoaPods

1. Wenn Sie [CocoaPods](https://cocoapods.org/) verwenden, installieren Sie `MSAL`. Erstellen Sie dazu zunächst in dem Ordner, in dem sich auch die Datei vom Typ `.xcodeproj` Ihres Projekts befindet, eine leere Datei namens `podfile`. Fügen Sie Folgendes zu `podfile` hinzu:

   ```
   use_frameworks!
   
   target '<your-target-here>' do
      pod 'MSAL'
   end
   ```

2. Ersetzen Sie `<your-target-here>` durch den Namen Ihres Projekts.
3. Navigieren Sie in einem Terminalfenster zu dem Ordner, der das erstellte `podfile`-Element enthält, und führen Sie `pod install` zum Installieren von MSAL aus.
4. Schließen Sie Xcode, und öffnen Sie `<your project name>.xcworkspace`, um das Projekt erneut in Xcode zu laden.

### <a name="carthage"></a>Carthage

Wenn Sie [Carthage](https://github.com/Carthage/Carthage) verwenden, installieren Sie `MSAL`, indem Sie die Bibliothek Ihrer Datei vom Typ `Cartfile` hinzufügen:

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

Führen Sie in einem Terminalfenster in dem Verzeichnis, in dem sich auch die aktualisierte Datei vom Typ `Cartfile` befindet, den folgenden Befehl aus, damit Carthage die Abhängigkeiten in Ihrem Projekt aktualisiert.

iOS:

```bash
carthage update --platform iOS
```

macOS:

```bash
carthage update --platform macOS
```

### <a name="manually"></a>Manuell

Sie können auch ein Git-Submodul oder das neueste Release als Framework in Ihrer Anwendung verwenden.

## <a name="add-your-app-registration"></a>Hinzufügen der App-Registrierung

Als Nächstes fügen Sie dem Code die App-Registrierung hinzu. 

Fügen Sie zunächst am Anfang der Dateien `ViewController.swift` und `AppDelegate.swift` bzw. `SceneDelegate.swift` die folgende import-Anweisung hinzu:

```swift
import MSAL
```

Fügen Sie anschließend `ViewController.swift` vor `viewDidLoad()` den folgenden Code hinzu:

```swift
// Update the below to your client ID you received in the portal. The below is for running the demo only
let kClientID = "Your_Application_Id_Here"
let kGraphEndpoint = "https://graph.microsoft.com/" // the Microsoft Graph endpoint
let kAuthority = "https://login.microsoftonline.com/common" // this authority allows a personal Microsoft account and a work or school account in any organization’s Azure AD tenant to sign in
    
let kScopes: [String] = ["user.read"] // request permission to read the profile of the signed-in user
    
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
var webViewParameters : MSALWebviewParameters?
var currentAccount: MSALAccount?
```

Oben müssen Sie nur den Wert ändern, der `kClientID` als [Anwendungs-ID](https://docs.microsoft.com/azure/active-directory/develop/developer-glossary#application-id-client-id) zugewiesen werden soll. Dieser Wert ist Teil der MSAL-Konfigurationsdaten, die Sie am Anfang dieses Tutorials zum Registrieren der Anwendung im Azure-Portal gespeichert haben.

## <a name="for-ios-only-configure-url-schemes"></a>Nur iOS: Konfigurieren von URL-Schemas

In diesem Schritt registrieren Sie `CFBundleURLSchemes`, damit der Benutzer nach der Anmeldung zurück zur App geleitet werden kann. `LSApplicationQueriesSchemes` ermöglicht Ihrer App darüber hinaus die Verwendung von Microsoft Authenticator.

Öffnen Sie `Info.plist` in Xcode als Quellcodedatei, und fügen Sie im Abschnitt `<dict>` Folgendes hinzu. Ersetzen Sie `[BUNDLE_ID]` durch den im Azure-Portal verwendeten Wert. Haben Sie den Code heruntergeladen, lautet dieser Wert `com.microsoft.identitysample.MSALiOS`. Wählen Sie bei der Erstellung eines eigenen Projekts Ihr Projekt in Xcode aus, und öffnen Sie die Registerkarte **General** (Allgemein). Die Paket-ID wird im Abschnitt **Identity** (Identität) angezeigt.

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msauth.[BUNDLE_ID]</string>
        </array>
    </dict>
</array>
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>msauthv2</string>
    <string>msauthv3</string>
</array>
```

## <a name="for-macos-only-configure-app-sandbox"></a>Nur macOS: Konfigurieren der App-Sandbox

1. Rufen Sie Ihr Xcode-Projekt auf, und klicken Sie auf „Einstellungen“ und dann auf die Registerkarte **Capabilities** (Funktionen) > **App-Sandbox**.
2. Aktivieren Sie das Kontrollkästchen **Outgoing Connections (Client)** (Ausgehende Verbindungen (Clients)). 

## <a name="create-your-apps-ui"></a>Erstellen der Benutzeroberfläche Ihrer App

Erstellen Sie nun eine Benutzeroberfläche mit einer Schaltfläche zum Aufrufen der Microsoft Graph-API, einer Schaltfläche zum Abmelden und einer Textansicht zum Anzeigen von Ausgaben. Fügen Sie dazu der Klasse `ViewController` den folgenden Code hinzu:

### <a name="ios-ui"></a>iOS-Benutzeroberfläche

```swift
var loggingText: UITextView!
var signOutButton: UIButton!
var callGraphButton: UIButton!
var usernameLabel: UILabel!

func initUI() {
        
    usernameLabel = UILabel()
    usernameLabel.translatesAutoresizingMaskIntoConstraints = false
    usernameLabel.text = ""
    usernameLabel.textColor = .darkGray
    usernameLabel.textAlignment = .right
        
    self.view.addSubview(usernameLabel)
        
    usernameLabel.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
    usernameLabel.rightAnchor.constraint(equalTo: view.rightAnchor, constant: -10.0).isActive = true
    usernameLabel.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
    usernameLabel.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
    // Add call Graph button
    callGraphButton  = UIButton()
    callGraphButton.translatesAutoresizingMaskIntoConstraints = false
    callGraphButton.setTitle("Call Microsoft Graph API", for: .normal)
    callGraphButton.setTitleColor(.blue, for: .normal)
    callGraphButton.addTarget(self, action: #selector(callGraphAPI(_:)), for: .touchUpInside)
    self.view.addSubview(callGraphButton)
        
    callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 120.0).isActive = true
    callGraphButton.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
    callGraphButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
    // Add sign out button
    signOutButton = UIButton()
    signOutButton.translatesAutoresizingMaskIntoConstraints = false
    signOutButton.setTitle("Sign Out", for: .normal)
    signOutButton.setTitleColor(.blue, for: .normal)
    signOutButton.setTitleColor(.gray, for: .disabled)
    signOutButton.addTarget(self, action: #selector(signOut(_:)), for: .touchUpInside)
    self.view.addSubview(signOutButton)
        
    signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
    signOutButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
    signOutButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
    let deviceModeButton = UIButton()
    deviceModeButton.translatesAutoresizingMaskIntoConstraints = false
    deviceModeButton.setTitle("Get device info", for: .normal);
    deviceModeButton.setTitleColor(.blue, for: .normal);
    deviceModeButton.addTarget(self, action: #selector(getDeviceMode(_:)), for: .touchUpInside)
    self.view.addSubview(deviceModeButton)
        
    deviceModeButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    deviceModeButton.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
    deviceModeButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
    deviceModeButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
    // Add logging textfield
    loggingText = UITextView()
    loggingText.isUserInteractionEnabled = false
    loggingText.translatesAutoresizingMaskIntoConstraints = false
        
    self.view.addSubview(loggingText)
        
    loggingText.topAnchor.constraint(equalTo: deviceModeButton.bottomAnchor, constant: 10.0).isActive = true
    loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
    loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: -10.0).isActive = true
    loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: 10.0).isActive = true
}

func platformViewDidLoadSetup() {
                
    NotificationCenter.default.addObserver(self,
                        selector: #selector(appCameToForeGround(notification:)),
                        name: UIApplication.willEnterForegroundNotification,
                        object: nil)
        
}
    
@objc func appCameToForeGround(notification: Notification) {
    self.loadCurrentAccount()
}

```

### <a name="macos-ui"></a>macOS-Benutzeroberfläche

```swift

var callGraphButton: NSButton!
var loggingText: NSTextView!
var signOutButton: NSButton!
    
var usernameLabel: NSTextField!

func initUI() {
        
    usernameLabel = NSTextField()
    usernameLabel.translatesAutoresizingMaskIntoConstraints = false
    usernameLabel.stringValue = ""
    usernameLabel.isEditable = false
    usernameLabel.isBezeled = false
    self.view.addSubview(usernameLabel)
        
    usernameLabel.topAnchor.constraint(equalTo: view.topAnchor, constant: 30.0).isActive = true
    usernameLabel.rightAnchor.constraint(equalTo: view.rightAnchor, constant: -10.0).isActive = true
        
    // Add call Graph button
    callGraphButton  = NSButton()
    callGraphButton.translatesAutoresizingMaskIntoConstraints = false
    callGraphButton.title = "Call Microsoft Graph API"
    callGraphButton.target = self
    callGraphButton.action = #selector(callGraphAPI(_:))
    callGraphButton.bezelStyle = .rounded
    self.view.addSubview(callGraphButton)
        
    callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
    callGraphButton.heightAnchor.constraint(equalToConstant: 34.0).isActive = true
        
    // Add sign out button
    signOutButton = NSButton()
    signOutButton.translatesAutoresizingMaskIntoConstraints = false
    signOutButton.title = "Sign Out"
    signOutButton.target = self
    signOutButton.action = #selector(signOut(_:))
    signOutButton.bezelStyle = .texturedRounded
    self.view.addSubview(signOutButton)
        
    signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
    signOutButton.heightAnchor.constraint(equalToConstant: 34.0).isActive = true
    signOutButton.isEnabled = false
        
    // Add logging textfield
    loggingText = NSTextView()
    loggingText.translatesAutoresizingMaskIntoConstraints = false
        
    self.view.addSubview(loggingText)
        
    loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
    loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
    loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: -10.0).isActive = true
    loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: -10.0).isActive = true
    loggingText.widthAnchor.constraint(equalToConstant: 500.0).isActive = true
    loggingText.heightAnchor.constraint(equalToConstant: 300.0).isActive = true
}

func platformViewDidLoadSetup() {}

```

Ersetzen Sie anschließend ebenfalls innerhalb der Klasse `ViewController` die Methode `viewDidLoad()` durch Folgendes:

```swift
    override func viewDidLoad() {

        super.viewDidLoad()

        initUI()
        
        do {
            try self.initMSAL()
        } catch let error {
            self.updateLogging(text: "Unable to create Application Context \(error)")
        }
        
        self.loadCurrentAccount()
        self.platformViewDidLoadSetup()
    }
```

## <a name="use-msal"></a>Verwendung von MSAL

### <a name="initialize-msal"></a>MSAL initialisieren

Fügen Sie der `ViewController`-Klasse die folgende `initMSAL`-Methode hinzu:

```swift
    func initMSAL() throws {
        
        guard let authorityURL = URL(string: kAuthority) else {
            self.updateLogging(text: "Unable to create authority URL")
            return
        }
        
        let authority = try MSALAADAuthority(url: authorityURL)
        
        let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
        self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
        self.initWebViewParams()
    }
```

Fügen Sie der Klasse `ViewController` nach der Methode `initMSAL` Folgendes hinzu:

### <a name="ios-code"></a>iOS-Code:

```swift
func initWebViewParams() {
        self.webViewParameters = MSALWebviewParameters(parentViewController: self)
    }
```

### <a name="macos-code"></a>macOS-Code:

```swift
func initWebViewParams() {
        self.webViewParameters = MSALWebviewParameters()
    }
```

### <a name="for-ios-only-handle-the-sign-in-callback"></a>Nur iOS: Behandeln des Anmelderückrufs

Öffnen Sie die Datei `AppDelegate.swift` . Zur Verarbeitung des Rückrufs nach der Anmeldung fügen Sie `MSALPublicClientApplication.handleMSALResponse` wie folgt der Klasse `appDelegate` hinzu:

```swift
// Inside AppDelegate...
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
}

```

**Bei Verwendung von Xcode 11** müssen Sie den MSAL-Rückruf stattdessen in `SceneDelegate.swift` platzieren.
Wenn Sie zur Erzielung von Kompatibilität mit älteren iOS-Versionen sowohl „UISceneDelegate“ als auch „UIApplicationDelegate“ unterstützen, muss der MSAL-Rückruf in beide Dateien eingefügt werden.

```swift
func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
        
        guard let urlContext = URLContexts.first else {
            return
        }
        
        let url = urlContext.url
        let sourceApp = urlContext.options.sourceApplication
        
        MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApp)
    }
```

#### <a name="acquire-tokens"></a>Abrufen von Token

Jetzt können Sie die Verarbeitungslogik der Anwendungsbenutzeroberfläche implementieren und Token interaktiv über MSAL beziehen.

MSAL macht zwei primäre Methoden zum Abrufen von Token verfügbar: `acquireTokenSilently()` und `acquireTokenInteractively()`: 

- `acquireTokenSilently()` versucht, einen Benutzer anzumelden und Token ohne Benutzerinteraktion abzurufen, wenn ein Konto vorhanden ist. `acquireTokenSilently()` erfordert die Angabe eines gültigen `MSALAccount`-Elements, das mit einer der APIs für die MSAL-Kontoenumeration abgerufen werden kann. In diesem Beispiel wird `applicationContext.getCurrentAccount(with: msalParameters, completionBlock: {})` zum Abrufen des aktuellen Kontos verwendet. 

- `acquireTokenInteractively()` zeigt stets die Benutzeroberfläche an, wenn versucht wird, den Benutzer anzumelden. Möglicherweise werden jedoch Sitzungscookies im Browser oder ein Konto in Microsoft Authenticator verwendet, um eine interaktive Anmeldung per SSO bereitzustellen.

Fügen Sie der `ViewController`-Klasse den folgenden Code hinzu:

```swift
    func getGraphEndpoint() -> String {
        return kGraphEndpoint.hasSuffix("/") ? (kGraphEndpoint + "v1.0/me/") : (kGraphEndpoint + "/v1.0/me/");
    }

    @objc func callGraphAPI(_ sender: AnyObject) {
        
        self.loadCurrentAccount { (account) in
            
            guard let currentAccount = account else {
                
                // We check to see if we have a current logged in account.
                // If we don't, then we need to sign someone in.
                self.acquireTokenInteractively()
                return
            }
            
            self.acquireTokenSilently(currentAccount)
        }
    }

    typealias AccountCompletion = (MSALAccount?) -> Void

    func loadCurrentAccount(completion: AccountCompletion? = nil) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        let msalParameters = MSALParameters()
        msalParameters.completionBlockQueue = DispatchQueue.main
                
        applicationContext.getCurrentAccount(with: msalParameters, completionBlock: { (currentAccount, previousAccount, error) in
            
            if let error = error {
                self.updateLogging(text: "Couldn't query current account with error: \(error)")
                return
            }
            
            if let currentAccount = currentAccount {
                
                self.updateLogging(text: "Found a signed in account \(String(describing: currentAccount.username)). Updating data for that account...")
                
                self.updateCurrentAccount(account: currentAccount)
                
                if let completion = completion {
                    completion(self.currentAccount)
                }
                
                return
            }
            
            self.updateLogging(text: "Account signed out. Updating UX")
            self.accessToken = ""
            self.updateCurrentAccount(account: nil)
            
            if let completion = completion {
                completion(nil)
            }
        })
    }
```

#### <a name="get-a-token-interactively"></a>Interaktives Abrufen eines Tokens

Der folgende Code ruft ein Token zum ersten Mal ab, indem ein `MSALInteractiveTokenParameters`-Objekt erstellt und `acquireToken` aufgerufen wird. Als Nächstes fügen Sie Code hinzu, der folgende Aktionen ausführt:

1. Erstellen von `MSALInteractiveTokenParameters` mit Bereichen
2. Aufrufen von `acquireToken()` mit den erstellten Parametern
3. Behandeln von Fehlern. Weitere Informationen finden Sie im [MSAL-Leitfaden zur Fehlerbehandlung für iOS und macOS](msal-handling-exceptions.md).
4. Verarbeiten der erfolgreichen Situation

Fügen Sie der `ViewController` -Klasse den folgenden Code hinzu.

```swift
func acquireTokenInteractively() {
        
    guard let applicationContext = self.applicationContext else { return }
    guard let webViewParameters = self.webViewParameters else { return }
        
    // #1
    let parameters = MSALInteractiveTokenParameters(scopes: kScopes, webviewParameters: webViewParameters)
    parameters.promptType = .selectAccount
        
    // #2
    applicationContext.acquireToken(with: parameters) { (result, error) in
            
        // #3
        if let error = error {
                
            self.updateLogging(text: "Could not acquire token: \(error)")
            return
        }
            
        guard let result = result else {
                
            self.updateLogging(text: "Could not acquire token: No result returned")
            return
        }
            
        // #4
        self.accessToken = result.accessToken
        self.updateLogging(text: "Access token is \(self.accessToken)")
        self.updateCurrentAccount(account: result.account)
        self.getContentWithToken()
    }
}    
```


#### <a name="get-a-token-silently"></a>Automatisches Abrufen eines Tokens

Fügen Sie der Klasse `ViewController` den folgenden Code hinzu, um automatisch ein aktualisiertes Token abzurufen. Ein `MSALSilentTokenParameters`-Objekt wird erstellt, und `acquireTokenSilent()` wird aufgerufen:

```swift
    
    func acquireTokenSilently(_ account : MSALAccount!) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        /**
         
         Acquire a token for an existing account silently
         
         - forScopes:           Permissions you want included in the access token received
         in the result in the completionBlock. Not all scopes are
         guaranteed to be included in the access token returned.
         - account:             An account object that we retrieved from the application object before that the
         authentication flow will be locked down to.
         - completionBlock:     The completion block that will be called when the authentication
         flow completes, or encounters an error.
         */
        
        let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
        
        applicationContext.acquireTokenSilent(with: parameters) { (result, error) in
            
            if let error = error {
                
                let nsError = error as NSError
                
                // interactionRequired means we need to ask the user to sign-in. This usually happens
                // when the user's Refresh Token is expired or if the user has changed their password
                // among other possible reasons.
                
                if (nsError.domain == MSALErrorDomain) {
                    
                    if (nsError.code == MSALError.interactionRequired.rawValue) {
                        
                        DispatchQueue.main.async {
                            self.acquireTokenInteractively()
                        }
                        return
                    }
                }
                
                self.updateLogging(text: "Could not acquire token silently: \(error)")
                return
            }
            
            guard let result = result else {
                
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```

### <a name="call-the-microsoft-graph-api"></a>Aufrufen der Microsoft Graph-API 

Sobald Sie ein Token erhalten haben, kann Ihre App es im HTTP-Header verwenden, um eine autorisierte Anforderung an Microsoft Graph zu stellen:

| Headerschlüssel    | value                 |
| ------------- | --------------------- |
| Authorization | Bearer \<access-token> |

Fügen Sie der `ViewController`-Klasse den folgenden Code hinzu:

```swift
    func getContentWithToken() {
        
        // Specify the Graph API endpoint
        let graphURI = getGraphEndpoint()
        let url = URL(string: graphURI)
        var request = URLRequest(url: url!)
        
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
        
        URLSession.shared.dataTask(with: request) { data, response, error in
            
            if let error = error {
                self.updateLogging(text: "Couldn't get graph result: \(error)")
                return
            }
            
            guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
                
                self.updateLogging(text: "Couldn't deserialize result JSON")
                return
            }
            
            self.updateLogging(text: "Result from Graph: \(result))")
            
            }.resume()
    }
```

Weitere Informationen zur Microsoft Graph-API finden Sie [hier](https://graph.microsoft.com).

### <a name="use-msal-for-sign-out"></a>Verwenden von MSAL zur Abmeldung

Als Nächstes fügen Sie Unterstützung für die Abmeldung hinzu.

> [!Important]
> Die Abmeldung mit MSAL entfernt alle bekannten Informationen über einen Benutzer aus der Anwendung sowie eine aktive Sitzung auf seinem Gerät, wenn dies durch die Gerätekonfiguration zugelassen wird. Optional können Sie den Benutzer auch über den Browser abmelden.

Fügen Sie zum Hinzufügen einer Abmeldefunktion den folgenden Code in die `ViewController`-Klasse ein. 

```swift 
@objc func signOut(_ sender: AnyObject) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache
             */
            
            let signoutParameters = MSALSignoutParameters(webviewParameters: self.webViewParameters!)
            signoutParameters.signoutFromBrowser = false // set this to true if you also want to signout from browser or webview
            
            applicationContext.signout(with: account, signoutParameters: signoutParameters, completionBlock: {(success, error) in
                
                if let error = error {
                    self.updateLogging(text: "Couldn't sign out account with error: \(error)")
                    return
                }
                
                self.updateLogging(text: "Sign out completed successfully")
                self.accessToken = ""
                self.updateCurrentAccount(account: nil)
            })
            
        }
    }
```

### <a name="enable-token-caching"></a>Aktivieren der Zwischenspeicherung von Token

MSAL speichert die Token Ihrer App standardmäßig in der iOS- oder macOS-Keychain zwischen. 

So aktivieren Sie die Zwischenspeicherung von Token:
1. Stellen Sie sicher, dass Ihre Anwendung ordnungsgemäß signiert ist.
2. Rufen Sie Ihr Xcode-Projekt auf, und klicken Sie auf „Settings“ (Einstellungen) und dann auf die Registerkarte **Capabilities** (Funktionen) > **Enable Keychain Sharing** (Keychain-Freigabe aktivieren).
3. Klicken Sie auf **+** , und geben Sie den folgenden Eintrag für **Keychain Groups** (Schlüsselbundgruppen) an: 3.a Geben Sie für iOS `com.microsoft.adalcache` ein. 3.b Geben Sie für macOS `com.microsoft.identity.universalstorage` ein.

### <a name="add-helper-methods"></a>Hinzufügen von Hilfsmethoden
Fügen Sie der Klasse `ViewController` die folgenden Hilfsmethoden hinzu, um das Beispiel abzuschließen.

### <a name="ios-ui"></a>iOS-Benutzeroberfläche:

``` swift
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.text = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.text = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
    
    func updateAccountLabel() {
        
        guard let currentAccount = self.currentAccount else {
            self.usernameLabel.text = "Signed out"
            return
        }
        
        self.usernameLabel.text = currentAccount.username
    }
    
    func updateCurrentAccount(account: MSALAccount?) {
        self.currentAccount = account
        self.updateAccountLabel()
        self.updateSignOutButton(enabled: account != nil)
    }
```

### <a name="macos-ui"></a>macOS-Benutzeroberfläche:

```swift
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.string = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.string = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
    
     func updateAccountLabel() {

         guard let currentAccount = self.currentAccount else {
            self.usernameLabel.stringValue = "Signed out"
            return
        }

        self.usernameLabel.stringValue = currentAccount.username ?? ""
        self.usernameLabel.sizeToFit()
     }

     func updateCurrentAccount(account: MSALAccount?) {
        self.currentAccount = account
        self.updateAccountLabel()
        self.updateSignOutButton(enabled: account != nil)
    }
```

### <a name="for-ios-only-get-additional-device-information"></a>Abrufen zusätzlicher Geräteinformationen (nur bei iOS)

Verwenden Sie den folgenden Code zum Lesen der aktuellen Gerätekonfiguration, einschließlich Informationen dazu, ob das Gerät als freigegeben konfiguriert ist

```swift
    @objc func getDeviceMode(_ sender: AnyObject) {
        
        if #available(iOS 13.0, *) {
            self.applicationContext?.getDeviceInformation(with: nil, completionBlock: { (deviceInformation, error) in
                
                guard let deviceInfo = deviceInformation else {
                    self.updateLogging(text: "Device info not returned. Error: \(String(describing: error))")
                    return
                }
                
                let isSharedDevice = deviceInfo.deviceMode == .shared
                let modeString = isSharedDevice ? "shared" : "private"
                self.updateLogging(text: "Received device info. Device is in the \(modeString) mode.")
            })
        } else {
            self.updateLogging(text: "Running on older iOS. GetDeviceInformation API is unavailable.")
        }
    }
```

### <a name="multi-account-applications"></a>Anwendungen mit mehreren Konten

Diese App ist für ein Szenario mit einem einzelnen Konto konzipiert. MSAL unterstützt auch Szenarien mit mehreren Konten, in diesem Fall müssen die Apps jedoch einige zusätzliche Schritte ausführen. Sie müssen eine Benutzeroberfläche erstellen, um dem Benutzer bei der Auswahl des Kontos zu helfen, das er für die einzelnen Aktionen verwenden möchte, die Token erfordern. Als Alternative kann Ihre Anwendung eine Heuristik implementieren, um das zu verwendende Konto durch Abfragen aller Konten über MSAL auszuwählen. Siehe beispielsweise die [API](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALPublicClientApplication.html#/c:objc(cs)MSALPublicClientApplication(im)accountsFromDeviceForParameters:completionBlock:) für `accountsFromDeviceForParameters:completionBlock:`

## <a name="test-your-app"></a>Testen Ihrer App

### <a name="run-locally"></a>Lokales Ausführen

Erstellen Sie die App, und stellen Sie sie auf einem Testgerät oder in einem Simulator bereit. Sie sollten sich anmelden und Token für Azure AD oder persönliche Microsoft-Konten abrufen können.

Wenn sich ein Benutzer zum ersten Mal bei Ihrer App anmeldet, wird er von Microsoft aufgefordert, den angeforderten Berechtigungen zuzustimmen.  Die meisten Benutzer können zustimmen, allerdings haben einige Azure AD-Mandanten die Zustimmung durch Benutzer deaktiviert, sodass Administratoren im Namen aller Benutzer zustimmen müssen. Zur Unterstützung dieses Szenarios müssen Sie die Bereiche Ihrer App im Azure-Portal registrieren.

Nach der Anmeldung zeigt die App die vom Microsoft Graph-Endpunkt `/me` zurückgegebenen Daten an.

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Sollten Probleme mit diesem Tutorial oder mit Microsoft Identity Platform auftreten, besuchen Sie die Seite [Hilfe und Support](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options).
