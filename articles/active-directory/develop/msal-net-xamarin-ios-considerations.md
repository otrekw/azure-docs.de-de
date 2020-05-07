---
title: Überlegungen zu Xamarin iOS (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über Überlegungen zur Verwendung von Xamarin iOS mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 7125559dd39e1626634dae7c45b0744bfff57d8c
ms.sourcegitcommit: d662eda7c8eec2a5e131935d16c80f1cf298cb6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82652654"
---
# <a name="considerations-for-using-xamarin-ios-with-msalnet"></a>Überlegungen zur Verwendung von Xamarin iOS mit MSAL.NET
Wenn Sie die Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET) in Xamarin iOS einsetzen, sollten Sie diese Punkte berücksichtigen: 

- Überschreiben und Implementieren der `OpenUrl`-Funktion in `AppDelegate`
- Aktivieren von Keychaingruppen
- Aktivieren von Tokencachefreigaben
- Aktivieren des Keychainzugriffs
- Grundlegendes zu den bekannten Problemen mit iOS 12 und der Authentifizierung

## <a name="implement-openurl"></a>Implementieren von OpenUrl

Überschreiben Sie die `OpenUrl`-Methode der abgeleiteten `FormsApplicationDelegate`-Klasse, und rufen Sie `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs` auf. Hier sehen Sie ein Beispiel:

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
    return true;
}
```

Führen Sie außerdem die folgenden Aufgaben aus: 
* Definieren eines URL-Schemas
* Vorschreiben von Berechtigungen für Ihre App für Aufrufe an andere Apps
* Zuweisen einer bestimmten Form für die Umleitungs-URL.
* Registrieren der Umleitungs-URL im [Azure-Portal](https://portal.azure.com).

### <a name="enable-keychain-access"></a>Aktivieren des Keychainzugriffs

Um den Keychainzugriff zu aktivieren, überzeugen Sie sich zunächst, dass Ihre Anwendung über eine Keychain-Zugriffsgruppe verfügt. Sie können die Keychainzugriffsgruppe beim Erstellen Ihrer Anwendung mithilfe der `WithIosKeychainSecurityGroup()`-API festlegen:

Um vom Cache und dem einmaligen Anmelden (SSO) profitieren zu können, legen Sie die Keychainzugriffsgruppe in allen Ihren Anwendungen auf denselben Wert fest.

In diesem Beispiel für das Setup wird MSAL 4.x verwendet:
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.adalcache")
     .Build();
```

Aktivieren Sie den Keychainzugriff außerdem in der `Entitlements.plist`-Datei. Verwenden Sie entweder die folgende Zugriffsgruppe oder eine eigene Zugriffsgruppe.

```xml
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.adalcache</string>
  </array>
</dict>
```

Wenn Sie die `WithIosKeychainSecurityGroup()`-API verwenden, hängt MSAL automatisch Ihre Sicherheitsgruppe an das Ende der *Team ID* (`AppIdentifierPrefix`) Ihrer Anwendung an. MSAL fügt Ihre Sicherheitsgruppe hinzu, denn dies erfolgt ebenfalls, wenn Sie Ihre Anwendung in Xcode erstellen. Aus diesem Grund müssen die Berechtigungen in der `Entitlements.plist`-Datei `$(AppIdentifierPrefix)` vor der Keychainzugriffsgruppe aufnehmen.

Weitere Informationen finden Sie in der [Dokumentation der iOS-Berechtigungen](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps). 

### <a name="enable-token-cache-sharing-across-ios-applications"></a>Aktivieren der Tokencachefreigabe über iOS-Anwendungen hinweg

