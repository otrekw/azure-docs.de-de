---
title: Einrichten einer benutzerdefinierten Domäne in Azure Static Web Apps
description: Erfahren Sie, wie Sie eine benutzerdefinierte Domäne mit kostenlosem SSL/TLS-Zertifikat zu Azure Static Web Apps zuordnen.
services: static-web-apps
author: burkeholland
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 05/12/2021
ms.author: buhollan
ms.openlocfilehash: fd8df4e162b33aef8a0e929da818e8b961953d9b
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110066111"
---
# <a name="set-up-a-custom-domain-with-free-certificate-in-azure-static-web-apps"></a>Einrichten einer benutzerdefinierten Domäne mit kostenlosem Zertifikat in Azure Static Web Apps

Standardmäßig wird von Azure Static Web Apps ein automatisch generierter Domänenname bereitgestellt. In diesem Artikel wird veranschaulicht, wie Sie einer Azure Static Web Apps-Anwendung den Namen einer benutzerdefinierten Domäne zuordnen.

## <a name="free-ssltls-certificate"></a>Kostenloses SSL/TLS-Zertifikat

Azure Static Web Apps stellt automatisch ein kostenloses SSL/TLS-Zertifikat für den automatisch generierten Domänennamen und alle benutzerdefinierten Domänen bereit, die Sie hinzufügen können.

## <a name="walkthrough-video"></a>Video mit exemplarischer Vorgehensweise

