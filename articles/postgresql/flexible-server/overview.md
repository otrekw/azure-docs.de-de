---
title: Azure Database for PostgreSQL – Flexible Server
description: Überblick über Azure Database for PostgreSQL – Flexible Server
author: sunilagarwal
ms.author: sunila
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 09/22/2020
ms.openlocfilehash: 268eedf6f9d64d52539e20006322b6b1dd9964e8
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91439965"
---
# <a name="azure-database-for-postgresql---flexible-server"></a>Azure Database for PostgreSQL – Flexible Server

[Azure Database for PostgreSQL](../overview.md) auf Grundlage der PostgreSQL Community Edition ist in drei Bereitstellungsmodi verfügbar:

- [Einzelserver](../overview-single-server.md)
- Flexible Server (Vorschau)
- Hyperscale (Citus)

In diesem Artikel erhalten Sie eine Übersicht und Einführung in die grundlegenden Konzepte des Modell zur Bereitstellung einer Flexible Server-Instanz.

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

## <a name="overview"></a>Übersicht

„Azure Database for PostgreSQL – Flexible Server“ ist ein vollständig verwalteter Datenbankdienst, der eine differenziertere Steuerung und mehr Flexibilität in Bezug auf Verwaltungsfunktionen und Konfigurationseinstellungen für Datenbanken bietet. Im Allgemeinen ermöglicht der Dienst mehr Flexibilität und die Anpassung der Serverkonfiguration an die Anforderungen der Benutzer. Die flexible Serverarchitektur ermöglicht es Benutzern, die Datenbank-Engine mit der Clientebene anzuordnen, um die Latenz zu verringern und Hochverfügbarkeit innerhalb einer und in mehreren Verfügbarkeitszonen zu erreichen. Flexible Server-Instanzen bieten auch eine bessere Kostenoptimierung mit der Möglichkeit, Ihren Server anzuhalten/zu starten, und dem Computetarif „Burstfähig“, der ideal für Workloads ist, die nicht ständig volle Computekapazität benötigen. Der Dienst unterstützt derzeit die Community-Version von PostgreSQL 11 und 12. Der Dienst ist derzeit in der Vorschauphase und in einer Vielzahl von [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/services/) verfügbar.

![Flexible Server: Übersicht](./media/overview/overview-flexible-server.png)


Flexible Server-Instanzen eignen sich am besten für Folgendes:

- Anwendungsentwicklungen, die bessere Steuerungs- und Anpassungsmöglichkeiten erfordern.
- Zonenredundante Hochverfügbarkeit
- Verwaltete Wartungsfenster
  
## <a name="high-availability"></a>Hochverfügbarkeit

Das Modell zur Bereitstellung von Flexible Server-Instanzen ist so konzipiert, dass es Hochverfügbarkeit innerhalb einer einzigen Verfügbarkeitszone und in mehreren Verfügbarkeitszonen unterstützt. In der Architektur sind die Compute- und Speicherebene voneinander getrennt. Die Datenbank-Engine läuft auf einem virtuellen Linux-Computer, während sich die Datendateien in Azure-Speicher befinden. Der Speicher verwaltet drei lokal redundante synchrone Kopien der Datenbankdateien, wodurch Datenbeständigkeit sichergestellt ist.

Wenn der Server bei geplanten oder ungeplanten Failoverereignissen ausfällt, gewährleistet der Dienst die Hochverfügbarkeit der Server mit dem folgenden automatisierten Verfahren:

1. Eine neue Computeressource in Form einer Linux-VM wird bereitgestellt.
2. Der Speicher mit Datendateien wird dem neuen virtuellen Computer zugeordnet.
3. Die PostgreSQL-Datenbank-Engine wird auf dem neuen virtuellen Computer online geschaltet.

Die folgen Abbildung zeigt den Übergang bei VM- und Speicherausfall.

 :::image type="content" source="./media/overview/overview-azure-postgres-flex-virtualmachine.png" alt-text="Flexible Server-Instanz: VM- und Speicherausfall":::

