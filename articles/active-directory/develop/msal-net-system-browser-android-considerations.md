---
title: Überlegungen zu Systembrowsern für Xamarin Android (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie mehr über Überlegungen zur Verwendung von Systembrowsern für Xamarin Android mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 8f755b42249a88a2d37117003d561f79aea6b170
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88165990"
---
#  <a name="xamarin-android-system-browser-considerations-for-using-msalnet"></a>Überlegungen zu Systembrowsern für Xamarin Android zur Verwendung von MSAL.NET

In diesem Artikel wird erörtert, was Sie beim Verwenden des Systembrowsers für Xamarin Android mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET) berücksichtigen sollten.

Seit der MSAL.NET 2.4.0-Vorschauversion unterstützt MSAL.NET andere Browser als Chrome. Zur Authentifizierung ist es nicht mehr erforderlich, dass Chrome auf dem Android-Gerät installiert ist.

Es empfiehlt sich, Browser zu verwenden, die benutzerdefinierte Registerkarten unterstützen. Dazu gehören unter anderem Folgende:

| Browser mit Unterstützung für benutzerdefinierte Registerkarten | Paketname |
|------| ------- |
|Chrome | com.android.chrome|
|Microsoft Edge | com.microsoft.emmx|
|Firefox | org.mozilla.firefox|
|Ecosia | com.ecosia.android|
|Kiwi | com.kiwibrowser.browser|
|Brave | com.brave.browser|

Über das Benennen von Browsern, die Unterstützung für benutzerdefinierte Registerkarten bieten, hinaus, ergaben unsere Tests Hinweise darauf, dass einige Browser, die keine benutzerdefinierten Registerkarten unterstützen, ebenfalls zur Authentifizierung geeignet sind. Zu diesen Browsern gehören Opera, Opera Mini, InBrowser und Maxthon. 

## <a name="tested-devices-and-browsers"></a>Getestete Geräte und Browser
In der folgenden Tabelle sind die Geräte und Browser aufgeführt, die auf Kompatibilität der Authentifizierung getestet wurden.

| Sicherungsmedium | Browser     |  Ergebnis  | 
| ------------- |:-------------:|:-----:|
| Huawei/One+ | Chrome\* | Pass|
| Huawei/One+ | Edge\* | Pass|
| Huawei/One+ | Firefox\* | Pass|
| Huawei/One+ | Brave\* | Pass|
| One+ | Ecosia\* | Pass|
| One+ | Kiwi\* | Pass|
| Huawei/One+ | Opera | Pass|
| Huawei | OperaMini | Pass|
| Huawei/One+ | InBrowser | Pass|
| One+ | Maxthon | Pass|
| Huawei/One+ | DuckDuckGo | Der Benutzer hat die Authentifizierung abgebrochen.|
| Huawei/One+ | UC Browser | Der Benutzer hat die Authentifizierung abgebrochen.|
| One+ | Dolphin | Der Benutzer hat die Authentifizierung abgebrochen.|
| One+ | CM Browser | Der Benutzer hat die Authentifizierung abgebrochen.|
| Huawei/One+ | Keiner installiert | AndroidActivityNotFound-Ausnahme|

\* Unterstützt benutzerdefinierte Registerkarten

## <a name="known-issues"></a>Bekannte Probleme

Wenn der Benutzer keinen Browser auf dem Gerät aktiviert hat, löst MSAL.NET eine `AndroidActivityNotFound`-Ausnahme aus.  
  - **Lösung**: Bitten Sie den Benutzer, einen Browser auf seinem Gerät zu aktivieren. Empfehlen Sie einen Browser, der benutzerdefinierte Registerkarten unterstützt.

Bei einem Authentifizierungsfehler (beispielsweise, wenn die Authentifizierung mit DuckDuckGo gestartet wird), gibt MSAL.NET `AuthenticationCanceled MsalClientException` zurück. 
  - **Ursache**: Es war kein Browser auf dem Gerät aktiviert, der benutzerdefinierte Registerkarten unterstützt. Die Authentifizierung wurde mit einem Browser gestartet, der den Authentifizierungsvorgang nicht abschließen konnte. 
  - **Lösung**: Bitten Sie den Benutzer, einen Browser auf seinem Gerät zu aktivieren. Empfehlen Sie einen Browser, der benutzerdefinierte Registerkarten unterstützt.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Codebeispiele finden Sie unter [Wahl zwischen einem eingebetteten Webbrowser und einem Systembrowser in Xamarin Android](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/MSAL.NET-uses-web-browser#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid) und [Webbenutzeroberfläche von eingebetteten im Vergleich mit Systembrowsern](msal-net-web-browsers.md#embedded-vs-system-web-ui).  