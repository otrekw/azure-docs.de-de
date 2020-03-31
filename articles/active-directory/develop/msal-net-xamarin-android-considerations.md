---
title: Überlegungen zu Xamarin Android (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über Überlegungen zur Verwendung von Xamarin Android mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81b55253d757f641979c6f72001803d7d38d9af3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132512"
---
# <a name="considerations-for-using-xamarin-android-with-msalnet"></a>Überlegungen zur Verwendung von Xamarin Android mit MSAL.NET
In diesem Artikel wird erörtert, was Sie beim Verwenden von Xamarin Android mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET) berücksichtigen sollten.

## <a name="set-the-parent-activity"></a>Festlegen der übergeordneten Aktivität

Legen Sie in Xamarin Android die übergeordnete Aktivität fest, sodass das Token nach der Interaktion zurückgegeben wird. Hier sehen Sie ein Codebeispiel:

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```

In MSAL 4.2 und höher können Sie diese Funktionalität auch auf der `PublicClientApplication`-Ebene festlegen. Verwenden Sie zu diesem Zweck einen Rückruf:

```csharp
// Requires MSAL.NET 4.2 or later
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => parentActivity)
  .Build();
```

Wenn Sie [CurrentActivityPlugin](https://github.com/jamesmontemagno/CurrentActivityPlugin) verwenden, sieht Ihr `PublicClientApplication`-Generatorcode wie das folgende Beispiel aus.

```csharp
// Requires MSAL.NET 4.2 or later
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```

## <a name="ensure-that-control-returns-to-msal"></a>Sicherstellen, dass die Steuerung wieder an MSAL übergeben wird 
Wenn der interaktive Teil des Authentifizierungsablaufs endet, achten Sie darauf, dass die Steuerung wieder an MSAL übergeben wird. Überschreiben Sie unter Android die `OnActivityResult`-Methode von `Activity`. Rufen Sie dann die `SetAuthenticationContinuationEventArgs`-Methode der `AuthenticationContinuationHelper`-MSAL-Klasse auf. 

Hier sehen Sie ein Beispiel:

```csharp
protected override void OnActivityResult(int requestCode, 
                                         Result resultCode, Intent data)
{
 base.OnActivityResult(requestCode, resultCode, data);
 AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode,
                                                                         resultCode,
                                                                         data);
}

```

Durch diese Zeile wird sichergestellt, dass die Steuerung nach Ende des interaktiven Teils des Authentifizierungsablaufs an die MSAL-Bibliothek zurückgegeben wird.

## <a name="update-the-android-manifest"></a>Aktualisieren des Android-Manifests
Die *AndroidManifest.xml*-Datei sollte die folgenden Werte enthalten:

<!--Intent filter to capture System Browser or Authenticator calling back to our app after sign-in-->
```
  <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
     <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="msauth"
                android:host="Enter_the_Package_Name"
                android:path="/Enter_the_Signature_Hash" />
     </intent-filter>
 </activity>
```

Ersetzen Sie den Wert `android:host=` durch den Paketnamen, den Sie im Azure-Portal registriert haben. Ersetzen Sie den Wert `android:path=` durch den Schlüsselhash, den Sie im Azure-Portal registriert haben. Der Signaturhash darf *nicht* URL-codiert sein. Der Signaturhash muss mit einem führenden Schrägstrich (`/`) beginnen.

Erstellen Sie alternativ [die Aktivität in Code](https://docs.microsoft.com/xamarin/android/platform/android-manifest#the-basics), statt *AndroidManifest.xml* manuell zu bearbeiten. Um die Aktivität im Code zu erstellen, erstellen Sie zuerst eine Klasse, die das `Activity`-Attribut und das `IntentFilter`-Attribut enthält. 

Hier sehen Sie ein Beispiel für eine Klasse, die die Werte der XML-Datei darstellt:

```csharp
  [Activity]
  [IntentFilter(new[] { Intent.ActionView },
        Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
        DataHost = "auth",
        DataScheme = "msal{client_id}")]
  public class MsalActivity : BrowserTabActivity
  {
  }
