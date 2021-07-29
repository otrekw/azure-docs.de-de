---
title: Leitfaden zur Migration von MySQL lokal zu Azure Database for MySQL – Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR)
description: Wie bei jedem unternehmenskritischen System ist die Strategie für Sicherung und Wiederherstellung sowie Notfallwiederherstellung (BCDR) ein wichtiger Bestandteil Ihres gesamten Systementwurfs.
ms.service: mysql
ms.subservice: migration-guide
ms.topic: how-to
author: arunkumarthiags
ms.author: arthiaga
ms.reviewer: maghan
ms.custom: ''
ms.date: 06/11/2021
ms.openlocfilehash: 4785c49c456b0008f0ec4c67cdee55d8118c76a9
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112082734"
---
# <a name="mysql-on-premises-to-azure-database-for-mysql-migration-guide-business-continuity-and-disaster-recovery-bcdr"></a>Leitfaden zur Migration von MySQL lokal zu Azure Database for MySQL – Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR)

## <a name="prerequisites"></a>Voraussetzungen

[Optimierung](11-optimization.md)

## <a name="back-up-and-restore"></a>Sichern und Wiederherstellen

Wie bei jedem unternehmenskritischen System ist die Strategie für Sicherung und Wiederherstellung sowie Notfallwiederherstellung (BCDR) ein wichtiger Bestandteil Ihres gesamten Systementwurfs. Wenn ein unvorhergesehenes Ereignis eintritt, sollten Sie in der Lage sein, den Stand Ihrer Daten zu einem bestimmten Zeitpunkt (Recovery Point Objective) in einem angemessenen Zeitraum (Recovery Time Objective) wiederherzustellen.

### <a name="backup"></a>Backup

Azure Database for MySQL unterstützt automatische Sicherungen standardmäßig 7 Tage lang. Es kann sinnvoll sein, dies in den aktuellen Maximalwert von 35 Tagen zu ändern. Sie müssen beachten, dass bei einer Änderung des Werts in 35 Tage Gebühren für über den zugeordneten Speicher hinausgehenden zusätzlichen Sicherungsspeicher anfallen.

Es gibt mehrere aktuelle Einschränkungen der Datenbanksicherungsfunktion, wie im Dokumentationsartikel [Sicherung und Wiederherstellung in Azure Database for MySQL](/azure/mysql/concepts-backup) beschrieben. Diese müssen Sie kennen, wenn Sie entscheiden, welche zusätzlichen Strategien implementiert werden sollen.

Sie müssen z. B. Folgendes beachten:

- Kein direkter Zugriff auf die Sicherungen

- Für Ebenen, die bis zu 4 TB zulassen, wird einmal pro Woche eine vollständige Sicherung durchgeführt, zweimal täglich eine differenzielle und alle fünf Minuten Protokolle.

