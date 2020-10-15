---
title: Erstellen eines Migrationsplans mit Azure Migrate | Microsoft-Dokumentation
description: Enthält Anleitungen zum Erstellen eines Migrationsplans mit Azure Migrate.
author: rashi-ms
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/21/2020
ms.author: rajosh
ms.openlocfilehash: db1de363856fd560fea97f8f9cdf542717c4cca3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87090092"
---
# <a name="build-migration-plan-with-azure-migrate"></a>Erstellen eines Migrationsplans mit Azure Migrate

Dieser Artikel bietet eine kurze Anleitung zum Erstellen Ihres Migrationsplans in Azure mit [Azure Migrate](migrate-services-overview.md). Wenn Sie weitere Fragen haben, sehen Sie sich die folgenden Ressourcen an:

- [Allgemeine Fragen](resources-faq.md) zu Azure Migrate
- Fragen zur [Azure Migrate-Appliance](common-questions-appliance.md)
- Fragen zur [Servermigration](common-questions-server-migration.md)
- Antworten auf Fragen im [Azure Migrate-Forum](https://aka.ms/AzureMigrateForum)

## <a name="define-the-goals-of-cloud-migration"></a>Definieren der Ziele der Cloudmigration

Bevor ein Migrationsplan ausgearbeitet wird, ist es wichtig, die [Motivation](/azure/cloud-adoption-framework/strategy/motivations) für den Umstieg in die Cloud, die zu erfolgreicheren Geschäftsergebnissen beitragen kann, zu verstehen und zu bewerten. Wie im [Cloud Adoption Framework für Azure](/azure/cloud-adoption-framework) erklärt wird, könnte es verschiedene Auslöser und Migrationsansätze geben, die für Ihr Unternehmen geeignet sind:  

**Wichtige Unternehmensereignisse** | **Ergebnis der Migration**
--- | ---
Ausstieg aus Rechenzentrum | Kostenersparnis
Fusionen, Übernahme oder Veräußerung | Verringerung der Anbieter- oder technischen Komplexität
Reduzierung der Kapitalkosten | Optimierung interner Vorgänge
Ende des Supports für unternehmenskritische Technologien | Erhöhen der Unternehmensflexibilität
Reaktion auf Änderungen bezüglich der Einhaltung gesetzlicher Bestimmungen | Vorbereitung auf neue technische Funktionen und Möglichkeiten
Neue Anforderungen an die Datenhoheit | Skalierung entsprechend den Marktanforderungen
Verringerung von Störungen und Verbesserung der IT-Stabilität | Skalierung entsprechend den geografischen Anforderungen

Ihre Migrationsmotivation kann Ihnen auch helfen, über die strategischen Ziele und Ergebnisse nachzudenken, die Sie mit einer Migration zu Azure erreichen möchten. Der nächste Schritt ist die Ermittlung und Planung eines Migrationspfades zu Azure, der auf Ihre Workloads zugeschnitten ist. Von der Azure Das Serverbewertungstool hilft Ihnen bei der Bewertung lokaler Workloads und stellt Anleitungen und Tools zur Verfügung, die Sie bei der Migration unterstützen.

## <a name="understand-your-digital-estate"></a>Verstehen Ihrer digitalen Ressourcen

Beginnen Sie damit, Ihre lokalen Infrastrukturen, Anwendungen und Abhängigkeiten zu verstehen, um die Workloads zu ermitteln, die Sie zu Azure migrieren möchten, und um optimierte Kostenprognosen zu erhalten. Das Serverbewertungstool hilft Ihnen dabei, die folgenden Fragen zu beantworten:

### <a name="what-workloads-are-in-use"></a>Welche Workloads werden verwendet?

Verwenden Sie die einfache Azure Migrate-Appliance, um ohne Agent eine Ermittlung Ihrer lokalen VMware-VMs, Hyper-V-VMs und physischen Servern auszuführen. Die kontinuierliche Ermittlung sammelt Konfigurations- und Leistungsmetadaten des Computers und kann auch verwendet werden, um den Bestand der auf Ihren lokalen Computern installierten und ausgeführten Anwendungen und Rollen/Features zu erhalten. Die Azure Migrate-Appliance sammelt:

- Metadatendetails zu den Computern, Datenträgern und NICs

- Installierte Anwendungen, einschließlich Anwendungen und Rollen/Features  

- Leistungsdaten, einschließlich CPU- und Arbeitsspeicherauslastung, Datenträger-IOPS und Durchsatz

Exportieren Sie als nächstes die Anwendungsbestandsliste, um alle SQL Server-Instanzen zu ermitteln, die auf Ihren Workloads ausgeführt werden, und verwenden Sie das Tool „Azure Migrate: Datenbankbewertung“ zum Verständnis der Bereitschaft.

 ![Anwendungsbestand im Portal](./media/concepts-migration-planning/application-inventory-portal.png)

 ![Export des Anwendungsbestands](./media/concepts-migration-planning/application-inventory-export.png)

Verwenden Sie zusammen mit den Ermittlungsdaten des Serverbewertungstools Ihre vorhandenen CMDB-Daten, um unsere Sicht auf Ihre Server- und Datenbankressourcen zu erstellen und die Serververteilung über Geschäftseinheiten, Anwendungseigentümer, geografische Regionen usw. zu verstehen, was wiederum bei der Priorisierung der zu migrierenden Workloads helfen kann.

### <a name="what-dependencies-exist-between-workloads"></a>Welche Abhängigkeiten bestehen zwischen Workloads?

Sobald Sie Ihre Server ermitteln haben, verwenden Sie die Abhängigkeitszuordnung ohne Agent, um serverübergreifende Abhängigkeiten und Optimierungsstrategien für die Verlagerung voneinander abhängiger Server nach Azure zu visualisieren und zu identifizieren. Die Visualisierung hilft zu verstehen, ob bestimmte Computer verwendet werden, oder ob sie stillgelegt werden können, anstatt migriert zu werden.  Analysieren Sie die Abhängigkeiten, um sicherzustellen, dass nichts übersehen wird, und unerwartete Ausfälle während der Migration zu vermeiden. Sobald Sie die Anwendungsinventarisierung und Abhängigkeitszuordnung durchgeführt haben, können Sie Gruppen mit hoher Vertrauenswürdigkeit erstellen und mit der Bewertung Ihrer Server beginnen.

 ![Zuordnung der Abhängigkeiten](./media/concepts-migration-planning/expand-client-group.png)

### <a name="are-they-optimized-and-sized-properly"></a>Sind sie optimiert und weisen die richtige Größe auf?

Da Azure die Flexibilität bietet, die Größe Ihrer Cloudkapazität im Laufe der Zeit zu ändern, bietet die Migration Ihnen eine Gelegenheit, die Ihren Servern zugewiesenen CPU- und Arbeitsspeicherressourcen zu optimieren. Erstellen Sie eine Bewertung der zuvor identifizierten Gruppe, um den Leistungsverlauf der Workloads zu verstehen, was bei Empfehlungen für größenangepasste VM-SKUs und Datenträger in Azure sehr wichtig sein wird.

## <a name="assess-your-readiness-for-migration"></a>Bewerten Ihrer Bereitschaft für die Migration

### <a name="readiness-and-suitability-analysis-for-azure"></a>Bereitschafts- und Eignungsanalyse für Azure
Exportieren Sie den Azure VM-Bewertungsbericht, und filtern Sie ihn nach den folgenden Bereitschaftskategorien, um die VM-Bereitschaft für Azure zu verstehen:

- **Bereit für Azure**: Sie können diese Computer ohne Änderungen zu Azure migrieren.  

- **Bedingt bereit für Azure:** Sie können diese Computer zu Azure migrieren, müssen jedoch geringfügige Änderungen auf diesen Servern gemäß den in der Bewertung bereitgestellten Anleitungen zur Problembehebung vornehmen.

- **Nicht bereit für Azure**: Sie können diese Computer nicht in der vorliegenden Form zu Azure migrieren und müssen die Probleme vor der Migration gemäß der Anleitung zur Problembehebung beseitigen.

- **Bereitschaft unbekannt**: Azure Migrate kann die Bereitschaft des Computers aufgrund unzureichender Metadaten nicht ermitteln.

Mithilfe der Datenbankbewertungen können Sie die Bereitschaft für die Migration Ihrer SQL Server-Datenbestände zu Azure SQL-Datenbank oder Azure SQL Managed Instances beurteilen. Für jede Ihrer SQL Server-Instanzen wird der prozentuale Status der Bereitschaft für die Migration angezeigt. Außerdem werden für jede dieser Instanzen das empfohlene Ziel in Azure, potenzielle Migrationsblocker, die Anzahl von wichtigen Änderungen, die Bereitschaft für Azure SQL-Datenbank/Azure SQL-VM und der Kompatibilitätsgrad angezeigt. Sie können sich eingehender mit den Auswirkungen der Migrationsblocker und Empfehlungen zu deren Behebung vertraut machen.

 ![Datenbankbewertungen](./media/concepts-migration-planning/database-assessment-portal.png)

### <a name="sizing-recommendations"></a>Größenempfehlungen

Nachdem ein Computer als bereit für Azure markiert wurde, werden bei der Serverbewertung Größenempfehlungen zur Identifizierung des virtuellen Azure-Computers und der Festplatten-SKU für Ihre VMs angegeben. Sie können wählen, ob die Größenempfehlung auf der Grundlage des Leistungsverlaufs (um die Ressourcen bei der Migration zu optimieren) oder auf der Grundlage ihrer lokalen Konfiguration ohne Berücksichtigung des Leistungsverlaufs angezeigt werden soll. Für Datenbanken können Sie die Empfehlungen zu Datenbank-SKU, Preisstufe und Computeebene in Ihrer Datenbankbewertung einsehen.  

### <a name="compute-assessments-to-get-estimated-costs-for-running-the-workloads-in-azure"></a>Computebewertungen, um geschätzte Kosten für die Ausführung der Workloads in Azure zu erhalten

Mit der *leistungsbasierten* Größenanpassungsoption in Bewertungen können Sie Workloads für Azure optimieren. Zusätzlich zur Größenanpassung gibt es nur wenige andere Möglichkeiten, Kosten zu sparen:

- **Reservierte Instanzen**: Mit reservierten Instanzen können Sie die Kosten im Vergleich zu nutzungsbasierten Tarifen mit Laufzeiten von einem oder drei Jahren unter Windows und Linux (VMs) erheblich verringern.

- **Azure-Hybridvorteile**: Sie können lokale Windows Server-Lizenzen mit Software Assurance in Azure portieren und mit den Optionen für reservierte Instanzen kombinieren.

- **Enterprise Agreement-Angebote (EA)** : Enterprise Agreement bietet integrierte Einsparungen, die für Ihr Abonnement gelten.

- **Angebote**: Es gibt mehrere Azure-Angebote, z. B. Dev/Test Pay-As-You-Go und Enterprise Dev/Test mit niedrigeren Tarifen für Entwicklungs- oder Test-VMs.

- **VM-Betriebszeit**: Sie können die Zeiten, an denen die Azure-VMs ausgeführt werden, in Tagen pro Monat und Stunden pro Tag angeben, um Kosten zu senken (gilt nicht für RI).

- **Target Region** (Zielregion): Sie können mehrere Bewertungen in verschiedenen Regionen erstellen, um zu vergleichen, ob die Migration in eine bestimmte Region in einer Geografie kostengünstiger sein könnte.

- **Leistungsbasierte Empfehlungen**: Als bewährte Methode sollten Sie die Empfehlungen für größenangepasste Azure-VMs verwenden, um Cloudkosten zu sparen.

### <a name="visualize-data"></a>Visualisieren von Daten

Sie können den Serverbewertungsbericht mit Azure-Bereitschaft und monatlicher Kostenverteilung im Portal anzeigen sowie die Bewertung exportieren, um weitere Visualisierungen auf die Ermittlungs- und Bewertungsdaten anzuwenden und den Migrationsplan reichhaltiger zu gestalten. Sie können mehrere Bewertungen für verschiedene Kombinationen von Eigenschaften erstellen und den Satz von Eigenschaften auswählen, der für Ihr Unternehmen am besten geeignet ist.  

 ![Bewertungsübersicht](./media/concepts-migration-planning/assessment-summary.png)

### <a name="evaluate-gaps-and-potential-blockers"></a>Auswerten von Lücken und potenziellen Blockern

Identifizieren Sie bei der Bestimmung der zu migrierenden Anwendungen und der zugrunden liegenden Infrastruktur die Ausfallzeitbeschränkungen für diese Anwendungen, und suchen Sie nach möglichen betrieblichen Abhängigkeiten zwischen Ihren Anwendungen und der zugrunde liegenden Infrastruktur. Diese Analyse kann Ihnen bei der Planung von Migrationen helfen, die Ihre Wiederherstellungszeit-Zielsetzung (RTO) erfüllen und minimalen bis keinen Datenverlust gewährleisten. Bevor Sie migrieren, sollten Sie Kompatibilitätsprobleme oder nicht unterstützte Features, die die Migration Ihrer Server und SQL-Datenbanken blockieren könnten, mithilfe des Serverbewertungsberichts und der Azure Migrate- Empfehlungen für die Datenbankbewertung überprüfen und abschwächen.

### <a name="first-workloads-to-target-and-approach"></a>Erste Workloads als Ziele und Ansatz

Da Sie nun über alle wichtigen Informationen verfügen, um Ihre Migrationsentscheidung zu treffen, sollten Sie Prioritäten setzen, welche Anwendungen und Workloads zuerst migriert werden sollten. Entwickeln Sie einen „Anwenden und Lernen“-Ansatz, um Ihre beabsichtigten Anwendungen systematisch und kontrollierbar zu migrieren, sodass Sie alle Mängel in dieser Strategie ausmerzen können, bevor Sie mit einer umfassenden Migration beginnen. Sie können auch strategische Faktoren wie Komplexität und Migrationszeit, geschäftliche Dringlichkeit, Produktions-/Nichtproduktionsumgebung, Compliance- und Sicherheitsanforderungen, Anwendungswissen usw. nutzen, um die zu migrierenden Anwendungsgruppen zu priorisieren.

Einige empfohlene Migrationsstrategien sind:

- **Priorisieren schneller Erfolge**: Sie können die Bewertungsberichte verwenden, um „niedrig hängende Früchte“ zu ermitteln, beispielsweise die Server und Datenbanken, die vollständig bereit sind und nur minimalen Aufwand für die Migration zu Azure erfordern:
    - Bereit für Azure: Exportieren Sie Ihren Bewertungsbericht, und filtern Sie alle Computer heraus, die für Azure bereit sind. Dabei kann es sich um die erste Gruppe von Computern handeln, für die Sie das Lift & Shift mit dem Tool „Azure Migrate: Servermigration“ ausführen.
    - Ende des Betriebssystemsupports: Exportieren Sie den Bewertungsbericht, und filtern Sie alle Computer heraus, auf denen die Betriebssysteme Windows Server 2008 und Windows Server 2008 R2 ausgeführt werden. Für diese SKUs endet der Support, und nur Azure bietet Ihnen 3 Jahre lang kostenlose Sicherheitsupdates, wenn Sie zu Azure migrieren. Wenn Sie Azure-Hybridvorteile kombinieren und reservierte Instanzen verwenden, könnte die Einsparung viel höher sein.
    - SQL Server-Migration: Verwenden Sie die Datenbankbewertungsempfehlungen, um die Datenbanken, die für Azure SQL-Datenbanken bereit sind, mithilfe von „Azure Migrate: Datenbankmigration“ zu migrieren, und Datenbanken, die für Azure SQL-VM bereit sind, mithilfe von Azure Migrate: Servermigration“ zu migrieren.
    - Ende des Softwaresupports: Exportieren Sie Ihren Anwendungsbestand, und filtern Sie alle Software/Erweiterungen heraus, die das Ende des Supports erreichen könnten. Diese Anwendungen sollten Sie priorisieren.
    - Überdimensionierte VMs: Exportieren Sie Ihren Bewertungsbericht, und filtern Sie Computer mit geringer CPU-Auslastung (%) und Arbeitsspeicherauslastung (%) heraus.  Sie können diese Gelegenheit nutzen, um zu einer größenangepassten VM in Azure zu migrieren und das zu sparen, was Sie für nicht ausgelastete Ressourcen bezahlt haben.
    - Kapazitätseinschränkungen: Exportieren Sie Ihren Bewertungsbericht, und filtern Sie Computer mit hoher CPU-Auslastung (%) und Arbeitsspeicherauslastung (%) heraus.  Indem Sie überlastete VMs zu Azure migrieren und die automatische Skalierungsfunktion nutzen, um größenangepasst den Bedarf zu decken, können Sie verhindern, dass diese VMs zusammenbrechen und gleichzeitig die Leistung steigern. Sie können sich auch den Bewertungsbericht ansehen, um Ihre Speichereinschränkungen zu verstehen, indem Sie die Datenträger-IOPS und den Durchsatz analysieren und den empfohlenen Datenträgertyp ermitteln, der Ihren Anforderungen am besten entspricht.

- **Fangen Sie klein an, und kommen Sie dann groß raus**: Beginnen Sie mit der Verlagerung von Anwendungen und Workloads, die ein minimales Risiko darstellen und weniger komplex sind, um Vertrauen in Ihre Migrationsstrategie zu schaffen. Sie könnten auch Ihre Azure Migrate-Bewertungsempfehlungen mit dem CMDB-Repository Ihres Unternehmens verknüpfen, um Dev/Test-Umgebungsworkloads in Ihren Pilotmigrationen zu finden und zu migrieren. Die Erkenntnisse dieser Pilotprojekte können bei der Migration von Produktionsworkloads genutzt werden.  

- **Erfüllen rechtlicher und branchenspezifischer Anforderungen**: Azure verwaltet das größte Kompatibilitätsportfolio der gesamten Branche, was die Breite und Tiefe des Angebots betrifft. Nutzen Sie dies als eine Gelegenheit, Ihre Migrationen zu Azure zu priorisieren und Ihre nationalen, regionalen und branchenspezifischen Normen und Gesetze einzuhalten. Dies gilt insbesondere für Organisationen, die mit geschäftskritischen oder sensiblen Informationen umgehen oder in stark regulierten Branchen tätig sind, in denen eine große Menge von Normen und Vorschriften gelten, die sich in bestimmten Fällen häufig ändern können, sodass es schwierig ist, mit ihnen Schritt zu halten.  

## <a name="finalize-the-migration-planandprepare-formigration"></a>Abschließen des Migrationsplans und Vorbereiten der Migration

Bevor Sie Ihren Migrationsplan abschließen, stellen Sie sicher, dass diese wichtigen Migrationserwägungen Ihre Migrationsplanung nicht behindern:

- Bewerten Sie Einschränkungen der Netzwerkbandbreite und -latenz, die zu unvorhergesehenen Verzögerungen führen und die Replikationsgeschwindigkeit der Migration stören können.

- Puffern Sie rechtzeitig, um Leistungs- und Benutzerakzeptanztests für die migrierten Anwendungen durchzuführen oder um nach der Migration App-Anpassungen vorzunehmen, wie z. B. die Aktualisierung von Datenbank-Verbindungszeichenfolgen und Webserverkonfigurationen, die Durchführung von Umstellungs- und Bereinigungstests usw.

- Überprüfen Sie die empfohlenen Azure-Berechtigungen und das für die Migration erforderliche Rollen- und Berechtigungsmodell für den Server-/Datenbankzugriff.

- Bereiten Sie Ihre Organisation vor, und stellen Sie sicher, dass die Mitarbeiter auf die digitale Umstellung vorbereitet sind. Eine solide Schulungsgrundlage ist wichtig für eine erfolgreiche Organisationsänderung. Informieren Sie sich über das kostenlose Training, das auf  [Microsoft Learn](/learn/azure/?ocid=CM_Discovery_Checklist_PDF) verfügbar ist, beispielsweise Kurse zu Azure-Grundlagen, Lösungsarchitektur und Sicherheit. Ermutigen Sie Ihr Team, sich auch über die  [Azure-Zertifizierung](https://www.microsoft.com/learning/certification-overview.aspx?ocid=CM_Discovery_Checklist_PDF) zu informieren.  

- Erhalten Sie ggf. Unterstützung für Ihre Implementierung. Viele Organisationen holen sich externe Unterstützung für ihre Cloudmigration. Für einen schnellen und zuverlässigen Wechsel zu Azure mit persönlicher Unterstützung ziehen Sie einen  [Azure Expert MSP](https://www.microsoft.com/solution-providers/search?cacheId=9c2fed4f-f9e2-42fb-8966-4c565f08f11e&ocid=CM_Discovery_Checklist_PDF)  oder  [Microsoft FastTrack for Azure](https://azure.microsoft.com/programs/azure-fasttrack/?ocid=CM_Discovery_Checklist_PDF) in Erwägung.  

Erstellen Sie einen effektiven Cloudmigrationsplan, der detaillierte Informationen über die Liste/Gruppen von Anwendungen, die Sie migrieren möchten, über die Anforderungen an Verfügbarkeit und Ausfallzeiten für Ihre Apps und Datenbanken sowie über die gewünschten Migrationsmeilensteine enthält. Ihr Migrationsplan sollte auch berücksichtigen, wie lange das Kopieren der Daten dauert, und einen angemessenen Puffer für Tests und Umstellungsaktivitäten nach der Migration vorsehen. Die Tests nach der Migration müssen Funktions-, Integrations-, Sicherheits- und Leistungstest-Anwendungsfälle umfassen, um sicherzustellen, dass die migrierten Anwendungen wie erwartet funktionieren und alle Datenbankobjekte und Datenbeziehungen erfolgreich in die Cloud übertragen wurden.  

Verwenden Sie diese Analyse, um eine Migrationsroadmap zu erstellen und ein Wartungsfenster zu deklarieren, um Ihre Anwendungen und Datenbanken mit minimalen oder ohne Ausfallzeiten zu migrieren und die potenziellen betrieblichen/geschäftlichen Auswirkungen während der Migration zu begrenzen.  

Wir empfehlen Ihnen, immer die Azure Migrate-Funktion  *Testmigration* zum Testen und Fortfahren zu verwenden, bevor Sie mit der vollständigen Migration zu Azure beginnen. Diese Echtdaten helfen Ihnen, den tatsächlich erforderlichen Zeitaufwand abzuschätzen und notwendige Anpassungen an Ihrem Migrationsplan vorzunehmen. Die Testmigration bietet zudem die Möglichkeit, potenzielle Probleme zu mit dem Migrationsplan zu ermitteln und zu beheben, bevor die tatsächliche Migration erfolgt.  

Sobald Sie bereit für die Migration sind, verwenden Sie das *Servermigrationstool* und den *Datenmigrationsdienst* von Azure Migrate für eine nahtlose und integrierte Migrationserfahrung mit Ende-zu-Ende-Nachverfolgung. Das Servermigrationstool unterstützt die Migration von VMs und Servern, die lokal im Rechenzentrum des Kunden oder in einer anderen privaten oder öffentlichen Cloud, z. B. AWS, GCP usw., gehostet werden, ohne dass es zu Ausfallzeiten kommt. Azure Database Migration Service ist ein vollständig verwalteter Dienst, der die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen mit minimaler Downtime ermöglicht.  

> [!NOTE]
> Für VMware-VMs verwendet die Serverbewertung das für die VM in vCenter Server angegebene Betriebssystem, um die Gastbetriebssystemanalyse durchzuführen. Für Linux-VMs, die unter VMware ausgeführt werden, wird die Kernelversion des Gastbetriebssystems derzeit nicht genau identifiziert.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich den Artikel zur  [Cloudmigration](/azure/architecture/cloud-adoption/getting-started/migrate)  des Frameworks für die Cloudeinführung (Cloud Adoption Framework) an.
- [Erste Schritte](https://youtu.be/wFfq3YPxYHE) mit Azure Migrate.
- Erstellen Sie eine Bewertung für [VMware-VMs](tutorial-assess-vmware.md) oder [Hyper-V-VMs](tutorial-assess-hyper-v.md).
