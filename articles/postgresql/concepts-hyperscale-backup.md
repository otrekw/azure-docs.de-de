---
title: 'Sicherung und Wiederherstellung: Hyperscale (Citus) – Azure Database for PostgreSQL'
description: Schutz von Daten vor versehentlicher Beschädigung oder Löschung
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 04/14/2021
ms.openlocfilehash: 7681e9c28bbbbcec06bcc1cf2bf469f1b4189d79
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107520172"
---
# <a name="backup-and-restore-in-azure-database-for-postgresql---hyperscale-citus"></a>Sicherung und Wiederherstellung in Azure Database for PostgreSQL: Hyperscale (Citus)

Bei der Option Hyperscale (Citus) für Azure Database for PostgreSQL werden automatisch Sicherungskopien für jeden Knoten erstellt und in einem lokal redundanten Speicher gespeichert. Mithilfe von Sicherungen können Sie den Stand Ihrer Hyperscale (Citus)-Servergruppe zu einem bestimmten Zeitpunkt wiederherstellen.
Sicherungen und Wiederherstellungen sind wesentliche Bestandteile jeder Strategie für Geschäftskontinuität, da Ihre Daten so vor versehentlichen Beschädigungen und Löschungen geschützt werden.

## <a name="backups"></a>Backups

Mindestens einmal pro Tag erstellt Azure Database for PostgreSQL Sicherungsmomentaufnahmen der Datendateien und des Datenbanktransaktionsprotokolls. Dank dieser Sicherungen können Sie für einen Server den Stand zu einem beliebigen Zeitpunkt innerhalb des Aufbewahrungszeitraums wiederherstellen. (Der Aufbewahrungszeitraum beträgt derzeit für alle Servergruppen 35 Tage.) Zur Verschlüsselung aller Sicherungen wird die AES-Verschlüsselung mit 256 Bit verwendet.

In Azure-Regionen, die Verfügbarkeitszonen unterstützen, werden Sicherungsmomentaufnahmen in drei Verfügbarkeitszonen gespeichert. Solange mindestens eine Verfügbarkeitszone online ist, kann die Hyperscale (Citus)-Servergruppe wiederhergestellt werden.

Sicherungskopien können nicht exportiert werden. Sie können nur für Wiederherstellungsvorgänge in Azure Database for PostgreSQL verwendet werden.

### <a name="backup-storage-cost"></a>Kosten für Sicherungsspeicher

Die aktuellen Preise für Sicherungsspeicher finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/postgresql/hyperscale-citus/) für Azure Database for PostgreSQL: Hyperscale (Citus).

## <a name="restore"></a>Restore

Sie können den Stand einer Hyperscale (Citus)-Servergruppe zu einem beliebigen Zeitpunkt innerhalb der letzten 35 Tage wiederherstellen.  Die Point-in-Time-Wiederherstellung ist für viele Szenarien hilfreich. Beispiele hierfür sind Fälle, in denen ein Benutzer versehentlich Daten löscht oder eine wichtige Tabelle oder Datenbank entfernt oder in denen eine Anwendung fälschlicherweise unbeschädigte Daten mit fehlerhaften Daten überschreibt.

> [!IMPORTANT]
> Gelöschte Hyperscale (Citus)-Servergruppen können nicht wiederhergestellt werden. Wenn Sie die Servergruppe löschen, werden alle Knoten, die zu dieser Servergruppe gehören, gelöscht und können nicht wiederhergestellt werden. Um Servergruppenressourcen nach der Bereitstellung vor versehentlichem Löschen oder unerwarteten Änderungen zu schützen, können Administratoren [Verwaltungssperren](../azure-resource-manager/management/lock-resources.md) nutzen.

Beim Wiederherstellungsprozess wird eine neue Servergruppe in derselben Azure-Region, demselben Abonnement und derselben Ressourcengruppe wie die ursprüngliche Servergruppe erstellt. Diese Servergruppe verfügt über die Konfiguration der ursprünglichen Servergruppe: Die Anzahl von Knoten und virtuellen Kernen sowie Speichergröße, Benutzerrollen, PostgreSQL-Version und Version der Citus-Erweiterung stimmen überein.

Die Firewalleinstellungen und PostgreSQL-Dienstparameter der ursprünglichen Servergruppe bleiben nicht erhalten. Sie werden auf die Standardwerte zurückgesetzt. Die Firewall verhindert alle Verbindungen. Sie müssen diese Einstellungen nach der Wiederherstellung manuell anpassen. Sehen Sie sich unsere Liste vorgeschlagener [Aufgaben nach der Wiederherstellung](howto-hyperscale-restore-portal.md#post-restore-tasks) an.

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich die Schritte zum [Wiederherstellen einer Servergruppe](howto-hyperscale-restore-portal.md) im Azure-Portal an.
* Erfahren Sie mehr über  [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md).
