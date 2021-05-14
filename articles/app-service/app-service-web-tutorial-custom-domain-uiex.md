---
title: 'Tutorial: Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens'
description: Hier erfahren Sie, wie Sie einen vorhandenen benutzerdefinierten DNS-Domänennamen einer Web-App, einem Back-End einer mobilen App oder einer API-App in Azure App Service hinzufügen.
keywords: App Service, Azure App Service, Domänenzuordnung, Domänenname, vorhandene Domäne, Hostname, Vanity-Domäne
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 08/25/2020
ms.custom: mvc, seodec18, devx-track-azurepowershell
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 1fcf8c681f4fad65209c27663045d4974be633f7
ms.sourcegitcommit: 3c460886f53a84ae104d8a09d94acb3444a23cdc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "107833243"
---
# <a name="tutorial-map-an-existing-custom-dns-name-to-azure-app-service"></a>Tutorial: Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure App Service

In diesem Tutorial erfahren Sie, wie Sie einen beliebigen <abbr title="Ein Domänenname, den Sie von einer Domänenregistrierungsstelle wie GoDaddy erworben haben, oder eine Unterdomäne Ihrer erworbenen Domäne.">benutzerdefinierten DNS-Domänennamen</abbr> zu <abbr title="Ein HTTP-basierter Dienst zum Hosten von Webanwendungen, REST-APIs und mobilen Back-End-Anwendungen">Azure App Service</abbr>.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Zuordnen einer Unterdomäne per <abbr title="Durch einen kanonischen DNS-Namenseintrag wird ein Domänenname einem anderen zugeordnet.">CNAME-Eintrag</abbr>.
> * Zuordnen einer Stammdomäne per <abbr title="Durch einen Adresseintrag im DNS wird ein Hostname einer IP-Adresse zugeordnet.">A-Eintrag</abbr>.
> * Zuordnen einer Platzhalterdomäne per CNAME-Eintrag
> * Umleiten der Standard-URL an ein benutzerdefiniertes Verzeichnis

<hr/> 

## <a name="1-prepare-your-environment"></a>1. Vorbereiten der Umgebung

* [Erstellen Sie eine App Service-App](./index.yml), oder verwenden Sie eine App, die Sie für ein anderes Tutorial erstellt haben.
* Vergewissern Sie sich, dass Sie DNS-Einträge für Ihre benutzerdefinierte Domäne bearbeiten können. Sollten Sie noch nicht über eine benutzerdefinierte Domäne verfügen, können Sie [eine App Service-Domäne erwerben](manage-custom-dns-buy-domain.md).

    <details>
        <summary>Was benötige ich, um DNS-Einträge bearbeiten zu können?</summary>
        Sie benötigen Zugriff auf die DNS-Registrierung für Ihren Domänenanbieter (beispielsweise GoDaddy). Um beispielsweise DNS-Einträge für <code>contoso.com</code> und <code>www.contoso.com</code> hinzuzufügen, müssen Sie die DNS-Einstellungen für die Stammdomäne <code>contoso.com</code> konfigurieren können.
    </details>

<hr/> 

## <a name="2-prepare-the-app"></a>2. Vorbereiten der App

Wenn Sie einer App einen benutzerdefinierten DNS-Namen zuordnen möchten, muss es sich beim <abbr title="Gibt den Standort, die Größe und die Features der Webserverfarm an, von der Ihre App gehostet wird.">App Service-Plan</abbr> der App um einen kostenpflichtigen Tarif handeln (also nicht um <abbr title="Ein Azure App Service-Tarif, in dem Ihre App auf den gleichen virtuellen Computern ausgeführt wird wie andere Apps (einschließlich Apps anderer Kunden). Dieser Tarif eignet sich für die Entwicklung und zu Testzwecken.">**Free (F1)**</abbr>). Weitere Informationen hierzu finden Sie in der [Übersicht über Azure App Service-Pläne](overview-hosting-plans.md).

#### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Öffnen Sie das [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

#### <a name="select-the-app-in-the-azure-portal"></a>Auswählen der App im Azure-Portal

