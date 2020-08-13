---
title: Verhindern von Unterdomänenübernahmen mit Azure DNS-Aliaseinträgen und der Überprüfung benutzerdefinierter Domänen von Azure App Service
description: Erfahren Sie, wie Sie die häufige Bedrohung mit hohem Schweregrad durch Unterdomänenübernahmen verhindern
services: security
author: memildin
manager: rkarlin
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/23/2020
ms.author: memildin
ms.openlocfilehash: 3d63ccc2c47bca9410b5b9105b90aa1f0cf5854a
ms.sourcegitcommit: 14bf4129a73de2b51a575c3a0a7a3b9c86387b2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87439277"
---
# <a name="prevent-dangling-dns-entries-and-avoid-subdomain-takeover"></a>Verhindern verwaister DNS-Einträge und Vermeiden von Unterdomänenübernahmen

In diesem Artikel werden die allgemeinen Sicherheitsrisiken einer Unterdomänenübernahme und die Schritte beschrieben, die Sie ergreifen können, um dieses Problem abzuschwächen.


## <a name="what-is-subdomain-takeover"></a>Was bedeutet Unterdomänenübernahme?

Unterdomänenübernahmen sind häufige Bedrohungen mit hohem Schweregrad für Unternehmen, die regelmäßig viele Ressourcen erstellen und löschen. Eine Unterdomänenübernahme kann auftreten, wenn Sie über einen DNS-Eintrag verfügen, der auf eine Azure-Ressource verweist, deren Bereitstellung aufgehoben wurde. Solche DNS-Einträge werden auch als „verwaiste DNS“-Einträge bezeichnet. CNAME-Einträge sind besonders anfällig für diese Bedrohung.

Ein häufiges Szenario für eine Unterdomänenübernahme:

1. Eine Website wird erstellt. 

    In diesem Beispiel ist dies `app-contogreat-dev-001.azurewebsites.net`.

1. Ein CNAME-Eintrag wird zum DNS hinzugefügt, das auf die Website verweist. 

    In diesem Beispiel wurde der folgende Anzeigename erstellt: `greatapp.contoso.com`.

1. Nach einigen Monaten wird die Website nicht mehr benötigt, sodass sie gelöscht wird. Der entsprechende DNS-Eintrag wird jedoch **nicht** gelöscht. 

    Der CNAME-DNS-Eintrag ist jetzt „verwaist“.

1. Fast unmittelbar nach dem Löschen der Website entdeckt ein Bedrohungsakteur die fehlende Website und erstellt eine eigene Website unter `app-contogreat-dev-001.azurewebsites.net`.

    Der für `greatapp.contoso.com` vorgesehene Datenverkehr geht nun an die Azure-Website des Bedrohungsakteurs, der den angezeigten Inhalt jetzt kontrolliert. 

    Das verwaiste DNS wurde ausgenutzt, und die Unterdomäne „GreatApp“ von Contoso wurde Opfer einer Unterdomänenübernahme. 

![Unterdomänenübernahme einer Website, deren Bereitstellung aufgehoben wurde](./media/subdomain-takeover/subdomain-takeover.png)



## <a name="the-risks-of-subdomain-takeover"></a>Risiken von Unterdomänenübernahmen

Wenn ein DNS-Eintrag auf eine Ressource verweist, die nicht verfügbar ist, hätte der Eintrag selbst aus der DNS-Zone entfernt worden sollen. Wenn er nicht gelöscht wurde, handelt es sich um einen „verwaisten DNS“-Eintrag, was eine Unterdomänenübernahme ermöglicht.

Verwaiste DNS-Einträge ermöglichen es Bedrohungsakteuren, den zugeordneten DNS-Namen zu kontrollieren, um böswillige Websites oder Dienste zu hosten. Böswillige Seiten und Dienste in der Unterdomäne einer Organisation können Folgendes verursachen:

- **Verlust der Kontrolle über den Inhalt der Unterdomäne** – negative Presse dazu, dass Ihre Organisation ihre Inhalte nicht schützen kann, Schädigung der Marke und Vertrauensverlust