```

### <a name="xamarinforms-43x-manifest"></a>Xamarin.Forms 4.3.X-Manifest

Xamarin.Forms 4.3.x generiert Code, der das `package`-Attribut in *AndroidManifest.xml* auf `com.companyname.{appName}` festlegt. Wenn Sie `DataScheme` als `msal{client_id}` verwenden, kann es sinnvoll sein, den Wert zu ändern, damit er mit dem Namen des `MainActivity.cs`-Namespace übereinstimmt.

## <a name="use-the-embedded-web-view-optional"></a>Verwenden der eingebetteten Webansicht (optional)

Standardmäßig verwendet MSAL.NET den Systemwebbrowser. Dieser Browser ermöglicht Ihnen, bei Verwendung von Webanwendungen und anderen Apps Einmaliges Anmelden (SSO) zu nutzen. In seltenen Fällen kann es sinnvoll sein, dass Ihr System eine eingebettete Webansicht verwendet. 

Dieses Codebeispiel zeigt das Einrichten einer eingebetteten Webansicht:

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

Weitere Informationen finden Sie unter [Verwenden von Webbrowsern für MSAL.NET](msal-net-web-browsers.md) und [Überlegungen zu Systembrowsern für Xamarin Android](msal-net-system-browser-android-considerations.md).


## <a name="troubleshoot"></a>Problembehandlung
Sie können eine neue Xamarin.Forms-Anwendung erstellen und einen Verweis auf das NuGet-Paket „MSAL.Net“ hinzufügen.
Wenn Sie ein Upgrade einer vorhandenen Xamarin.Forms-Anwendung auf die MSAL.NET-Vorschauversion 1.1.2 oder höher ausführen, treten jedoch möglicherweise Probleme beim Erstellen auf.

Problembehandlung bei Erstellungsproblemen:

- Aktualisieren Sie das vorhandene MSAL.NET-NuGet-Paket auf MSAL.NET Preview 1.1.2 oder höher.
- Überprüfen Sie, ob Xamarin.Forms automatisch auf Version 2.5.0.122203 aktualisiert wird. Aktualisieren Sie erforderlichenfalls Xamarin.Forms auf diese Version.
- Überprüfen Sie, ob Xamarin.Android.Support.v4 automatisch auf Version 25.4.0.2 aktualisiert wird. Aktualisieren Sie erforderlichenfalls auf Version 25.4.0.2:
- Vergewissern Sie sich, dass alle Xamarin.Android.Support-Pakete Version 25.4.0.2 als Zielversion aufweisen.
- Bereinigen Sie die Anwendung, oder erstellen Sie sie neu.
- Versuchen Sie in Visual Studio, die maximale Anzahl der parallelen Projektbuilds auf 1 festzulegen. Wählen Sie zu diesem Zweck **Optionen** > **Projekte und Projektmappen** > **Erstellen und Ausführen** > **Maximale Anzahl paralleler Projektbuilds** aus.
- Wenn Sie Builds an der Befehlszeile erstellen und in Ihrem Befehl `/m` verwenden, versuchen Sie, dieses Element aus dem Befehl zu entfernen.

### <a name="error-the-name-authenticationcontinuationhelper-doesnt-exist-in-the-current-context"></a>Error: Der Name „AuthenticationContinuationHelper“ ist im aktuellen Kontext nicht vorhanden.

Wenn ein Fehler darauf hinweist, dass `AuthenticationContinuationHelper` im aktuellen Kontext nicht vorhanden ist, hat Visual Studio möglicherweise die Android.csproj*-Datei fehlerhaft aktualisiert. Manchmal enthält der *\<HintPath>* Dateipfad fälschlicherweise *netstandard13* anstelle von *monoandroid90*.

Dieses Beispiel enthält einen ordnungsgemäßen Dateipfad:

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in dem Beispiel einer [mobilen Xamarin-Anwendung, die Microsoft Identity Platform verwendet](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations). In der folgenden Tabelle sind die relevanten Informationen aus der Infodatei zusammengefasst.

| Beispiel | Plattform | BESCHREIBUNG |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin.iOS, Android, UWP | Eine einfache Xamarin Forms-App, die die Verwendung von MSAL zeigt, um persönliche Microsoft-Konten und Azure AD über den Azure AD 2.0-Endpunkt zu authentifizieren. Die App zeigt außerdem den Zugriff auf Microsoft Graph und das resultierende Token. <br>![Topologie](media/msal-net-xamarin-android-considerations/topology.png) |
