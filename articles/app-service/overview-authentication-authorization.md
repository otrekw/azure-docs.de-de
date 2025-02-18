---
title: Authentifizierung und Autorisierung
description: Erfahren Sie etwas über die integrierte Authentifizierungs- und Autorisierungsunterstützung in Azure App Service und Azure Functions sowie darüber, wie sie zum Schutz Ihrer App vor unbefugtem Zugriff beitragen kann.
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.topic: article
ms.date: 03/29/2021
ms.reviewer: mahender
ms.custom: seodec18, fasttrack-edit, has-adal-ref
ms.openlocfilehash: a362e99e9da7cf4c41f042364792a05a27b1aa6a
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111892686"
---
# <a name="authentication-and-authorization-in-azure-app-service-and-azure-functions"></a>Authentifizierung und Autorisierung in Azure App Service und Azure Functions

Azure App Service bietet integrierte Authentifizierungs- und Autorisierungsfunktionen (manchmal auch als "Easy Auth" bezeichnet), sowie [Azure-Funktionen](../azure-functions/functions-overview.md), sodass Sie Benutzer anmelden und auf Daten zugreifen können, indem Sie minimalen oder gar keinen Code in Ihrer Web-App, Ihrer RESTful-API und Ihrem mobilen Backend schreiben. In diesem Artikel wird beschrieben, wie App Service zur Vereinfachung der Authentifizierung und Autorisierung für Ihre App beiträgt.

## <a name="why-use-the-built-in-authentication"></a>Warum die eingebaute Authentifizierung verwenden?

Die Nutzung dieser Funktion für die Authentifizierung und Autorisierung ist nicht verpflichtend. Sie können die gebündelten Sicherheitsfeatures im Webframework Ihrer Wahl verwenden oder eigene Hilfsprogramme schreiben. Sie müssen jedoch sicherstellen, dass Ihre Lösung mit den neuesten Sicherheits-, Protokoll- und Browser-Aktualisierungen auf dem neuesten Stand bleibt.

Das Implementieren einer sicheren Lösung für die Authentifizierung (Anmeldung von Benutzern) und die Autorisierung (Zugriff auf sichere Daten) kann einen erheblichen Aufwand in Anspruch nehmen. Sie müssen sicherstellen, dass Sie die bewährten Methoden und Standards der Branche befolgen und ihre Implementierung auf dem neuesten Stand halten. Die integrierte Authentifizierungsfunktion für App Service und Azure Functions kann Ihnen Zeit und Mühe ersparen, indem sie eine sofort einsatzbereite Authentifizierung mit föderierten Identitätsanbietern bietet, sodass Sie sich auf den Rest Ihrer Anwendung konzentrieren können.

- Der Azure App Service ermöglicht es Ihnen, eine Vielzahl von Authentifizierungsfunktionen in Ihre Web-App oder API zu integrieren, ohne Sie selbst zu implementieren.
- Es ist direkt in die Plattform integriert und erfordert keine bestimmte Sprache, kein SDK, Sicherheitskenntnisse weder die Verwendung jeglichen Codes.
- Sie können mit mehreren Anmelde-Anbietern integrieren. Beispielsweise Azure AD, Facebook, Google, Twitter.

## <a name="identity-providers"></a>Identitätsanbieter