- Für Ebenen, die bis zu 16 TB zulassen, werden Sicherungen auf Momentaufnahmenbasis durchgeführt.

    > [!NOTE]
    > [Einige Regionen](/azure/mysql/concepts-pricing-tiers#storage) unterstützen noch keinen Speicher von bis zu 16 TB.

### <a name="restore"></a>Restore

Redundanz (lokal oder Georedundanz) muss während der Servererstellung konfiguriert werden. Eine Geowiederherstellung kann jedoch durchgeführt werden und ermöglicht die Änderung dieser Optionen während des Wiederherstellungsprozesses. Das Ausführen eines Wiederherstellungsvorgangs kann die Konnektivität vorübergehend beenden, und alle Anwendungen sind während des Wiederherstellungsvorgangs ausgeschaltet.

Während einer Datenbankwiederherstellung müssen alle unterstützenden Elemente außerhalb der Datenbank wiederhergestellt werden. Überprüfen Sie den Migrationsprozess. Weitere Informationen finden Sie unter [Durchführen der Aufgaben nach der Wiederherstellung](/azure/mysql/concepts-backup#perform-post-restore-tasks).

## <a name="read-replicas"></a>Lesereplikate

[Lesereplikate](/azure/mysql/concepts-read-replicas) können verwendet werden, um den MySQL-Lesedurchsatz zu erhöhen, die Leistung für regionale Benutzer zu verbessern und die Notfallwiederherstellung zu implementieren. Beachten Sie beim Erstellen eines oder mehrerer Lesereplikate, dass zusätzliche Gebühren für die gleiche Compute- und Speichernutzung wie für den primären Server anfallen.

## <a name="deleted-servers"></a>Gelöschte Server

Wenn ein Administrator oder ein böswilliger Benutzer den Server im Azure-Portal oder über automatisierte Methoden löscht, werden alle Sicherungen und Lesereplikate gelöscht. Es ist wichtig, dass [Ressourcensperren](/azure/azure-resource-manager/management/lock-resources) für die Azure Database for MySQL-Ressourcengruppe erstellt werden, um den Instanzen eine zusätzliche Löschschutzebene hinzuzufügen.

## <a name="regional-failure"></a>Regionaler Ausfall

Wenn ein seltener regionalen Ausfall eintritt, können georedundante Sicherungen oder ein Lesereplikat verwendet werden, um die Datenworkloads wieder auszuführen. Sowohl die Georeplikation als auch ein Lesereplikat sollten zur Verfügung stehen, um den besten Schutz vor unerwarteten regionalen Ausfällen zu bieten.

> [!NOTE]
> Mit dem Ändern der Datenbankserverregion ändert sich auch der Endpunkt, sodass die Anwendungskonfigurationen entsprechend aktualisiert werden müssen.

### <a name="load-balancers"></a>Load Balancer

Wenn die Anwendung aus vielen verschiedenen Instanzen auf der ganzen Welt besteht, ist es möglicherweise nicht möglich, alle Clients zu aktualisieren. Verwenden Sie einen [Azure Load Balancer](/azure/load-balancer/load-balancer-overview) oder ein [Application Gateway](/azure/application-gateway/overview), um eine nahtlose Failoverfunktion zu implementieren. Obwohl sie hilfreich und zeitsparend sind, sind diese Tools für regionale Failoverfunktionen nicht erforderlich.

## <a name="wwi-scenario"></a>WWI-Szenario

WWI wollte die Failoverfunktionen von Lesereplikaten testen, sodass sie die unten beschriebenen Schritte ausgeführt haben.

### <a name="creating-a-read-replica"></a>Erstellen eines Lesereplikats

- Öffnen Sie das Azure-Portal.

- Navigieren Sie zur Azure Database for MySQL-Instanz.

- Wählen Sie unter **Einstellungen** die Option **Replikation** aus.

- Wählen Sie **Replikat hinzufügen**.

- Geben Sie einen Servernamen ein.

- Wählen Sie die Region aus.

- Wählen Sie **OK** aus, und warten Sie, bis die Instanz bereitgestellt ist. Je nach Größe der Hauptinstanz kann die Replikation einige Zeit dauern.

    > [!NOTE]
    > Für jedes Replikat fallen zusätzliche Gebühren an, die denen für die Hauptinstanz entsprechen.

### <a name="fail-over-to-read-replica"></a>Failover zum Lesereplikat

Sobald ein Lesereplikat erstellt und der Replikationsprozess abgeschlossen ist, kann es für ein Failover verwendet werden. Die Replikation wird während eines Failovers beendet und macht das Lesereplikat zu einer eigenen Hauptinstanz.

Failoverschritte:

- Öffnen Sie das Azure-Portal.

- Navigieren Sie zur Azure Database for MySQL-Instanz.

- Wählen Sie unter **Einstellungen** die Option **Replikation** aus.

- Wählen Sie eines der Lesereplikate aus.

- Wählen Sie **Replikation beenden** aus. Dadurch wird das Lesereplikat unterbrochen.

- Ändern Sie alle Anwendungsverbindungszeichenfolgen so, dass sie auf die neue Hauptinstanz verweisen.

## <a name="bcdr-checklist"></a>BCDR-Prüfliste

- Ändern Sie die Sicherungshäufigkeit, um die Anforderungen zu erfüllen.

- Richten Sie Lesereplikate für leseintensive Workloads und regionales Failover ein.

- Erstellen Sie Ressourcensperren für Ressourcengruppen.

- Implementieren Sie eine Lastenausgleichsstrategie für Anwendungen für schnelles Failover.  


> [!div class="nextstepaction"]
> [Security](./13-security.md)
