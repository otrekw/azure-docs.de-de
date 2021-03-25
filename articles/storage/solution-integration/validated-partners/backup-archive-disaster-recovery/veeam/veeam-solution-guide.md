---
title: Sichern Ihrer Daten in Azure mit Veeam
titleSuffix: Azure Blob Storage Docs
description: Diese Webseite bietet einen Überblick über die zu berücksichtigenden Faktoren und die auszuführenden Schritte, um Azure als Speicherziel und Wiederherstellungsstandort für Veeam Backup and Recovery zu verwenden.
keywords: Veeam, Backup to Cloud, Sicherung in der Cloud, Backup, Sicherung, Backup to Azure, Sicherung in Azure, Disaster Recovery, Notfallwiederherstellung, Business Continuity, Geschäftskontinuität
author: karauten
ms.author: karauten
ms.date: 11/11/2020
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.openlocfilehash: 6d4b005a3f9c79ff14f5ba4053dc651c1ede56e0
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102124430"
---
# <a name="backup-to-azure-with-veeam"></a>Sicherung in Azure mit Veeam

Dieser Artikel ist ein Leitfaden für die Integration einer Veeam-Infrastruktur in Azure Blob Storage. Er enthält Informationen zu Voraussetzungen, Azure Storage-Prinzipien sowie Anleitungen zu Implementierung und Betrieb. In diesem Artikel geht es nur um die Nutzung von Azure als Offsitesicherungsziel und als Wiederherstellungsstandort nach einem Notfall, durch den ein normaler Betrieb an Ihrem primären Standort nicht möglich ist. Veeam bietet mit Veeam Replication außerdem eine Lösung mit niedrigerer RTO. Mithilfe dieser Lösung können Sie eine Standby-VM einrichten, die sofort gestartet werden kann. So sind Sie im Fall der Fälle schneller wieder betriebsbereit und können den Schutz Ihre Ressourcen in einer Azure-Produktionsumgebung umgehend wiederherstellen. Veeam bietet auch dedizierte Tools zur Sicherung von Azure- und Office 365-Ressourcen. Diese Funktionen werden im vorliegenden Dokument nicht behandelt. 

## <a name="reference-architecture-for-on-premises-to-azure-and-in-azure-deployments"></a>Referenzarchitektur für Sicherungsbereitstellungen vom lokalen Standort zu Azure und für Azure-interne Bereitstellungen

![Referenzarchitektur: Veeam zu Azure](../media/veeam-architecture.png)

Ihre vorhandene Veeam-Bereitstellung lässt sich problemlos in Azure integrieren, indem Sie einfach ein oder mehrere Azure Storage-Konten als Cloudsicherungsrepository hinzufügen. Veeam ermöglicht darüber hinaus die Wiederherstellung von Sicherungen vom lokalen Standort in Azure und bietet damit einen Standort zur bedarfsbasierten Wiederherstellung in Azure.

## <a name="veeam-interoperability-matrix"></a>Interoperabilitätsmatrix für Veeam
| Workload | GPv2 und Blob Storage | Unterstützung für kalte Speicherebene | Unterstützung für Archivspeicherebene | Unterstützung für die Data Box-Produktfamilie |
|-----------------------|--------------------|--------------------|-------------------------|-------------------------|
| Lokale VMs/Daten | v10a | v10a | Nicht verfügbar | 10a* |
| Virtuelle Azure-Computer | v10a | v10a | Nicht verfügbar | 10a* |
| Azure Blob | v10a | v10a | Nicht verfügbar | 10a* |
| Azure Files | v10a | v10a | Nicht verfügbar | 10a* | 

> [!Note]
Veeam Backup and Replication unterstützt die REST-API nur für Azure Data Box, daher wird Azure Data Box Disk nicht unterstützt. Unterstützung für die Archivspeicherebene von Azure Blob Storage wird für Veeam v11 erwartet.

## <a name="before-you-begin"></a>Voraussetzungen

Mit ein wenig Vorausplanung können Sie sich schon bald zum Kreis der vielen zufriedenen Kunden zählen, die Azure als Offsitesicherungsziel und als Wiederherstellungsstandort verwenden.