- **Cookie-Harvesting von ahnungslosen Besuchern** – es ist üblich, dass Web-Apps Sitzungscookies für Unterdomänen (*.contoso.com) verfügbar machen, sodass jede Unterdomäne darauf zugreifen kann. Bedrohungsakteure können mithilfe einer Unterdomänenübernahme eine authentisch wirkende Seite erstellen, die von ahnungslosen Besuchern aufgerufen wird, und anschließend deren Cookies sammeln (einschließlich sicherer Cookies). Ein gängiges Missverständnis besteht darin, dass SSL-Zertifikate Ihre Website und die Cookies Ihrer Benutzer vor einer Übernahme schützen. Bedrohungsakteure können die gehackte Unterdomäne verwenden, um ein gültiges SSL-Zertifikat zu beantragen und abzurufen. Gültige SSL-Zertifikate gewähren ihnen Zugriff auf sichere Cookies und führen außerdem dazu, dass die böswillige Website als legitim wahrgenommen wird.

- **Phishingkampagnen** – authentisch aussehende Unterdomänen können in Phishingkampagnen verwendet werden. Dies gilt für böswillige Websites sowie für MX-Einträge, die es dem Bedrohungsakteur ermöglichen, E-Mails zu empfangen, die an eine legitime Unterdomäne einer bekanntermaßen sicheren Marke adressiert sind.

- **Weitere Risiken** – böswillige Websites können auch für andere klassische Angriffe wie XSS, CSRF, CORS-Umgehungen u. a. verwendet werden.



## <a name="preventing-dangling-dns-entries"></a>Verhindern verwaister DNS-Einträge

Ein wichtiger Teil des Sicherheitsprogramms besteht darin, sicherzustellen, dass Ihre Organisation Prozesse implementiert hat, die verwaiste DNS-Einträge und daraus resultierende Unterdomänenübernahmen verhindern.

Die derzeit verfügbaren Präventionsmaßnahmen sind im Folgenden aufgeführt.


### <a name="use-azure-dns-alias-records"></a>Verwenden von Azure DNS-Aliaseinträgen

