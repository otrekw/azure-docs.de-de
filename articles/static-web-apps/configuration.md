---
title: Konfigurieren von Azure Static Web Apps
description: Erfahren Sie, wie Sie Routen konfigurieren, Sicherheitsregeln erzwingen und globale Einstellungen für Azure Static Web Apps festlegen.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: cshoe
ms.openlocfilehash: 324a8e75488d74fc6aa52e499b8dde616cd9beb5
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102034046"
---
# <a name="configure-azure-static-web-apps"></a>Konfigurieren von Azure Static Web Apps

Die Konfiguration für Azure Static Web Apps erfolgt in der Datei _staticwebapp.config.json_, die die folgenden Einstellungen steuert:

- Routing
- Authentifizierung
- Autorisierung
- Fallbackregeln
- Außerkraftsetzungen von HTTP-Antworten
- Globale HTTP-Headerdefinitionen
- Benutzerdefinierte MIME-Typen

## <a name="file-location"></a>Dateispeicherort

Der für die Datei _staticwebapp.config.json_ empfohlene Speicherort ist der Ordner, der in der [Workflowdatei](./github-actions-workflow.md) als `app_location` festgelegt wurde. Die Datei kann jedoch an einem beliebigen Ort im Quellcodeordner Ihrer Anwendung abgelegt werden.