Wenn zonenredundante Hochverfügbarkeit konfiguriert ist, wird vom Dienst ein unmittelbar betriebsbereiter Standbyserver in der Verfügbarkeitszone innerhalb derselben Azure-Region bereitgestellt und verwaltet. Die Datenänderungen auf dem Quellserver werden synchron auf den Standbyserver repliziert, damit kein Datenverlust entsteht. Bei zonenredundanter Hochverfügbarkeit geht der Standbyserver nach dem Auslösen des geplanten oder ungeplanten Failoverereignisses sofort online und steht für die Verarbeitung eingehender Transaktionen zur Verfügung. Dies ermöglicht die Resilienz des Diensts bei Ausfall einer Verfügbarkeitszone innerhalb einer Azure-Region, die mehrere Verfügbarkeitszonen unterstützt, wie in der folgenden Abbildung gezeigt.

 :::image type="content" source="./media/business-continuity/concepts-zone-redundant-high-availability-architecture.png" alt-text="Flexible Server-Instanz: VM- und Speicherausfall":::

 Weitere Informationen finden Sie in der [Dokumentation zur Hochverfügbarkeit](./concepts-high-availability.md).

## <a name="automated-patching-with-managed-maintenance-window"></a>Automatisches Patchen mit verwaltetem Wartungsfenster

Der Dienst führt automatisches Patchen für die zugrunde liegende Hardware, das Betriebssystem und die Datenbank-Engine durch. Das Patchen umfasst Sicherheits- und Softwareupdates. Für die PostgreSQL-Engine sind im Rahmen des geplanten Wartungsreleases auch Upgrades von Nebenversionen enthalten. Benutzer können den Patchzeitplan so konfigurieren, dass er vom System verwaltet wird, oder einen eigenen Zeitplan definieren. Im Rahmen des Wartungszeitplans wird der Patch angewendet. Möglicherweise muss der Server als Teil des Patchprozesses neu gestartet werden, um das Update abzuschließen. Mithilfe des benutzerdefinierten Zeitplans können Benutzer ihren Patchzyklus überlegt gestalten und ein Wartungsfenster mit minimalen Auswirkungen auf das Unternehmen wählen. Im Allgemeinen gilt für den Dienst im Rahmen der kontinuierlichen Integration und Veröffentlichung ein monatlicher Releasezeitplan.

## <a name="automatic-backups"></a>Automatische Sicherungen

Der Flexible Server-Dienst erstellt automatisch Serversicherungen und speichert diese in vom Benutzer lokal konfiguriertem zonenredundanten Speicher. Mithilfe von Sicherungen können Sie den Server zu einem beliebigen Zeitpunkt innerhalb des Aufbewahrungszeitraums der Sicherung wiederherstellen. Die Standardaufbewahrungsdauer für Sicherungen beträgt sieben Tage. Die Aufbewahrungsdauer kann optional auf bis zu 35 Tage festgelegt werden. Zur Verschlüsselung aller Sicherungen wird die AES-Verschlüsselung mit 256 Bit verwendet. Weitere Informationen finden Sie unter [Sicherungen](./concepts-backup-restore.md).

## <a name="adjust-performance-and-scale-within-seconds"></a>Anpassen von Leistung und Skalierung in nur wenigen Sekunden

Der Flexible Server-Dienst ist in drei Computetarifen verfügbar: „Burstfähig“, „Universell“ und „Arbeitsspeicheroptimiert“. Der Tarif „Burstfähig“ eignet sich am besten für die kostengünstige Entwicklung und Workloads mit geringer Parallelität, die nicht ständig die volle Computekapazität benötigen. Die Tarife „Universell“ und „Arbeitsspeicheroptimiert“ eignen sich besser für Produktionsworkloads, die eine hohe Parallelität, Skalierung und vorhersagbare Leistung erfordern. Sie können Ihre erste Anwendung mit einer kleinen Datenbank für wenige Euro im Monat erstellen und die Skalierung dann so ändern, dass sie den Anforderungen Ihrer Lösung entspricht.

## <a name="stopstart-server-to-lower-tco"></a>Anhalten/Starten des Servers zum Senken der Gesamtkosten