App Service nutzt die [Verbundidentität](https://en.wikipedia.org/wiki/Federated_identity). Dabei werden die Benutzeridentitäten und der Authentifizierungsablauf von einem externen Identitätsanbieter für Sie verwaltet. Die folgenden Identitätsanbieter sind standardmäßig verfügbar:

| Anbieter | Anmeldungsendpunkt | Leitfäden zur Vorgehensweise |
| - | - | - |
| [Microsoft Identitätsplattform](../active-directory/fundamentals/active-directory-whatis.md) | `/.auth/login/aad` | [Microsoft Identitätsplattform-App-Service-Anmeldung](configure-authentication-provider-aad.md) |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` | [App Service Facebook-Anmeldung](configure-authentication-provider-facebook.md) |
| [Google](https://developers.google.com/identity/choose-auth) | `/.auth/login/google` | [Google-Anmeldung App Service](configure-authentication-provider-google.md) |
| [Twitter](https://developer.twitter.com/en/docs/basics/authentication) | `/.auth/login/twitter` | [App Service Twitter-Anmeldung](configure-authentication-provider-twitter.md) |
| Jeder [OpenID Connect](https://openid.net/connect/)-Anbieter (Vorschau) | `/.auth/login/<providerName>` | [App Service OpenID Connect-Anmeldung](configure-authentication-provider-openid-connect.md) |

Wenn Sie die Authentifizierung und Autorisierung mit einem dieser Anbieter aktivieren, ist der entsprechende Anmeldungsendpunkt für die Benutzerauthentifizierung und die Überprüfung von Authentifizierungstoken vom Anbieter verfügbar. Sie können Ihren Benutzern problemlos eine beliebige Anzahl von diesen Anmeldeoptionen bereitstellen.

## <a name="considerations-for-using-built-in-authentication"></a>Überlegungen zur Verwendung der integrierten Authentifizierung

Durch Aktivieren dieser Funktion werden alle nicht sicheren HTTP-Anforderungen an Ihre Anwendung automatisch an HTTPS umgeleitet, unabhängig von der App Service-Konfigurationseinstellung zum Erzwingen von HTTPS. Sie können dies mit der  `requireHttps` Einstellung in der V2-Konfiguration deaktivieren. Wir empfehlen jedoch die Verwendung von HTTPS, und Sie sollten sicherstellen, dass keine Sicherheits-Token jemals über nicht sichere HTTP-Verbindungen übertragen werden.

Der App Service kann zur Authentifizierung mit oder ohne Einschränkung des Zugriffs auf Ihre Website-Inhalte und APIs verwendet werden. Um den App-Zugriff nur auf authentifizierte Benutzer zu beschränken, legen Sie die **Aktion fest, die ausgeführt werden soll, wenn die Anfrage nicht authentifiziert ist**, um sich mit einem der konfigurierten Identitätsanbieter anzumelden. Um den Zugriff zu authentifizieren, aber nicht zu beschränken, setzen Sie die **Aktion bei nicht authentifizierter Anforderung** auf „Anonyme Anforderungen zulassen (keine Aktion)."

> [!NOTE]
> Sie sollten jeder App-Registrierung eine eigene Erlaubnis und Zustimmung geben. Vermeiden Sie das Teilen von Berechtigungen zwischen Umgebungen, indem Sie separate App-Registrierungen für gesonderte Bereitstellungsslots verwenden. Wenn Sie neuen Code testen, kann diese Vorgehensweise dabei helfen, Probleme zu verhindern, die sich auf die Produktions-App auswirken können.

## <a name="how-it-works"></a>Funktionsweise

[Funktions-Architektur unter Windows (Bereitstellung ohne Container)](#feature-architecture-on-windows-non-container-deployment))

[Funktions-Architektur für Linux und Container](#feature-architecture-on-linux-and-containers)

[Authentifizierungsablauf](#authentication-flow)

[Autorisierungsverhalten](#authorization-behavior)

[Benutzer-/Anwendungsansprüche](#user-and-application-claims)

[Tokenspeicher](#token-store)

[Protokollierung und Ablaufverfolgung](#logging-and-tracing)

#### <a name="feature-architecture-on-windows-non-container-deployment"></a>Funktionsarchitektur unter Windows (Nicht-Container-Einsatz)

Das Modul für Authentifizierung und Autorisierung wird in der gleichen Sandbox wie Ihr Anwendungscode ausgeführt. Wenn es aktiviert ist, wird es von allen eingehenden HTTP-Anforderungen durchlaufen, bevor diese von Ihrem Anwendungscode behandelt werden.

:::image type="content" source="media/app-service-authentication-overview/architecture.png" alt-text="Ein Architekturdiagramm, das zeigt, wie Anforderungen durch einen Prozess in der Website-Sandbox abgefangen werden, der mit Identitätsanbietern interagiert, bevor Datenverkehr an die bereitgestellte Website zugelassen wird" lightbox="media/app-service-authentication-overview/architecture.png":::

Dieses Modul erledigt einige Dinge für Ihre App:

- Authentifizierung von Benutzern mit dem angegebenen Anbieter
- Überprüfung, Speicherung und Aktualisierung von Token
- Verwaltung der authentifizierten Sitzung
- Einfügen von Identitätsinformationen in Anforderungsheader

Das Modul wird getrennt vom Anwendungscode ausgeführt und mithilfe von App-Einstellungen konfiguriert. Weder SDKs noch bestimmte Sprachen oder Änderungen am Anwendungscode sind erforderlich. 

#### <a name="feature-architecture-on-linux-and-containers"></a>Funktions-Architektur für Linux und Container

Das Modul für Authentifizierung und Autorisierung wird in einem separaten Container isoliert von Ihrem Anwendungscode ausgeführt. Über das sogenannte [Botschaftermuster](/azure/architecture/patterns/ambassador) interagiert es mit dem eingehenden Datenverkehr, um ähnliche Funktionen wie unter Windows auszuführen. Da es nicht in einem Prozess ausgeführt wird, ist keine direkte Integration in bestimmte Sprachframeworks möglich. Die relevanten Informationen für Ihre App werden jedoch wie unten erläutert mithilfe von Anforderungsheadern weitergeleitet.

#### <a name="authentication-flow"></a>Authentifizierungsfluss

Der Authentifizierungsablauf gilt für alle Anbieter, unterscheidet sich jedoch abhängig davon, ob die Anmeldung mit dem SDK des Anbieters erfolgen soll:

- Ohne Anbieter-SDK: Die Anwendung delegiert die Verbundanmeldung an App Service. Dies ist normalerweise bei Browser-Apps der Fall, die dem Benutzer die Anmeldeseite des Anbieters anzeigen können. Der Servercode verwaltet den Anmeldevorgang, daher wird er auch als _servergesteuerter Datenfluss_ oder _Serverfluss_ bezeichnet. Dieser Fall gilt für Browser-Apps. Es gilt auch für native Apps, die Benutzer mithilfe des Client SDK von Mobile Apps anmelden, da das SDK eine Webansicht zum Anmelden von Benutzern mit der App Service-Authentifizierung öffnet.
- Mit Anbieter-SDK: Die Anwendung meldet den Benutzer manuell beim Anbieter an und sendet dann das Authentifizierungstoken zur Überprüfung an App Service. Dies ist normalerweise bei Apps ohne Browser der Fall, die dem Benutzer die Anmeldeseite des Anbieters nicht anzeigen können. Der Anwendungscode verwaltet den Anmeldevorgang, daher wird er auch als _clientgesteuerter Datenfluss_ oder _Clientfluss_ bezeichnet. Dieser Fall gilt für REST-APIs [Azure Functions](../azure-functions/functions-overview.md) und JavaScript-Browserclients sowie Browser-Apps, die eine höhere Flexibilität beim Anmeldevorgang erfordern. Er gilt auch für native mobile Apps, die Benutzer mithilfe des Anbieter-SDKs anmelden.

Bei Aufrufen aus einer vertrauenswürdigen Browser-App in App Service an eine weitere REST-API in App Service oder [Azure Functions](../azure-functions/functions-overview.md) kann über den servergesteuerten Datenfluss authentifiziert werden. Weitere Informationen finden Sie unter [Anpassen der Authentifizierung und Autorisierung in App Service](app-service-authentication-how-to.md).

Die folgende Tabelle zeigt die Schritte des Authentifizierungsablaufs.

| Schritt | Ohne Anbieter-SDK | Mit Anbieter-SDK |
| - | - | - |
| 1. Anmeldung des Benutzers | Client wird zu `/.auth/login/<provider>` umgeleitet. | Clientcode meldet Benutzer direkt mit dem Anbieter-SDK an und erhält ein Authentifizierungstoken. Informationen finden Sie in der Dokumentation des Anbieters. |
| 2. Nachauthentifizierung | Anbieter leitet Client zu `/.auth/login/<provider>/callback` um. | Clientcode [sendet Token des Anbieters](app-service-authentication-how-to.md#validate-tokens-from-providers) zur Überprüfung an `/.auth/login/<provider>`. |
| 3. Einrichten der authentifizierten Sitzung | App Service fügt der Antwort ein authentifiziertes Cookie hinzu. | App Service gibt das eigene Authentifizierungstoken an den Clientcode zurück. |
| 4. Bereitstellen von authentifiziertem Inhalt | Client schließt Authentifizierungscookie in nachfolgenden Anforderungen (die automatisch vom Browser verarbeitet werden) ein. | Clientcode stellt Authentifizierungstoken im `X-ZUMO-AUTH`-Header (der automatisch von Mobile Apps-Client-SDKs verarbeitet wird) bereit. |

Für Clientbrowser kann App Service alle nicht authentifizierten Benutzer automatisch an `/.auth/login/<provider>` weiterleiten. Sie können Benutzern auch einen oder mehrere `/.auth/login/<provider>`-Links zur Anmeldung bei Ihrer App mit dem Anbieter ihrer Wahl bereitstellen.

<a name="authorization"></a>

#### <a name="authorization-behavior"></a>Autorisierungsverhalten

Im [Azure-Portal](https://portal.azure.com) können Sie die App Service-Autorisierung mit einer Reihe von Verhaltensweisen konfigurieren, wenn eine eingehende Anforderung nicht authentifiziert ist. Die folgenden Überschriften beschreiben die Optionen.

**Nicht-authentifizierte Anforderungen gestatten**

Diese Option verweist die Autorisierung von nicht authentifiziertem Datenverkehr an Ihren Anwendungscode. Für authentifizierte Anforderungen übergibt App Service auch Authentifizierungsinformationen in den HTTP-Headern.

Diese Option bietet mehr Flexibilität bei der Verarbeitung anonymer Anforderungen. Beispielsweise können Sie für Ihre Benutzer [mehrere Anmeldungsanbieter bereitstellen](app-service-authentication-how-to.md#use-multiple-sign-in-providers). Sie müssen jedoch Code schreiben.

**Erfordern Sie die Authentifizierung**

Diese Option wird den nicht-authentifizierten Datenverkehr für Ihre Anwendung ablehnen. Diese Ablehnung kann als eine Umleitungs-Aktion an einen der konfigurierten Identitäts-Anbietern stattfinden. In diesen Fällen wird ein Browser Client an `/.auth/login/<provider>` den von Ihnen gewählten Anbieter umgeleitet. Wenn die anonyme Anforderung von einer nativen mobilen App stammt, wird `HTTP 401 Unauthorized` als Antwort zurückgegeben. Sie können die Ablehnung auch so konfigurieren, dass Sie ein `HTTP 401 Unauthorized` oder `HTTP 403 Forbidden` für alle Anforderungen ist.

Mit dieser Option müssen Sie in Ihrer App keinen Authentifizierungscode schreiben. Eine genauere Autorisierung, z.B. rollenspezifische Autorisierung, kann durch das Untersuchen der Ansprüche des Benutzers durchgeführt werden (siehe [Zugriff auf Benutzeransprüche](app-service-authentication-how-to.md#access-user-claims)).

> [!CAUTION]
> Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite wünschen, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen.

> [!NOTE]
> Standardmäßig kann jeder Benutzer in Ihrem Azure AD-Mandanten ein Token für Ihre Anwendung aus Azure AD anfordern. Sie können die [Anwendung in Azure AD konfigurieren](../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md), wenn Sie den Zugriff auf die Anwendung auf eine bestimmte Gruppe von Benutzern beschränken möchten.


#### <a name="user-and-application-claims"></a>Benutzer-/Anwendungsansprüche

App Service stellt Ihrem Code für alle Sprachenframeworks die Ansprüche im eingehenden Token (ob dieses von einem authentifizierten Endbenutzer oder einer Clientanwendung stammt) zur Verfügung, indem sie in die Anforderungsheader eingefügt werden. Für ASP.NET 4.6-Apps füllt App Service die [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)-Eigenschaft mit Ansprüchen des authentifizierten Benutzers, sodass Sie dem standardmäßigen .NET Codemuster einschließlich des `[Authorize]`-Attributs folgen können. Auf ähnliche Weise füllt App Service die `_SERVER['REMOTE_USER']`-Variable für PHP-Apps. Für Java-Apps sind die Ansprüche [über das Tomcat-Servlet zugänglich](configure-language-java.md#authenticate-users-easy-auth).

Für [Azure Functions](../azure-functions/functions-overview.md) wird `ClaimsPrincipal.Current` nicht für .NET Code aufgefüllt, Sie können die Benutzeransprüche jedoch weiterhin in den Anforderungsheadern finden oder das `ClaimsPrincipal`-Objekt aus dem Anforderungskontext oder selbst durch einen Bindungsparameter abrufen. Weitere Informationen finden Sie unter [Arbeiten mit Clientidentitäten](../azure-functions/functions-bindings-http-webhook-trigger.md#working-with-client-identities).

Weitere Informationen finden Sie unter [Zugriff auf Benutzeransprüche](app-service-authentication-how-to.md#access-user-claims).

Für .NET Core unterstützt [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) das Auffüllen des aktuellen Benutzers mit dem „Authentifizierung/Autorisierung“-Feature. Weitere Informationen finden Sie im [Microsoft.Identity.Web-Wiki](https://github.com/AzureAD/microsoft-identity-web/wiki/1.2.0#integration-with-azure-app-services-authentication-of-web-apps-running-with-microsoftidentityweb), oder sehen Sie sich eine Demonstration dazu in diesem [Tutorial für eine Web-App, die auf Microsoft Graph zugreift](./scenario-secure-app-access-microsoft-graph-as-user.md?tabs=command-line#install-client-library-packages), an.

#### <a name="token-store"></a>Tokenspeicher

App Service bietet einen integrierten Tokenspeicher. Dabei handelt es sich um ein Repository mit Token, die den Benutzern Ihrer Web-Apps, APIs oder nativen mobilen Apps zugeordnet sind. Wenn Sie die Authentifizierung für jeden Anbieter aktivieren, ist dieser Tokenspeicher sofort für Ihre App verfügbar. Wenn Ihr Anwendungscode im Auftrag des Benutzers auf Daten dieser Anbieter zugreifen muss, z.B um:

- Beiträge in der Facebook-Chronik des authentifizierten Benutzers zu veröffentlichen
- Lesen der Unternehmensdaten des Benutzers mit der Microsoft Graph-API

In der Regel müssen Sie Code schreiben, um diese Token in Ihrer Anwendung zu erfassen, speichern und aktualisieren. Mit dem Tokenspeicher [rufen Sie die Token bei Bedarf einfach ab](app-service-authentication-how-to.md#retrieve-tokens-in-app-code) und [weisen App Service an, sie zu aktualisieren](app-service-authentication-how-to.md#refresh-identity-provider-tokens), wenn sie ungültig werden. 

Die ID-, Zugriffs- und Aktualisierungstoken werden für die authentifizierte Sitzung zwischengespeichert und sind nur für den zugehörigen Benutzer zugänglich.  

Wenn Sie in Ihrer App nicht mit Token arbeiten müssen, können Sie auf der Seite **Authentifizierung/Autorisierung** Ihrer App den Tokenspeicher deaktivieren.

#### <a name="logging-and-tracing"></a>Protokollierung und Nachverfolgung

Wenn Sie die [Anwendungsprotokollierung](troubleshoot-diagnostic-logs.md) aktivieren, werden Ablaufverfolgungen für Authentifizierung und Autorisierung direkt in den Protokolldateien angezeigt. Wenn ein unerwarteter Authentifizierungsfehler angezeigt wird, können Sie alle Details dazu problemlos in den vorhandenen Anwendungsprotokollen finden. Wenn Sie die [Ablaufverfolgung für Anforderungsfehler](troubleshoot-diagnostic-logs.md) aktivieren, sehen Sie genau, welche Rolle das Modul für Authentifizierung und Autorisierung möglicherweise bei einer fehlgeschlagenen Anforderung gespielt hat. Suchen Sie in den Ablaufverfolgungsprotokollen nach Verweisen auf ein Modul namens `EasyAuthModule_32/64`.

## <a name="more-resources"></a>Weitere Ressourcen

- [How-To: Konfigurieren Sie Ihre App Service- oder Azure Functions-App für die Verwendung des Azure AD-Logins](configure-authentication-provider-aad.md)
- [Erweiterte Verwendung der Authentifizierung und Autorisierung in Azure App Service](app-service-authentication-how-to.md)

Beispiele:
- [Tutorial: Hinzufügen der Authentifizierung zu Ihrer Web-App in Azure App Service](scenario-secure-app-authentication-app-service.md)
- [Tutorial: Umfassendes Authentifizieren und Autorisieren von Benutzern in Azure App Service (Windows oder Linux)](tutorial-auth-aad.md)
- [.NET Core-Integration von Azure AppService EasyAuth (Drittanbieter)](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth)
- [Azure App Service-Authentifizierung mit .NET Core (Drittanbieter)](https://github.com/kirkone/KK.AspNetCore.EasyAuthAuthentication)