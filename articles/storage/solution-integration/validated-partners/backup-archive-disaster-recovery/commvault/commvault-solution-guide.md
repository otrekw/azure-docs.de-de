---
title: Sichern Ihrer Daten in Azure mit Commvault
titleSuffix: Azure Blob Storage Docs
description: Diese Webseite bietet einen Überblick über die zu berücksichtigenden Faktoren und die auszuführenden Schritte, um Azure als Speicherziel und Wiederherstellungsstandort für Commvault Complete Backup and Recovery zu verwenden.
keywords: Commvault, Backup to Cloud, Sicherung in der Cloud, Backup, Sicherung, Backup to Azure, Sicherung in Azure, Disaster Recovery, Notfallwiederherstellung, Business Continuity, Geschäftskontinuität
author: karauten
ms.author: karauten
ms.date: 11/11/2020
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.openlocfilehash: f340a06f3b6b7c8fc0d78051fb0496dcab11fb5d
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102122747"
---
# <a name="back-up-to-azure-with-commvault"></a>Sichern in Azure mit Commvault

Dieser Artikel ist ein Leitfaden für die Integration einer Commvault-Infrastruktur in Azure Blob Storage. Er enthält Informationen zu Voraussetzungen, Azure Storage-Prinzipien sowie Anleitungen zu Implementierung und Betrieb. In diesem Artikel geht es nur um die Nutzung von Azure als Offsite-Sicherungsziel und Wiederherstellungsstandort nach einem Notfall, durch den ein normaler Betrieb an Ihrem primären Standort nicht möglich ist. Commvault bietet mit Commvault Live Sync auch eine Lösung mit niedrigerer RTO. Damit können Sie eine sofort startfähige Standby-VM einrichten, sind im Fall der Fälle schneller wieder betriebsbereit und schützen Ihre Ressourcen in einer Azure-Produktionsumgebung. Diese Funktionen werden im vorliegenden Dokument nicht behandelt. 

## <a name="reference-architecture-for-on-premises-to-azure-and-in-azure-deployments"></a>Referenzarchitektur für Sicherungsbereitstellungen vom lokalen Standort in Azure und für Azure-interne Bereitstellungen

![Referenzarchitektur: Commvault zu Azure](../media/commvault-diagram.png)

Ihre vorhandene Commvault-Bereitstellung lässt sich problemlos in Azure integrieren, indem Sie einfach ein oder mehrere Azure Storage-Konten als Cloudsicherungsziel hinzufügen. Commvault ermöglicht auch die Wiederherstellung von Sicherungen vom lokalen Standort in Azure und bietet damit einen Standort zur bedarfsbasierten Wiederherstellung in Azure.   

## <a name="commvault-interoperability-matrix"></a>CommVault-Interoperabilitätsmatrix
| Workload | GPv2 und Blob Storage | Unterstützung für kalte Speicherebene | Unterstützung für Archivspeicherebene | Unterstützung für die Data Box-Produktfamilie |
|-----------------------|--------------------|--------------------|-------------------------|-------------------------|
| Lokale VMs/Daten | v11.5 | v11.5 | v11.10 | v11.10 |
| Virtuelle Azure-Computer | v11.5 | v11.5 | v11.5 | Nicht verfügbar |
| Azure Blob | v11.6 | v11.6 | v11.6 | Nicht verfügbar |
| Azure Files | v11.6 | v11.6 | v11.6 | Nicht verfügbar | 

## <a name="before-you-begin"></a>Voraussetzungen

Mit ein bisschen Vorausplanung können Sie sich schon bald zum Kreis der vielen glücklichen Kunden zählen, die Azure als Offsite-Sicherungsziel und Wiederherstellungsstandort verwenden.

### <a name="are-you-new-to-azure"></a>Haben Sie Azure bisher noch nicht eingesetzt?

