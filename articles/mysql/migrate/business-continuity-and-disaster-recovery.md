---
title: 'Leitfaden zur Migration einer lokalen MySQL-Instanz zu Azure Database for MySQL: Business Continuity & Disaster Recovery (BCDR)'
description: Wie bei jedem unternehmenskritischen System ist die Strategie für Sicherung und Wiederherstellung sowie Notfallwiederherstellung (Business Continuity & Disaster Recovery, BCDR) ein wichtiger Bestandteil Ihres gesamten Systementwurfs.
ms.service: mysql
ms.subservice: migration-guide
ms.topic: how-to
author: arunkumarthiags
ms.author: arthiaga
ms.reviewer: maghan
ms.custom: ''
ms.date: 05/26/2021
ms.openlocfilehash: 6eb944d72f5d4add393079d9fff8e83d37a18e06
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111961543"
---
# <a name="mysql-on-premises-to-azure-database-for-mysql-migration-guide-business-continuity-and-disaster-recovery-bcdr"></a>Leitfaden zur Migration einer lokalen MySQL-Instanz zu Azure Database for MySQL: Business Continuity & Disaster Recovery (BCDR)

### <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Wie bei jedem unternehmenskritischen System ist die Strategie für Sicherung und Wiederherstellung sowie Notfallwiederherstellung (Business Continuity & Disaster Recovery, BCDR) ein wichtiger Bestandteil Ihres gesamten Systementwurfs. Wenn ein unvorhergesehenes Ereignis eintritt, müssen Sie in der Lage sein, den Stand Ihrer Daten zu einem bestimmten Zeitpunkt (Recovery Point Objective) in einem angemessenen Zeitraum (Recovery Time Objective) wiederherzustellen.

#### <a name="backup"></a>Backup

Azure Database for MySQL unterstützt standardmäßig automatische Sicherungen für sieben Tage. Es kann sinnvoll sein, dies in den aktuellen Maximalwert von 35 Tagen zu ändern. Beachten Sie, dass bei einer Änderung des Werts in 35 Tage Gebühren für zusätzlichen, über den zugeordneten Speicher hinausgehenden Sicherungsspeicher anfallen.

Es gibt aktuell mehrere Einschränkungen für das Datenbanksicherungsfeature. Diese werden im Dokumentationsartikel [Sicherung und Wiederherstellung in Azure Database for MySQL](../concepts-backup.md) beschrieben. Es ist wichtig, mit diesen Einschränkungen vertraut zu sein, um entscheiden zu können, welche zusätzlichen Strategien ggf. implementiert werden müssen.