Ab MSAL 2.x können Sie eine Keychainzugriffsgruppe angeben, die zum Beibehalten des Tokencaches über mehrere Anwendungen hinweg verwendet wird. Diese Einstellung ermöglicht die gemeinsame Verwendung des Tokencaches über mehrere Anwendungen hinweg, die die gleiche Keychainzugriffsgruppe aufweisen. Sie können den Tokencache unter [ADAL.NET](https://aka.ms/adal-net)-Anwendungen, MSAL.NET Xamarin.iOS-Anwendungen und nativen iOS-Anwendungen teilen, die in [ADAL.objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc) oder [MSAL.objc](https://github.com/AzureAD/microsoft-authentication-library-for-objc) entwickelt wurden.

Durch Teilen des Tokencaches ermöglichen Sie einmaliges Anmelden (SSO) für alle diese Anwendungen, die die gleiche Keychainzugriffsgruppe verwenden.

Um diese Cachefreigabe zu aktivieren, verwenden Sie die `WithIosKeychainSecurityGroup()`-Methode, um die Keychainzugriffsgruppe in allen Anwendungen, die den gleichen Cache verwenden, auf den gleichen Wert festzulegen. Das erste Codebeispiel in diesem Artikel zeigt, wie die Methode verwendet wird.

Weiter oben in diesem Artikel haben Sie erfahren, dass MSAL `$(AppIdentifierPrefix)` hinzufügt, wenn Sie die `WithIosKeychainSecurityGroup()`-API verwenden. MSAL fügt dieses Element hinzu, weil die Team-ID `AppIdentifierPrefix` sicherstellt, dass nur Anwendungen, die vom gleichen Verleger stammen, den Keychainzugriff gemeinsam verwenden können.

> [!NOTE]
> Die `KeychainSecurityGroup`-Eigenschaft ist veraltet.
> 
> Seit MSAL 2.x waren Entwickler gezwungen, beim Verwenden der `KeychainSecurityGroup`-Eigenschaft das Präfix `TeamId` einzuschließen. Aber seit MSAL 2.7.x löst MSAL das `TeamId`-Präfix der neuen `iOSKeychainSecurityGroup`-Eigenschaft zur Laufzeit auf. Wenn Sie diese Eigenschaft verwenden, schließen Sie nicht das Präfix `TeamId` in den Wert ein. Das Präfix ist nicht erforderlich.
>
> Da die `KeychainSecurityGroup`-Eigenschaft veraltet ist, verwenden Sie die `iOSKeychainSecurityGroup`-Eigenschaft.

### <a name="use-microsoft-authenticator"></a>Verwenden von Microsoft Authenticator

Die Anwendung kann Microsoft Authenticator als Broker verwenden, um Folgendes zu aktivieren:

- **SSO**: Wenn Sie SSO aktivieren, müssen sich Ihre Benutzer nicht bei jeder Anwendung anmelden.
- **Geräteidentifikation**: Verwenden Sie die Geräteidentifikation, um eine Authentifizierung durch Zugriff auf das Gerätezertifikat zu implementieren. Dieses Zertifikat wird auf dem Gerät erstellt, wenn es dem Arbeitsplatz hinzugefügt wird. Die Anwendung ist bereit, wenn die Mandantenadministratoren den bedingten Zugriff auf die Geräte aktivieren.
- **Überprüfung der Anwendungsidentifikation**: Wenn eine Anwendung den Broker aufruft, übergibt sie ihre Umleitungs-URL. Der Broker überprüft die Umleitungs-URL.

Weitere Informationen zum Aktivieren eines Brokers finden Sie unter [Verwenden von Microsoft Authenticator oder des Microsoft Intune-Unternehmensportals für Xamarin iOS- und Android-Anwendungen](msal-net-use-brokers-with-xamarin-apps.md).

## <a name="known-issues-with-ios-12-and-authentication"></a>Bekannte Probleme mit iOS 12 und der Authentifizierung
Microsoft hat eine [Sicherheitsempfehlung](https://github.com/aspnet/AspNetCore/issues/4647) zu einer Inkompatibilität zwischen iOS 12 und einigen Authentifizierungsarten herausgegeben. Diese Inkompatibilität führt zu Problemen mit der Anmeldung über soziale Medien sowie mit WSFed- und OIDC-Anmeldungen. Der Sicherheitsratgeber vermittelt Entwicklern Kenntnisse zum Entfernen von Sicherheitseinschränkungen in ASP.NET aus ihren Anwendungen, um sie mit iOS 12 kompatibel zu machen.  

Beim Entwickeln von MSAL.NET-Anwendungen für Xamarin iOS stellen Sie möglicherweise eine Endlosschleife fest, wenn Sie versuchen, sich über iOS 12 bei Websites anzumelden. Dieses Verhalten tritt in ähnlicher Weise bei diesem [ADAL-Problem](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329) auf. 

Sie stellen möglicherweise auch ein Problem mit der ASP.NET Core-OICD-Authentifizierung bei iOS 12 Safari fest. Weitere Informationen finden Sie in der Beschreibung dieses [WebKit-Problems](https://bugs.webkit.org/show_bug.cgi?id=188165).

## <a name="next-steps"></a>Nächste Schritte

Informationen zu Eigenschaften für Xamarin iOS finden Sie im Absatz [iOS-spezifische Überlegungen](https://github.com/Azure-Samples/active-directory-xamarin-native-v2/tree/master/1-Basic#ios-specific-considerations) oder der README.md-Datei des folgenden Beispiels:

Beispiel | Plattform | BESCHREIBUNG
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, Universelle Windows-Plattform (UWP) | Eine einfache Xamarin Forms-App, die die Verwendung von MSAL zeigt, um persönliche Microsoft-Konten und Azure AD über den Azure AD 2.0-Endpunkt zu authentifizieren. Die App zeigt darüber hinaus, wie das resultierende Token für den Zugriff auf Microsoft Graph verwendet wird.

<!--- https://github.com/Azure-Samples/active-directory-xamarin-native-v2/blob/master/ReadmeFiles/Topology.png -->