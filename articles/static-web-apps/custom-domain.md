---
title: Einrichten einer benutzerdefinierten Domäne in Azure Static Web Apps
description: Es wird beschrieben, wie Sie Azure Static Web Apps eine benutzerdefinierte Domäne zuordnen.
services: static-web-apps
author: burkeholland
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: buhollan
ms.openlocfilehash: 61ec96a35fac6a033fe6c8b65cff156ba63e5e58
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2020
ms.locfileid: "87563346"
---
# <a name="setup-a-custom-domain-in-azure-static-web-apps-preview"></a>Einrichten einer benutzerdefinierten Domäne in Azure Static Web Apps (Vorschau)

Standardmäßig wird von Azure Static Web Apps ein automatisch generierter Domänenname bereitgestellt. In diesem Artikel wird veranschaulicht, wie Sie einer Azure Static Web Apps-Anwendung den Namen einer benutzerdefinierten Domäne zuordnen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein erworbener Domänenname
- Zugriff auf die DNS-Konfigurationseigenschaften für Ihre Domäne

Beim Konfigurieren von Domänennamen werden A-Einträge verwendet, um Stammdomänen (z. B. `example.com`) einer IP-Adresse zuzuordnen. Stammdomänen müssen direkt einer IP-Adresse zugeordnet werden, weil die DNS-Spezifikation die Zuordnung von einer Domäne zu einer anderen nicht zulässt.

## <a name="dns-configuration-options"></a>DNS-Konfigurationsoptionen

Für eine Anwendung sind verschiedene Arten von DNS-Konfigurationen verfügbar.

