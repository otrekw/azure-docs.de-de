---
title: Authentifizierung und Autorisierung für Azure Static Web Apps
description: Es wird beschrieben, wie Sie verschiedene Autorisierungsanbieter verwenden, um Ihre statische App zu schützen.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: ab41a336c32a1827c23f4c4619f47dc294a4d2ea
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103419285"
---
# <a name="authentication-and-authorization-for-azure-static-web-apps-preview"></a>Authentifizierung und Autorisierung für Azure Static Web Apps (Vorschau)

Mit Azure Static Web Apps wird der Authentifizierungsablauf optimiert, indem die Authentifizierung mit den folgenden Anbietern verwaltet wird:

- Azure Active Directory
- GitHub
- Facebook
- Google<sup>1</sup>
- Twitter

Mit anbieterspezifischen [Einladungen](#invitations) werden Benutzer Rollen zugeordnet, und autorisierten Benutzern wird Zugriff auf [Routen](routes.md) mithilfe von Regeln gewährt, die in der Datei _routes.json_ definiert sind.

Alle Authentifizierungsanbieter sind standardmäßig aktiviert. [Blockieren Sie den Zugriff](#block-an-authorization-provider) mit einer benutzerdefinierten Routenregel, um einen Authentifizierungsanbieter auszuschließen.

Die Bereiche Authentifizierung und Autorisierung weisen starke Überschneidungen mit Routingkonzepten auf. Lesen Sie zusätzlich zu diesem Artikel den [Leitfaden zum Routing](routes.md).

## <a name="roles"></a>Rollen

Jeder Benutzer, der auf eine statische Web-App zugreift, gehört mindestens einer Rolle an.  Benutzer können zwei integrierten Rollen angehören:

- **anonymous**: Alle Benutzer gehören automatisch der Rolle _anonymous_ an.
- **authenticated**: Alle Benutzer, die angemeldet sind, gehören der Rolle _authenticated_ an.

Zusätzlich zu den integrierten Rollen können Sie neue Rollen erstellen, sie mithilfe von Einladungen Benutzern zuweisen und in der Datei _routes.json_ darauf verweisen.

## <a name="role-management"></a>Rollenverwaltung

### <a name="add-a-user-to-a-role"></a>Hinzufügen eines Benutzers zu einer Rolle

Um Ihrer Website Benutzer hinzuzufügen, generieren Sie Einladungen, mit denen Sie Benutzer bestimmten Rollen zuordnen können. Rollen werden in der Datei _routes.json_ definiert und verwaltet.

<a name="invitations" id="invitations"></a>

#### <a name="create-an-invitation"></a>Erstellen einer Einladung

Einladungen gelten spezifisch für einzelne Autorisierungsanbieter. Aus diesem Grund sollten Sie die Anforderungen Ihrer App beachten, wenn Sie die zu unterstützenden Anbieter auswählen. Einige Anbieter legen die E-Mail-Adresse eines Benutzers offen, während andere nur den Benutzernamen für die Website angeben.

<a name="provider-user-details" id="provider-user-details"></a>

| Autorisierungsanbieter | Offenlegung von Benutzerdaten  |
| ---------------------- | ----------------- |
| Azure Active Directory | E-Mail-Adresse     |
| Facebook               | E-Mail-Adresse     |
| GitHub                 | username          |
| Google<sup>1</sup>     | E-Mail-Adresse     |
| Twitter                | username          |

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu einer Static Web Apps-Ressource.
1. Klicken Sie unter _Einstellungen_ auf **Rollenverwaltung**.
1. Klicken Sie auf die Schaltfläche **Einladen**.
1. Wählen Sie in der Liste mit den Optionen den Eintrag _Autorisierungsanbieter_ aus.
1. Fügen Sie im Feld _Invitee details_ (Details zum Eingeladenen) den Benutzernamen oder die E-Mail-Adresse des Empfängers hinzu.
    - Geben Sie bei GitHub und Twitter den Benutzernamen ein. Geben Sie für alle anderen Anbieter die E-Mail-Adresse des Empfängers ein.
1. Wählen Sie in der Dropdownliste _Domäne_ die Domäne Ihrer statischen Website aus.
    - Die von Ihnen ausgewählte Domäne ist die Domäne, die in der Einladung angezeigt wird. Falls Sie über eine benutzerdefinierte Domäne verfügen, die Ihrer Website zugeordnet ist, möchten Sie wahrscheinlich diese benutzerdefinierte Domäne auswählen.
1. Fügen Sie im Feld _Rolle_ eine Liste mit Rollennamen (durch Kommas getrennt) ein.
1. Geben Sie die Anzahl von Stunden für die maximale Gültigkeitsdauer der Einladung ein.
    - Die Obergrenze beträgt 168 Stunden (7 Tage).
1. Klicken Sie auf die Schaltfläche **Generieren**.
1. Kopieren Sie den Link aus dem Feld _Einladungslink_.
1. Senden Sie eine E-Mail mit dem Einladungslink an die Person, der Sie Zugriff auf Ihre App erteilen möchten.

Wenn der Benutzer in der Einladung auf den Link klickt, wird er aufgefordert, sich mit seinem entsprechenden Konto anzumelden. Nach der erfolgreichen Anmeldung wird der Benutzer den ausgewählten Rollen zugeordnet.

> [!CAUTION]
> Stellen Sie sicher, dass Ihre Routenregeln nicht in Konflikt mit Ihren ausgewählten Authentifizierungsanbietern stehen. Wenn ein Anbieter durch eine Routenregel blockiert wird, können Benutzer keine Einladungen akzeptieren.

### <a name="update-role-assignments"></a>Aktualisieren von Rollenzuweisungen

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu einer Static Web Apps-Ressource.
1. Klicken Sie unter _Einstellungen_ auf **Rollenverwaltung**.
1. Klicken Sie in der Liste auf den Benutzer.
1. Bearbeiten Sie die Liste mit den Rollen im Feld _Rolle_.
1. Klicken Sie auf die Schaltfläche **Aktualisieren**.

### <a name="remove-user"></a>Entfernen von Benutzern

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu einer Static Web Apps-Ressource.
1. Klicken Sie unter _Einstellungen_ auf **Rollenverwaltung**.
1. Suchen Sie in der Liste nach dem Benutzer.
1. Aktivieren Sie das Kontrollkästchen in der Zeile des Benutzers.
1. Klicken Sie auf die Schaltfläche **Löschen**.

Beachten Sie beim Entfernen eines Benutzers Folgendes:

1. Wenn ein Benutzer entfernt wird, werden seine Berechtigungen ungültig.
1. Die weltweite Verteilung kann einige Minuten dauern.
1. Wenn der Benutzer wieder der App hinzugefügt wird, ändert sich die [`userId`](user-information.md).

## <a name="remove-personal-identifying-information"></a>Entfernen von personenbezogenen Informationen

Wenn Sie als Endbenutzer für eine Anwendung Ihre Einwilligung erteilen, hat die Anwendung je nach Identitätsanbieter Zugriff auf Ihre E-Mail-Adresse oder Ihren Benutzernamen. Nachdem diese Informationen bereitgestellt wurden, trifft der Besitzer der Anwendung die Entscheidung, wie personenbezogene Informationen verwaltet werden.

Endbenutzer müssen sich an die Administratoren der einzelnen Web-Apps wenden, um diese Informationen für ihre Systeme zu widerrufen.

Senden Sie über die folgende URL eine Anfrage, um personenbezogene Informationen von der Azure Static Web Apps-Plattform entfernen zu lassen und zu verhindern, dass diese Informationen über die Plattform bei zukünftigen Anforderungen bereitgestellt werden:

```url
https://identity.azurestaticapps.net/.auth/purge/<AUTHENTICATION_PROVIDER_NAME>
```

Senden Sie unter der folgenden URL eine Anfrage, um für einzelne Apps zu verhindern, dass die Plattform diese Informationen bei zukünftigen Anforderungen bereitstellt:

```url
https://<WEB_APP_DOMAIN_NAME>/.auth/purge/<AUTHENTICATION_PROVIDER_NAME>
```

## <a name="system-folder"></a>Systemordner

Azure Static Web Apps nutzt den Systemordner `/.auth`, um Zugriff auf autorisierungsbezogene APIs zu gewähren. Anstatt die Routen im Ordner `/.auth` direkt für Endbenutzer offenzulegen, können Sie erwägen, [Routingregeln](routes.md) für die Erstellung von benutzerfreundlichen URLs festzulegen.

## <a name="login"></a>Anmeldename

Verwenden Sie die folgende Tabelle, um die anbieterspezifische Anmelderoute zu ermitteln.

| Autorisierungsanbieter | Anmelderoute             |
| ---------------------- | ----------------------- |
| Azure Active Directory | `/.auth/login/aad`      |
| Facebook               | `/.auth/login/facebook` |
| GitHub                 | `/.auth/login/github`   |
| Google<sup>1</sup>     | `/.auth/login/google`   |
| Twitter                | `/.auth/login/twitter`  |

Wenn Sie sich beispielsweise mit GitHub anmelden möchten, können Sie einen Anmeldelink wie den folgenden Codeausschnitt einfügen:

```html
<a href="/.auth/login/github">Login</a>
```

Falls Sie mehr als einen Anbieter unterstützen möchten, müssen Sie auf Ihrer Website dafür jeweils einen anbieterspezifischen Link bereitstellen.

Sie können eine [Routenregel](./configuration.md#routes) verwenden, um einen Standardanbieter einer benutzerfreundlichen Route zuzuordnen, z. B. _/login_.

```json
{
  "route": "/login",
  "redirect": "/.auth/login/github"
}
```

### <a name="post-login-redirect"></a>Umleitung nach der Anmeldung

Wenn ein Benutzer nach der Anmeldung zu einer bestimmten Seite zurückgeleitet werden soll, geben Sie eine URL im `post_login_redirect_uri`-Abfragezeichenfolgenparameter an.

## <a name="logout"></a>Logout

Mit der Route `/.auth/logout` werden Benutzer von der Website abgemeldet. Sie können einen Link zu Ihrer Sitenavigation hinzufügen, um dem Benutzer das Abmelden zu ermöglichen. Dies ist im folgenden Beispiel dargestellt.

```html
<a href="/.auth/logout">Log out</a>
```

Sie können eine [Routenregel](./configuration.md#routes) verwenden, um eine benutzerfreundliche Route wie _/logout_ zuzuordnen.

```json
{
  "route": "/logout",
  "redirect": "/.auth/logout"
}
```

### <a name="post-logout-redirect"></a>Umleitung nach der Abmeldung

Wenn ein Benutzer nach der Abmeldung zu einer bestimmten Seite zurückgeleitet werden soll, geben Sie eine URL im `post_logout_redirect_uri`-Abfragezeichenfolgenparameter an.

## <a name="block-an-authorization-provider"></a>Blockieren eines Autorisierungsanbieters

Unter Umständen möchten Sie für Ihr App die Verwendung eines Autorisierungsanbieters verhindern. Es kann beispielsweise sein, dass für Ihre App standardmäßig nur [Anbieter genutzt werden sollen, die E-Mail-Adressen offenlegen](#provider-user-details).

Zum Blockieren eines Anbieters können Sie [Routenregeln](routes.md) erstellen, um einen 404-Fehler für Anforderungen zurückzugeben, die an die blockierte anbieterspezifische Route gesendet werden. Fügen Sie beispielsweise die folgende Routenregel hinzu, um Twitter als Anbieter auszuschließen.

```json
{
  "route": "/.auth/login/twitter",
  "statusCode": "404"
}
```

## <a name="restrictions"></a>Beschränkungen

Allgemeine Einschränkungen und Grenzwerte finden Sie im [Artikel zu Kontingenten](quotas.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Zugreifen auf Daten der Benutzerauthentifizierung und -autorisierung](user-information.md)

<sup>1</sup> Externe Zertifizierung ausstehend.