Microsoft bietet ein Framework, das Ihnen den Einstieg in Azure erleichtert. Das [Cloud Adoption Framework](https://docs.microsoft.com/azure/architecture/cloud-adoption/) \(CAF\) ist ein detailliertes Konzept für die digitale Transformation in Unternehmen und ein umfassender Leitfaden für die Planung der Cloudeinführung in einer Produktionsumgebung. Das CAF enthält eine [Azure-Einrichtungsleitfaden](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/azure-setup-guide/) mit Schrittanleitungen für Unternehmen, die bisher noch nicht mit Azure arbeiten. Der Leitfaden hilft beim schnellen und sicheren Einstieg und ist in einer interaktiven Version im [Azure-Portal](https://portal.azure.com/?feature.quickstart=true#blade/Microsoft_Azure_Resources/QuickstartCenterBlade) zu finden. Sie finden dort Beispielarchitekturen und spezielle Best Practices für die Bereitstellung von Anwendungen sowie kostenlose Schulungsressourcen, mit denen Sie die notwendigen Fachkenntnisse zu Azure erhalten.

### <a name="consider-the-network-between-your-location-and-azure"></a>Überlegungen zum Netzwerk zwischen Ihrem Standort und Azure

Unabhängig davon, ob Sie Cloudressourcen zum Ausführen von Produktions-, Test- oder Entwicklungsumgebungen oder als Sicherungsziel und Wiederherstellungsstandort verwenden – Sie müssen Ihre Bandbreitenanforderungen für das anfängliche Sicherungsseeding sowie für die täglichen Datenübertragungen genau kennen. 

Azure Data Box bietet eine Möglichkeit zum Übertragen der anfänglichen Sicherungsbaseline an Azure, ohne dafür zusätzliche Bandbreite zu benötigen, wenn diese Übertragung voraussichtlich länger dauert, als Ihr Unternehmen tolerieren kann. Wenn Sie ein Speicherkonto erstellen, können Sie das Tool zum Schätzen von Datenübertragungsanforderungen verwenden, um zu ermitteln, wie lange die Übertragung einer anfänglichen Sicherung dauern wird.

![Azure Storage: Tool zum Schätzen der Datenübertragungsanforderungen](../media/az-storage-transfer.png)

Denken Sie daran: Sie benötigen genügend Netzwerkkapazität, um tägliche Datenübertragungen innerhalb des erforderlichen Übertragungsfensters (Sicherungsfenster) zu unterstützen, ohne die Produktionsanwendungen zu beeinträchtigen. In diesem Abschnitt werden die Tools und Techniken beschrieben, die für die Bewertung Ihrer Netzwerkanforderungen verfügbar sind.

#### <a name="how-can-you-determine-how-much-bandwidth-you-will-need"></a>Wie ermitteln Sie, wie viel Bandbreite Sie benötigen?

-  Berichte von Ihrer Sicherungssoftware. 
  Commvault bietet Standardberichte, um die [Änderungsrate](https://documentation.commvault.com/commvault/v11_sp19/article?p=39699.htm) und die [Gesamtgröße des Sicherungssatzes](https://documentation.commvault.com/commvault/v11_sp19/article?p=39621.htm) für die anfängliche Baselineübertragung zu Azure zu bestimmen.
- Von der Sicherungsoftware unabhängige Bewertungs- und Berichterstellungstools wie:
  - [MiTrend](https://mitrend.com/)
  - [Aptare](https://www.veritas.com/insights/aptare-it-analytics)
  - [Datavoss](https://www.datavoss.com/)

#### <a name="how-will-i-know-how-much-headroom-i-have-with-my-current-internet-connection"></a>Woher weiß ich, wie viel Spielraum ich mit meiner aktuellen Internetverbindung habe?

Es ist wichtig zu wissen, wie viel Spielraum an ungenutzter Bandbreite Sie täglich haben. So können Sie sinnvoll bewerten, ob Sie Ihre zeitbezogenen Ziele für den anfänglichen Upload erreichen (falls Sie nicht Azure Data Box für ein Offlineseeding verwenden) und ob tägliche Sicherungen basierend auf der oben ermittelten Änderungsrate und Ihrem Sicherungsfenster ausgeführt werden können. Im Folgenden sehen Sie einige Methoden, mit denen Sie den Spielraum an Bandbreite ermitteln können, der für Sicherungen in Azure zur Verfügung steht.

- Sind Sie bereits Azure ExpressRoute-Kunde? Sehen Sie sich die [Leitungsnutzung](https://docs.microsoft.com/azure/expressroute/expressroute-monitoring-metrics-alerts#circuits-metrics) im Azure-Portal an.
- Sie können sich an Ihren ISP wenden. Dieser sollte Ihnen Berichte zur Verfügung stellen können, die Ihre derzeitige tägliche und monatliche Nutzung veranschaulichen.
- Es gibt eine Reihe von Tools, die durch Überwachung des Netzwerkdatenverkehrs auf Ebene des Routers bzw. Switches die Nutzung messen können. Dazu gehören z. B. folgende:
  - [Solarwinds Bandwidth Analyzer Pack](https://www.solarwinds.com/network-bandwidth-analyzer-pack?CMP=ORG-BLG-DNS)
  - [Paessler PRTG](https://www.paessler.com/bandwidth_monitoring)
  - [Cisco Network Assistant](https://www.cisco.com/c/en/us/products/cloud-systems-management/network-assistant/index.html)
  - [WhatsUp Gold](https://www.whatsupgold.com/network-traffic-monitoring)

### <a name="choosing-the-right-storage-options"></a>Auswählen der richtigen Speicheroptionen

Bei Verwendung von Azure als Speicherziel nutzen viele Kunden [Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)\. Azure Blob Storage ist die Objektspeicherlösung von Microsoft. Blob Storage ist für die Speicherung großer Mengen an unstrukturierten Daten optimiert, also Daten, für die kein Datenmodell und keine Datendefinition zutrifft. Darüber hinaus ist Azure Storage beständig, hochverfügbar, sicher und skalierbar. Die Plattform von Microsoft bietet Flexibilität, sodass Sie den richtigen Speicher für die richtige Workload auswählen und damit das [Maß an Resilienz](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=/azure/storage/blobs/toc.json) bereitstellen können, das Sie für Ihre internen SLAs benötigen. Blob Storage ist ein Dienst mit nutzungsbasierter Bezahlung. Ihnen wird Folgendes [monatlich in Rechnung gestellt](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers?tabs=azure-portal#pricing-and-billing): die Menge der gespeicherten Daten, der Zugriff auf diese Daten und bei den Speicherebenen „Kalt“ und „Archiv“ ein Mindestzeitraum für die Aufbewahrung. In den folgenden Tabellen sind die Resilienz- und Tieringoptionen aufgeführt, die für Sicherungsdaten gelten.

**Resilienzoptionen für Azure Blob Storage**:

|  |Lokal redundant  |Zonenredundant  |Geografisch redundant  |Geozonenredundant  |
|---------|---------|---------|---------|---------|
|Effektive Anzahl von Kopien     | 3         | 3         | 6         | 6 |
|Anzahl von Verfügbarkeitszonen     | 1         | 3         | 2         | 4 |
|Anzahl von Regionen     | 1         | 1         | 2         | 2 |
|Manuelles Failover zu sekundärer Region     | Nicht verfügbar         | Nicht verfügbar         | Ja         | Ja |

**Azure Blob Storage-Ebenen**:

|  | Heiße Speicherebene   |Kalte Speicherebene   | Archivzugriffsebene |
| ----------- | ----------- | -----------  | -----------  |
| Verfügbarkeit | 99,9 %         | 99 %         | Offline      |
| Nutzungsgebühren | Höhere Speicherkosten, geringere Zugriffs- und Transaktionskosten | Geringere Speicherkosten, höhere Zugriffs- und Transaktionskosten | Niedrigste Speicherkosten, höchste Zugriffs- und Transaktionskosten |
| Mindestzeitraum für Datenaufbewahrung erforderlich | Nicht verfügbar | 30 Tage | 180 Tage |
| Latenz (Zeitraum bis zum ersten Byte) | Millisekunden | Millisekunden | Stunden |

#### <a name="sample-backup-to-azure-cost-model"></a>Beispielkostenmodell für die Sicherung in Azure

Das Konzept der nutzungsbasierten Bezahlung kann für Kunden, die noch nicht mit der öffentlichen Cloud vertraut sind, zunächst etwas abschreckend wirken. Sie zahlen zwar nur für die tatsächlich genutzte Kapazität, aber auch für Transaktionen (Lesen und Schreiben) sowie für [ausgehende Daten](https://azure.microsoft.com/pricing/details/bandwidth/), die wieder in Ihre lokale Umgebung eingelesen werden, wenn der [Azure ExpressRoute Direct Local-Plan oder der ExpressRoute-Plan mit Datenflatrate](https://azure.microsoft.com/pricing/details/expressroute/) verwendet werden, bei denen ausgehende Daten von Azure inbegriffen sind. Sie können im [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/) eine Was-wäre-wenn-Analyse basierend auf den Listenpreisen oder anhand der [Preise für reservierte Azure Storage-Kapazitäten](https://docs.microsoft.com/azure/cost-management-billing/reservations/save-compute-costs-reservations) ausführen. Diese Option kann bis zu 38 % Einsparungen einbringen. Im Folgenden sehen Sie eine Beispielrechnung zur Modellierung der monatlichen Kosten für die Sicherung in Azure. Dies ist nur ein Beispiel, ***die Preise für Ihre Lösung können aufgrund verschiedener hier nicht erfasster Aktivitäten abweichen***:


|Kostenfaktor  |Monatliche Kosten  |
|---------|---------|
|100 TB Sicherungsdaten auf der kalten Speicherebene     |USD 1556,48         |
|2 TB neu geschriebene Daten pro Tag × 30 Tage     |USD 39 für Transaktionen          |
|Geschätzter monatlicher Gesamtbetrag     |USD 1595,48         |
|---------|---------|
|Einmalige Wiederherstellung von 5 TB am lokalen Standort über das öffentliche Internet   | USD 491,26         |


> [!Note] 
Diese Schätzung wurde im Azure-Preisrechner mit nutzungsbasierter Bezahlung für „USA, Osten“ generiert und basiert auf dem Commvault-Standardwert von 32 MB Unterblockgröße, der 65.536 PUT-Anforderungen, d. h. Schreibtransaktionen, pro Tag generiert. Dieses Beispiel ist auf Ihre Anforderungen möglicherweise nicht anwendbar.

## <a name="implementation-and-operational-guidance"></a>Leitfaden für Implementierung und Betrieb

Dieser Abschnitt bietet eine kurze Übersicht über das Hinzufügen von Azure Storage zu einer lokalen Commvault-Bereitstellung. Wenn Sie detailliertere Anleitungen sowie Überlegungen zur Planung benötigen, lesen Sie den [Commvault Azure Architecture Guide](https://www.commvault.com/resources/public-cloud-architecture-guide-for-microsoft-azure-v11-sp16) (Handbuch zur Commvault-Azure-Architektur).

1. Öffnen Sie das Azure-Portal, und suchen Sie nach „Speicherkonten“, oder klicken Sie auf das Symbol für Standarddienste.
    
    1. ![Azure-Portal](../media/azure-portal.png)
  
    1. ![Speicherkonten im Azure-Portal](../media/locate-storage-account.png)

2. Wählen Sie „Konto hinzufügen“ aus. Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine neue. Geben Sie einen eindeutigen Namen an. Wählen Sie die Region aus. Wählen Sie „Standard“ als Leistungsebene aus. Behalten Sie den Kontotyp „Storage V2“ bei. Wählen Sie die Replikationsstufe aus, die Ihren SLAs entspricht. Wählen Sie die Standardebene aus, die von Ihrer Sicherungssoftware verwendet wird. Azure Storage stellt die Speicherebenen „Heiß“, „Kalt“ und „Archiv“ innerhalb eines einzelnen Kontos zur Verfügung, und die Commvault-Richtlinien ermöglichen Ihnen, mehrere Ebenen zu nutzen, um den Lebenszyklus Ihrer Daten effektiv zu verwalten. Fahren Sie mit dem nächsten Schritt fort. 

    ![Erstellen eines Speicherkontos](../media/account-create-1.png)

3. Behalten Sie die standardmäßigen Netzwerkoptionen zunächst bei, und fahren Sie mit „Datenschutz“ fort. Hier können Sie die Option „Vorläufiges Löschen“ aktivieren, die Ihnen ermöglicht, eine versehentlich gelöschte Sicherungsdatei innerhalb des definierten Aufbewahrungszeitraums wiederherzustellen. Diese Option bietet Schutz vor versehentlichen oder böswilligen Löschvorgängen. 
    
    ![Erstellen eines Speicherkontos, Teil 2](../media/account-create-2.png)

4. Wir empfehlen, die Standardeinstellungen aus dem Bildschirm „Erweitert“ für Sicherungen in Azure zu übernehmen.

    ![Erstellen eines Speicherkontos, Teil 3](../media/account-create-3.png) 

5. Fügen Sie Tags für die Organisation hinzu, wenn Sie das Tagging nutzen, und erstellen Sie das Konto. Jetzt stehen Ihnen mehrere Petabyte an bedarfsbasiertem Speicher zur Verfügung!

6. Sie müssen nur noch zwei schnelle Schritte ausführen, dann können Sie das Konto Ihrer Commvault-Umgebung hinzufügen. Navigieren Sie im Azure-Portal zu dem gerade erstellten Konto, und wählen Sie auf dem Portalblatt unter dem Menü „Blobdienst“ die Option „Container“ aus. Fügen Sie einen neuen Container hinzu, und geben Sie einen aussagekräftigen Namen an. Navigieren Sie dann in den Einstellungen zu „Zugriffsschlüssel“, und kopieren Sie den Namen des Speicherkontos und einen der beiden Zugriffsschlüssel. In den nächsten Schritten benötigen Sie den Namen des Containers, den Namen des Kontos und den Zugriffsschlüssel.
    
    ![Erstellen eines Containers](../media/container.png)
    
    ![Kontoinformationen erfassen](../media/access-key.png)

7. ***(Optional)*** Sie können Ihrer Bereitstellung zusätzliche Sicherheitsschichten hinzufügen.
    
    1. Konfigurieren Sie den rollenbasierten Zugriff, um den Kreis der Personen einzuschränken, die Änderungen an Ihrem Speicherkonto vornehmen dürfen. [Weitere Informationen finden Sie hier](https://docs.microsoft.com/azure/storage/common/authorization-resource-provider?toc=/azure/storage/blobs/toc.json)
    1. Schränken Sie den Kontozugriff mithilfe der [Storage-Firewall](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal) auf bestimmte Netzwerksegmente ein, um Zugriffe von Punkten außerhalb Ihres Unternehmensnetzwerks zu verhindern.

    ![Storage-Firewall](../media/storage-firewall.png) 

    1. Legen Sie eine [Löschsperre](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources) für das Storage-Konto fest, um ein versehentliches Löschen des Kontos zu verhindern.

    ![Ressourcensperre](../media/resource-lock.png)
    
    1. Konfigurieren Sie [zusätzliche Best Practices für die Sicherheit](https://docs.microsoft.com/azure/storage/blobs/security-recommendations).
    
1. Navigieren Sie im Commvault Command Center zu „Manage > Security > Credential Manager“ (Verwalten > Sicherheit > Anmeldeinformationen-Manager). Wählen Sie einen „Cloud Account“ (Cloudkonto) aus, Microsoft Azure Storage als „Vendor Type“ (Anbietertyp), und wählen Sie den „Media Agent“ aus, der Daten in und aus Azure überträgt, und fügen Sie „Storage Account Name“ (Speicherkontonamen) und „Access Key“ (Zugriffsschlüssel) hinzu.
    
    ![Commvault-Anmeldeinformationen](../media/commvault-credential.png)

9. Navigieren Sie als Nächstes im Commvault Command Center zu „Storage > Cloud“ (Speicher > Cloud). Wählen Sie „Add“ (Hinzufügen) aus. Geben Sie einen Anzeigenamen für das Speicherkonto ein, und wählen Sie dann „Microsoft Azure Storage“ in der Liste „Type“ (Typ) aus. Wählen Sie einen Media Agent-Server aus, der zum Übertragen von Sicherungen zu Azure Storage verwendet wird. Fügen Sie den erstellten Container hinzu, wählen Sie den „Storage Tier“ (Speicherebene) aus, der im Azure Storage-Konto genutzt werden soll, und wählen Sie die in Schritt 8 erstellten „Credentials“ (Anmeldeinformationen) aus. Wählen Sie zum Schluss aus, ob deduplizierte Sicherungen übertragen werden sollen oder nicht, und wählen Sie einen Speicherort für die Deduplizierungsdatenbank aus.
    
     ![Hinzufügen von Commvault-Speicher](../media/commvault-add-storage.png)

10. Fügen Sie schließlich Ihre neue Azure Storage-Ressource einem vorhandenen oder neuen Plan im Commvault Command Center über „Manage > Plans“ (Verwalten > Pläne) als „Backup Destination“ (Sicherungsziel) hinzu.

    ![Hinzufügen von Commvault-Speicher](../media/commvault-plan.png)

11. ***(Optional)*** Wenn Sie Azure als Wiederherstellungsstandort oder Commvault zum Migrieren von Servern und Anwendungen zu Azure nutzen möchten, sollten Sie einen VSA-Proxy in Azure bereitzustellen. Detaillierte Anweisungen finden Sie [hier](https://documentation.commvault.com/commvault/v11/article?p=106208.htm).  

### <a name="azure-alerting-and-performance-monitoring"></a>Warnungen und Leistungsüberwachung in Azure

Es ist ratsam, sowohl Ihre Azure-Ressourcen also auch die Fähigkeit von Commvault zur Nutzung dieser Ressourcen zu überwachen – genauso wie bei jedem anderen Speicherziel auch, auf das Sie sich bei der Speicherung Ihrer Sicherungen verlassen. Eine Kombination aus Azure Monitor- und Commvault Command Center-Überwachung hilft Ihnen, Ihre Umgebung zu schützen.

#### <a name="microsoft-azure-portal"></a>Microsoft Azure-Portal

Mit [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource) bietet Microsoft Azure eine stabile Überwachungslösung. Sie können [Azure Monitor konfigurieren](https://docs.microsoft.com/azure/storage/common/monitor-storage?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-powershell#configuration), um die Kapazität, Transaktionen, Verfügbarkeit, Authentifizierung und viele weitere Aspekte von Azure Storage nachzuverfolgen. Eine vollständige Referenz der nachverfolgten Metriken finden Sie [hier](https://docs.microsoft.com/azure/storage/common/monitor-storage-reference). Folgende Metriken sind nützlich: BlobCapacity zum Sicherstellen, dass das [Kapazitätslimit des Storage-Kontos](https://docs.microsoft.com/azure/storage/common/scalability-targets-standard-account) nicht überschritten wird; der eingehende und ausgehende Datenverkehr zum Nachverfolgen der Menge an Daten, die in Ihr Azure Storage-Konto geschrieben und daraus gelesen werden; SuccessE2ELatency zum Nachverfolgen der Roundtripzeit für Anforderungen an und von Azure Storage und Ihrem Medien-Agent. 

Sie können auch [Protokollwarnungen](https://docs.microsoft.com/azure/service-health/alerts-activity-log-service-notifications) erstellen, um die Integrität des Azure Storage-Diensts nachzuverfolgen, und sich jederzeit das [Azure-Statusdashboard](https://status.azure.com/status) ansehen.

#### <a name="commvault-command-center"></a>Commvault Command Center

[Erstellen von Warnungen für Cloud-Speicherpools](https://documentation.commvault.com/commvault/v11/article?p=100514_3.htm)

[Wo Kunden Leistungsberichte und den Abschluss von Aufträgen anzeigen und die grundlegende Problembehandlung starten können](https://documentation.commvault.com/commvault/v11/article?p=95306_1.htm) 

### <a name="how-to-open-support-cases"></a>Eröffnen von Supportfällen

Wenn Sie Unterstützung bei Ihrer Lösung für die Sicherung in Azure benötigen, empfiehlt es sich, sowohl bei Commvault als auch bei Azure einen Supportfall zu eröffnen, damit unsere Supportorganisationen bei Bedarf zusammenarbeiten können.

#### <a name="how-to-open-a-case-with-commvault"></a>So eröffnen Sie einen Supportfall bei Commvault

Navigieren Sie zur [Commvault Support Site](https://www.commvault.com/support), melden Sie sich an, und eröffnen Sie einen Fall.

Wenn Sie sich über die für Sie verfügbaren Supportvertragsoptionen informieren müssen, finden Sie weitere Informationen unter [Commvault Support Options](https://ma.commvault.com/support) (Optionen für den Commvault Support).

Sie können einen Fall auch telefonisch eröffnen oder den Commvault Support per E-Mail erreichen:

Gebührenfrei: +1 877-780-3077

[Weltweite Supporttelefonnummern](https://ma.commvault.com/Support/TelephoneSupport)

[Commvault Support per E-Mail](mailto:support@commvault.com)

#### <a name="how-to-open-a-case-with-the-azure-support-team"></a>So eröffnen Sie einen Fall beim Azure-Supportteam

Suchen Sie im [Azure-Portal](https://portal.azure.com) oben in der Suchleiste nach „Support“, und wählen Sie „+ Neue Supportanfrage“ aus. 
> [!Note]
Geben Sie beim Eröffnen des Falls an, dass Sie Unterstützung für „Azure Storage“ oder „Azure-Netzwerk“ benötigen, **NICHT** für „Azure Backup“. Azure Backup ist ein nativer Dienst von Microsoft Azure, und Ihr Fall würde falsch weitergeleitet.

### <a name="links-to-relevant-commvault-documentation"></a>Links zu relevanter Commvault-Dokumentation

Commvault-Dokumentation mit weiteren Informationen:

[Commvault-Benutzerhandbuch](https://documentation.commvault.com/commvault/v11/article?p=37684_1.htm)

[Commvault Azure Architecture Guide](https://www.commvault.com/resources/public-cloud-architecture-guide-for-microsoft-azure-v11-sp16) (Handbuch zur Commvault-Azure-Architektur) 

### <a name="link-to-marketplace-offering"></a>Link zum Marketplace-Angebot

Sie können zum Schützen Ihrer in Azure ausgeführten Workloads auch weiterhin die Commvault-Lösung verwenden, die Sie kennen und der Sie vertrauen. Commvault hat die Bereitstellung der Commvault-Lösung in Azure zum Schutz von Azure-VMs und vielen weiteren Azure-Diensten sehr einfach gestaltet.

[Bereitstellen von Commvault über den Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/commvault.commvault?tab=Overview)

[Azure Datasheet](https://www.commvault.com/resources/microsoft-azure-cloud-platform-datasheet)

[Umfassende Liste der unterstützten Azure-Features und -Dienste](https://documentation.commvault.com/commvault/v11/article?p=109795_1.htm)

[Verwenden von Commvault zum Schützen von SAP HANA in Azure](https://azure.microsoft.com/resources/protecting-sap-hana-in-azure/)

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie auf den folgenden externen Websites weitere Ressourcen, um Informationen zu speziellen Nutzungsszenarien zu erhalten:

[Verwenden von Commvault zum Migrieren von Servern und Anwendungen zu Azure](https://www.commvault.com/resources/demonstration-vmware-to-azure-migrations-with-commvault)

[Schützen von SAP in Azure mit Commvault](https://www.youtube.com/watch?v=4ZGGE53mGVI)

[Schützen von Office 365 mit Commvault](https://www.youtube.com/watch?v=dl3nvAacxZU)