Der Flexible Server-Dienst ermöglicht Ihnen, Server bei Bedarf anzuhalten und zu starten, um Ihre Gesamtkosten zu senken. Die Abrechnung des Computetarifs wird sofort beendet, sobald der Server angehalten wird. Dies kann erhebliche Kosteneinsparungen bei Entwicklung, Tests und zeitgebundenen, vorhersehbaren Produktionsworkloads ermöglichen. Der Server verbleibt sieben Tage im angehaltenen Zustand, es sei denn, er wird früher wieder gestartet.

## <a name="enterprise-grade-security"></a>Sicherheit auf Unternehmensniveau

Der flexible Serverdienst nutzt das FIPS 140-2-zertifizierte Kryptografiemodul für die Speicherverschlüsselung ruhender Daten. Daten wie Sicherungen und temporäre Dateien, die während der Ausführung von Abfragen erstellt wurden, werden verschlüsselt. Der Dienst verwendet das in der Azure Storage-Verschlüsselung vorgesehene AES-256-Bit-Verschlüsselungsverfahren, wobei die Schlüssel (standardmäßig) vom System verwaltet werden. Der Dienst verschlüsselt Daten während der Übertragung mit Transport Layer Security (SSL/TLS), was standardmäßig erzwungen wird. Der Dienst erzwingt und unterstützt nur die TLS-Version 1.2.

Flexible Server-Instanzen ermöglichen den vollständigen privaten Zugriff auf die Server mithilfe eines virtuellen Azure-Netzwerks (VNET-Integration). Server im virtuellen Azure-Netzwerk können nur über private IP-Adressen erreicht und verbunden werden. Bei der VNET-Integration wird der öffentliche Zugriff verweigert, sodass Server nicht über öffentliche Endpunkte erreicht werden können.

## <a name="monitoring-and-alerting"></a>Überwachung und Warnung

Der Flexible Server-Dienst verfügt über integrierte Funktionen für Leistungsüberwachung und Warnungen. Alle Azure-Metriken werden im Minutentakt erfasst, und für jede Metrik steht ein Verlauf von 30 Tagen zur Verfügung. Sie können Warnungen für die Metriken konfigurieren. Der Dienst macht Hostservermetriken verfügbar, um die Ressourcenauslastung zu überwachen, und ermöglicht die Konfiguration von Protokollen langsamer Abfragen. Mit diesen Tools können Sie Ihre Workloads schnell optimieren und Ihren Server für optimale Leistung konfigurieren.

## <a name="migration"></a>Migration

Der Dienst führt die Community-Version von PostgreSQL aus. Dies ermöglicht vollständige Anwendungskompatibilität und verursacht minimale Refactoringkosten für die Migration vorhandener Anwendungen, die für die PostgreSQL-Engine entwickelt wurden, zu Flexible Server. 

- **Sichern und Wiederherstellen**: Für Offlinemigrationen, wenn sich Benutzer Ausfallzeiten leisten können, kann das Sichern und Wiederherstellen mit Communitytools wie pg_dump und pg_restore die schnellste Möglichkeit der Migration sein. Ausführliche Informationen finden Sie unter [Migrieren durch Sicherungen und Wiederherstellungen](https://docs.microsoft.com/azure/postgresql/howto-migrate-using-dump-and-restore).
- **Azure Database Migration Service**: Für eine nahtlose und einfachere Migration zu Flexible Server mit minimalen Ausfallzeiten können Sie Azure Database Migration Service einsetzen. Weitere Informationen finden Sie unter [DMS über Portal](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online-portal) und [DMS über CLI](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online). Sie können von Azure Database for PostgreSQL-Einzelserver zu Flexible Server migrieren. Weitere Informationen finden Sie in [diesem Artikel zu DMS](https://docs.microsoft.com/azure/dms/tutorial-azure-postgresql-to-azure-postgresql-online-portal).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun eine Einführung in den Bereitstellungsmodus für Azure Database for PostgreSQL – Flexible Server gelesen haben, sind Sie bereit, Ihren ersten Server zu erstellen: [Erstellen einer Instanz von Azure Database for PostgreSQL – Flexible Server im Azure-Portal](./quickstart-create-server-portal.md)


