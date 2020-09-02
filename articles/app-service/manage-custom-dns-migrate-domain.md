---
title: Migrieren eines aktiven DNS-Namens
description: Erfahren Sie, wie Sie einen benutzerdefinierte DNS-Domänennamen, der bereits einer Livewebsite zugewiesen ist, ohne Ausfallzeiten zu Azure App Service migrieren.
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.topic: article
ms.date: 08/25/2020
ms.custom: seodec18
ms.openlocfilehash: c51745b7760573aa3c6ae067e9a6c1cc315f8e56
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88871393"
---
# <a name="migrate-an-active-dns-name-to-azure-app-service"></a>Migrieren eines aktiven DNS-Namens zu Azure App Service

In diesem Artikel erfahren Sie, wie Sie einen aktiven DNS-Namen ohne Ausfallzeiten zu [Azure App Service](../app-service/overview.md) migrieren.

Wenn Sie eine Livewebsite und den dazugehörigen DNS-Domänennamen zu App Service migrieren, wird über diesen DNS-Namen bereits Livedatenverkehr abgewickelt. Damit während der Migration keine Ausfallzeiten bei der DNS-Auflösung auftreten, können Sie den aktiven DNS-Namen vorab an Ihre App Service-App binden.

Wenn ein Ausfall der DNS-Auflösung kein Problem darstellt, finden Sie weitere Informationen unter [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure App Service](app-service-web-tutorial-custom-domain.md).

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie den folgenden Vorbereitungsschritt aus:

