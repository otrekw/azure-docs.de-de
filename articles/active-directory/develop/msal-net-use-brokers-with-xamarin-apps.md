---
title: Verwenden von Brokern mit Xamarin iOS und Android | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Xamarin iOS-Anwendungen eingerichtet werden, die Microsoft Authenticator und die Microsoft Authentication Library for .NET (MSAL.NET) verwenden können. Erfahren Sie außerdem, wie Sie von der Azure AD Authentication Library for .NET (ADAL.NET) zu Microsoft Authentication Library for .NET (MSAL.NET) migrieren.
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/08/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 25b8aa9b5e80720e9543dafce7970404a62b7d1f
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77132642"
---
# <a name="use-microsoft-authenticator-or-intune-company-portal-on-xamarin-applications"></a>Verwenden von Microsoft Authenticator oder des Intune-Unternehmensportals für Xamarin-Anwendungen

Unter Android und iOS ermöglichen Broker wie Microsoft Authenticator und das Android-spezifische Microsoft Intune-Unternehmensportal Folgendes:

- **Einmaliges Anmelden (Single Sign-On, SSO)** : Die Benutzer müssen sich nicht bei jeder Anwendung anmelden.
- **Geräteidentifikation**: Der Broker greift auf das Gerätezertifikat zu. Dieses Zertifikat wird auf dem Gerät erstellt, wenn es dem Arbeitsplatz hinzugefügt wird.
- **Überprüfung der Anwendungsidentifikation**: Wenn eine Anwendung den Broker aufruft, übergibt sie ihre Umleitungs-URL. Die URL wird dann vom Broker überprüft.

Verwenden Sie zum Aktivieren einer dieser Funktionen den `WithBroker()`-Parameter, wenn Sie die `PublicClientApplicationBuilder.CreateApplication`-Methode aufrufen. Der `.WithBroker()`-Parameter ist standardmäßig auf „true“ festgelegt. 

