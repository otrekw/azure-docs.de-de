---
title: Übersicht – Azure Database for MySQL Flexible Server
description: Erfahren Sie mehr zu Azure Database for MySQL Flexible Server, einem relationalen Datenbankdienst in der Microsoft-Cloud, der auf MySQL Community Edition basiert.
author: savjani
ms.service: mysql
ms.author: pariks
ms.custom: mvc, references_regions
ms.topic: overview
ms.date: 6/19/2021
ms.openlocfilehash: b13dd8cb1965399d0d8ab19891a986fe2b585d4b
ms.sourcegitcommit: 8669087bcbda39e3377296c54014ce7b58909746
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2021
ms.locfileid: "114403039"
---
# <a name="azure-database-for-mysql---flexible-server-preview"></a>Azure Database for MySQL-Server Flexible Server (Vorschau)

[[!INCLUDE[applies-to-mysql-flexible-server](../includes/applies-to-mysql-flexible-server.md)]


Azure Database for MySQL, ein Dienst, der auf MySQL Community Edition basiert, ist in zwei Bereitstellungsmodi verfügbar:

- Einzelner Server
- Flexible Server (Vorschau)

In diesem Artikel erhalten Sie eine Übersicht und Einführung in die grundlegenden Konzepte des Modells zur Bereitstellung einer Flexible Server-Instanz. Informationen dazu, wie Sie entscheiden, welche Bereitstellungsoption für Ihre Workload geeignet ist, finden Sie unter [Auswählen der richtigen MySQL Server-Option in Azure](./../select-right-deployment-type.md).

## <a name="overview"></a>Übersicht

Azure Database for MySQL Flexible Server ist ein vollständig verwalteter Datenbankdienst, der eine differenziertere Steuerung und mehr Flexibilität bei den Verwaltungsfunktionen und Konfigurationseinstellungen der Datenbank bietet. Im Allgemeinen ermöglicht der Dienst mehr Flexibilität und die Anpassung der Serverkonfiguration an die Anforderungen der Benutzer. Mit der Flexible Server-Architektur können Benutzer Hochverfügbarkeit in einer einzelnen Verfügbarkeitszone sowie in mehreren Verfügbarkeitszonen auswählen. Flexible Server-Instanzen bieten außerdem bessere Steuerelemente für die Kostenoptimierung mit der Möglichkeit, den Server und burstfähige SKUs anzuhalten/zu starten. Dies eignet sich hervorragend für Workloads, die nicht durchgehend die gesamte Computekapazität benötigen. Der Dienst unterstützt derzeit die Community-Version von MySQL 5.7 und 8.0. Der Dienst ist derzeit in der Vorschauphase und in einer Vielzahl von [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/services/) verfügbar.

Flexible Server-Instanzen eignen sich am besten für Folgendes:

- Anwendungsentwicklungen, die bessere Steuerungs- und Anpassungsmöglichkeiten erfordern.
- Zonenredundante Hochverfügbarkeit
- Verwaltete Wartungsfenster

![Flexible Server-Konzeptdiagramm](media/overview/1-flexible-server-conceptual-diagram.png) 

## <a name="high-availability-within-and-across-availability-zones"></a>Hochverfügbarkeit in Verfügbarkeitszonen und verfügbarkeitszonenübergreifend

Das Modell zur Bereitstellung flexibler Server ist so konzipiert, dass es Hochverfügbarkeit innerhalb einer einzigen Verfügbarkeitszone und in mehreren Verfügbarkeitszonen unterstützt. In der Architektur sind die Compute- und Speicherebene voneinander getrennt. Die Datenbank-Engine läuft auf einem virtuellen Linux-Computer, während sich die Datendateien im Azure-Remotespeicher vom Typ „Premium“ befinden. Der Speicher verwaltet drei lokal redundante synchrone Kopien der Datenbankdateien, sodass die Datenbeständigkeit jederzeit sichergestellt wird.

Wenn der Server in einer einzelnen Verfügbarkeitszone aufgrund geplanter oder ungeplanter Ereignisse ausfällt, gewährleistet der Dienst die Hochverfügbarkeit der Server mit dem folgenden automatisierten Verfahren:

1. Eine neue Computeressource in Form einer VM wird bereitgestellt.
2. Der Speicher mit Datendateien wird dem neuen virtuellen Computer zugeordnet.
3. Die MySQL-Datenbank-Engine wird auf dem neuen virtuellen Computer online geschaltet.
4. Clientanwendungen können erneut eine Verbindung herstellen, sobald der Server für die Annahme von Verbindungen bereit ist.

:::image type="content" source="media/overview/2-flexible-server-architecture.png" alt-text="Konzeptdiagramm einer einzelnen Hochverfügbarkeitszone":::

Wenn zonenredundante Hochverfügbarkeit konfiguriert ist, wird vom Dienst ein unmittelbar betriebsbereiter Standbyserver in der Verfügbarkeitszone innerhalb derselben Azure-Region bereitgestellt und verwaltet. Die Datenänderungen auf dem Quellserver werden synchron auf den Standbyserver repliziert, damit kein Datenverlust entsteht. Bei zonenredundanter Hochverfügbarkeit geht der Standbyserver nach dem Auslösen des geplanten oder ungeplanten Failoverereignisses sofort online und steht für die Verarbeitung eingehender Transaktionen zur Verfügung. Für gewöhnlich dauert das Failover zwischen 60 und 120 Sekunden. Dies ermöglicht dem Dienst, Hochverfügbarkeit zu unterstützen, und bietet eine verbesserte Resilienz mit Toleranz für Ausfälle einzelner Verfügbarkeitszonen in einer bestimmten Azure-Region.

Weitere Informationen finden Sie unter [Hochverfügbarkeitskonzepte](concepts-high-availability.md).

:::image type="content" source="media/overview/3-flexible-server-overview-zone-redundant-ha.png" alt-text="Konzeptdiagramm der zonenredundanten Hochverfügbarkeit":::

## <a name="automated-patching-with-managed-maintenance-window"></a>Automatisches Patchen mit verwaltetem Wartungsfenster

Der Dienst führt automatisches Patchen für die zugrunde liegende Hardware, das Betriebssystem und die Datenbank-Engine durch. Das Patchen umfasst Sicherheits- und Softwareupdates. Für die MySQL-Engine sind im Rahmen des geplanten Wartungsreleases auch kleinere Versionsupgrades enthalten. Benutzer können den Patchzeitplan so konfigurieren, dass er vom System verwaltet wird, oder einen eigenen Zeitplan definieren. Im Rahmen des Wartungszeitplans wird der Patch angewendet. Möglicherweise muss der Server als Teil des Patchprozesses neu gestartet werden, um das Update abzuschließen. Mithilfe des benutzerdefinierten Zeitplans können Benutzer ihren Patchzyklus überlegt gestalten und ein Wartungsfenster mit minimalen Auswirkungen auf das Unternehmen wählen. Im Allgemeinen gilt für den Dienst im Rahmen der kontinuierlichen Integration und Veröffentlichung ein monatlicher Releasezeitplan.

Weitere Informationen finden Sie unter [Geplante Wartung](concepts-maintenance.md). 

## <a name="automatic-backups"></a>Automatische Sicherungen

Für Azure Database for MySQL Flexible Server werden Sicherungen automatisch erstellt und in einem vom Benutzer konfigurierten lokal redundanten oder georedundanten Speicher gespeichert. Mithilfe von Sicherungen können Sie den Server zu einem beliebigen Zeitpunkt innerhalb des Aufbewahrungszeitraums der Sicherung wiederherstellen. Die Standardaufbewahrungsdauer für Sicherungen beträgt sieben Tage. Die Aufbewahrungsdauer kann optional auf bis zu 35 Tage festgelegt werden. Zur Verschlüsselung aller Sicherungen wird die AES-Verschlüsselung mit 256 Bit verwendet.

Weitere Informationen finden Sie unter [Sicherungskonzepten](concepts-backup-restore.md).

## <a name="network-isolation"></a>Netzwerkisolation

Um eine Verbindung mit Ihrer Azure Database for MySQL Flexible Server-Bereitstellung herzustellen, haben Sie zwei Netzwerkoptionen: den **privaten Zugriff** (VNET-Integration) und den **öffentlichen Zugriff (zugelassene IP-Adressen)** . 

- **Privater Zugriff (VNET-Integration):** Sie können Ihren flexiblen Server in Ihrer [Azure Virtual Network](../../virtual-network/virtual-networks-overview.md)-Instanz bereitstellen. Virtuelle Azure-Netzwerke ermöglichen eine private und sichere Netzwerkkommunikation. Ressourcen in einem virtuellen Netzwerk können über private IP-Adressen kommunizieren.

  Die VNET-Integration bietet sich an, wenn Sie die folgenden Funktionen benötigen:

  - Verbinden von Azure-Ressourcen im gleichen virtuellen Netzwerk mit Ihrem flexiblen Server über private IP-Adressen
  - Herstellen einer Verbindung von Azure-externen Ressourcen mit Ihrem flexiblen Server über ein VPN oder eine ExpressRoute-Verbindung
  - Kein öffentlicher Endpunkt

- **Öffentlicher Zugriff (zugelassene IP-Adressen)** : Sie können Ihren flexiblen Server mit einem öffentlichen Endpunkt bereitstellen. Der öffentliche Endpunkt ist eine öffentlich auflösbare DNS-Adresse. Der Ausdruck „zugelassene IP-Adressen“ bezieht sich auf einen Bereich von IP-Adressen, denen Sie die Berechtigung erteilen, auf den Server zuzugreifen. Diese Berechtigungen heißen **Firewallregeln**.

Weitere Informationen finden Sie unter [Netzwerkkonzepte](concepts-networking.md).

## <a name="adjust-performance-and-scale-within-seconds"></a>Anpassen von Leistung und Skalierung in nur wenigen Sekunden

Der Flexible Server-Dienst ist in drei SKU-Tarifen verfügbar: „Burstfähig“, „Allgemeiner Zweck“ und „Arbeitsspeicheroptimiert“. Der Tarif „Burstfähig“ eignet sich am besten für die kostengünstige Entwicklung und Workloads mit geringer Parallelität, die nicht ständig die volle Computekapazität benötigen. Die Tarife „Universell“ und „Arbeitsspeicheroptimiert“ eignen sich besser für Produktionsworkloads, die eine hohe Parallelität, Skalierung und vorhersagbare Leistung erfordern. Sie können Ihre erste App mit einer kleinen Datenbank für wenige USD im Monat erstellen und die Skalierung dann nahtlos so ändern, dass sie den Anforderungen Ihrer Lösung entspricht. Die Speicherskalierung geschieht online und unterstützt das automatische Wachstum des Speichers. Durch die dynamische Skalierung kann Ihre Datenbank transparent auf schnell wechselnde Ressourcenanforderungen reagieren. Sie zahlen nur für die Ressourcen, die Sie verwenden. 

Weitere Informationen finden Sie unter [Compute- und Speicherkonzepte](concepts-compute-storage.md).

## <a name="scale-out-your-read-workload-with-up-to-10-read-replicas"></a>Horizontale Skalierung Ihrer Leseworkload mit bis zu zehn Lesereplikaten

MySQL ist eine der beliebtesten Datenbank-Engines für die Ausführung von Web- und mobilen Anwendungen im Internet. Viele unserer Kunden verwenden es für ihre Onlinebildungsdienste, Videostreamingdienste, digitalen Zahlungslösungen, E-Commerce-Plattformen, Gamingdienste, Nachrichtenportale sowie für Websites für Behörden und das Gesundheitswesen. Diese Dienste müssen in dem Maße, wie der Datenverkehr im Web oder bei mobilen Anwendungen zunimmt, genutzt und skaliert werden.

Auf der Anwendungsseite wird die Anwendung typischerweise in Java oder PHP entwickelt und migriert, um in  [Azure-VM-Skalierungsgruppen](../../virtual-machine-scale-sets/overview.md) oder [Azure App Services](../../app-service/overview.md) ausgeführt zu werden, oder sie wird containerisiert, um unter [Azure Kubernetes Service (AKS)](../../aks/intro-kubernetes.md) ausgeführt zu werden. Mit VM-Skalierungsgruppen, App Service oder AKS als zugrunde liegende Infrastruktur wird die Anwendungsskalierung vereinfacht, indem neue virtuelle Computer sofort bereitgestellt und die zustandslosen Komponenten von Anwendungen repliziert werden, um die Anforderungen zu erfüllen, aber oft wird die Datenbank als zentrale zustandsbehaftete Komponente zum Engpass.

Mithilfe des Lesereplikatfeatures können Sie Daten von einer Azure Database for MySQL Flexible Server-Instanz auf einem schreibgeschützten Server replizieren. Sie können vom Quellserver auf **bis zu zehn Replikate** replizieren. Replikate werden asynchron mithilfe des auf der [Position der nativen, binären Protokolldatei (binlog) basierenden Replikationsverfahrens](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html) der MySQL-Engine aktualisiert. Sie können eine Proxylösung für den Lastenausgleich wie [ProxySQL](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042) verwenden, um Ihre Anwendungsworkload nahtlos horizontal auf Lesereplikate zu skalieren, ohne dass Refactoringkosten anfallen. 

Weitere Informationen finden Sie unter [Lesereplikatkonzepte](concepts-read-replicas.md).

## <a name="setup-hybrid-or-multi-cloud-data-synchronization-with-data-in-replication"></a>Einrichten der Hybrid- oder Multi-Cloud-Datensynchronisierung mit Datenreplikation

Die Datenreplikation ermöglicht das Synchronisieren von Daten von einem externen MySQL-Server mit dem Dienst Azure Database for MySQL Flexible. Der externe Server kann lokal, in virtuellen Computern, in Azure Database for MySQL Single Server oder in einem Datenbankdienst vorhanden sein, der von anderen Cloudanbietern gehostet wird. Die Datenreplikation basiert auf der Position der binären Protokolldatei (binlog). Zu den wichtigsten Szenarien, bei denen die Verwendung der Datenreplikation infrage kommt, zählen Folgende:
* Synchronisierung von Hybriddaten
* Synchronisierung von Daten in verschiedenen Clouds
* Migration zu Flexible Server mit minimaler Ausfallzeit

Weitere Informationen finden Sie unter [Datenreplikationskonzepte](concepts-data-in-replication.md).


## <a name="stopstart-server-to-optimize-cost"></a>Anhalten/Starten des Servers zum Optimieren der Kosten

Der Flexible Server-Dienst ermöglicht Ihnen, Server bei Bedarf anzuhalten und zu starten, um Ihre Kosten zu optimieren. Die Abrechnung des Computetarifs wird sofort beendet, sobald der Server angehalten wird. Dies kann Ihnen erhebliche Kosteneinsparungen bei Entwicklung, Tests und zeitgebundenen, vorhersehbaren Produktionsworkloads ermöglichen. Der Server verbleibt sieben Tage im angehaltenen Zustand, es sei denn, er wird früher wieder gestartet.

Weitere Informationen finden Sie unter [Serverkonzepte](concept-servers.md).

## <a name="enterprise-grade-security-and-privacy"></a>Sicherheit und Datenschutz auf Unternehmensniveau

Der flexible Serverdienst nutzt das FIPS 140-2-zertifizierte Kryptografiemodul für die Speicherverschlüsselung ruhender Daten. Daten wie Sicherungen und temporäre Dateien, die während der Ausführung von Abfragen erstellt wurden, werden verschlüsselt. Der Dienst verwendet das in der Azure Storage-Verschlüsselung enthaltene AES-256-Bit-Verschlüsselungsverfahren, wobei die Schlüssel (standardmäßig) vom System verwaltet werden.

Der Dienst verschlüsselt Daten während der Übertragung mit der Transport Layer Security, was standardmäßig erzwungen wird. Flexible Server unterstützt nur verschlüsselte Verbindungen mit Transport Layer Security (TLS 1.2), und alle eingehenden Verbindungen mit TLS 1.0 und TLS 1.1 werden verweigert.

Weitere Informationen finden Sie unter [Verwenden verschlüsselter Verbindungen mit flexiblen Servern](https://docs.mongodb.com/manual/tutorial/configure-ssl).

Flexible Server ermöglicht den vollständigen privaten Zugriff auf die Server mithilfe der Integration des [virtuellen Azure-Netzwerks](../../virtual-network/virtual-networks-overview.md) (VNET). Server im virtuellen Azure-Netzwerk können nur über private IP-Adressen erreicht und verbunden werden. Bei der VNET-Integration wird der öffentliche Zugriff verweigert, sodass Server nicht über öffentliche Endpunkte erreicht werden können.

Weitere Informationen finden Sie in den [Netzwerkkonzepten](concepts-networking.md).

## <a name="monitoring-and-alerting"></a>Überwachung und Warnung

Der Flexible Server-Dienst verfügt über integrierte Funktionen für Leistungsüberwachung und Warnungen. Alle Azure-Metriken werden im Minutentakt erfasst, und für jede Metrik steht ein Verlauf von 30 Tagen zur Verfügung. Sie können Warnungen für die Metriken konfigurieren. Der Dienst macht Hostservermetriken verfügbar, um die Ressourcenauslastung zu überwachen, und ermöglicht die Konfiguration von Protokollen langsamer Abfragen. Mit diesen Tools können Sie Ihre Workloads schnell optimieren und Ihren Server für optimale Leistung konfigurieren.

Weitere Informationen finden Sie unter [Überwachungskonzepte](concepts-monitoring.md).

## <a name="migration"></a>Migration

Der Dienst führt die MySQL-Community-Version aus. Dies ermöglicht vollständige Anwendungskompatibilität und erfordert minimale Refactoringkosten für die Migration vorhandener Anwendungen, die in der MySQL-Engine entwickelt wurden, zu Flexible Server. Die Migration zu Flexible Server kann mithilfe der folgenden Option durchgeführt werden:

### <a name="offline-migrations"></a>Offlinemigrationen
*   Verwenden von Azure Data Migration Service, wenn die Netzwerkbandbreite zwischen Quelle und Azure gut ist (z. B. ExpressRoute mit hoher Geschwindigkeit). Weitere Informationen finden Sie in den Schrittanleitungen – [Durchführen einer Offlinemigration von MySQL zu Azure Database for MySQL mithilfe von DMS – Azure Database Migration Service](../../dms/tutorial-mysql-azure-mysql-offline-portal.md)
*   Verwenden Sie mydumper/myloader, um die Komprimierungseinstellungen zu nutzen, um Daten effizient über Netzwerke mit niedriger Geschwindigkeit (z. B. öffentliches Internet) zu verschieben. Weitere Informationen finden Sie in den Schritt-für-Schritt-Anweisungen [Migrieren großer Datenbanken zu Azure Database for MySQL mit mydumper/myloader](../../mysql/concepts-migrate-mydumper-myloader.md)

### <a name="online-or-minimal-downtime-migrations"></a>Onlinemigrationen oder Migrationen mit minimaler Ausfallzeit
Verwenden Sie die Datenreplikation mit einer konsistenten Sicherung/Wiederherstellung von mydumper/myloader für das anfängliche Seeding. Weitere Informationen finden Sie unter – [Tutorial: Migration von Azure Database for MySQL mit minimaler Ausfallzeit – Single Server zu Azure Database for MySQL – Flexible Server](../../mysql/howto-migrate-single-flexible-minimum-downtime.md)

Weitere Informationen finden Sie im [Migrationshandbuch für Azure Database for MySQL](../../mysql/migrate/mysql-on-premises-azure-db/01-mysql-migration-guide-intro.md)

## <a name="azure-regions"></a>Azure-Regionen

Ein Vorteil der Ausführung Ihrer Workload in Azure ist die globale Reichweite. Der flexible Server für Azure Database for MySQL ist derzeit in den folgenden Azure-Regionen verfügbar:

| Region | Verfügbarkeit | Zonenredundante Hochverfügbarkeit |
| --- | --- | --- |
| Australien (Osten) | :heavy_check_mark: | :heavy_check_mark: |
| Brasilien Süd | :heavy_check_mark: | :x: |
| Kanada, Mitte | :heavy_check_mark: | :x: |
| USA (Mitte) | :heavy_check_mark: | :x: |
| East US | :heavy_check_mark: | :heavy_check_mark: |
| USA (Ost) 2 | :heavy_check_mark: | :heavy_check_mark: |
| Frankreich, Mitte | :heavy_check_mark: | :x:|
| Deutschland, Westen-Mitte | :heavy_check_mark: | :x: |
| Japan, Osten | :heavy_check_mark: | :heavy_check_mark: |
| Korea, Mitte | :heavy_check_mark: | :x: |
| Nordeuropa | :heavy_check_mark: | :heavy_check_mark: |
| Asien, Südosten | :heavy_check_mark: | :heavy_check_mark: |
| Schweiz, Norden | :heavy_check_mark: | :x: |
| UK, Süden | :heavy_check_mark: | :heavy_check_mark: |
| USA (Westen) | :heavy_check_mark: | :x: |
| USA, Westen 2 | :heavy_check_mark: | :heavy_check_mark: |
| Europa, Westen | :heavy_check_mark: | :heavy_check_mark: |
## <a name="contacts"></a>Kontakte

Sollten Sie Fragen oder Vorschläge im Zusammenhang mit der Verwendung eines flexiblen Servers für Azure Database for MySQL haben, können Sie eine E-Mail an das zuständige Team senden ([@Ask Azure DB for MySQL](mailto:AskAzureDBforMySQL@service.microsoft.com)). Bei dieser E-Mail-Adresse handelt es sich nicht um einen Alias für technischen Support.

Weitere Kontaktmöglichkeiten:

- Wenn Sie den Azure-Support kontaktieren möchten, [fordern Sie im Azure-Portal ein Ticket an](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Um ein Problem mit Ihrem Konto zu beheben, richten Sie im Azure-Portal eine [Anfrage an den Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
- Wenn Sie Feedback abgeben oder Vorschläge für neue Features einreichen möchten, erstellen Sie einen Eintrag über [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun die Grundlagen zum Bereitstellungsmodus Azure Database for MySQL Single Server kennen, können Sie mit den folgenden Themen fortfahren:

- Erstellen Sie Ihren ersten Server.
  - [Erstellen einer Azure Database for MySQL Flexible Server-Instanz mit dem Azure-Portal](quickstart-create-server-portal.md)
  - [Erstellen einer Azure Database for MySQL Flexible Server-Instanz mithilfe der Azure CLI](quickstart-create-server-cli.md)
  - [Erstellen einer Azure Database for MySQL Flexible Server-Instanz mithilfe der Azure CLI](how-to-manage-server-portal.md)

- Erstellen Sie Ihre erste App in Ihrer bevorzugten Sprache:
  - [Python](connect-python.md)
  - [PHP](connect-php.md)