- [Vergewissern Sie sich, dass sich Ihre App Service-App nicht im FREE-Tarif befindet.](app-service-web-tutorial-custom-domain.md#checkpricing)

## <a name="bind-the-domain-name-preemptively"></a>Binden des Domänennamens bereits im Vorfeld

Wenn Sie eine benutzerdefinierte Domäne bereits im Vorfeld binden, haben Sie die folgenden beiden Vorgänge ausgeführt, bevor Sie Änderungen an Ihren bestehenden DNS-Einträgen vornehmen:

- Überprüfen des Domänenbesitzes
- Aktivieren des Domänennamens für Ihre App

Wenn Sie dann Ihren benutzerdefinierten DNS-Namen von der alten Website zur App Service-App migrieren, treten bei der DNS-Auflösung keinerlei Ausfälle auf.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="get-domain-verification-id"></a>Abrufen der Verifizierungs-ID für eine Domäne

Um die Domänenverifizierungs-ID für Ihre App abzurufen, befolgen Sie die Schritte unter [Abrufen der Verifizierungs-ID für eine Domäne](app-service-web-tutorial-custom-domain.md#get-domain-verification-id).

### <a name="create-domain-verification-record"></a>Erstellen eines Domänenüberprüfungseintrags

Fügen Sie zum Überprüfen des Domänenbesitzes einen TXT-Eintrag für die Domänenüberprüfung hinzu. Der Hostname für den TXT-Eintrag hängt vom Typ des DNS-Eintrags ab, den Sie zuordnen möchten. Sehen Sie sich dazu die folgende Tabelle an (`@` stellt in der Regel die Stammdomäne dar.):

| DNS-Beispieleintrag | TXT-Host | TXT-Wert |
| - | - | - |
| \@ (Stamm) | _asuid_ | [Domänenverifizierungs-ID für Ihre App](app-service-web-tutorial-custom-domain.md#get-domain-verification-id) |
| www (Unterdomäne) | _asuid.www_ | [Domänenverifizierungs-ID für Ihre App](app-service-web-tutorial-custom-domain.md#get-domain-verification-id) |
| \* (Platzhalter) | _asuid_ | [Domänenverifizierungs-ID für Ihre App](app-service-web-tutorial-custom-domain.md#get-domain-verification-id) |

Beachten Sie auf der Seite mit den DNS-Einträgen den Eintragstyp des DNS-Namens, den Sie migrieren möchten. App Service unterstützt Zuordnungen von CNAME- und A-Einträgen.

> [!NOTE]
> Platzhalterdatensätze (`*`) überprüfen keine Unterdomänen mit vorhandenem CNAME-Eintrag. Möglicherweise müssen Sie für jede Unterdomäne explizit einen TXT-Eintrag erstellen.

### <a name="enable-the-domain-for-your-app"></a>Aktivieren der Domäne für Ihre App

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich der App-Seite die Option **Benutzerdefinierte Domänen** aus. 

    ![Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

1. Wählen Sie auf der Seite **Benutzerdefinierte Domänen** die Option **Benutzerdefinierte Domäne hinzufügen** aus.

    ![Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

1. Geben Sie den vollqualifizierten Domänennamen der Domäne ein, die Sie migrieren möchten. Dieser entspricht dem erstellten TXT-Eintrag, z. B. `contoso.com`, `www.contoso.com` oder `*.contoso.com`. Wählen Sie **Überprüfen** aus.

    Die Schaltfläche **Benutzerdefinierte Domäne hinzufügen** ist aktiviert. 

1. Vergewissern Sie sich, dass **Typ des Hostnamenseintrags** auf den DNS-Eintragstyp festgelegt ist, den Sie migrieren möchten. Wählen Sie **Hostnamen hinzufügen**.

    ![Hinzufügen des DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

    Unter Umständen dauert es eine Weile, bis der neue Hostname auf der Seite **Benutzerdefinierte Domänen** der App angezeigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

    ![Hinzugefügter CNAME-Eintrag](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

    Ihr benutzerdefinierter DNS-Name ist nun in Ihrer Azure-App aktiviert. 

## <a name="remap-the-active-dns-name"></a>Neuzuordnen des aktiven DNS-Namens

Nun muss nur noch der aktive DNS-Eintrag neu zugeordnet werden, damit er auf App Service verweist. Momentan verweist er immer noch auf Ihre alte Website.

<a name="info"></a>

### <a name="copy-the-apps-ip-address-a-record-only"></a>Kopieren der IP-Adresse der App (nur A-Eintrag)

Wenn Sie einen CNAME-Eintrag neu zuordnen, überspringen Sie diesen Abschnitt. 

Für die Neuzuordnung eines A-Eintrags benötigen Sie die externe IP-Adresse der App Service-App (zu finden auf der Seite **Benutzerdefinierte Domänen**).

Kopieren Sie auf der Seite **Benutzerdefinierte Domänen** die IP-Adresse der App.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-the-dns-record"></a>Aktualisieren des DNS-Eintrags

Wählen Sie auf der DNS-Eintragsseite Ihres Domänenanbieters den neu zuzuordnenden DNS-Eintrag aus.

Ordnen Sie für das Stammdomänenbeispiel `contoso.com` den A- oder CNAME-Eintrag wie bei den Beispielen in der folgenden Tabelle zu: 

| FQDN-Beispiel | Eintragstyp | Host | Wert |
| - | - | - | - |
| contoso.com (Stammdomäne) | Ein | `@` | IP-Adresse aus dem Schritt [Kopieren der IP-Adresse der App](#info) |
| www\.contoso.com (Unterdomäne) | CNAME | `www` | _&lt;App-Name&gt;.azurewebsites.net_ |
| \*.contoso.com (Platzhalter) | CNAME | _\*_ | _&lt;App-Name&gt;.azurewebsites.net_ |

Speichern Sie die Einstellungen.

DNS-Abfragen werden umgehend nach der DNS-Verteilung zu Ihrer App Service-App aufgelöst.

## <a name="migrate-domain-from-another-app"></a>Migrieren einer Domäne aus einer anderen App

Sie können eine aktive benutzerdefinierte Domäne in Azure zwischen Abonnements oder innerhalb desselben Abonnements migrieren. Eine solche Migration ohne Ausfallzeiten erfordert jedoch, dass die Quell-App und die Ziel-App zu einem bestimmten Zeitpunkt derselben benutzerdefinierten Domäne zugewiesen sind. Daher müssen Sie sicherstellen, dass die beiden Apps nicht in derselben Bereitstellungseinheit (intern als Webspace bezeichnet) bereitgestellt sind. Ein Domänenname kann nur einer App in jeder Bereitstellungseinheit zugewiesen werden.

Sie finden die Bereitstellungseinheit für Ihre App, indem Sie sich den Domänennamen der FTP/S-URL `<deployment-unit>.ftp.azurewebsites.windows.net` ansehen. Überprüfen Sie, und stellen Sie sicher, dass sich die Bereitstellungseinheit der Quell-App von der der Ziel-App unterscheidet. Die Bereitstellungseinheit einer App wird von dem [App Service-Plan bestimmt](overview-hosting-plans.md), in dem sie sich befindet. Er wird nach dem Zufallsprinzip von Azure ausgewählt, wenn Sie den Plan erstellen, und kann nicht geändert werden. Azure stellt nur sicher, dass sich zwei Pläne in derselben Bereitstellungseinheit befinden, wenn Sie [sie in derselben Ressourcengruppe *und* derselben Region erstellen](app-service-plan-manage.md#create-an-app-service-plan), aber es verfügt über keine Logik, um sicherzustellen, dass sich die Pläne in unterschiedlichen Bereitstellungseinheiten befinden. Die einzige Möglichkeit, einen Plan in einer anderen Bereitstellungseinheit zu erstellen, besteht darin, einen Plan in einer neuen Ressourcengruppe oder Region zu erstellen, bis Sie eine andere Bereitstellungseinheit erhalten.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Sie ein benutzerdefiniertes TLS/SSL-Zertifikat an App Service binden.

> [!div class="nextstepaction"]
> [Schützen eines benutzerdefinierten DNS-Namens mit einer TLS-Bindung in Azure App Service](configure-ssl-bindings.md)
