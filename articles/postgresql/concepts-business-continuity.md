---
title: Geschäftskontinuität – Azure Database for PostgreSQL – Einzelserver
description: Dieser Artikel bietet Informationen zur Geschäftskontinuität (Point-in-Time-Wiederherstellung, Rechenzentrumsausfälle, Geowiederherstellung, Replikate), wenn Sie den Dienst Azure Database for PostgreSQL verwenden.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/07/2020
ms.openlocfilehash: 5fb82c6098352076307f71eee022074a247e3cd9
ms.sourcegitcommit: 3e8058f0c075f8ce34a6da8db92ae006cc64151a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92629339"
---
# <a name="overview-of-business-continuity-with-azure-database-for-postgresql---single-server"></a>Übersicht über die Geschäftskontinuität mit Azure Database for PostgreSQL – Einzelserver

Diese Übersicht beschreibt die Funktionen, die Azure Database for PostgreSQL für Geschäftskontinuität und Notfallwiederherstellung bereitstellt. Erfahren Sie etwas über Optionen für die Wiederherstellung nach Störungen, die Datenverluste nach sich ziehen oder dazu führen können, dass Ihre Datenbank und Ihre Anwendungen nicht mehr verfügbar sind. Erfahren Sie, was zu tun ist, wenn ein Benutzer- oder Anwendungsfehler die Datenintegrität gefährdet, eine Azure-Region ausfällt oder Wartungsaufgaben für Ihre Anwendung ausgeführt werden müssen.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>Features zum Sicherstellen der Geschäftskontinuität

Wenn Sie Ihren Plan für die Geschäftskontinuität entwickeln, müssen Sie ermitteln, wie viel Zeit maximal vergehen darf, bis die Anwendung nach einer Störung vollständig wiederhergestellt ist – diese Zeitspanne ist Ihre RTO (Recovery Time Objective). Sie müssen auch herausfinden, wie viele kürzlich durchgeführte Datenupdates (in einem bestimmten Zeitraum) verloren gehen dürfen, wenn die Anwendung nach einer Störung wiederhergestellt wird – diese Zeitspanne ist Ihre RPO (Recovery Point Objective).

Azure Database for PostgreSQL bietet Geschäftskontinuitätsfeatures, die georedundante Sicherungen mit der Möglichkeit zum Initiieren der Geowiederherstellung und zum Bereitstellen von Lesereplikaten in einer anderen Region umfassen. Jedes Feature weist unterschiedliche Eigenschaften für die Wiederherstellungszeit und mögliche Datenverluste auf. Beim Feature für die [Geowiederherstellung](concepts-backup.md) wird ein neuer Server mithilfe der Sicherungsdaten erstellt, die aus einer anderen Region repliziert werden. Die Gesamtzeit der Wiederherstellung hängt von der Größe der Datenbank und der Anzahl der wiederherzustellenden Protokolle ab. Die Gesamtzeit zum Einrichten des Servers variiert von wenigen Minuten bis zu einigen Stunden. Mit [Lesereplikaten](concepts-read-replicas.md) werden Transaktionsprotokolle vom primären Standort asynchron an das Replikat gestreamt. Die Verzögerung zwischen dem primären Standort und dem Replikat hängt von der Wartezeit zwischen den Standorten und der Menge der zu übertragenden Daten ab. Bei einem Ausfall eines primären Standorts (z. B. einem Verfügbarkeitszonenfehler) ermöglicht das Heraufstufen des Replikats kürzere RTOs und geringere Datenverluste. 

In der folgenden Tabelle werden RTO und RPO in einem typischen Szenario verglichen:

| **Funktion** | **Grundlegend** | **Allgemeiner Zweck** | **Arbeitsspeicheroptimiert** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Point-in-Time-Wiederherstellung von Sicherung | Beliebiger Wiederherstellungspunkt innerhalb der Aufbewahrungsdauer | Beliebiger Wiederherstellungspunkt innerhalb der Aufbewahrungsdauer | Beliebiger Wiederherstellungspunkt innerhalb der Aufbewahrungsdauer |
| Geowiederherstellung von georeplizierten Sicherungen | Nicht unterstützt | RTO: variiert <br/>RPO < 1 Stunde | RTO: variiert <br/>RPO < 1 Stunde |
| Lesereplikate | RTO – Minuten* <br/>RPO < 5 Min.* | RTO – Minuten* <br/>RPO < 5 Min.*| RTO – Minuten* <br/>RPO < 5 Min.*|

\* RTOs und RPOs können in einigen Fällen in Abhängigkeit von verschiedenen Faktoren wie der Workload der primären Datenbank und der Wartezeit zwischen Regionen deutlich höher sein. 

## <a name="recover-a-server-after-a-user-or-application-error"></a>Wiederherstellen eines Servers nach einem Benutzer- oder Anwendungsfehler