| Zweck                                 | Then                                                |
| -----------------------------------------------| --------------------------------------------------- |
| Unterstützung `www.example.com` oder `blog.example.net`| [Zuordnen eines CNAME-Eintrags](#map-a-cname-record)           |
| Unterstützung für `example.com`                          | [Konfigurieren einer Stammdomäne](#configure-a-root-domain) |
| Verweisen auf `www.example.com` für alle Unterdomänen      | [Zuordnen eines Platzhalters](#map-a-wildcard-domain)            |

## <a name="map-a-cname-record"></a>Zuordnen eines CNAME-Eintrags

Mit einem CNAME-Eintrag wird eine Domäne einer anderen zugeordnet. Sie können einen CNAME-Eintrag verwenden, um `www.example.com`, `blog.example.com` oder jede andere Unterdomäne der automatisch generierten Domäne zuzuordnen, die von Azure Static Web Apps bereitgestellt wird.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

1. Suchen Sie nach **Static Web Apps**, und wählen Sie die Option aus.

1. Wählen Sie auf der Seite _Static Web Apps_ den Namen Ihrer App aus.

1. Klicken Sie im Menü auf **Benutzerdefinierte Domänen**.

1. Kopieren Sie im Fenster _Benutzerdefinierte Domänen_ die URL im Feld **Wert**.

### <a name="configure-dns-provider"></a>Konfigurieren des DNS-Anbieters

1. Melden Sie sich bei der Website Ihres Domänenanbieters an.

2. Suchen Sie die Seite für die Verwaltung von DNS-Einträgen. Da jeder Domänenanbieter eine eigene Benutzeroberfläche für DNS-Einträge hat, finden Sie Informationen dazu jeweils in der Dokumentation des Anbieters. Suchen Sie nach Bereichen der Website, die mit **Domänenname**, **DNS** oder **Namenserververwaltung** gekennzeichnet sind.

3. Die Seite für DNS-Einträge erreichen Sie häufig in Ihren Kontoinformationen über einen Link wie **Eigene Domänen**. Navigieren Sie auf diese Seite, und suchen Sie nach einem Link, der einen ähnlichen Namen wie **Zonendatei**, **DNS-Einträge** oder **Erweiterte Konfiguration** hat.

    Der folgende Screenshot zeigt ein Beispiel für eine Seite mit DNS-Einträgen:

    :::image type="content" source="media/custom-domain/example-record-ui.png" alt-text="Beispiel für die Konfiguration eines DNS-Anbieters":::

4. Erstellen Sie einen neuen **CNAME**-Eintrag mit den folgenden Werten:

    | Einstellung             | Wert                     |
    | ------------------- | ------------------------- |
    | type                | CNAME                     |
    | Host                | www                       |
    | Wert               | Aus Zwischenablage einfügen |
    | TTL (falls zutreffend) | Standardwert übernehmen    |

5. Speichern Sie die Änderungen mit Ihrem DNS-Anbieter.

### <a name="validate-cname"></a>Überprüfen von CNAME

1. Wechseln Sie zurück zum Fenster _Benutzerdefinierte Domänen_ im Azure-Portal.

1. Geben Sie Ihre Domäne (einschließlich `www`) im Abschnitt _Benutzerdefinierte Domäne überprüfen_ ein.

1. Klicken Sie auf die Schaltfläche **Überprüfen**.

Nachdem die benutzerdefinierte Domäne nun konfiguriert wurde, kann es mehrere Stunden dauern, bis die Änderungen vom DNS-Anbieter an alle weltweiten Standorte weitergegeben wurden. Sie können den Stand der Weitergabe unter [dnspropagation.net](https://dnspropagation.net) überprüfen. Geben Sie Ihre benutzerdefinierte Domäne (einschließlich `www`) ein, wählen Sie in der Dropdownliste die Option „CNAME“ aus, und klicken Sie auf **Starten**.

Nachdem Ihre DNS-Änderungen eingefügt wurden, gibt die Website die automatisch generierte URL Ihrer statischen Web-App zurück (z. B. _random-name-123456789c.azurestaticapps.net_).

## <a name="configure-a-root-domain"></a>Konfigurieren einer Stammdomäne

Die Stammdomäne ist Ihre Domäne abzüglich aller Unterdomänen (einschließlich `www`). Für `www.example.com` lautet die Stammdomäne beispielsweise `example.com`. Dies wird auch als „APEX-Domäne“ bezeichnet.

Unterstützung für Stammdomänen ist während der Vorschauphase nicht verfügbar. Der Blogbeitrag zum Thema [Konfigurieren von Stammdomänen in Azure Static Web Apps](https://burkeholland.github.io/posts/static-app-root-domain) enthält aber ausführliche Informationen dazu, wie Sie die Unterstützung für Stammdomänen mit einer statischen Web-App konfigurieren.

## <a name="map-a-wildcard-domain"></a>Zuordnen einer Platzhalterdomäne

Es kann vorkommen, dass bei Ihnen der gesamte Datenverkehr, der an eine Unterdomäne gesendet wird, an eine andere Domäne weitergeleitet werden soll. Ein häufiges Beispiel ist die Zuordnung des gesamten Unterdomänen-Datenverkehrs zu `www.example.com`. Auch wenn dann `w.example.com` anstelle von `www.example.com` eingegeben wird, wird die Anforderung trotzdem an `www.example.com` gesendet.

### <a name="configure-dns-provider"></a>Konfigurieren des DNS-Anbieters

1. Melden Sie sich bei der Website Ihres Domänenanbieters an.

2. Suchen Sie die Seite für die Verwaltung von DNS-Einträgen. Da jeder Domänenanbieter eine eigene Benutzeroberfläche für DNS-Einträge hat, finden Sie Informationen dazu jeweils in der Dokumentation des Anbieters. Suchen Sie nach Bereichen der Website, die mit **Domänenname**, **DNS** oder **Namenserververwaltung** gekennzeichnet sind.

3. Die Seite für DNS-Einträge erreichen Sie häufig in Ihren Kontoinformationen über einen Link wie **Eigene Domänen**. Navigieren Sie auf diese Seite, und suchen Sie nach einem Link, der einen ähnlichen Namen wie **Zonendatei**, **DNS-Einträge** oder **Erweiterte Konfiguration** hat.

    Der folgende Screenshot zeigt ein Beispiel für eine Seite mit DNS-Einträgen:

    :::image type="content" source="media/custom-domain/example-record-ui.png" alt-text="Beispiel für die Konfiguration eines DNS-Anbieters":::

4. Erstellen Sie einen neuen **CNAME**-Eintrag mit den folgenden Werten, und ersetzen Sie `www.example.com` durch den Namen Ihrer benutzerdefinierten Domäne.

    | Einstellung | Wert                  |
    | ------- | ---------------------- |
    | type    | CNAME                  |
    | Host    | \*                     |
    | Wert   | www.example.com        |
    | TTL     | Standardwert übernehmen |

5. Speichern Sie die Änderungen mit Ihrem DNS-Anbieter.

Nachdem die Platzhalterdomäne nun konfiguriert wurde, kann es mehrere Stunden dauern, bis die Änderungen an alle weltweiten Standorte verteilt wurden. Sie können den Stand der Weitergabe unter [dnspropagation.net](https://dnspropagation.net) überprüfen. Geben Sie Ihre benutzerdefinierte Domäne mit allen Unterdomänen (zusätzlich zu `www`) ein, und wählen Sie in der Dropdownliste die Option „CNAME“ und dann **Starten** aus.

Nachdem Ihre DNS-Änderungen eingefügt wurden, kehrt die Website zu Ihrer benutzerdefinierten Domäne zurück, die für Ihre statische Web-App (z. B. `www.example.com`) konfiguriert wurde.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfigurieren von App-Einstellungen](application-settings.md)
