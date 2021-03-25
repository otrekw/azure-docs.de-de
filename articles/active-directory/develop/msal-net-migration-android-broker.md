---
title: Migrieren von Xamarin Android-Apps mithilfe von Brokern zu MSAL.NET
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie, wie Xamarin Android-Apps, die Microsoft Authenticator oder das Intune-Unternehmensportal verwenden, von ADAL.NET zu MSAL.NET migriert werden.
author: aiwang
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/31/2020
ms.author: aiwang
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 01af0e620ecb100839f7b1101e5ff9fcfc874eea
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92206668"
---
# <a name="migrate-android-applications-that-use-a-broker-from-adalnet-to-msalnet"></a>Migrieren von Android-Anwendungen, die einen Broker verwenden, von ADAL.NET zu MSAL.NET

Wenn Sie eine Xamarin Android-App besitzen, die derzeit die Azure Active Directory-Authentifizierungsbibliothek für .NET (ADAL.NET) und einen [Authentifizierungsbroker](msal-android-single-sign-on.md) nutzt, ist es an der Zeit, zur [Microsoft-Authentifizierungsbibliothek für .NET ](msal-overview.md) (MSAL.NET) zu migrieren.

## <a name="prerequisites"></a>Voraussetzungen

* Eine Xamarin Android-App, die bereits in einen Broker integriert ist ([Microsoft Authenticator](https://play.google.com/store/apps/details?id=com.azure.authenticator) oder [Intune-Unternehmensportal](https://play.google.com/store/apps/details?id=com.microsoft.windowsintune.companyportal)), und ADAL.NET, die Sie zu MSAL.NET migrieren müssen

## <a name="step-1-enable-the-broker"></a>Schritt 1: Aktivieren des Brokers

<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>
In ADAL.NET ist die Brokerunterstützung pro Authentifizierung aktiviert.

Sie mussten im `PlatformParameters`-Konstruktor `useBroker` auf *true* festlegen, um den Broker aufzurufen:

```CSharp
public PlatformParameters(
        Activity callerActivity,
        bool useBroker)
```

Im plattformspezifischen Seitenrenderercode für Android legen Sie das `useBroker`-Flag auf „true“ fest:

```CSharp
page.BrokerParameters = new PlatformParameters(
        this,
        true,
        PromptBehavior.SelectAccount);
```

Fügen Sie dann die Parameter in den Aufruf zum Abrufen eines Tokens (AcquireToken-Aufruf) ein:

```CSharp
AuthenticationResult result =
        await
            AuthContext.AcquireTokenAsync(
                Resource,
                ClientId,
                new Uri(RedirectURI),
                platformParameters)
                .ConfigureAwait(false);
```

</td><td>
In MSAL.NET wird die Brokerunterstützung wird pro PublicClientApplication aktiviert.

Verwenden Sie den Parameter `WithBroker()` (standardmäßig auf „true“ festgelegt), um den Broker aufzurufen:

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithRedirectUri(redirectUriOnAndroid)
                .Build();
```

Im AcquireToken-Aufruf:

```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```
</table>

## <a name="step-2-set-an-activity"></a>Schritt 2: Festlegen einer Aktivität

In ADAL.NET haben Sie eine Aktivität (in der Regel „MainActivity“) als Teil von „PlatformParameters“ übergeben, wie in [Schritt 1: Aktivieren des Brokers](#step-1-enable-the-broker) gezeigt.

MSAL.NET verwendet auch eine Aktivität, sie ist jedoch nicht für die reguläre Android-Verwendung ohne Broker erforderlich. Damit Sie den Broker verwenden können, legen Sie die Aktivität zum Senden und Empfangen von Antworten vom Broker fest.

<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>
Die Aktivität wird an „PlatformParameters“ auf der Android-spezifischen Plattform übergeben.

```CSharp
page.BrokerParameters = new PlatformParameters(
          this,
          true,
          PromptBehavior.SelectAccount);
```
</td><td>

In MSAL.NET führen Sie zwei Schritte aus, um die Aktivität für Android festzulegen:

1. Legen Sie in `MainActivity.cs` das `App.RootViewController`-Element auf `MainActivity` fest, um sicherzustellen, dass eine Aktivität mit dem Aufruf des Brokers vorhanden ist.

    Wird das Element nicht ordnungsgemäß festgelegt wurde, erhalten Sie möglicherweise den folgenden Fehler: `"Activity_required_for_android_broker":"Activity is null, so MSAL.NET cannot invoke the Android broker. See https://aka.ms/Brokered-Authentication-for-Android"`.

1. Verwenden Sie im AcquireTokenInteractive-Aufruf `.WithParentActivityOrWindow(App.RootViewController)`, und übergeben Sie den Verweis an die Aktivität, die Sie verwenden. In diesem Beispiel wird „MainActivity“ verwendet.

**Beispiel:**

In *App.cs*:

```CSharp
   public static object RootViewController { get; set; }
```

In *MainActivity.cs*:

```CSharp
   LoadApplication(new App());
   App.RootViewController = this;
```

Im AcquireToken-Aufruf:

```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```
</table>

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Android-spezifischen Überlegungen bei der Verwendung von MSAL.NET mit Xamarin finden Sie unter [Überlegungen zur Verwendung von Xamarin Android mit MSAL.NET](msal-net-xamarin-android-considerations.md).