Sie können die Sicherungen des Diensts verwenden, um einen Server nach verschiedenen Störungen wiederherzustellen. Es kann passieren, dass ein Benutzer versehentlich Daten, eine wichtige Tabelle oder sogar eine ganze Datenbank löscht. Es kann auch vorkommen, dass eine Anwendung aufgrund eines Anwendungsfehlers unbeabsichtigt gültige Daten mit ungültigen Daten überschreibt usw.

Sie können eine **Point-in-Time-Wiederherstellung** durchführen, um eine Kopie Ihres Servers zu einem als fehlerfrei bekannten Zeitpunkt zu erstellen. Dieser Zeitpunkt muss innerhalb der Aufbewahrungszeit für Sicherungen liegen, die Sie für den Server konfiguriert haben. Nach der Wiederherstellung der Daten auf dem neuen Server können Sie entweder den ursprünglichen Server durch den wiederhergestellten Server ersetzen oder die benötigten Daten vom wiederhergestellten Server auf den ursprünglichen Server kopieren.

> [!IMPORTANT]
> Gelöschte Server **können nicht** wiederhergestellt werden. Wenn Sie den Server löschen, werden auch alle Datenbanken gelöscht, die zum Server gehören, und können nicht wiederhergestellt werden. Verwenden Sie die [Azure-Ressourcensperre](../azure-resource-manager/management/lock-resources.md), um einer versehentlichen Löschung Ihres Servers vorzubeugen.

## <a name="recover-from-an-azure-data-center-outage"></a>Wiederherstellen nach dem Ausfall eines Azure-Rechenzentrums

Es kommt zwar sehr selten vor, aber es ist möglich, dass ein Azure-Rechenzentrum ausfällt. Ein Ausfall kann den Geschäftsbetrieb einige wenige Minuten oder auch mehrere Stunden unterbrechen.

Eine Möglichkeit ist, einfach zu warten, bis der Server wieder online ist, wenn der Rechenzentrumsausfall behoben wurde. Dies funktioniert bei Anwendungen, bei denen der Server für eine längere Zeit offline sein kann, z.B. in einer Entwicklungsumgebung. Wenn ein Rechenzentrum ausfällt, wissen Sie nicht, wie lange der Ausfall dauert, daher ist diese Option nur dann in Erwägung zu ziehen, wenn Sie Ihren Server eine Zeit lang nicht benötigen.

## <a name="geo-restore"></a>Geowiederherstellung

Das Geowiederherstellungsfeature stellt den Server mithilfe georedundanter Sicherungen wieder her. Die Sicherungen werden in der [gekoppelten Region](../best-practices-availability-paired-regions.md) Ihres Servers gehostet. Diese Sicherungen können für eine Wiederherstellung in einer beliebigen anderen Region verwendet werden. Durch die Geowiederherstellung wird ein neuer Server mit den Daten aus den Sicherungen erstellt. Weitere Informationen zur Geowiederherstellung finden Sie im [Konzeptartikel zur Sicherung und Wiederherstellung](concepts-backup.md).

> [!IMPORTANT]
> Die Geowiederherstellung ist nur möglich, wenn Sie den Server mit einem georedundanten Sicherungsspeicher bereitgestellt haben. Wenn Sie von lokal redundanten zu georedundanten Sicherungen für einen bestehenden Server wechseln möchten, müssen Sie mit „pg_dump“ ein Speicherabbild Ihres bestehenden Servers erstellen und ihn auf einem neu erstellten Server wiederherstellen, der mit georedundanten Sicherungen konfiguriert ist.

## <a name="cross-region-read-replicas"></a>Regionsübergreifende Lesereplikate
Mithilfe regionsübergreifender Lesereplikate können Sie Ihre BCDR-Planung (Business Continuity & Disaster Recovery) verbessern. Lesereplikate werden mithilfe der physischen Replikationstechnologie von PostgreSQL asynchron aktualisiert. Weitere Informationen zu Lesereplikaten, zu verfügbaren Regionen und zum Failover finden Sie im [Konzeptartikel zu Lesereplikaten](concepts-read-replicas.md). 

## <a name="faq"></a>Häufig gestellte Fragen
### <a name="where-does-azure-database-for-postgresql-store-customer-data"></a>Wo werden Kundendaten von Azure Database for PostgreSQL gespeichert?
Standardmäßig werden Kundendaten von Azure Database for PostgreSQL in der Region gespeichert, in der sie bereitgestellt werden. Sie werden nicht aus dieser Region verschoben. Kunden können jedoch optional [georedundanten Sicherungen](concepts-backup.md#backup-redundancy-options) aktivieren oder [regionsübergreifenden Lesereplikate](concepts-read-replicas.md#cross-region-replication) zum Speichern von Daten in einer anderen Region erstellen.


## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [automatisierten Sicherungen in Azure Database for PostgreSQL](concepts-backup.md). 
- Erfahren Sie, wie Sie eine Wiederherstellung mithilfe des [Azure-Portals](howto-restore-server-portal.md) oder der [Azure CLI](howto-restore-server-cli.md) ausführen.
- Weitere Informationen zu [Lesereplikaten in Azure Database for PostgreSQL](concepts-read-replicas.md).
