---
title: Tokenbasierte (HTTP/2)-Authentifizierung für APNS in Azure Notification Hubs | Microsoft-Dokumentation
description: In diesem Thema wird erläutert, wie die neue Tokenauthentifizierung für APNS verwendet wird.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 11/14/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 02/13/2019
ms.openlocfilehash: 56689981d6c85c844fefbec6a4ec4aeb041dbc7f
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74111963"
---
# <a name="token-based-http2-authentication-for-apns"></a>Tokenbasierte (HTTP/2)-Authentifizierung für APNS

## <a name="overview"></a>Übersicht

In diesem Artikel wird erläutert, wie das neue HTTP/2-Protokoll für APNS mit tokenbasierter Authentifizierung verwendet wird.

Zu den Hauptvorteilen des neuen Protokolls zählen:

* Die Tokengenerierung ist relativ einfach (im Vergleich zu Zertifikaten).
* Keine Ablaufdaten mehr – Sie steuern die Authentifizierungstoken und deren Sperrung
* Nutzlasten können jetzt bis zu 4 KB betragen
* Synchrones Feedback
* Sie verwenden das neueste Apple-Protokoll – Zertifikate verwenden weiterhin das binäre Protokolls, das demnächst veraltet sein wird

Dieser Mechanismus lässt sich in zwei Schritten ausführen:

* Abrufen der erforderlichen Informationen aus dem Apple-Entwicklerkontoportal.
* Konfigurieren Ihres Benachrichtigungshubs mit den neuen Informationen.

Notification Hubs ist jetzt für die Verwendung des neuen Authentifizierungssystems mit APNS eingerichtet.

Beachten Sie Folgendes: Wenn Sie von der Verwendung von Zertifikatanmeldeinformationen für APNS migriert sind, setzen die Tokeneigenschaften Ihr Zertifikat in unserem System außer Kraft, aber Ihre Anwendung empfängt weiterhin nahtlos Benachrichtigungen.

## <a name="obtaining-authentication-information-from-apple"></a>Abrufen von Authentifizierungsinformationen von Apple

Um die tokenbasierte Authentifizierung zu aktivieren, benötigen Sie die folgenden Eigenschaften aus Ihrem Apple-Entwicklerkonto:

### <a name="key-identifier"></a>Schlüsselbezeichner

Den Schlüsselbezeichner können Sie auf der Seite **Keys** unter **Zertifikate, Bezeichner und Profile** in Ihrem Apple-Entwicklerkonto abrufen.

![](./media/notification-hubs-push-notification-http2-token-authentification/keys.png)

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier-and-application-name"></a>Anwendungsbezeichner und Anwendungsname

Anwendungsname und -bezeichner sind ebenfalls auf der Seite **Zertifikate, Bezeichner und Profile** in Entwicklerkonto verfügbar:

![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

### <a name="configure-via-the-net-sdk-or-the-azure-portal"></a>Konfigurieren über das .NET SDK oder das Azure-Portal

Sie können Ihren Hub mithilfe unseres [neuesten Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs) oder im Azure-Portal zur Verwendung der tokenbasierten Authentifizierung konfigurieren. Um die tokenbasierte Authentifizierung im Portal zu aktivieren, melden Sie sich beim Azure-Portal an, und wechseln Sie zum Bereich **Einstellungen > Apple (APNS)** Ihres Benachrichtigungshubs. Wählen Sie **Token** in der Eigenschaft **Authentifizierungsmodus** aus, um Ihren Hub mit allen relevanten Tokeneigenschaften zu aktualisieren.

![Konfigurieren des Tokens](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

* Geben Sie die Eigenschaften ein, die Sie in Ihrem Apple-Entwicklerkonto abgerufen haben.
* Wählen Sie den Anwendungsmodus aus (**Produktion** oder **Sandbox**).
* Klicken Sie auf die Schaltfläche **Speichern**, um Ihre APNS-Anmeldeinformationen zu aktualisieren.

Tokenbasierte Anmeldeinformationen bestehen aus den folgenden Feldern:

* **Schlüssel-ID**: Der Bezeichner des privaten Schlüssels, der im Apple-Entwicklerportal generiert wurde, beispielsweise `2USFGKSKLT`.
* **Team-ID**: Wird auch als „Präfix“ oder „App-Präfix“ bezeichnet. Dies ist der Bezeichner für die Organisation im Apple-Entwicklerportal, beispielsweise `S4V3D7CHJR`.
* **Bündel-ID**: Wird auch als „App-ID“ bezeichnet. Dies ist der Bündelbezeichner für die Anwendung, beispielsweise `com.microsoft.nhubsample2019`. Beachten Sie, dass Sie einen Schlüssel für viele Apps verwenden können. Dieser Wert wird dem `apns-topic`-HTTP-Header zugeordnet, wenn eine Benachrichtigung gesendet wird, und wird verwendet, um auf die jeweilige Anwendung abzuzielen.
* **Token**: Wird auch als „Schlüssel“ oder „privater Schlüssel“ bezeichnet. Dies wird aus der im Apple-Entwicklerportal generierten P8-Datei abgerufen. Für den Schlüssel muss APNS aktiviert sein (Dies wird beim Generieren des Schlüssels im Apple-Entwicklerportal ausgewählt). Bei dem Wert müssen die PEM-Kopf-/Fußzeile entfernt werden, wenn Sie ihn für das NH-Portal/die API bereitstellen.
* **Endpunkt**: Dies ist eine Umschaltfläche auf dem Blatt „Notification Hubs-Portal“ und ein Zeichenfolgenfeld in der API. Gültige Werte sind `https://api.push.apple.com` oder `https://api.sandbox.push.apple.com`. Notification Hubs verwendet diesen Wert entweder für die Produktions- oder für die Sandbox-Umgebung zum Senden von Benachrichtigungen. Dies muss der `aps-environment`-Berechtigung in der App entsprechen. Andernfalls stimmen die generierten APNS-Gerätetoken nicht mit der Umgebung überein, und die Benachrichtigungen können nicht gesendet werden.

Das folgende Codebeispiel veranschaulicht die richtige Verwendung:

```csharp
NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
string token = "YOUR PRIVATE KEY HERE";
string keyId = "YOUR KEY ID HERE";
string appName = "YOUR APP NAME HERE";
string appId = "YOUR APP ID HERE";
NotificationHubDescription desc = new NotificationHubDescription("PATH TO YOUR HUB");
desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
nm.UpdateNotificationHubAsync(desc);
```

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Azure-Benachrichtigungshubinstanz über das Azure-Portal](create-notification-hub-portal.md)
* [Konfigurieren eines Benachrichtigungshubs im Azure-Portal](create-notification-hub-portal.md)