Azure DNS-[Aliaseinträge](https://docs.microsoft.com/azure/dns/dns-alias#scenarios) verhindern verwaiste Verweise durch eine Kopplung des Lebenszyklus eines DNS-Eintrags mit einer Azure-Ressource. Stellen Sie sich beispielsweise einen DNS-Eintrag vor, der als Aliaseintrag qualifiziert wird, um auf eine öffentliche IP-Adresse oder auf ein Traffic Manager-Profil zu verweisen. Wenn Sie diese zugrunde liegenden Ressourcen löschen, wird der DNS-Aliaseintrag ein leerer Eintragssatz. Er verweist nicht mehr auf die gelöschte Ressource. Beachten Sie jedoch, dass es Grenzen in Bezug darauf gibt, was Sie mit Aliaseinträgen schützen können. Dies ist derzeit auf Folgendes beschränkt:

- Azure Front Door
- Traffic Manager-Profile
- Azure Content Delivery Network-Endpunkte (CDN)
- Öffentliche IP-Adressen

Trotz der aktuell begrenzten Dienstangebote empfiehlt es sich, nach Möglichkeit Aliaseinträge zu verwenden, um sich vor Unterdomänenübernahmen zu schützen.

[Erfahren Sie mehr](https://docs.microsoft.com/azure/dns/dns-alias#capabilities) über die Funktionen von Azure DNS-Aliaseinträgen.



### <a name="use-azure-app-services-custom-domain-verification"></a>Verwenden der Überprüfung benutzerdefinierter Domänen von Azure App Service

Wenn Sie DNS-Einträge für Azure App Service erstellen, erstellen Sie auch einen asuid.{subdomain}-TXT-Eintrag mit der Verifizierungs-ID für die Domäne. Wenn ein solcher TXT-Datensatz vorhanden ist, kann kein anderes Azure-Abonnement die benutzerdefinierte Domäne verifizieren und übernehmen. 

Diese Datensätze verhindern nicht, dass jemand den Azure App Service mit dem gleichen Namen wie in Ihrem CNAME-Eintrag erstellt. Wenn Bedrohungsakteure nicht nachweisen können, dass sie Besitzer des Domänennamens sind, können sie keinen Datenverkehr empfangen oder den Inhalt kontrollieren.

[Erfahren Sie mehr](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain) darüber, wie Azure App Service ein vorhandener benutzerdefinierter DNS-Name zugeordnet wird.



### <a name="build-and-automate-processes-to-mitigate-the-threat"></a>Erstellen und Automatisieren von Prozessen zum Abschwächen der Bedrohung

Entwickler und Betriebsteams müssen häufig Bereinigungsprozesse durchführen, damit keine Bedrohungen durch verwaiste DNS entstehen. Mithilfe der folgenden Vorgehensweisen können Sie sicherstellen, dass Ihre Organisation diese Bedrohung vermeidet. 

- **Erstellen von Verfahren zur Prävention:**

    - Informieren Sie Ihre Anwendungsentwickler darüber, dass sie beim Löschen von Ressourcen Adressen immer umleiten.

    - Nehmen Sie „DNS-Eintrag entfernen“ auf die Liste der Punkte auf, die bei der Außerbetriebsetzung eines Diensts beachtet werden müssen.

    - Legen Sie [Löschsperren](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources) für alle Ressourcen mit einem benutzerdefinierten DNS-Eintrag fest. Eine Löschsperre dient als Indikator dafür, dass die Zuordnung entfernt werden muss, bevor die Bereitstellung der Ressource aufgehoben wird. Maßnahmen wie diese funktionieren nur in Kombination mit internen Schulungsprogrammen.

- **Erstellen von Verfahren zur Ermittlung:**

    - Überprüfen Sie Ihre DNS-Einträge regelmäßig, um sicherzustellen, dass alle Ihre Unterdomänen Azure-Ressourcen zugeordnet sind, auf die Folgendes zutrifft:

        - Vorhanden – fragen Sie die DNS-Zonen in Bezug auf Ressourcen ab, die auf Azure-Unterdomänen wie *.azurewebsites.net oder *.cloudapp.azure.com verweisen (siehe dazu [diese Referenzliste](azure-domains.md)).
        - In Ihrem Besitz – stellen Sie sicher, dass Sie alle Ressourcen besitzen, auf die Ihre DNS-Unterdomänen verweisen.

    - Pflegen Sie einen Dienstkatalog mit Ihren Azure-FQDN-Endpunkten (Fully Qualified Domain Name, vollqualifizierter Domänenname) und den Anwendungsbesitzern. Führen Sie zum Erstellen des Dienstkatalogs das folgende Azure Resource Graph-Abfrageskript aus. Das Skript projiziert die FQDN-Endpunktinformationen der Ressourcen, auf die Sie Zugriff haben, und gibt sie in einer CSV-Datei aus. Wenn Sie Zugriff auf alle Abonnements Ihres Mandanten haben, berücksichtigt das Skript all diese Abonnements, wie im folgenden Beispielskript gezeigt. Um die Ergebnisse auf eine bestimmte Gruppe von Abonnements zu beschränken, bearbeiten Sie das Skript wie gezeigt.

        >[!IMPORTANT]
        > **Berechtigungen**: Führen Sie die Abfrage als Benutzer aus, der Zugriff auf all Ihre Azure-Abonnements hat. 
        >
        > **Einschränkungen** – bei Azure Resource Graph gibt es Einschränkungen in Bezug auf die Drosselung und die Auslagerung, die Sie beachten müssen, wenn Sie über eine große Azure-Umgebung verfügen. [Erfahren Sie mehr](https://docs.microsoft.com/azure/governance/resource-graph/concepts/work-with-data) über das Arbeiten mit großen Datasets von Azure-Ressourcen. Das folgende Beispielskript verwendet eine Batchverarbeitung für Abonnements, um diese Einschränkungen zu vermeiden.

        ```powershell
        
            # Fetch the full array of subscription IDs.
            $subscriptions = Get-AzSubscription

            $subscriptionIds = $subscriptions.Id
                   # Output file path and names
                   $date = get-date
                   $fdate = $date.ToString("MM-dd-yyy hh_mm_ss tt")
                   $fdate #log to console
                   $rpath = [Environment]::GetFolderPath("MyDocuments") + '\' # Feel free to update your path.
                   $rname = 'Tenant_FQDN_Report_' + $fdate + '.csv' # Feel free to update the document name.
                   $fpath = $rpath + $rname
                   $fpath #This is the output file of FQDN report.

            # query
            $query = "where type in ('microsoft.network/frontdoors',
                                    'microsoft.storage/storageaccounts',
                                    'microsoft.cdn/profiles/endpoints',
                                    'microsoft.network/publicipaddresses',
                                    'microsoft.network/trafficmanagerprofiles',
                                    'microsoft.containerinstance/containergroups',
                                    'microsoft.apimanagement/service',
                                    'microsoft.web/sites',
                                    'microsoft.web/sites/slots')
                        | extend FQDN = case(
                            type =~ 'microsoft.network/frontdoors', properties['cName'],
                            type =~ 'microsoft.storage/storageaccounts', parse_url(tostring(properties['primaryEndpoints']['blob'])).Host,
                            type =~ 'microsoft.cdn/profiles/endpoints', properties['hostName'],
                            type =~ 'microsoft.network/publicipaddresses', properties['dnsSettings']['fqdn'],
                            type =~ 'microsoft.network/trafficmanagerprofiles', properties['dnsConfig']['fqdn'],
                            type =~ 'microsoft.containerinstance/containergroups', properties['ipAddress']['fqdn'],
                            type =~ 'microsoft.apimanagement/service', properties['hostnameConfigurations']['hostName'],
                            type =~ 'microsoft.web/sites', properties['defaultHostName'],
                            type =~ 'microsoft.web/sites/slots', properties['defaultHostName'],
                            '')
                        | project id, ['type'], name, FQDN
                        | where isnotempty(FQDN)";

            # Paging helper cursor
            $Skip = 0;
            $First = 1000;

            # If you have large number of subscriptions, process them in batches of 2,000.
            $counter = [PSCustomObject] @{ Value = 0 }
            $batchSize = 2000
            $response = @()

            # Group the subscriptions into batches.
            $subscriptionsBatch = $subscriptionIds | Group -Property { [math]::Floor($counter.Value++ / $batchSize) }

            # Run the query for each subscription batch with paging.
            foreach ($batch in $subscriptionsBatch)
            { 
                $Skip = 0; #Reset after each batch.

                $response += do { Start-Sleep -Milliseconds 500;   if ($Skip -eq 0) {$y = Search-AzGraph -Query $query -First $First -Subscription $batch.Group ; } `
                else {$y = Search-AzGraph -Query $query -Skip $Skip -First $First -Subscription $batch.Group } `
                $cont = $y.Count -eq $First; $Skip = $Skip + $First; $y; } while ($cont)
            }

            # View the completed results of the query on all subscriptions.
            $response | Export-Csv -Path $fpath -Append 

        ```

        Hier finden Sie eine Liste der Typen und zugehörigen `FQDNProperty`-Werte, wie in der obigen Resource Graph-Abfrage angegeben:

        |Ressourcenname  | `<ResourceType>`  | `<FQDNproperty>`  |
        |---------|---------|---------|
        |Azure Front Door|microsoft.network/frontdoors|properties.cName|
        |Azure Blob Storage|microsoft.storage/storageaccounts|properties.primaryEndpoints.blob|
        |Azure CDN|microsoft.cdn/profiles/endpoints|properties.hostName|
        |Öffentliche IP-Adressen|microsoft.network/publicipaddresses|properties.dnsSettings.fqdn|
        |Azure Traffic Manager|microsoft.network/trafficmanagerprofiles|properties.dnsConfig.fqdn|
        |Azure Container Instances|microsoft.containerinstance/containergroups|properties.ipAddress.fqdn|
        |Azure API Management|microsoft.apimanagement/service|properties.hostnameConfigurations.hostName|
        |Azure App Service|microsoft.web/sites|properties.defaultHostName|
        |Azure App Service – Slots|microsoft.web/sites/slots|properties.defaultHostName|


- **Erstellen von Verfahren zur Korrektur:**
    - Wenn Sie verwaiste DNS-Einträge finden, muss das Team überprüfen, ob eine Gefährdung aufgetreten ist.
    - Untersuchen Sie, weshalb die Adresse nicht umgeleitet wurde, als die Ressource außer Betrieb gesetzt wurde.
    - Löschen Sie den DNS-Eintrag, wenn er nicht mehr verwendet wird, oder verweisen Sie ihn auf die korrekte Azure-Ressource (FQDN), die sich im Besitz Ihrer Organisation befindet.
 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu verwandten Diensten und Azure-Features, die Sie verwenden können, um sich vor Unterdomänenübernahmen zu schützen, finden Sie auf den folgenden Seiten.

- [Azure DNS unterstützt die Verwendung von Aliaseinträgen für benutzerdefinierte Domänen](https://docs.microsoft.com/azure/dns/dns-alias#prevent-dangling-dns-records)

- [Verwenden der Verifizierungs-ID für eine Domäne beim Hinzufügen benutzerdefinierter Domänen in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain#get-domain-verification-id) 

- [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mit Azure PowerShell](https://docs.microsoft.com/azure/governance/resource-graph/first-query-powershell)