1. Suchen Sie nach **App Services**, und wählen Sie diese Option aus.

   ![Screenshot: Auswählen von App Services](./media/app-service-web-tutorial-custom-domain/app-services.png)

1. Wählen Sie auf der Seite **App Services** den Namen Ihrer Azure-App aus.

   ![Screenshot: Navigation im Portal zu einer Azure-App](./media/app-service-web-tutorial-custom-domain/select-app.png)

    Die Verwaltungsseite der App Service-App wird angezeigt.

<a name="checkpricing" aria-hidden="true"></a>

#### <a name="check-the-pricing-tier"></a>Überprüfen des Tarifs

1. Scrollen Sie im linken Bereich der App-Seite zum Abschnitt **Einstellungen**, und wählen Sie **Hochskalieren (App Service-Plan)** aus.

   ![Screenshot: Menü „Hochskalieren (App Service-Plan)“](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

1. Der aktuelle Tarif der App wird durch einen blauen Rahmen hervorgehoben. Vergewissern Sie sich, dass sich die App nicht im Tarif **F1** befindet. Benutzerdefiniertes DNS wird im Tarif **F1** nicht unterstützt.

   ![Screenshot: Empfohlene Tarife](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

1. Wenn sich der App Service-Plan nicht im Tarif **F1** befindet, schließen Sie die Seite **Hochskalieren**, und fahren Sie mit [3. Abrufen der Verifizierungs-ID für eine Domäne](#3-get-a-domain-verification-id) fort.

<a name="scaleup" aria-hidden="true"></a>

#### <a name="scale-up-the-app-service-plan"></a>Hochskalieren des App Service-Plans

1. Wählen Sie einen der kostenpflichtigen Tarife aus (**D1**, **B1**, **B2**, **B3** oder einen beliebigen Tarif aus der Kategorie **Produktion**). Wählen Sie **Alle Optionen anzeigen** aus, um weitere Optionen anzuzeigen.

1. Wählen Sie **Übernehmen**.

   ![Screenshot: Überprüfen des Tarifs](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

   Wenn die unten angegebene Benachrichtigung angezeigt wird, ist der Skalierungsvorgang abgeschlossen.

   ![Screenshot: Bestätigung des Skalierungsvorgangs](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<hr/> 

<a name="cname" aria-hidden="true"></a>

## <a name="3-get-a-domain-verification-id"></a>3. Abrufen der Verifizierungs-ID für eine Domäne

Wenn Sie Ihrer App eine benutzerdefinierte Domäne hinzufügen möchten, müssen Sie den Besitz der Domäne bestätigen, indem Sie bei Ihrem Domänenanbieter eine Verifizierungs-ID als TXT-Datensatz hinzufügen. 

1. Wählen Sie im linken Bereich Ihrer App-Seite die Option **Benutzerdefinierte Domänen** aus. 
1. Kopieren Sie auf der Seite **Benutzerdefinierte Domänen** die ID im Feld **Verifizierungs-ID für benutzerdefinierte Domänen** für den nächsten Schritt.

    ![Screenshot: ID im Feld „Verifizierungs-ID für benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/get-custom-domain-verification-id.png)

    <details>
        <summary>Warum ist das erforderlich?</summary>
        Durch das Hinzufügen von Domänenverifizierungs-IDs zu Ihrer benutzerdefinierten Domäne können verwaiste DNS-Einträge und Unterdomänenübernahmen verhindert werden. Benutzerdefinierte Domänen, die Sie zuvor ohne diese Überprüfungs-ID konfiguriert haben, sollten Sie vor dem gleichen Risiko schützen, indem Sie Ihrem DNS-Datensatz die Überprüfungs-ID hinzufügen. Weitere Informationen zu dieser allgemeinen Bedrohung mit hohem Schweregrad finden Sie unter <a href="/azure/security/fundamentals/subdomain-takeover">Verhindern verwaister DNS-Einträge und Vermeiden von Unterdomänenübernahmen</a>.
    </details>
    
<a name="info"></a>

3. **(Nur A-Eintrag)** Um einen <abbr title="Durch einen Adresseintrag im DNS wird ein Hostname einer IP-Adresse zugeordnet.">A-Eintrag</abbr>zuordnen zu können, benötigen Sie die externe IP-Adresse der App. Kopieren Sie auf der Seite **Benutzerdefinierte Domänen** den Wert der **IP-Adresse**.

   ![Screenshot: Navigation im Portal zu einer Azure-App](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

<hr/> 

## <a name="4-create-the-dns-records"></a>4. Erstellen der DNS-Einträge

1. Melden Sie sich bei der Website Ihres Domänenanbieters an.

    <details>
        <summary>Kann ich das DNS meines Domänenanbieters mithilfe von Azure verwalten?</summary>
        Sie können Azure DNS verwenden, um DNS-Einträge für Ihre Domäne zu verwalten und einen benutzerdefinierten DNS-Namen für Azure App Service zu konfigurieren. Weitere Informationen finden Sie unter <a href="/azure/dns/dns-delegate-domain-azure-dns">Tutorial: Hosten Ihrer Domäne in Azure DNS</a>.
    </details>

1. Suchen Sie die Seite für die Verwaltung von DNS-Einträgen. 

    <details>
        <summary>Wie finde ich die Seite?</summary>
        <p>Da jeder Domänenanbieter eine eigene Benutzeroberfläche für DNS-Einträge hat, finden Sie Informationen dazu jeweils in der Dokumentation des Anbieters. Suchen Sie nach Bereichen der Website, die mit <strong>Domänenname</strong>, <strong>DNS</strong> oder <strong>Namenserververwaltung</strong> gekennzeichnet sind.</p>
        <p>Die Seite für DNS-Einträge erreichen Sie häufig in Ihren Kontoinformationen über einen Link wie <strong>Eigene Domänen</strong>. Navigieren Sie zu dieser Seite, und suchen Sie nach einem Link, der beispielsweise den Namen <strong>Zonendatei</strong>, <strong>DNS-Einträge</strong> oder <strong>Erweiterte Konfiguration</strong> hat.</p>
    </details>

   Der folgende Screenshot zeigt ein Beispiel für eine Seite mit DNS-Einträgen:

   ![Screenshot: Beispiel für eine Seite mit DNS-Einträgen](../../includes/media/app-service-web-access-dns-records-no-h/example-record-ui.png)

1. Wählen Sie **Hinzufügen** oder das entsprechende Widget aus, um einen Eintrag zu erstellen. 

1. Wählen Sie die Art des zu erstellenden Eintrags aus, und befolgen Sie die Anweisungen. Sie können entweder einen <abbr title="Durch einen kanonischen Namenseintrag im DNS wird ein Domänenname (ein Alias) einem anderen Domänennamen (dem kanonischen Namen) zugeordnet.">CNAME-Eintrag</abbr> oder einen <abbr title="Durch einen Adresseintrag im DNS wird ein Hostname einer IP-Adresse zugeordnet.">A-Eintrag</abbr> verwenden, um App Service einen benutzerdefinierten DNS-Namen zuzuordnen. 

    <details>
        <summary>Welchen Eintrag soll ich verwenden?</summary>
        <div>
            <ul>
            <li>Wenn Sie die Stammdomäne (z. B. <code>contoso.com</code>) zuordnen möchten, verwenden Sie einen A-Eintrag. Verwenden Sie für den Stammeintrag nicht den CNAME-Eintrag. (Weitere Informationen finden Sie in <a href="https://en.wikipedia.org/wiki/CNAME_record">diesem Wikipedia-Eintrag</a>.)</li>
            <li>Wenn Sie eine Unterdomäne (z. B. <code>www.contoso.com</code>) zuordnen möchten, verwenden Sie einen CNAME-Eintrag.</li>
            <li>Mit einem A-Eintrag können Sie der IP-Adresse der App zwar direkt eine Unterdomäne zuordnen, es ist jedoch möglich, dass sich <a href="/azure/app-service/overview-inbound-outbound-ips#when-inbound-ip-changes">die IP-Adresse ändert</a>. Der CNAME-Eintrag wird dagegen dem (weniger änderungsanfälligen) Hostnamen der App zugeordnet.</li>
            <li>Wenn Sie eine <a href="https://en.wikipedia.org/wiki/Wildcard_DNS_record">Platzhalterdomäne</a> (z. B. <code>*.contoso.com</code>) zuordnen möchten, verwenden Sie einen CNAME-Eintrag.</li>
            </ul>
        </div>
    </details>
    
# <a name="cname"></a>[CNAME](#tab/cname)

Erstellen Sie für eine Unterdomäne wie `www` in `www.contoso.com` zwei Einträge gemäß der folgenden Tabelle:

| Eintragstyp | Host | Wert | Kommentare |
| - | - | - |
| CNAME | `<subdomain>` (z. B. `www`) | `<app-name>.azurewebsites.net` | Die Domänenzuordnung selbst |
| TXT | `asuid.<subdomain>` (z. B. `asuid.www`) | [Die zuvor abgerufene Verifizierungs-ID](#3-get-a-domain-verification-id) | App Service greift auf den TXT-Eintrag `asuid.<subdomain>` zu, um den Besitz der benutzerdefinierten Domäne zu überprüfen. |

![Screenshot: Navigation im Portal zu einer Azure-App](./media/app-service-web-tutorial-custom-domain/cname-record.png)
    
# <a name="a"></a>[A](#tab/a)

Erstellen Sie für eine Stammdomäne wie `contoso.com` zwei Einträge gemäß der folgenden Tabelle:

| Eintragstyp | Host | Wert | Kommentare |
| - | - | - |
| Ein | `@` | IP-Adresse aus dem Schritt [Kopieren der IP-Adresse der App](#3-get-a-domain-verification-id) | Die Domänenzuordnung selbst (`@` stellt in der Regel die Stammdomäne dar.) |
| TXT | `asuid` | [Die zuvor abgerufene Verifizierungs-ID](#3-get-a-domain-verification-id) | App Service greift auf den TXT-Eintrag `asuid.<subdomain>` zu, um den Besitz der benutzerdefinierten Domäne zu überprüfen. Verwenden Sie für die Stammdomäne `asuid`. |

![Screenshot: Seite mit DNS-Einträgen](./media/app-service-web-tutorial-custom-domain/a-record.png)

<details>
<summary>Was geschieht, wenn ich eine Unterdomäne einem A-Eintrag zuordnen möchte?</summary>
Wenn Sie zum Hinzufügen einer Unterdomäne (etwa `www.contoso.com`) einen A-Eintrag anstelle eines empfohlenen CNAME-Eintrags verwenden möchten, sollten Ihr A-Eintrag und TXT-Eintrag stattdessen wie in der folgenden Tabelle aussehen:

<div class="table-scroll-wrapper"><table class="table"><caption class="visually-hidden">Tabelle 3</caption>
<thead>
<tr>
<th>Eintragstyp</th>
<th>Host</th>
<th>Wert</th>
</tr>
</thead>
<tbody>
<tr>
<td>Ein</td>
<td><code>&lt;subdomain&gt;</code> (z. B. <code>www</code>)</td>
<td>IP-Adresse aus dem Schritt <a href="#info" data-linktype="self-bookmark">Kopieren der IP-Adresse der App</a></td>
</tr>
<tr>
<td>TXT</td>
<td><code>asuid.&lt;subdomain&gt;</code> (z. B. <code>asuid.www</code>)</td>
<td><a href="#3-get-a-domain-verification-id" data-linktype="self-bookmark">Die zuvor abgerufene Verifizierungs-ID</a></td>
</tr>
</tbody>
</table></div>
</details>

# <a name="wildcard-cname"></a>[Platzhalter (CNAME)](#tab/wildcard)

Erstellen Sie für eine Platzhalterdomäne wie `*` in `*.contoso.com` zwei Einträge gemäß der folgenden Tabelle:

| Eintragstyp | Host | Wert | Kommentare |
| - | - | - |
| CNAME | `*` | `<app-name>.azurewebsites.net` | Die Domänenzuordnung selbst |
| TXT | `asuid` | [Die zuvor abgerufene Verifizierungs-ID](#3-get-a-domain-verification-id) | App Service greift auf den TXT-Eintrag `asuid` zu, um den Besitz der benutzerdefinierten Domäne zu überprüfen. |

![Screenshot: Navigation zu einer Azure-App](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)
    
-----

<details>
<summary>Meine Änderungen werden gelöscht, nachdem ich die Seite verlassen habe.</summary>
<p>Bei einigen Anbietern wie GoDaddy werden Änderungen an DNS-Einträgen erst wirksam, wenn Sie einen gesonderten Link <strong>Änderungen speichern</strong> wählen.</p>
</details>

<hr/>

## <a name="5-enable-the-mapping-in-your-app"></a>5. Aktivieren der Zuordnung in Ihrer App

1. Wählen Sie im linken Bereich der App-Seite im Azure-Portal die Option **Benutzerdefinierte Domänen** aus.

    ![Screenshot: Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

1. Wählen Sie **Benutzerdefinierte Domäne hinzufügen**.

    ![Screenshot: Element zum Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

# <a name="cname"></a>[CNAME](#tab/cname)

3. Geben Sie den vollqualifizierten Domänennamen ein, für den Sie einen CNAME-Eintrag hinzugefügt haben, z.B. `www.contoso.com`.

1. Wählen Sie **Überprüfen** aus. Die Seite **Benutzerdefinierte Domäne hinzufügen** wird angezeigt.

1. Stellen Sie sicher, dass der **Typ des Hostnamenseintrags** auf **CNAME (www\.beispiel.com oder eine beliebige Unterdomäne)** festgelegt ist. Wählen Sie **Benutzerdefinierte Domäne hinzufügen**.

    ![Screenshot: Schaltfläche „Benutzerdefinierte Domäne hinzufügen“](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

    Unter Umständen dauert es eine Weile, bis die neue benutzerdefinierte Domäne auf der Seite **Benutzerdefinierte Domänen** der App angezeigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

    ![Screenshot: Hinzufügen des CNAME-Eintrags](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

    <details>
        <summary>Was hat es mit der Warnung <strong>Nicht sicher</strong> auf sich?</summary>
        Eine Warnbezeichnung für Ihre benutzerdefinierte Domäne bedeutet, dass diese noch nicht an ein TLS-/SSL-Zertifikat gebunden ist. Für alle HTTPS-Anforderungen von einem Browser an Ihre benutzerdefinierte Domäne wird abhängig vom Browser ein Fehler oder eine Warnung angezeigt. Weitere Informationen zum Hinzufügen einer TLS-/SSL-Bindung finden Sie unter <a href="/azure/app-service/configure-ssl-bindings">Schützen eines benutzerdefinierten DNS-Namens mit einer TLS-/SSL-Bindung in Azure App Service</a>.
    </details>

    Wenn Sie einen Schritt ausgelassen haben oder Ihnen zu einem früheren Zeitpunkt ein Tippfehler unterlaufen ist, wird am unteren Rand der Seite ein Überprüfungsfehler angezeigt.

    ![Screenshot: Überprüfungsfehler](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a" aria-hidden="true"></a>

<a name="enable-a" aria-hidden="true"></a>

# <a name="a"></a>[A](#tab/a)

3. Geben Sie den vollqualifizierten Domänennamen ein, für den Sie den A-Eintrag konfiguriert haben, z.B. `contoso.com`. 

1. Wählen Sie **Überprüfen** aus. Die Seite **Benutzerdefinierte Domäne hinzufügen** wird angezeigt.

1. Stellen Sie sicher, dass die Option **Typ des Hostnamenseintrags** auf **A-Datensatz (beispiel.com)** festgelegt ist. Wählen Sie **Benutzerdefinierte Domäne hinzufügen**.

    ![Screenshot: Hinzufügen eines DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

    Unter Umständen dauert es eine Weile, bis die neue benutzerdefinierte Domäne auf der Seite **Benutzerdefinierte Domänen** der App angezeigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

    ![Screenshot: Hinzufügen eines A-Eintrags](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

    <details>
        <summary>Was hat es mit der Warnung <strong>Nicht sicher</strong> auf sich?</summary>
        Eine Warnbezeichnung für Ihre benutzerdefinierte Domäne bedeutet, dass diese noch nicht an ein TLS-/SSL-Zertifikat gebunden ist. Für alle HTTPS-Anforderungen von einem Browser an Ihre benutzerdefinierte Domäne wird abhängig vom Browser ein Fehler oder eine Warnung angezeigt. Weitere Informationen zum Hinzufügen einer TLS-/SSL-Bindung finden Sie unter <a href="/azure/app-service/configure-ssl-bindings">Schützen eines benutzerdefinierten DNS-Namens mit einer TLS-/SSL-Bindung in Azure App Service</a>.
    </details>
    
    Wenn Sie einen Schritt ausgelassen haben oder Ihnen zu einem früheren Zeitpunkt ein Tippfehler unterlaufen ist, wird am unteren Rand der Seite ein Überprüfungsfehler angezeigt.
    
    ![Screenshot: Überprüfungsfehler](./media/app-service-web-tutorial-custom-domain/verification-error.png)
    
<a name="wildcard" aria-hidden="true"></a>

# <a name="wildcard-cname"></a>[Platzhalter (CNAME)](#tab/wildcard)

3. Geben Sie einen vollqualifizierten Domänennamen ein, der zur Platzhalterdomäne passt. Für `*.contoso.com` können Sie beispielsweise `sub1.contoso.com`, `sub2.contoso.com`, `*.contoso.com` oder eine beliebige andere Zeichenfolge verwenden, die dem Platzhaltermuster entspricht. Wählen Sie anschließend **Überprüfen** aus.

    Die Schaltfläche **Benutzerdefinierte Domäne hinzufügen** ist aktiviert.

1. Stellen Sie sicher, dass der **Typ des Hostnamenseintrags** auf **CNAME-Eintrag (www\.beispiel.com oder eine beliebige Unterdomäne)** festgelegt ist. Wählen Sie **Benutzerdefinierte Domäne hinzufügen**.

    ![Screenshot: Hinzufügen eines DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

    Unter Umständen dauert es eine Weile, bis die neue benutzerdefinierte Domäne auf der Seite **Benutzerdefinierte Domänen** der App angezeigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

    <details>
        <summary>Was hat es mit der Warnung <strong>Nicht sicher</strong> auf sich?</summary>
        Eine Warnbezeichnung für Ihre benutzerdefinierte Domäne bedeutet, dass diese noch nicht an ein TLS-/SSL-Zertifikat gebunden ist. Für alle HTTPS-Anforderungen von einem Browser an Ihre benutzerdefinierte Domäne wird abhängig vom Browser ein Fehler oder eine Warnung angezeigt. Weitere Informationen zum Hinzufügen einer TLS-/SSL-Bindung finden Sie unter <a href="/azure/app-service/configure-ssl-bindings">Schützen eines benutzerdefinierten DNS-Namens mit einer TLS-/SSL-Bindung in Azure App Service</a>.
    </details>

-----

<hr/> 

## <a name="6-test-in-a-browser"></a>6. Testen in einem Browser

Navigieren Sie zu den DNS-Namen, die Sie zuvor konfiguriert haben.

![Screenshot: Navigation zu einer Azure-App](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

<a name="resolve-404-not-found" aria-hidden="true"></a>
<details>
<summary>Bei mir tritt ein Fehler vom Typ „HTTP 404 (Nicht gefunden)“ auf.</summary>
<ul>
<li>Der konfigurierten benutzerdefinierten Domäne fehlt ein A-Eintrag oder ein CNAME-Eintrag.</li>
<li>Im Browserclient ist die alte IP-Adresse Ihrer Domäne zwischengespeichert. Leeren Sie den Cache, und testen Sie die DNS-Auflösung erneut. Auf einem Windows-Computer können Sie den Cache mithilfe von <code>ipconfig /flushdns</code> leeren.</li>
</ul>
</details>

<hr/> 

## <a name="migrate-an-active-domain"></a>Migrieren einer aktiven Domäne

Informationen zum Migrieren einer Livewebsite und ihres DNS-Domänennamens zu App Service ohne Ausfallzeiten finden Sie unter [Migrieren einer aktiven benutzerdefinierten Domäne zu Azure App Service](manage-custom-dns-migrate-domain.md).

<hr/> 

<a name="virtualdir" aria-hidden="true"></a>

## <a name="redirect-to-a-custom-directory"></a>Umleitung zu einem benutzerdefinierten Verzeichnis

<details>
<summary>Benötige ich das?</summary>
<p>Das hängt von Ihrer App ab. App Service leitet Webanforderungen standardmäßig an das Stammverzeichnis des App-Codes weiter. Manche Webframeworks starten jedoch nicht im Stammverzeichnis. <a href="https://laravel.com/">Laravel</a> startet beispielsweise im Unterverzeichnis <code>public</code>. Um das DNS-Beispiel für <code>contoso.com</code> fortzusetzen: Auf eine solche App kann zwar unter <code>http://contoso.com/public</code> zugegriffen werden, es empfiehlt sich aber, <code>http://contoso.com</code> stattdessen an das Verzeichnis <code>public</code> weiterzuleiten. </p>
</details>

Hierbei handelt es sich zwar um ein gängiges Szenario, aber eigentlich nicht um eine benutzerdefinierte Domänenzuordnung, da hier das virtuelle Verzeichnis innerhalb Ihrer App angepasst wird.

1. Wählen Sie im linken Bereich Ihrer Web-App-Seite die Option **Anwendungseinstellungen** aus.

1. Das virtuelle Stammverzeichnis `/` am unteren Seitenrand verweist standardmäßig auf `site\wwwroot` (also auf das Stammverzeichnis Ihres App-Codes). Ändern Sie die Angabe, um stattdessen beispielsweise auf `site\wwwroot\public` zu verweisen, und speichern Sie die Änderung.

    ![Screenshot: Anpassen eines virtuellen Verzeichnisses](./media/app-service-web-tutorial-custom-domain/customize-virtual-directory.png)

1. Überprüfen Sie nach Abschluss des Vorgangs das Ergebnis, indem Sie im Browser zum Stammpfad Ihrer App navigieren (beispielsweise zu `http://contoso.com` oder `http://<app-name>.azurewebsites.net`).

<hr/> 

## <a name="automate-with-scripts"></a>Automatisieren mit Skripts

Durch die [Azure CLI](/cli/azure/install-azure-cli) oder durch [Azure PowerShell](/powershell/azure/) können Sie mithilfe von Skripts die Verwaltung von benutzerdefinierten Domänen automatisieren.

#### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Mit dem folgenden Befehl wird ein konfigurierter benutzerdefinierter DNS-Name zu einer App Service-App hinzugefügt.

```bash 
az webapp config hostname add \
    --webapp-name <app-name> \
    --resource-group <resource_group_name> \
    --hostname <fully_qualified_domain_name>
``` 

Weitere Informationen finden Sie unter [Zuordnen einer benutzerdefinierten Domäne zu einer Web-App](scripts/cli-configure-custom-domain.md).

#### <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Mit dem folgenden Befehl wird ein konfigurierter benutzerdefinierter DNS-Name zu einer App Service-App hinzugefügt.

```powershell  
Set-AzWebApp `
    -Name <app-name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app-name>.azurewebsites.net")
```

Weitere Informationen finden Sie unter [Zuordnen einer benutzerdefinierten Domäne zu einer Web-App](scripts/powershell-configure-custom-domain.md).

<hr/> 

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Tutorial fort, um sich über das Binden eines benutzerdefinierten TLS-/SSL-Zertifikats an eine Web-App zu informieren:

> [!div class="nextstepaction"]
> [Schützen eines benutzerdefinierten DNS-Namens mit einer TLS-/SSL-Bindung in Azure App Service](configure-ssl-bindings.md)