### <a name="are-you-new-to-azure"></a>Haben Sie Azure bisher noch nicht eingesetzt?

Microsoft bietet ein Framework, das Ihnen den Einstieg in Azure erleichtert. Das [Cloud Adoption Framework](https://docs.microsoft.com/azure/architecture/cloud-adoption/) \(CAF\) ist ein detailliertes Konzept für die digitale Transformation in Unternehmen und ein umfassender Leitfaden für die Planung der Cloudeinführung in einer Produktionsumgebung. Das CAF enthält eine [Azure-Einrichtungsleitfaden](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/azure-setup-guide/) mit Schrittanleitungen für Unternehmen, die bisher noch nicht mit Azure arbeiten. Der Leitfaden hilft beim schnellen und sicheren Einstieg und ist in einer interaktiven Version im [Azure-Portal](https://portal.azure.com/?feature.quickstart=true#blade/Microsoft_Azure_Resources/QuickstartCenterBlade) zu finden. Sie finden dort Beispielarchitekturen und spezielle Best Practices für die Bereitstellung von Anwendungen sowie kostenlose Schulungsressourcen, mit denen Sie die notwendigen Fachkenntnisse zu Azure erhalten.

### <a name="consider-the-network-between-your-location-and-azure"></a>Überlegungen zum Netzwerk zwischen Ihrem Standort und Azure

Unabhängig davon, ob Sie Cloudressourcen zum Ausführen von Produktions-, Test- oder Entwicklungsumgebungen oder als Sicherungsziel und Wiederherstellungsstandort verwenden – Sie müssen Ihre Bandbreitenanforderungen für das anfängliche Sicherungsseeding sowie für die täglichen Datenübertragungen genau kennen.

Azure Data Box bietet eine Möglichkeit zum Übertragen der anfänglichen Sicherungsbaseline an Azure, ohne dafür zusätzliche Bandbreite zu benötigen, wenn diese Übertragung voraussichtlich länger dauert, als Ihr Unternehmen tolerieren kann. Wenn Sie ein Speicherkonto erstellen, können Sie das Tool zum Schätzen von Datenübertragungsanforderungen verwenden, um zu ermitteln, wie lange die Übertragung einer anfänglichen Sicherung dauern wird.

![Azure Storage: Tool zum Schätzen der Datenübertragungsanforderungen](../media/az-storage-transfer.png)

Denken Sie daran: Sie benötigen genügend Netzwerkkapazität, um tägliche Datenübertragungen innerhalb des erforderlichen Übertragungsfensters (Sicherungsfenster) zu unterstützen, ohne die Produktionsanwendungen zu beeinträchtigen. In diesem Abschnitt werden die Tools und Techniken beschrieben, die für die Bewertung Ihrer Netzwerkanforderungen verfügbar sind.

#### <a name="how-can-you-determine-how-much-bandwidth-you-will-need"></a>Wie ermitteln Sie, wie viel Bandbreite Sie benötigen?

Es stehen mehrere Bewertungsoptionen zur Verfügung, mit denen Sie die Änderungsrate und die Gesamtgröße des Sicherungssatzes für die Übertragung der anfänglichen Baseline an Azure ermitteln können. Hier einige Beispiele für Bewertungs- und Berichterstellungstools:
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
|Effektiv Anzahl von Kopien     | 3         | 3         | 6         | 6 |
|Anzahl von Verfügbarkeitszonen     | 1         | 3         | 2         | 4 |
|Anzahl von Regionen     | 1         | 1         | 2         | 2 |
|Manuelles Failover zu sekundärer Region     | Nicht verfügbar         | Nicht verfügbar         | Ja         | Ja |

**Azure Blob Storage-Ebenen**:

|  | Heiße Speicherebene   |Kalte Speicherebene   | Archivzugriffsebene |
| ----------- | ----------- | -----------  | -----------  |
| Verfügbarkeit | 99,9 %         | 99 %         | Offline      |
| Nutzungsgebühren | Höhere Speicherkosten, geringere Zugriffs- und Transaktionskosten | Geringere Speicherkosten, höhere Zugriffs- und Transaktionskosten | Niedrigste Speicherkosten, höchste Zugriffs- und Transaktionskosten |
| Mindestzeitraum für Datenaufbewahrung erforderlich | Nicht verfügbar | 30 Tage | 180 Tage |
| Latenz (Zeit bis zum ersten Byte) | Millisekunden | Millisekunden | Stunden |

#### <a name="sample-backup-to-azure-cost-model"></a>Beispielkostenmodell für die Sicherung in Azure

Das Konzept der nutzungsbasierten Bezahlung kann für Kunden, die noch nicht mit der öffentlichen Cloud vertraut sind, zunächst etwas abschreckend wirken. Sie zahlen zwar nur für die tatsächlich genutzte Kapazität, aber auch für Transaktionen (Lesen und Schreiben) sowie für [ausgehende Daten](https://azure.microsoft.com/pricing/details/bandwidth/), die wieder in Ihre lokale Umgebung eingelesen werden, wenn der [Azure ExpressRoute Direct Local-Plan oder der ExpressRoute-Plan mit Datenflatrate](https://azure.microsoft.com/pricing/details/expressroute/) verwendet werden, bei denen ausgehende Daten von Azure inbegriffen sind. Sie können im [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/) eine Was-wäre-wenn-Analyse basierend auf den Listenpreisen oder anhand der [Preise für reservierte Azure Storage-Kapazitäten](https://docs.microsoft.com/azure/cost-management-billing/reservations/save-compute-costs-reservations) ausführen. Mithilfe dieser Option können Einsparungen von bis zu 38 % erzielt werden. Im Folgenden sehen Sie eine Beispielrechnung zur Modellierung der monatlichen Kosten für die Sicherung in Azure. Dies ist nur ein Beispiel, ***die Preise für Ihre Lösung können aufgrund verschiedener hier nicht erfasster Aktivitäten abweichen***:


|Kostenfaktor  |Monatliche Kosten  |
|---------|---------|
|100 TB Sicherungsdaten auf der kalten Speicherebene     |USD 1556,48         |
|2 TB neu geschriebene Daten pro Tag × 30 Tage     |USD 72 für Transaktionen          |
|Geschätzter monatlicher Gesamtbetrag     |USD 1628,48         |
|---------|---------|
|Einmalige Wiederherstellung von 5 TB am lokalen Standort über das öffentliche Internet   | USD 527,26         |

> [!Note]
Diese Schätzung wurde über den Azure-Preisrechner anhand der nutzungsbasierter Bezahlung in der Region „USA, Osten“ generiert und basiert auf dem Veeam-Standardwert von 256-K-Blöcken für Übertragungen im WAN. Dieses Beispiel ist auf Ihre Anforderungen möglicherweise nicht anwendbar.

## <a name="implementation-and-operational-guidance"></a>Leitfaden für Implementierung und Betrieb

Dieser Abschnitt bietet eine kurze Übersicht über das Hinzufügen von Azure Storage zu einer lokalen Veeam-Bereitstellung. Wenn Sie detailliertere Anleitungen sowie Überlegungen zur Planung benötigen, lesen Sie das Dokument [Veeam Cloud Connect Backup](https://helpcenter.veeam.com/docs/backup/cloud/cloud_backup.html?ver=100).

1. Öffnen Sie das Azure-Portal, und suchen Sie nach „Speicherkonten“, oder klicken Sie auf das Symbol für Standarddienste.

      ![Azure-Portal](../media/azure-portal.png)

      ![Speicherkonten im Azure-Portal](../media/locate-storage-account.png)

2. Wählen Sie „Konto hinzufügen“ aus. Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine neue. Geben Sie einen eindeutigen Namen an. Wählen Sie die Region aus. Wählen Sie „Standard“ als Leistungsebene aus. Behalten Sie den Kontotyp „Storage V2“ bei. Wählen Sie die Replikationsstufe aus, die Ihren SLAs entspricht. Wählen Sie die Standardebene aus, die von Ihrer Sicherungssoftware verwendet wird. Azure Storage stellt die Speicherebenen „Heiß“, „Kalt“ und „Archiv“ innerhalb eines einzelnen Kontos zur Verfügung, und die Veeam-Richtlinien ermöglichen es Ihnen, mehrere Ebenen zu nutzen, um den Lebenszyklus Ihrer Daten effektiv zu verwalten. Fahren Sie mit dem nächsten Schritt fort. 
    
      ![Erstellen eines Speicherkontos](../media/account-create-1.png)

3. Behalten Sie die standardmäßigen Netzwerkoptionen zunächst bei, und fahren Sie mit „Datenschutz“ fort. Hier können Sie die Option „Vorläufiges Löschen“ aktivieren, die es Ihnen ermöglicht, eine versehentlich gelöschte Sicherungsdatei innerhalb des definierten Aufbewahrungszeitraums wiederherzustellen. Diese Option bietet Schutz vor versehentlichen oder böswilligen Löschvorgängen. 
    ![Erstellen eines Speicherkontos, Teil 2](../media/account-create-2.png)

4. Wir empfehlen, die Standardeinstellungen aus dem Bildschirm „Erweitert“ für Sicherungen in Azure zu übernehmen.

    ![Erstellen eines Speicherkontos, Teil 3](../media/account-create-3.png) 

5. Fügen Sie Tags für die Organisation hinzu, wenn Sie das Tagging nutzen, und erstellen Sie das Konto. Jetzt stehen Ihnen mehrere Petabyte an bedarfsbasiertem Speicher zur Verfügung!

6. Sie müssen nur noch zwei schnelle Schritte ausführen, dann können Sie das Konto zu Ihrer Veeam-Umgebung hinzufügen. Navigieren Sie im Azure-Portal zu dem gerade erstellten Konto, und wählen Sie auf dem Portalblatt unter dem Menü „Blobdienst“ die Option „Container“ aus. Fügen Sie einen neuen Container hinzu, und geben Sie einen aussagekräftigen Namen an. Navigieren Sie dann in den Einstellungen zu „Zugriffsschlüssel“, und kopieren Sie den Namen des Speicherkontos und einen der beiden Zugriffsschlüssel. In den nächsten Schritten benötigen Sie den Namen des Containers, den Namen des Kontos und den Zugriffsschlüssel.

    ![Erstellen eines Containers](../media/container-b.png)
    
    ![Kontoinformationen erfassen](../media/access-key.png)
    
    > [!Note]
Veeam Backup and Replication bietet zusätzliche Optionen zum Herstellen einer Verbindung mit Azure. Für den Anwendungsfall in diesem Artikel mit Microsoft Azure Blob Storage als Sicherungsziel wird die oben beschriebene Methode als Best Practice empfohlen.

7. ***(Optional)*** Sie können Ihrer Bereitstellung zusätzliche Sicherheitsschichten hinzufügen.

     1. Konfigurieren Sie den rollenbasierten Zugriff, um den Kreis der Personen einzuschränken, die Änderungen an Ihrem Speicherkonto vornehmen dürfen. [Weitere Informationen finden Sie hier](https://docs.microsoft.com/azure/storage/common/authorization-resource-provider?toc=/azure/storage/blobs/toc.json)

    1. Schränken Sie den Kontozugriff mithilfe der [Storage-Firewall](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal) auf bestimmte Netzwerksegmente ein, um Zugriffe von Punkten außerhalb Ihres Unternehmensnetzwerks zu verhindern.

    ![Storage-Firewall](../media/storage-firewall.png) 

    1. Legen Sie eine [Löschsperre](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources) für das Storage-Konto fest, um ein versehentliches Löschen des Kontos zu verhindern.

    ![Ressourcensperre](../media/resource-lock.png), Teil 1 Konfigurieren Sie [zusätzliche Best Practices für die Sicherheit](https://docs.microsoft.com/azure/storage/blobs/security-recommendations). 
8. Navigieren Sie in der Veeam Backup and Replication Management Console zu „Backup Infrastructure“ (Sicherungsinfrastruktur), klicken Sie mit der rechten Maustaste auf den Übersichtsbereich, und wählen Sie „Add Backup Repository“ (Sicherungsrepository hinzufügen) aus, um den Konfigurations-Assistenten zu öffnen. Wählen Sie im Dialogfeld „Object Storage“ (Objektspeicher) nacheinander „Microsoft Azure Blob Storage“ und „Azure Blob Storage“ aus.
    
    ![Veeam-Repository-Assistent, Bildschirm a](../media/veeam-repo-a.png)

    ![Veeam-Repository-Assistent, Bildschirm b](../media/veeam-repo-b.png)

    ![Veeam-Repository-Assistent, Bildschirm c](../media/veeam-repo-c.png)

9. Geben Sie als Nächstes einen Namen und eine Beschreibung für Ihr neues Microsoft Azure-Blob-Repository ein.
    
    ![Veeam-Repository-Assistent, Bildschirm d](../media/veeam-repo-d.png)

10. Im nächsten Schritt fügen Sie die Anmeldeinformationen für den Zugriff auf Ihr Azure Storage-Konto hinzu. Wählen Sie im Cloud Credential Manager die Option „Microsoft Azure Storage Account“ (Microsoft Azure Storage-Konto) aus, und geben Sie den Namen des Storage-Kontos und den Zugriffsschlüssel ein. Wählen Sie als Region „Azure Global“ und ggf. einen Gatewayserver aus.
    
    ![Veeam-Repository-Assistent, Bildschirm e](../media/veeam-repo-e.png)

> [!Note]
Wenn Sie keinen Veeam-Gatewayserver verwenden möchten, stellen Sie sicher, dass alle aufskalierten Repositoryerweiterungen über direkten Internetzugriff verfügen.

11. Wählen Sie im Containerregister Ihren Azure Storage-Container aus. Wählen Sie dann einen Ordner aus, in dem die Sicherungen gespeichert werden sollen, oder erstellen Sie einen neuen Ordner. Sie können auch eine „weiche“ Obergrenze für die Gesamtspeicherkapazität definieren, die von Veeam verwendet werden darf (empfohlen). Überprüfen Sie die angezeigten Informationen in der Zusammenfassung, und schließen Sie das Konfigurationstool ab. Das neue Repository kann jetzt bei der Konfiguration von Sicherungsaufträgen ausgewählt werden.

    ![Veeam-Repository-Assistent, Bildschirm f](../media/veeam-repo-f.png)
    
    ![Veeam-Repository-Assistent, Bildschirm g](../media/veeam-repo-g.png)

### <a name="azure-alerting-and-performance-monitoring"></a>Warnungen und Leistungsüberwachung in Azure

Es ist ratsam, sowohl Ihre Azure-Ressourcen also auch die Fähigkeit von Veeam zur Nutzung dieser Ressourcen zu überwachen – genauso wie bei jedem anderen Speicherziel auch, auf das Sie sich bei der Speicherung Ihrer Sicherungen verlassen. Mit einer Kombination aus Azure Monitor und den Überwachungsfunktionen von Veeam (Registerkarte mit Statistiken im Auftragsknoten der Veeam Management Console oder ausführlichere Optionen wie Veeam One Reporter) können Sie die Integrität und den Schutz Ihrer Umgebung sicherstellen.

#### <a name="microsoft-azure-portal"></a>Microsoft Azure-Portal

Mit [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource) bietet Microsoft Azure eine stabile Überwachungslösung. Sie können [Azure Monitor konfigurieren](https://docs.microsoft.com/azure/storage/common/monitor-storage?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-powershell#configuration), um die Kapazität, Transaktionen, Verfügbarkeit, Authentifizierung und viele weitere Aspekte von Azure Storage nachzuverfolgen. Eine vollständige Referenz der nachverfolgten Metriken finden Sie [hier](https://docs.microsoft.com/azure/storage/common/monitor-storage-reference). Folgende Metriken sind nützlich: BlobCapacity zum Sicherstellen, dass das [Kapazitätslimit des Storage-Kontos](https://docs.microsoft.com/azure/storage/common/scalability-targets-standard-account) nicht überschritten wird; der eingehende und ausgehende Datenverkehr zum Nachverfolgen der Menge an Daten, die in Ihr Azure Storage-Konto geschrieben und daraus gelesen werden; SuccessE2ELatency zum Nachverfolgen der Roundtripzeit für Anforderungen an und von Azure Storage und Ihrem Medien-Agent. 

Sie können auch [Protokollwarnungen](https://docs.microsoft.com/azure/service-health/alerts-activity-log-service-notifications) erstellen, um die Integrität des Azure Storage-Diensts nachzuverfolgen, und sich jederzeit das [Azure-Statusdashboard](https://status.azure.com/status) ansehen.

#### <a name="veeam-reporting"></a>Berichterstellung in Veeam

[Configuring Veeam One Reporting](https://helpcenter.veeam.com/docs/one/reporter/configure_reporter.html?ver=100) (Konfigurieren der Berichterstellung in Veeam One)

[Veeam Backup and Replication Alarms](https://helpcenter.veeam.com/docs/one/monitor/backup_alarms.html?ver=100) (Warnungen von Veeam Backup and Replication) 

### <a name="how-to-open-support-cases"></a>Eröffnen von Supportfällen

Wenn Sie Unterstützung bei Ihrer Lösung für die Sicherung in Azure benötigen, empfiehlt es sich, sowohl bei Veeam als auch bei Azure einen Supportfall zu eröffnen, damit unsere Supportorganisationen bei Bedarf zusammenarbeiten können.

#### <a name="how-to-open-a-case-with-veeam"></a>So eröffnen Sie einen Supportfall bei Veeam

Navigieren Sie zum [Veeam Customer Support Portal](https://www.veeam.com/support.html) (Veeam-Portal für den Kundensupport), melden Sie sich an, und eröffnen Sie einen Fall.

Informationen zu den Supportoptionen, die Ihnen von Veeam zur Verfügung gestellt werden, finden Sie in der [Veeam Customer Support Policy](https://www.veeam.com/veeam_software_support_policy_ds.pdf) (Richtlinie für den Veeam-Kundensupport)

Sie können einen Fall auch telefonisch eröffnen:

[Weltweite Supporttelefonnummern](https://www.veeam.com/contacts.html?ad=in-text-link#support-numbers)

#### <a name="how-to-open-a-case-with-the-azure-support-team"></a>So eröffnen Sie einen Fall beim Azure-Supportteam

Suchen Sie im [Azure-Portal](https://portal.azure.com) oben in der Suchleiste nach „Support“, und wählen Sie „+ Neue Supportanfrage“ aus. 
> [!Note]
Geben Sie beim Eröffnen des Falls an, dass Sie Unterstützung für „Azure Storage“ oder „Azure-Netzwerk“ benötigen, **NICHT** für „Azure Backup“. Azure Backup ist ein nativer Dienst von Microsoft Azure, und Ihr Fall würde falsch weitergeleitet.

### <a name="links-to-relevant-veeam-documentation"></a>Links zu relevanter Veeam-Dokumentation

Veeam-Dokumentation mit weiteren Informationen:

[Veeam User Guide](https://helpcenter.veeam.com/docs/backup/hyperv/overview.html?ver=100) (Veeam-Benutzerhandbuch)

[Veeam Architecture Guide](https://helpcenter.veeam.com/docs/backup/vsphere/backup_architecture.html?ver=100) (Veeam-Architekturleitfaden)

### <a name="link-to-marketplace-offering"></a>Link zum Marketplace-Angebot

Sie können zum Schützen Ihrer in Azure ausgeführten Workloads auch weiterhin die Veeam-Lösung verwenden, die Sie kennen und der Sie vertrauen. Veeam hat die Bereitstellung der Veeam-Lösung in Azure zum Schutz von Azure-VMs und vielen weiteren Azure-Diensten sehr einfach gestaltet.

[Bereitstellen von Veeam Backup and Replication über den Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/veeam.veeam-backup-replication?tab=overview)

[Azure-Datenblatt](https://www.veeam.com/backup-azure.html?ad=menu-products)


## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie auf den folgenden externen Websites weitere Ressourcen, um Informationen zu speziellen Nutzungsszenarien zu erhalten:

[Veeam How to Videos](https://www.veeam.com/how-to-videos.html?ad=menu-resources) (Anleitungsvideos von Veeam)

[Veeam Technical Documentations](https://www.veeam.com/documentation-guides-datasheets.html?ad=menu-resources) (Technische Dokumentation von Veeam)

[Veeam Knowledge Base and FAQ](https://www.veeam.com/knowledge-base.html?ad=menu-resources) (Wissensdatenbank und häufig gestellte Fragen von Veeam)