Beachten Sie beispielsweise Folgendes:

  - Kein direkter Zugriff auf die Sicherungen

  - Für Tarife bis 4 TB wird einmal pro Woche eine vollständige Sicherung, zweimal täglich eine differenzielle Sicherung und alle fünf Minuten eine Protokollsicherung durchgeführt.

  - Für Sicherungen bis 16 TB werden Sicherungen auf Momentaufnahmebasis durchgeführt.

    > [!NOTE] 
    > [Einige Regionen](../concepts-pricing-tiers.md#storage) unterstützen noch keinen Speicher von bis zu 16 TB.

#### <a name="restore"></a>Restore

Redundanz (lokal oder Georedundanz) muss während der Servererstellung konfiguriert werden. Eine Geowiederherstellung kann jedoch durchgeführt werden und ermöglicht die Änderung dieser Optionen während des Wiederherstellungsprozesses. Die Durchführung eines Wiederherstellungsvorgangs führt dazu, dass die Konnektivität vorübergehend unterbrochen wird und die Anwendungen nicht verfügbar sind.

Während einer Datenbankwiederherstellung müssen auch alle unterstützenden Elemente außerhalb der Datenbank wiederhergestellt werden. Sehen Sie sich den Migrationsprozess an. Weitere Informationen finden Sie unter [Durchführen der Aufgaben nach der Wiederherstellung](../concepts-backup.md#perform-post-restore-tasks).

### <a name="read-replicas"></a>Lesereplikate

[Lesereplikate](../concepts-read-replicas.md) können verwendet werden, um den MySQL-Lesedurchsatz zu erhöhen, die Leistung für regionale Benutzer zu verbessern und die Notfallwiederherstellung zu implementieren. Beachten Sie beim Erstellen einzelner oder mehrerer Lesereplikate, dass zusätzliche Gebühren für die gleichen Compute- und Speicherressourcen anfallen wie beim primären Server.

### <a name="deleted-servers"></a>Gelöschte Server

Wenn ein Administrator oder ein böswilliger Benutzer den Server im Azure-Portal oder über automatisierte Methoden löscht, werden auch alle Sicherungen und Lesereplikate gelöscht. Es ist wichtig, [Ressourcensperren](../../azure-resource-manager/management/lock-resources.md) für die Azure Database for MySQL-Ressourcengruppe zu erstellen, um den Instanzen eine zusätzliche Löschschutzebene hinzuzufügen.

### <a name="regional-failure"></a>Regionaler Fehler

Für den seltenen Fall eines regionalen Ausfalls können Sie georedundante Sicherungen oder ein Lesereplikat verwenden, um zu gewährleisten, dass die Datenworkloads wieder ausgeführt werden können. Zum bestmöglichen Schutz vor unerwarteten regionalen Ausfällen sollte sowohl die Georeplikation als auch ein Lesereplikat zur Verfügung stehen.

> [!NOTE]
> Nach einer Änderung der Datenbankserverregion ändert sich auch der Endpunkt, sodass die Anwendungskonfigurationen entsprechend aktualisiert werden müssen.

#### <a name="load-balancers"></a>Load Balancer

Wenn die Anwendung aus vielen verschiedenen Instanzen auf der ganzen Welt besteht, ist es möglicherweise nicht möglich, alle Clients zu aktualisieren. Verwenden Sie [Azure Load Balancer](../../load-balancer/load-balancer-overview.md) oder [Application Gateway](../../application-gateway/overview.md), um eine nahtlose Failoverfunktion zu implementieren. Diese Tools sind zwar hilfreich und zeitsparend, für regionale Failoverfunktionen sind sie allerdings nicht erforderlich.

### <a name="wwi-scenario"></a>WWI-Szenario

Von WWI wurden die folgenden Schritte ausgeführt, um die Failoverfunktionen von Lesereplikaten zu testen.

#### <a name="creating-a-read-replica"></a>Erstellen eines Lesereplikats

  - Öffnen Sie das Azure-Portal.

  - Navigieren Sie zur Azure Database for MySQL-Instanz.

  - Wählen Sie unter **Einstellungen** die Option **Replikation** aus.

  - Wählen Sie **Replikat hinzufügen**.

  - Geben Sie einen Servernamen ein.

  - Wählen Sie die Region aus.

  - Wählen Sie **OK** aus, und warten Sie, bis die Instanz bereitgestellt wurde. Je nach Größe der Hauptinstanz kann die Replikation einige Zeit dauern.

    > [!NOTE]
    > Für jedes Replikat fallen zusätzliche Gebühren an, die denen für die Hauptinstanz entsprechen.

#### <a name="failover-to-read-replica"></a>Failover zum Lesereplikat

Sobald ein Lesereplikat erstellt wurde und der Replikationsprozess abgeschlossen ist, kann es für ein Failover verwendet werden. Die Replikation wird während eines Failovers beendet und das Lesereplikat zu einer eigenen Hauptinstanz gemacht.

Failoverschritte:

  - Öffnen Sie das Azure-Portal.

  - Navigieren Sie zur Azure Database for MySQL-Instanz.

  - Wählen Sie unter **Einstellungen** die Option **Replikation** aus.

  - Wählen Sie eines der Lesereplikate aus.

  - Wählen Sie **Replikation beenden** aus. Dadurch wird das Lesereplikat unterbrochen.

  - Ändern Sie alle Anwendungsverbindungszeichenfolgen so, dass sie auf die neue Hauptinstanz verweisen.

### <a name="bcdr-checklist"></a>BCDR-Prüfliste

  - Passen Sie die Sicherungshäufigkeit an die Anforderungen an.

  - Richten Sie Lesereplikate für leseintensive Workloads und regionales Failover ein.

  - Erstellen Sie Ressourcensperren für Ressourcengruppen.

  - Implementieren Sie eine Lastenausgleichsstrategie für Anwendungen für schnelles Failover.  


> [!div class="nextstepaction"]
> [Security](./security.md)