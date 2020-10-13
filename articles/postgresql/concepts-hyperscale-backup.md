---
title: 'Sicherung und Wiederherstellung: Hyperscale (Citus) – Azure Database for PostgreSQL'
description: Schutz von Daten vor versehentlicher Beschädigung oder Löschung
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: 2d781ca7293d4bd95ae62eadc50295ca14c2d381
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91314929"
---
# <a name="backup-and-restore-in-azure-database-for-postgresql---hyperscale-citus"></a>Sicherung und Wiederherstellung in Azure Database for PostgreSQL: Hyperscale (Citus)

Bei der Option Hyperscale (Citus) für Azure Database for PostgreSQL werden automatisch Sicherungskopien für jeden Knoten erstellt und in einem lokal redundanten Speicher gespeichert. Mithilfe von Sicherungskopien können Sie den Stand Ihres Hyperscale (Citus)-Clusters zu einem bestimmten Zeitpunkt wiederherstellen. Sicherungen und Wiederherstellungen sind wesentliche Bestandteile jeder Strategie für Geschäftskontinuität, da Ihre Daten so vor versehentlichen Beschädigungen und Löschungen geschützt werden.

## <a name="backups"></a>Backups

Mindestens einmal pro Tag erstellt Azure Database for PostgreSQL Sicherungsmomentaufnahmen der Datendateien und des Datenbanktransaktionsprotokolls. Dank dieser Sicherungen können Sie für einen Server den Stand zu einem beliebigen Zeitpunkt innerhalb des Aufbewahrungszeitraums wiederherstellen. (Der Aufbewahrungszeitraum beträgt derzeit für alle Cluster 35 Tage.) Zur Verschlüsselung aller Sicherungen wird die AES-Verschlüsselung mit 256 Bit verwendet.

In Azure-Regionen, die Verfügbarkeitszonen unterstützen, werden Sicherungsmomentaufnahmen in drei Verfügbarkeitszonen gespeichert. Solange mindestens eine Verfügbarkeitszone online ist, kann der Hyperscale (Citus)-Cluster wiederhergestellt werden.

Sicherungskopien können nicht exportiert werden. Sie können nur für Wiederherstellungsvorgänge in Azure Database for PostgreSQL verwendet werden.

### <a name="backup-storage-cost"></a>Kosten für Sicherungsspeicher

Die aktuellen Preise für Sicherungsspeicher finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/postgresql/hyperscale-citus/) für Azure Database for PostgreSQL: Hyperscale (Citus).

## <a name="restore"></a>Restore

In Azure Database for PostgreSQL wird beim Wiederherstellen eines Hyperscale (Citus)-Clusters aus den Sicherungskopien der ursprünglichen Knoten ein neuer Cluster erstellt. 

> [!IMPORTANT]
>Sie können den Cluster mit Hyperscale (Citus) nur innerhalb desselben Abonnements und derselben Ressourcengruppe und mit einem anderen Clusternamen wiederherstellen.


> [!IMPORTANT]
> Gelöschte Hyperscale (Citus)-Cluster können nicht wiederhergestellt werden. Wenn Sie den Cluster löschen, werden alle Knoten, die zu diesem Cluster gehören, gelöscht und können nicht wiederhergestellt werden. Administratoren können [Verwaltungssperren](/azure/azure-resource-manager/management/lock-resources) nutzen, um Clusterressourcen nach der Bereitstellung vor versehentlichem Löschen oder unerwarteten Änderungen zu schützen.

### <a name="point-in-time-restore-pitr"></a>Point-in-Time-Wiederherstellung

Sie können den Stand eines Clusters zu einem beliebigen Zeitpunkt innerhalb der letzten 35 Tage wiederherstellen.
Die Point-in-Time-Wiederherstellung ist für viele Szenarien hilfreich. Beispiele hierfür sind Fälle, in denen ein Benutzer versehentlich Daten löscht oder eine wichtige Tabelle oder Datenbank entfernt oder in denen eine Anwendung fälschlicherweise unbeschädigte Daten mit fehlerhaften Daten überschreibt.

Beim Wiederherstellungsprozess wird ein neuer Cluster in derselben Azure-Region, demselben Abonnement und derselben Ressourcengruppe wie der ursprüngliche Cluster erstellt. Dieser Cluster verfügt über die Konfiguration des ursprünglichen Clusters: Die Anzahl von Knoten und virtuellen Kernen sowie Speichergröße, Benutzerrollen, PostgreSQL-Version und Version der Citus-Erweiterung stimmen überein.

Die Firewalleinstellungen und PostgreSQL-Dienstparameter der ursprünglichen Servergruppe bleiben nicht erhalten. Sie werden auf die Standardwerte zurückgesetzt. Die Firewall verhindert alle Verbindungen. Sie müssen diese Einstellungen nach der Wiederherstellung manuell anpassen.

> [!IMPORTANT]
> Sie müssen eine Supportanfrage stellen, um eine Point-in-Time-Wiederherstellung Ihres Hyperscale (Citus)-Clusters durchzuführen.

### <a name="post-restore-tasks"></a>Aufgaben nach der Wiederherstellung

Nach beiden Wiederherstellungsverfahren sollten Sie die folgenden Aufgaben durchführen, um Ihre Benutzer und Anwendungen wieder in einen betriebsbereiten Zustand zu versetzen:

* Umleiten von Clients und Clientanwendungen an den neuen Server, wenn der neue Server den ursprünglichen Server ersetzen soll
* Sicherstellen, dass geeignete Firewallregeln auf Serverebene und VNet-Regeln vorhanden sind, damit Benutzer eine Verbindung herstellen können Diese Regeln werden nicht von der ursprünglichen Servergruppe kopiert.
* Passen Sie PostgreSQL-Serverparameter nach Bedarf an. Diese Parameter werden nicht von der ursprünglichen Servergruppe kopiert.
* Sicherstellen, dass geeignete Anmeldungen und Berechtigungen auf Datenbankebene vorhanden sind
* Konfigurieren der erforderlichen Warnungen

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über  [Azure-Verfügbarkeitszonen](/azure/availability-zones/az-overview).
* Legen Sie  [vorgeschlagene Warnungen](/azure/postgresql/howto-hyperscale-alert-on-metric#suggested-alerts) für Hyperscale (Citus)-Servergruppen fest.