Befolgen Sie außerdem die Anweisungen in den folgenden Abschnitten, um die Brokerauthentifizierung für [iOS](#brokered-authentication-for-ios)-Anwendungen oder [Android](#brokered-authentication-for-android)-Anwendungen einzurichten.

## <a name="brokered-authentication-for-ios"></a>Brokerauthentifizierung für iOS

Führen Sie die folgenden Schritte aus, um Ihre Xamarin.iOS-App für die Kommunikation mit der [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458)-App zu aktivieren.

### <a name="step-1-enable-broker-support"></a>Schritt 1: Aktivieren der Brokerunterstützung
Sie müssen Brokerunterstützung für einzelne Instanzen von `PublicClientApplication` aktivieren. Die Unterstützung ist standardmäßig deaktiviert. Wenn Sie `PublicClientApplication` über `PublicClientApplicationBuilder`erstellen, verwenden Sie den `WithBroker()`-Parameter, wie im folgenden Beispiel gezeigt. Der `WithBroker()`-Parameter ist standardmäßig auf „true“ festgelegt.

```csharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

### <a name="step-2-enable-keychain-access"></a>Schritt 2: Aktivieren des Keychainzugriffs

Um den Keychainzugriff zu aktivieren, müssen Sie über eine Keychain-Zugriffsgruppe für Ihre Anwendung verfügen. Sie können die `WithIosKeychainSecurityGroup()`-API verwenden, um die Keychainzugriffsgruppe festzulegen, wenn Sie die Anwendung erstellen:

```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
      
     .WithIosKeychainSecurityGroup("com.microsoft.adalcache")
     .Build();
```

Weitere Informationen finden Sie unter [Aktivieren des Keychainzugriffs](msal-net-xamarin-ios-considerations.md#enable-keychain-access).

### <a name="step-3-update-appdelegate-to-handle-the-callback"></a>Schritt 3: Aktualisieren von AppDelegate zum Verarbeiten des Rückrufs
Wenn die Microsoft Authentication Library for .NET (MSAL.NET) den Broker aufruft, führt der Broker einen Rückruf Ihrer Anwendung über die `OpenUrl`-Methode der `AppDelegate`-Klasse aus. Da MSAL auf die Antwort vom Broker wartet, muss Ihre Anwendung kooperieren, damit der Rückruf von MSAL.NET erfolgen kann. Um diese Kooperation zu ermöglichen, müssen Sie die Datei `AppDelegate.cs` zur Außerkraftsetzung der folgenden Methode aktualisieren.

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, 
                             string sourceApplication,
                             NSObject annotation)
{
    if (AuthenticationContinuationHelper.IsBrokerResponse(sourceApplication))
    {
      AuthenticationContinuationHelper.SetBrokerContinuationEventArgs(url);
      return true;
    }
    
    else if (!AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url))
    {               
         return false;                
    }
    
    return true;     
}           
```

Diese Methode wird jedes Mal aufgerufen, wenn die Anwendung gestartet wird. Sie wird verwendet, um die Antwort vom Broker zu verarbeiten und den von MSAL.NET eingeleiteten Authentifizierungsprozess abzuschließen.

### <a name="step-4-set-uiviewcontroller"></a>Schritt 4: Festlegen von „UIViewController()“
Sie müssen in der Datei `AppDelegate.cs` ein Objektfenster festlegen. Normalerweise brauchen Sie für Xamarin iOS kein Objektfenster festzulegen. Sie benötigen aber ein Objektfenster, um an den Broker senden und Antworten von ihm empfangen zu können. 

So richten Sie das Objektfenster ein: 
1. Legen Sie in der `AppDelegate.cs`-Datei `App.RootViewController` auf einen neuen `UIViewController()` fest. Mit dieser Zuweisung wird sichergestellt, dass der Aufruf an den Broker `UIViewController`enthält. Wenn diese Einstellung falsch zugewiesen wird, wird möglicherweise der folgende Fehler angezeigt:

      `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`

1. Verwenden Sie im `AcquireTokenInteractive`-Aufruf `.WithParentActivityOrWindow(App.RootViewController)`, und übergeben Sie dann den Verweis auf das Objektfenster, das Sie verwenden möchten.

    In `App.cs`:
    
    ```csharp
       public static object RootViewController { get; set; }
    ```
    
    In `AppDelegate.cs`:
    
    ```csharp
       LoadApplication(new App());
       App.RootViewController = new UIViewController();
    ```
    
    Im `AcquireToken`-Aufruf:
    
    ```csharp
    result = await app.AcquireTokenInteractive(scopes)
                 .WithParentActivityOrWindow(App.RootViewController)
                 .ExecuteAsync();
    ```

### <a name="step-5-register-a-url-scheme"></a>Schritt 5: Registrieren eines URL-Schemas
MSAL.NET verwendet URLs, um den Broker aufzurufen und die Brokerantwort dann an Ihre App zurückzugeben. Um den Roundtrip abzuschließen, registrieren Sie in der Datei `Info.plist` ein URL-Schema für Ihre App.

Der `CFBundleURLSchemes`-Name muss das Präfix `msauth.` enthalten. Stellen Sie dem Präfix `CFBundleURLName` nach. 

Im URL-Schema bezeichnet `BundleId` eindeutig die App `$"msauth.(BundleId)"`. Wenn `BundleId` also `com.yourcompany.xforms` ist, lautet das URL-Schema `msauth.com.yourcompany.xforms`.

> [!NOTE]
> Dieses URL-Schema wird Teil des Umleitungs-URI, der Ihre App eindeutig identifiziert, wenn sie die Antwort vom Broker empfängt.

```XML
 <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.yourcompany.xforms</string>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>msauth.com.yourcompany.xforms</string>
        </array>
      </dict>
    </array>
```

### <a name="step-6-add-the-broker-identifier-to-the-lsapplicationqueriesschemes-section"></a>Schritt 6: Hinzufügen der Broker-ID im Abschnitt LSApplicationQueriesSchemes
MSAL verwendet `–canOpenURL:`, um zu überprüfen, ob der Broker auf dem Gerät installiert ist. Unter iOS 9 wurden die Schemas, die von einer Anwendung abgefragt werden können, von Apple fest vorgegeben. 

Fügen Sie dem Abschnitt `LSApplicationQueriesSchemes` der `Info.plist`-Datei `msauthv2` hinzu, wie im folgenden Beispiel zu sehen:

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
      <string>msauthv3</string>
    </array>
```

### <a name="step-7-register-your-redirect-uri-in-the-application-portal"></a>Schritt 7: Registrieren des Umleitungs-URI im Anwendungsportal
Wenn Sie den Broker verwenden, gilt für den Umleitungs-URI eine zusätzliche Anforderung. Der Umleitungs-URI _muss_ das folgende Format aufweisen:
```csharp
$"msauth.{BundleId}://auth"
```

Hier sehen Sie ein Beispiel: 

```csharp
public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; 
```
Beachten Sie, dass der Umleitungs-URI mit dem `CFBundleURLSchemes`-Namen übereinstimmt, den Sie in der Datei `Info.plist` hinzugefügt haben.

### <a name="step-8-make-sure-the-redirect-uri-is-registered-with-your-app"></a>Schritt 8: Sicherstellen, dass der Umleitungs-URI bei Ihrer App registriert wird

Der Umleitungs-URI muss im [App-Registrierungsportal](https://portal.azure.com) als gültiger Umleitungs-URI für Ihre Anwendung registriert werden. 

Das App-Registrierungsportal stellt eine neue Benutzerumgebung zur Verfügung, die Sie beim Berechnen des Brokerantwort-URIs aus der Paket-ID unterstützt. 

So berechnen Sie den Umleitungs-URI:

1. Wählen Sie im App-Registrierungsportal **Authentifizierung** > **Neue Benutzeroberfläche ausprobieren** aus.

   ![Ausprobieren der neuen Benutzeroberfläche für die App-Registrierung](media/msal-net-use-brokers-with-xamarin-apps/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

1. Wählen Sie **Eine Plattform hinzufügen** aus.

   ![Hinzufügen einer Plattform](media/msal-net-use-brokers-with-xamarin-apps/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

1. Wenn die Liste der Plattformen unterstützt wird, wählen Sie **iOS** aus.

   ![Konfigurieren von iOS](media/msal-net-use-brokers-with-xamarin-apps/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

1. Geben Sie die Paket-ID wie angefordert ein, und wählen Sie dann **Konfigurieren** aus.

   ![Geben Sie die Paket-ID ein.](media/msal-net-use-brokers-with-xamarin-apps/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

Wenn Sie die Schritte abgeschlossen haben, wird der Umleitungs-URI berechnet.

![Kopieren des Umleitungs-URI](media/msal-net-use-brokers-with-xamarin-apps/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

## <a name="brokered-authentication-for-android"></a>Brokerauthentifizierung für Android

MSAL.NET unterstützt nur die Xamarin.iOS-Plattform. Broker für die Xamarin.Android-Plattform werden noch nicht unterstützt.

Die native MSAL-Android-Bibliothek unterstützt bereits Brokerauthentifizierung. Weitere Informationen finden Sie unter [Brokerauthentifizierung unter Android](brokered-auth.md).

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über [Überlegungen zur Verwendung der Universellen Windows-Plattform mit MSAL.NET](msal-net-uwp-considerations.md).