> [!VIDEO https://channel9.msdn.com/Shows/5-Things/Configuring-a-custom-domain-with-Azure-Static-Web-Apps/player?format=ny]

## <a name="prerequisites"></a>Voraussetzungen

- Ein erworbener Domänenname
- Zugriff auf die DNS-Konfigurationseigenschaften für Ihre Domäne

## <a name="dns-configuration-options"></a>DNS-Konfigurationsoptionen

Für eine Anwendung sind verschiedene Arten von DNS-Konfigurationen verfügbar.

| Szenario                                                                                 | Beispiel                                | Domänenvalidierungsmethode | DNS-Eintragstyp |
| ---------------------------------------------------------------------------------------- | -------------------------------------- | ------------------------ | --------------- |
| [Hinzufügen einer Stamm-/Apex-Domäne](#add-domain-using-txt-record-validation)                        | `mydomain.com`, `example.co.uk`        | TXT                      | ALIAS           |
| [Hinzufügen einer Unterdomäne](#add-domain-using-cname-record-validation)                             | `www.mydomain.com`, `foo.mydomain.com` | CNAME                    | CNAME           |
| [Übertragen einer derzeit verwendeten Unterdomäne](#add-domain-using-txt-record-validation) | `www.mydomain.com`, `foo.mydomain.com` | TXT                      | CNAME           |

## <a name="add-domain-using-cname-record-validation"></a>Hinzufügen einer Domäne mithilfe der CNAME-Eintragsvalidierung

Die CNAME-Eintragsvalidierung ist die empfohlene Methode zum Hinzufügen einer benutzerdefinierten Domäne, die jedoch nur für Unterdomänen funktioniert. Wenn Sie eine Stammdomäne (`mydomain.com`) hinzufügen möchten, fahren Sie mit [Hinzufügen einer Domäne mithilfe der TXT-Eintragsvalidierung](#add-domain-using-txt-record-validation) und anschließendem [Erstellen eines ALIAS-Eintrags](#create-an-alias-record) fort.

> [!IMPORTANT]
> Wenn Ihre Unterdomäne derzeit einer Livesite zugeordnet ist und Sie nicht bereit sind, sie in Ihre statische Web-App zu übertragen, verwenden Sie die [TXT-Eintragsvalidierung](#add-domain-using-txt-record-validation).

### <a name="enter-your-subdomain"></a>Eintragen Ihrer Unterdomäne

1. Öffnen Sie Ihre statische Web-App im [Azure-Portal](https://portal.azure.com).

1. Wählen Sie im Menü **Benutzerdefinierte Domänen** aus.

1. Wählen Sie die Schaltfläche **Hinzufügen** aus.

1. Geben Sie im Feld _Domänenname_ Ihre Unterdomäne ein. Stellen Sie sicher, dass Sie sie ohne jegliche Protokolle eingeben. Beispiel: `www.mydomain.com`.

   :::image type="content" source="media/custom-domain/add-subdomain.png" alt-text="Bildschirm „Domäne hinzufügen“ mit der benutzerdefinierten Unterdomäne im Eingabefeld":::

1. Wählen Sie die Schaltfläche **Weiter** aus, um zum Schritt _Überprüfen + Konfigurieren_ zu wechseln.

### <a name="configure-cname-with-your-domain-provider"></a>Konfigurieren des CNAME-Eintrags mit Ihrem Domänenanbieter

Sie müssen einen CNAME-Eintrag mit Ihrem Domänenanbieter konfigurieren. Azure DNS wird empfohlen, aber diese Schritte funktionieren mit jedem Domänenanbieter.

# <a name="azure-dns"></a>[Azure DNS](#tab/azure-dns)

1. Stellen Sie sicher, dass **CNAME** in der Dropdownliste _Typ des Hostnamenseintrags_ ausgewählt ist.

1. Kopieren Sie den Wert im Feld _Wert_ in die Zwischenablage, indem Sie das **Kopieren**-Symbol auswählen.

   :::image type="content" source="media/custom-domain/copy-cname.png" alt-text="Bildschirm „Überprüfen + Hinzufügen“ mit ausgewähltem CNAME-Eintrag und umrandetem Kopieren-Symbol":::

1. Öffnen Sie ihre Azure DNS-Zone in einer separaten Browserregisterkarte oder einem gesonderten Browserfenster im Azure-Portal.

1. Wählen Sie die Schaltfläche **+ Eintragssatz** aus.

1. Erstellen Sie einen neuen **CNAME**-Eintragssatz mit den folgenden Werten.

   | Einstellung          | Wert                                     |
   | ---------------- | ----------------------------------------- |
   | Name             | Ihre Unterdomäne, z. B. `www`             |
   | type             | CNAME                                     |
   | Alias-Eintragssatz | No                                        |
   | TTL              | Standardwert übernehmen                    |
   | TTL-Einheit         | Standardwert übernehmen                    |
   | Alias            | Fügen Sie den Domänennamen aus der Zwischenablage ein. |

1. Wählen Sie **OK** aus.

   :::image type="content" source="media/custom-domain/azure-dns-cname.png" alt-text="Azure DNS-Bildschirm „Eintragssatz“ mit hervorgehobenen Feldern für Name, Typ und Alias":::

[!INCLUDE [validate CNAME](../../includes/static-web-apps-validate-cname.md)]

# <a name="other-dns"></a>[Anderes DNS](#tab/other-dns)

1. Stellen Sie sicher, dass **CNAME** in der Dropdownliste _Typ des Hostnamenseintrags_ ausgewählt ist.

1. Kopieren Sie den Wert im Feld _Wert_ in die Zwischenablage, indem Sie das **Kopieren**-Symbol auswählen.

   :::image type="content" source="media/custom-domain/copy-cname.png" alt-text="Bildschirm „Überprüfen + Hinzufügen“ mit ausgewähltem CNAME-Eintrag und umrandetem Kopieren-Symbol":::

1. Melden Sie sich auf einer separaten Browserregisterkarte oder in einem separaten Browserfenster bei der Website Ihres Domänenanbieters an.

1. Suchen Sie die Seite für die Verwaltung von DNS-Einträgen. Da jeder Domänenanbieter eine eigene Benutzeroberfläche für DNS-Einträge hat, finden Sie Informationen dazu jeweils in der Dokumentation des Anbieters. Suchen Sie nach Bereichen der Website, die mit **Domänenname**, **DNS** oder **Namenserververwaltung** gekennzeichnet sind.

1. Die Seite für DNS-Einträge erreichen Sie häufig in Ihren Kontoinformationen über einen Link wie **Eigene Domänen**. Navigieren Sie auf diese Seite, und suchen Sie nach einem Link, der einen ähnlichen Namen wie **Zonendatei**, **DNS-Einträge** oder **Erweiterte Konfiguration** hat.

   Der folgende Screenshot zeigt ein Beispiel für eine Seite mit DNS-Einträgen:

   :::image type="content" source="media/custom-domain/example-record-ui.png" alt-text="Beispiel für die Konfiguration eines DNS-Anbieters":::

1. Erstellen Sie einen neuen **CNAME**-Eintrag mit den folgenden Werten.

   | Einstellung             | Wert                                     |
   | ------------------- | ----------------------------------------- |
   | type                | CNAME                                     |
   | Host                | Ihre Unterdomäne, z. B. `www`             |
   | Wert               | Fügen Sie den Domänennamen aus der Zwischenablage ein. |
   | TTL (falls zutreffend) | Standardwert übernehmen                    |

1. Speichern Sie die Änderungen mit Ihrem DNS-Anbieter.

[!INCLUDE [validate CNAME](../../includes/static-web-apps-validate-cname.md)]

---

## <a name="add-domain-using-txt-record-validation"></a>Hinzufügen einer Domäne mithilfe der TXT-Eintragsvalidierung

Azure verwendet einen TXT-Eintrag, um zu überprüfen, ob Sie eine Domäne besitzen. Dies bietet sich an, wenn Sie eine der folgenden Aktionen ausführen möchten:

1. Sie möchten eine Stammdomäne konfigurieren (d. h. `mydomain.com`). Die Überprüfung, ob Sie die Domäne besitzen, ist erforderlich, bevor Sie einen ALIAS-Eintrag erstellen können, der die Stammdomäne konfiguriert.

1. Sie möchten eine Unterdomäne ohne Ausfallzeiten übertragen. Mit der TXT-Eintragsvalidierungsmethode können Sie überprüfen, ob Sie die Domäne besitzen, und für statische Web-Apps den Prozess der Ausstellung eines Zertifikats für diese Domäne durchlaufen. Anschließend können Sie Ihre Domäne jederzeit mit einem CNAME-Eintrag so ändern, dass sie auf Ihre statische Web-App verweist.

#### <a name="enter-your-domain"></a>Eingeben Ihrer Domäne

1. Öffnen Sie Ihre statische Web-App im [Azure-Portal](https://portal.azure.com).

1. Wählen Sie im Menü **Benutzerdefinierte Domänen** aus.

1. Wählen Sie die Schaltfläche **Hinzufügen** aus.

1. Geben Sie in das Feld _Domänenname_ entweder Ihre Stammdomäne (d. h. `mydomain.com`) oder Ihre Unterdomäne (d. h. `www.mydomain.com`) ein.

   :::image type="content" source="media/custom-domain/add-domain.png" alt-text="Bildschirm „Domäne hinzufügen“ mit der benutzerdefinierten Domäne im Eingabefeld":::

1. Klicken Sie auf die Schaltfläche **Weiter**, um zum Schritt _Überprüfen + Konfigurieren_ zu wechseln.

#### <a name="configure-txt-record-with-your-domain-provider"></a>Konfigurieren eines TXT-Eintrags mit Ihrem Domänenanbieter

Sie müssen einen TXT-Eintrag mit Ihrem Domänenanbieter konfigurieren. Azure DNS wird empfohlen, aber diese Schritte funktionieren mit jedem Domänenanbieter.

# <a name="azure-dns"></a>[Azure DNS](#tab/azure-dns)

1. Stellen Sie sicher, dass die Dropdownliste „Typ des Hostnamenseintrags“ auf „TXT“ festgelegt ist.

1. Wählen Sie die Schaltfläche **Code generieren** aus.

   :::image type="content" source="media/custom-domain/generate-code.png" alt-text="Hinzufügen eines benutzerdefinierten Bildschirms mit hervorgehobener Schaltfläche „Code generieren“":::

   Diese Aktion generiert einen eindeutigen Code, dessen Verarbeitung bis zu einer Minute dauern kann.

1. Wählen Sie das Symbol für die Zwischenablage neben dem Code aus, um den Wert in die Zwischenablage zu kopieren.

   :::image type="content" source="media/custom-domain/copy-code.png" alt-text="Bildschirm „Benutzerdefinierte Domäne hinzufügen“ mit hervorgehobener Schaltfläche „Code kopieren“":::

1. Öffnen Sie ihre Azure DNS-Zone in einer separaten Browserregisterkarte oder einem gesonderten Browserfenster im Azure-Portal.

1. Wählen Sie die Schaltfläche **+ Eintragssatz** aus.

1. Erstellen Sie einen neuen **TXT**-Eintragssatz mit den folgenden Werten.

   | Einstellung  | Wert                                       |
   | -------- | ------------------------------------------- |
   | Name     | `@` für Stammdomäne, oder geben Sie die Unterdomäne ein. |
   | Typ     | TXT                                         |
   | TTL      | Standardwert übernehmen                      |
   | TTL-Einheit | Standardwert übernehmen                      |
   | Wert    | Fügen Sie den Code aus der Zwischenablage ein.          |

1. Wählen Sie **OK** aus.

   :::image type="content" source="media/custom-domain/azure-dns-txt.png" alt-text="Azure DNS-Bildschirm „Eintragssatz“ mit hervorgehobenen Feldern für Name, Typ und Wert":::

[!INCLUDE [validate TXT record](../../includes/static-web-apps-validate-txt.md)]

# <a name="other-dns"></a>[Anderes DNS](#tab/other-dns)

1. Stellen Sie sicher, dass die Dropdownliste „Typ des Hostnamenseintrags“ auf „TXT“ festgelegt ist.

1. Wählen Sie die Schaltfläche **Code generieren** aus.

   :::image type="content" source="media/custom-domain/generate-code.png" alt-text="Hinzufügen eines benutzerdefinierten Bildschirms mit hervorgehobener Schaltfläche „Code generieren“":::

   Diese Aktion generiert einen eindeutigen Code, dessen Verarbeitung bis zu einer Minute dauern kann.

1. Wählen Sie das Symbol für die Zwischenablage neben dem Code aus, um den Wert in die Zwischenablage zu kopieren.

   :::image type="content" source="media/custom-domain/copy-code.png" alt-text="Bildschirm „Benutzerdefinierte Domäne hinzufügen“ mit hervorgehobener Schaltfläche „Code kopieren“":::

1. Melden Sie sich auf einer separaten Browserregisterkarte oder in einem separaten Browserfenster bei der Website Ihres Domänenanbieters an.

1. Suchen Sie die Seite für die Verwaltung von DNS-Einträgen. Da jeder Domänenanbieter eine eigene Benutzeroberfläche für DNS-Einträge hat, finden Sie Informationen dazu jeweils in der Dokumentation des Anbieters. Suchen Sie nach Bereichen der Website, die mit **Domänenname**, **DNS** oder **Namenserververwaltung** gekennzeichnet sind.

   > [!NOTE]
   > Die Seite für DNS-Einträge erreichen Sie häufig in Ihren Kontoinformationen über einen Link wie **Eigene Domänen**. Navigieren Sie auf diese Seite, und suchen Sie nach einem Link, der einen ähnlichen Namen wie **Zonendatei**, **DNS-Einträge** oder **Erweiterte Konfiguration** hat.

1. Erstellen Sie einen neuen **TXT**-Eintragssatz mit den folgenden Werten:

   | Einstellung             | Wert                                       |
   | ------------------- | ------------------------------------------- |
   | Typ                | TXT                                         |
   | Host                | `@` für Stammdomäne, oder geben Sie die Unterdomäne ein. |
   | Wert               | Fügen Sie den Code aus der Zwischenablage ein.          |
   | TTL (falls zutreffend) | Standardwert übernehmen                      |

> [!NOTE]
> Einige DNS-Anbieter ändern das „@“ automatisch in Ihre Stammdomäne (d.h. „mydomain.com“). Dies ist ein erwartetes Verhalten, und der Validierungsprozess funktioniert weiterhin.

[!INCLUDE [create repository from template](../../includes/static-web-apps-validate-txt.md)]

---

## <a name="create-an-alias-record"></a>Erstellen eines ALIAS-Eintrags

Mit einem ALLIAS-Eintrag wird eine Domäne einer anderen zugeordnet. Er wird speziell für Stammdomänen (d. h. `mydomain.com`) verwendet. In diesem Abschnitt erstellen Sie einen ALIAS-Eintrag, der Ihre Stammdomäne der automatisch generierten URL Ihrer statischen Web-App zuordnet.

# <a name="azure-dns"></a>[Azure DNS](#tab/azure-dns)

> [!IMPORTANT]
> Ihre Azure DNS Zone sollte sich im selben Abonnement wie Ihre statische Web-App befinden.

1. Öffnen Sie die Azure DNS-Zone Ihrer Domäne im Azure-Portal.

1. Wählen Sie die Schaltfläche **+ Eintragssatz** aus.

1. Erstellen Sie einen neuen **A**-Eintragssatz mit den folgenden Werten.

   | Einstellung          | Wert                              |
   | ---------------- | ---------------------------------- |
   | Name             | @                                  |
   | Typ             | A: Alias-Eintrag für IPv4-Adresse   |
   | Alias-Eintragssatz | Yes                                |
   | Aliastyp       | Azure-Ressource                     |
   | Subscription     | \<Your Subscription>               |
   | Azure-Ressource   | \<Your Static Web App>             |
   | TTL              | Standardwert übernehmen             |
   | TTL-Einheit         | Standardwert übernehmen             |

1. Wählen Sie **OK** aus.

   :::image type="content" source="media/custom-domain/azure-dns-alias.png" alt-text="Azure DNS-Bildschirm „Eintragssatz“ mit hervorgehobenen Feldern für Name, Typ, Alias und Ressource":::

Nachdem die Stammdomäne nun konfiguriert wurde, kann es mehrere Stunden dauern, bis die Änderungen vom DNS-Anbieter an alle weltweiten Standorte weitergegeben wurden.

# <a name="other-dns"></a>[Anderes DNS](#tab/other-dns)

> [!IMPORTANT]
> Ihr Domänenanbieter muss [ALIAS](../dns/dns-alias.md)- oder ANAME-Einträge oder CNAME-Vereinfachung unterstützen.

1. Öffnen Sie Ihre statische Web-App im [Azure-Portal](https://portal.azure.com).

1. Wählen Sie im Menü **Benutzerdefinierte Domäne** aus.

1. Kopieren Sie die automatisch generierte URL Ihrer statischen Web-App aus dem Bildschirm „Benutzerdefinierte Domäne“.

   :::image type="content" source="media/custom-domain/auto-generated.png" alt-text="Übersichtsseite einer statischen Web-App mit hervorgehobenem Symbol „URL kopieren“":::

1. Melden Sie sich bei der Website Ihres Domänenanbieters an.

1. Suchen Sie die Seite für die Verwaltung von DNS-Einträgen. Da jeder Domänenanbieter eine eigene Benutzeroberfläche für DNS-Einträge hat, finden Sie Informationen dazu jeweils in der Dokumentation des Anbieters. Suchen Sie nach Bereichen der Website, die mit **Domänenname**, **DNS** oder **Namenserververwaltung** gekennzeichnet sind.

   > [!NOTE]
   > Die Seite für DNS-Einträge erreichen Sie häufig in Ihren Kontoinformationen über einen Link wie **Eigene Domänen**. Navigieren Sie auf diese Seite, und suchen Sie nach einem Link, der einen ähnlichen Namen wie **Zonendatei**, **DNS-Einträge** oder **Erweiterte Konfiguration** hat.

1. Erstellen Sie einen neuen **ALIAS**-Eintrag mit den folgenden Werten:

   | Einstellung             | Wert                                                          |
   | ------------------- | -------------------------------------------------------------- |
   | Typ                | ALIAS oder ANAME (CNAME verwenden, falls ALIAS nicht verfügbar ist)                    |
   | Host                | @                                                              |
   | Wert               | Fügen Sie den Domänennamen aus der Zwischenablage ein.                      |
   | TTL (falls zutreffend) | Standardwert übernehmen                                         |

> [!IMPORTANT]
> Wenn Ihr Domänenanbieter keinen ALIAS- oder ANAME-Eintragstyp anbietet, verwenden Sie stattdessen einen CNAME-Typ. Viele Anbieter bieten dieselbe Funktionalität wie der ALIAS-Eintragstyp über den CNAME-Eintragstyp und ein Feature namens „CNAME-Vereinfachung“ (Flattening).

Nachdem die Stammdomäne nun konfiguriert wurde, kann es mehrere Stunden dauern, bis die Änderungen vom DNS-Anbieter an alle weltweiten Standorte weitergegeben wurden.

---

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfigurieren von App-Einstellungen](application-settings.md)