Details finden Sie unter [Beispielkonfigurationsdatei](#example-configuration-file).

> [!IMPORTANT]
> Die Datei [_routes.json_](./routes.md) wird ignoriert, wenn die Datei _staticwebapp.config.json_ vorhanden ist.

## <a name="routes"></a>Routen

Mit Routenregeln können Sie das Muster der URLs definieren, die den Zugriff auf Ihre Anwendung im Web erlauben. Routen werden als Array von Routingregeln definiert. Beispiele der Nutzung finden Sie unter [Beispielkonfigurationsdatei](#example-configuration-file).

- Regeln werden in der Reihenfolge ausgeführt, in der sie im Array `routes` angezeigt werden.
- Die Regelauswertung endet bei der ersten Übereinstimmung, denn Routingregeln werden nicht miteinander verkettet.
- Sie haben die vollständige Kontrolle über benutzerdefinierte Rollennamen.
  - Es gibt einige integrierte Rollennamen, zu denen [`anonymous`](./authentication-authorization.md) und [`authenticated`](./authentication-authorization.md) gehören.

Die Belange des Routings überschneiden sich erheblich mit den Konzepten der Authentifizierung (Identifizierung des Benutzers) und Autorisierung (Zuweisung von Berechtigungen zum Benutzer). Lesen Sie zusätzlich zu diesem Artikel den Leitfaden zur [Authentifizierung und Autorisierung](authentication-authorization.md).

Die Standarddatei für statische Inhalte ist *index.html*.

## <a name="defining-routes"></a>Definieren von Routen

Jede Regel besteht aus einem Routenmuster und einer oder mehreren optionalen Regeleigenschaften. Beispiele der Nutzung finden Sie unter [Beispielkonfigurationsdatei](#example-configuration-file).

| Regeleigenschaft  | Erforderlich | Standardwert | Comment                                                      |
| -------------- | -------- | ------------- | ------------------------------------------------------------ |
| `route`        | Ja      | –          | Das vom Aufrufer angeforderte Routenmuster.<ul><li>[Platzhalter](#wildcards) am Ende der Routenpfade werden unterstützt.<ul><li>Für die Route _admin/\*_ ergeben sich beispielsweise Übereinstimmungen mit allen Routen unter dem Pfad _admin_.</ul></ul>|
| `rewrite`        | Nein       | –          | Definiert die Datei oder den Pfad, die bzw. der von der Anforderung zurückgegeben wird.<ul><li>Ist sich gegenseitig ausschließend mit einer `redirect`-Regel<li>Neuschreibungsregeln ändern nicht den Speicherort des Browsers.<li>Werte müssen relativ zum Stamm der App sein</ul>  |
| `redirect`        | Nein       | –          | Legt das Ziel der Datei- oder Pfadumleitung für eine Anforderung fest.<ul><li>Ist sich gegenseitig ausschließend mit einer `rewrite`-Regel.<li>Umleitungsregeln ändern den Speicherort des Browsers.<li>Der Standardantwortcode ist [`302`](https://developer.mozilla.org/docs/Web/HTTP/Status/302) (temporäre Umleitung), aber Sie können ihn mit [`301`](https://developer.mozilla.org/docs/Web/HTTP/Status/301) überschreiben (permanente Umleitung).</ul> |
| `allowedRoles` | Nein       | anonymous     | Definiert eine Liste von Rollennamen, die für den Zugriff auf eine Route erforderlich sind. <ul><li>Gültige Zeichen sind `a-z`, `A-Z`, `0-9` und `_`.<li>Die integrierte Rolle [`anonymous`](./authentication-authorization.md) gilt für alle nicht authentifizierten Benutzer.<li>Die integrierte Rolle [`authenticated`](./authentication-authorization.md) gilt für alle angemeldeten Benutzer.<li>Benutzern muss mindestens eine Rolle zugewiesen sein.<li>Rollen werden nach dem Muster _OR_ (ODER) abgeglichen.<ul><li>Wenn einem Benutzer eine beliebige aufgelistete Rolle zugewiesen ist, wird der Zugriff gewährt.</ul><li>Einzelne Benutzer werden Rollen mithilfe von [Einladungen](authentication-authorization.md) zugeordnet.</ul> |
| `headers`<a id="route-headers"></a> | Nein | – | Gruppe von [HTTP-Headern](https://developer.mozilla.org/docs/Web/HTTP/Headers), die der Antwort hinzugefügt werden. <ul><li>Routenspezifische Header überschreiben [`globalHeaders`](#global-headers), wenn der routenspezifische Header dem globalen Header in der Antwort entspricht.<li>Um einen Header zu entfernen, legen Sie den Wert auf eine leere Zeichenfolge fest.</ul> |
| `statusCode`   | Nein       | `200`, `301` oder `302` für Umleitungen | Der [HTTP-Statuscode](https://developer.mozilla.org/docs/Web/HTTP/Status) der Antwort. |
| `methods` | Nein | Alle Methoden | Liste der Anforderungsmethoden, die einer Route entsprechen. Verfügbare Methoden: `GET`, `HEAD`, `POST`, `PUT`, `DELETE`, `CONNECT`, `OPTIONS`, `TRACE` und `PATCH`. |

Jede Eigenschaft hat in der Anforderungs-/Antwortpipeline einen bestimmten Zweck.

| Zweck | Eigenschaften |
|---|---|
| Routen abgleichen | `route`, `methods` |
| Autorisieren, nachdem für eine Route eine Übereinstimmung gefunden wurde | `allowedRoles` |
| Verarbeiten, nachdem für eine Route eine Übereinstimmung gefunden und sie autorisiert wurde | `rewrite` (ändert Anforderung) <br><br>`redirect`, `headers`, `statusCode` (ändert Antwort) |

## <a name="securing-routes-with-roles"></a>Schützen von Routen mit Rollen

Routen werden durch Hinzufügen eines oder mehrerer Rollennamen zum Array `allowedRoles` einer Regel abgesichert. Benutzer werden benutzerdefinierten Rollen über [Einladungen](./authentication-authorization.md) zugeordnet. Beispiele der Nutzung finden Sie unter [Beispielkonfigurationsdatei](#example-configuration-file).

Standardmäßig gehört jeder Benutzer zur integrierten Rolle `anonymous`, und alle angemeldeten Benutzer sind Mitglieder der Rolle `authenticated`.

Wenn Sie eine Route beispielsweise nur auf authentifizierte Benutzer beschränken möchten, fügen Sie die integrierte Rolle `authenticated` dem Array `allowedRoles` hinzu.

```json
{
  "route": "/profile",
  "allowedRoles": ["authenticated"]
}
```

Sie können je nach Bedarf im Array `allowedRoles` neue Rollen erstellen. Um eine Route nur auf Administratoren zu beschränken, können Sie eine eigene Rolle mit dem Namen `administrator` im Array `allowedRoles` definieren.

```json
{
  "route": "/admin",
  "allowedRoles": ["administrator"]
}
```

- Sie haben die volle Kontrolle über Rollennamen. Es gibt keine Liste, an die sich Ihre Rollen halten müssen.
- Einzelne Benutzer werden Rollen mithilfe von [Einladungen](authentication-authorization.md) zugeordnet.

## <a name="wildcards"></a>Platzhalter

Platzhalterregeln stimmen mit allen Anforderungen in einem Routenmuster überein, werden nur am Ende eines Pfads unterstützt und können nach Dateierweiterung gefiltert werden. Beispiele der Nutzung finden Sie unter [Beispielkonfigurationsdatei](#example-configuration-file).

Falls Sie beispielsweise Routen für eine Kalenderanwendung implementieren möchten, können Sie alle URLs neu schreiben, die unter die Route _calendar_ fallen, um eine einzelne Datei bereitzustellen.

```json
{
  "route": "/calendar/*",
  "rewrite": "/calendar.html"
}
```

Für die Datei _calendar.html_ kann dann das clientseitige Routing verwendet werden, um eine andere Ansicht für URL-Varianten wie `/calendar/january/1`, `/calendar/2020` und `/calendar/overview` bereitzustellen.

Sie können Platzhalterübereinstimmungen nach Dateierweiterung filtern. Wenn Sie z. B. eine Regel hinzufügen möchten, die einen Abgleich nur auf HTML-Dateien in einem bestimmten Pfad vorsieht, könnten Sie die folgende Regel erstellen:

```json
{
  "route": "/articles/*.html",
  "headers" : {
    "Cache-Control": "public, max-age=604800, immutable"
  }
}
```

Um nach mehreren Dateierweiterungen zu filtern, schließen Sie die Optionen in geschweifte Klammern ein, wie in diesem Beispiel gezeigt:

```json
{
  "route": "/images/thumbnails/*.{png,jpg,gif}",
  "headers": {
    "Cache-Control": "public, max-age=604800, immutable"
  }
}
```

Gängige Anwendungsfälle für Platzhalterrouten sind z. B.:

- Bereitstellen einer bestimmten Datei für ein ganzes Pfadmuster
- Zuordnen verschiedener HTTP-Methoden zu einem ganzen Pfadmuster
- Erzwingen von Authentifizierungs- und Autorisierungsregeln
- Implementieren spezieller Zwischenspeicherregeln

## <a name="fallback-routes"></a>Fallbackrouten

Single-Page-Anwendungen basieren häufig auf clientseitigem Routing. Diese clientseitigen Routingregeln aktualisieren den Fensterspeicherort des Browsers, ohne Anforderungen zurück an den Servern zu senden. Wenn Sie die Seite aktualisieren oder direkt zu URLs navigieren, die mit clientseitigen Routingregeln generiert wurden, ist eine serverseitige Fallbackroute erforderlich, um die richtige HTML-Seite bereitzustellen (die im Allgemeinen für Ihre clientseitige App _index.html_ ist).

Sie können Ihre Anwendung so konfigurieren, dass sie Regeln verwendet, die eine Fallbackroute implementieren, wie im folgenden Beispiel gezeigt, in dem ein Pfadplatzhalter mit Dateifilter verwendet wird:

```json
{
  "navigationFallback": {
    "rewrite": "/index.html",
    "exclude": ["/images/*.{png,jpg,gif}", "/css/*"]
  }
}
```

Die nachstehende Beispieldateistruktur zeigt, dass die folgenden Ergebnisse mit dieser Regel möglich sind.

```files
├── images
│   ├── logo.png
│   ├── headshot.jpg
│   └── screenshot.gif
│
├── css
│   └── global.css
│
└── index.html
```

| Anforderungen an | Rückgabe | Mit dem Status |
| --- | --- | --- |
| */about/* | Die Datei */index.html* | `200` |
| */images/logo.png* | Die Bilddatei  | `200` |
| */images/icon.svg* | Die Datei */index.html*, da die Dateierweiterung *svg* nicht im Filter `/images/*.{png,jpg,gif}` aufgeführt ist   | `200` |
| */images/unknown.png* | Fehler „Datei nicht gefunden“  | `404` |
| */css/unknown.css* | Fehler „Datei nicht gefunden“  | `404` |
| */css/global.css* | Die Stylesheetdatei | `200` |
| Alle anderen Dateien außerhalb der Ordner */images* und */css* | Die Datei */index.html* | `200` |

## <a name="global-headers"></a>Globale Header

Der Abschnitt `globalHeaders` stellt eine Gruppe von [HTTP-Headern](https://developer.mozilla.org/docs/Web/HTTP/Headers) bereit, die auf jede Antwort angewendet werden, es sei denn, sie werden durch eine Regel für [Routenheader](#route-headers) überschrieben. Andernfalls wird die Vereinigung sowohl der Header aus der Route als auch der globalen Header zurückgegeben.

Beispiele der Nutzung finden Sie unter [Beispielkonfigurationsdatei](#example-configuration-file).

Um einen Header zu entfernen, legen Sie den Wert auf eine leere Zeichenfolge (`""`) fest.

Zu den gängigen Anwendungsfällen für globale Header gehören u. a.:

- Benutzerdefinierte Cacheregeln
- Erzwingen von Sicherheitsrichtlinien
- Codierungseinstellungen

## <a name="response-overrides"></a>Außerkraftsetzungen von Antworten

Der Abschnitt `responseOverrides` bietet die Möglichkeit, eine benutzerdefinierte Antwort zu definieren, wenn der Server sonst einen Fehlercode zurückgeben würde. Beispiele der Nutzung finden Sie unter [Beispielkonfigurationsdatei](#example-configuration-file).

Die folgenden HTTP-Codes sind zur Außerkraftsetzung verfügbar:

| Statuscode | Bedeutung | Mögliche Ursache |
| --- | --- | --- |
| [400](https://developer.mozilla.org/docs/Web/HTTP/Status/400) | Ungültige Anforderung | Link zu ungültiger Einladung |
| [401](https://developer.mozilla.org/docs/Web/HTTP/Status/401) | Nicht autorisiert | Anforderung eingeschränkter Seiten bei fehlender Authentifizierung |
| [403](https://developer.mozilla.org/docs/Web/HTTP/Status/403) | Verboten |<ul><li>Benutzer ist angemeldet, verfügt aber nicht über die erforderlichen Rollen, um die Seite anzuzeigen.<li>Benutzer ist angemeldet, aber die Laufzeit kann die Benutzerdetails nicht aus seinen Identitätsansprüchen abrufen.<li>Es sind zu viele Benutzer mit benutzerdefinierten Rollen bei der Website angemeldet, daher kann die Laufzeit den Benutzer nicht anmelden.</ul> |
| [404](https://developer.mozilla.org/docs/Web/HTTP/Status/404) | Nicht gefunden | Datei nicht gefunden |

Die folgende Beispielkonfiguration zeigt, wie Sie einen Fehlercode überschreiben können.

```json
{
    "responseOverrides": {
        "400" : {
            "rewrite": "/invalid-invitation-error.html",
            "statusCode": 200
        },
        "401": {
            "statusCode": 302,
            "redirect": "/login"
        },
        "403": {
            "rewrite": "/custom-forbidden-page.html",
            "statusCode": 200
        },
        "404": {
            "rewrite": "/custom-404.html",
            "statusCode": 200
        }
    }
}
```

## <a name="example-configuration-file"></a>Beispielkonfigurationsdatei

```json
{
    "routes": [
        {
            "route": "/profile",
            "allowedRoles": ["authenticated"]
        },
        {
            "route": "/admin/*",
            "allowedRoles": ["administrator"]
        },
        {
            "route": "/images/*",
            "headers": {
                "cache-control": "must-revalidate, max-age=15770000"
            }
        },
        {
            "route": "/api/*",
            "methods": [ "GET" ],
            "allowedRoles": ["registeredusers"]
        },
        {
            "route": "/api/*",
            "methods": [ "PUT", "POST", "PATCH", "DELETE" ],
            "allowedRoles": ["administrator"]
        },
        {
            "route": "/api/*",
            "allowedRoles": ["authenticated"]
        },
        {
            "route": "/customers/contoso",
            "allowedRoles": ["administrator", "customers_contoso"]
        },
        {
            "route": "/login",
            "rewrite": "/.auth/login/github"
        },
        {
            "route": "/.auth/login/twitter",
            "statusCode": 404,
        },
        {
            "route": "/logout",
            "redirect": "/.auth/logout"
        },
        {
            "route": "/calendar/*",
            "rewrite": "/calendar.html"
        },
        {
            "route": "/specials",
            "redirect": "/deals",
            "statusCode": 301
        }
    ],
    "navigationFallback": {
      "rewrite": "index.html",
      "exclude": ["/images/*.{png,jpg,gif}", "/css/*"]
    },
    "responseOverrides": {
        "400" : {
            "rewrite": "/invalid-invitation-error.html"
        },
        "401": {
            "redirect": "/login",
            "statusCode": 302
        },
        "403": {
            "rewrite": "/custom-forbidden-page.html"
        },
        "404": {
            "rewrite": "/404.html"
        }
    },
    "globalHeaders": {
        "content-security-policy": "default-src https: 'unsafe-eval' 'unsafe-inline'; object-src 'none'"
    },
    "mimeTypes": {
        ".json": "text/json"
    },
}
```

Überprüfen Sie auf Grundlage der obigen Konfiguration die folgenden Szenarien.

| Anforderungen an | Ergebnis |
| --- | --- |
| _/profile_ | Für authentifizierte Benutzer wird die Datei _/profile/index.html_ bereitgestellt. Nicht authentifizierte Benutzer werden zu _/login_ umgeleitet. |
| _/admin/_ | Für authentifizierte Benutzer mit der Rolle _administrator_ wird die Datei _/admin/index.html_ bereitgestellt. Für authentifizierte Benutzer ohne die Rolle _administrator_ wird ein `403`-Fehler<sup>1</sup> angezeigt. Nicht authentifizierte Benutzer werden zu _/login_ umgeleitet. |
| _/logo.png_ | Stellt das Image mit einer benutzerdefinierten Cacheregel bereit, deren maximales Alter bei etwas mehr als 182 Tagen (15.770.000 Sekunden) liegt. |
| _/api/admin_ | `GET`-Anforderungen von authentifizierten Benutzern in der Rolle _registeredusers_ werden an die API gesendet. Authentifizierte Benutzer ohne die Rolle _registeredusers_ und nicht authentifizierte Benutzer erhalten den Fehler `401`.<br/><br/>Die Anforderungen `POST`, `PUT`, `PATCH` und `DELETE` von authentifizierten Benutzern mit der Rolle _administrator_ werden an die API gesendet. Für authentifizierte Benutzer ohne die Rolle _administrator_ und nicht authentifizierte Benutzer wird der Fehler `401` angezeigt. |
| _/customers/contoso_ | Für authentifizierte Benutzer mit entweder der Rolle _administrator_ oder _customers\_contoso_ wird die Datei _/customers/contoso/index.html_ bereitgestellt. Für authentifizierte Benutzer ohne die Rolle _administrator_ oder _customers\_contoso_ wird der Fehler`403`<sup>1</sup> angezeigt. Nicht authentifizierte Benutzer werden zu _/login_ umgeleitet. |
| _/login_ | Nicht authentifizierte Benutzer werden aufgefordert, sich bei GitHub zu authentifizieren. |
| _/.auth/login/twitter_ | Da die Autorisierung bei Twitter durch die Routenregel deaktiviert ist, wird der Fehler `404` zurückgegeben, wodurch ein Fallback mit Bereitstellung von _/index.html_ mit dem Statuscode `200` erfolgt. |
| _/logout_ | Benutzer werden von allen Authentifizierungsanbietern abgemeldet. |
| _/calendar/2021/01_ | Für den Browser wird die Datei _/calendar.html_ bereitgestellt. |
| _/specials_ | Der Browser wird dauerhaft zu _/deals_ umgeleitet. |
| _/data.json_ | Die mit dem MIME-Typ `text/json` bereitgestellte Datei. |
| _/about_ oder ein beliebiger Ordner, der mit clientseitigen Routingmustern übereinstimmt | Die Datei _/index.html_ wird mit dem Statuscode `200` bereitgestellt. |
| Eine nicht vorhandene Datei im Ordner _/images/_ . | Ein `404`-Fehler. |

<sup>1</sup> Sie können eine benutzerdefinierte Fehlerseite bereitstellen, indem Sie eine [Antwortüberschreibungsregel](#response-overrides) verwenden.

## <a name="restrictions"></a>Beschränkungen

Die folgenden Einschränkungen gelten für die Datei _staticwebapps.config.json_.

- Maximale Dateigröße: 100 KB
- Maximal 50 verschiedene Rollen

Allgemeine Einschränkungen und Grenzwerte finden Sie im [Artikel zu Kontingenten](quotas.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Einrichten der Authentifizierung und Autorisierung](authentication-authorization.md)
