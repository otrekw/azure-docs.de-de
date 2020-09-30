---
title: Übersicht über Verwaltungsvorgänge
titleSuffix: Azure SQL Managed Instance
description: Enthält Informationen zur Dauer der Verwaltungsvorgänge für Azure SQL Managed Instance und zu den bewährten Methoden.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: overview
author: urosmil
ms.author: urmilano
ms.reviewer: sstein, MashaMSFT
ms.date: 07/10/2020
ms.openlocfilehash: 2da7311e61aa39be69a6a0a29eff686baaad7ebf
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91323191"
---
# <a name="overview-of-azure-sql-managed-instance-management-operations"></a>Übersicht über die Verwaltungsvorgänge für Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL Managed Instance bietet Verwaltungsvorgänge, die Sie zum automatischen Bereitstellen von neuen verwalteten Instanzen, Aktualisieren von Instanzeigenschaften und Löschen von nicht mehr benötigten Instanzen verwenden können.

## <a name="what-are-management-operations"></a>Was sind Verwaltungsvorgänge?

Alle Verwaltungsvorgänge können wie folgt kategorisiert werden:

- Instanzbereitstellung (Erstellung neuer Instanzen)
- Instanzaktualisierung (Änderung von Instanzeigenschaften, z. B. virtuelle Kerne oder reservierter Speicher)
- Instanzlöschung

Für die Unterstützung von [Bereitstellungen in virtuellen Azure-Netzwerken](../../virtual-network/virtual-network-for-azure-services.md) und zur Sicherstellung der Isolation und Sicherheit für Kunden werden für SQL Managed Instance [virtuelle Cluster](connectivity-architecture-overview.md#high-level-connectivity-architecture) genutzt. Der virtuelle Cluster stellt eine dedizierte Gruppe mit isolierten virtuellen Computern dar, die im Subnetz des virtuellen Netzwerks des Kunden bereitgestellt werden. Grundsätzlich führt jede Bereitstellung einer verwalteten Instanz in einem leeren Subnetz dazu, dass ein neuer virtueller Cluster erstellt wird.

Nachfolgende Verwaltungsvorgänge für verwaltete Instanzen wirken sich möglicherweise auf den zugrunde liegenden virtuellen Cluster aus. Änderungen, die sich auf den zugrunde liegenden Cluster auswirken, haben unter Umständen Auswirkungen auf die Dauer von Verwaltungsvorgängen, da die Bereitstellung zusätzlicher virtueller Computern mit einem Mehraufwand verbunden ist, den Sie berücksichtigen müssen, wenn Sie neue Bereitstellungen oder Updates für vorhandene verwaltete Instanzen planen.


## <a name="duration"></a>Duration

Die Dauer von Vorgängen für den virtuellen Cluster kann variieren. Sie dauern aber in der Regel am längsten. 

Im Anschluss finden Sie einige übliche Werte (basierend auf vorhandenen Diensttelemetriedaten):

- **Erstellung von virtuellen Clustern**:  Die Erstellung ist ein synchroner Schritt der Vorgänge für die Instanzverwaltung. <br/> **90 % der Vorgänge werden innerhalb von vier Stunden abgeschlossen**.
- **Änderung der Größe von virtuellen Clustern (Vergrößerung oder Verkleinerung)** : Die Vergrößerung ist ein synchroner Schritt, und die Verkleinerung wird asynchron durchgeführt (ohne Auswirkung auf die Dauer von Instanzverwaltungsvorgängen). <br/>**90 % der Clustervergrößerungen werden in weniger als 2,5 Stunden abgeschlossen**.
- **Löschung von virtuellen Clustern**: Der Löschvorgang ist ein asynchroner Schritt, der in einem leeren virtuellen Cluster aber auch [manuell initiiert](virtual-cluster-delete.md) werden kann. In diesem Fall wird der Vorgang synchron durchgeführt. <br/>**90 % der Löschungen virtueller Cluster werden innerhalb von 1,5 Stunden abgeschlossen**.

Darüber hinaus kann die Verwaltung von Instanzen auch einen der Vorgänge in gehosteten Datenbanken umfassen. Dies führt zu einer längeren Dauer:

- **Anfügen von Datenbankdateien aus Azure Storage**:  Ein synchroner Schritt, z. B. das Hoch- oder Herunterskalieren von Computeressourcen (virtuelle Kerne) oder Speicher im Tarif „Universell“. <br/>**90 % dieser Vorgänge werden innerhalb von fünf Minuten abgeschlossen**.
- **Seeding der Always On-Verfügbarkeitsgruppe**: Ein synchroner Schritt, z. B. Compute (V-Kerne) oder die Skalierung des Speichers im Tarif „Unternehmenskritisch“ sowie die Änderung der Dienstebene von „Universell“ in „Unternehmenskritisch“ (oder umgekehrt). Die Dauer dieses Vorgangs ist proportional zur Gesamtgröße der Datenbank sowie zur aktuellen Datenbankaktivität (Anzahl aktiver Transaktionen). Aufgrund der Datenbankaktivität beim Aktualisieren einer Instanz kann die Gesamtdauer erheblich variieren. <br/>**90 % dieser Vorgänge werden mindestens mit 220 GB pro Stunde durchgeführt.**

Die folgenden Tabellen enthalten eine Zusammenfassung von Vorgängen und der jeweils üblichen Gesamtdauer auf der Grundlage der Vorgangskategorie:

**Kategorie: Bereitstellung**

|Vorgang  |Segment mit langer Ausführungsdauer  |Geschätzte Dauer  |
|---------|---------|---------|
|Erste Instanz in einem leeren Subnetz|Erstellung eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von 4 Stunden abgeschlossen.|
|Erste Instanz einer anderen Hardwaregeneration in einem nicht leeren Subnetz (z. B. erste Gen5-Instanz in einem Subnetz mit Gen4-Instanzen)|Erstellung eines virtuellen Clusters<sup>1</sup>|90 % der Vorgänge werden innerhalb von 4 Stunden abgeschlossen.|
|Nachfolgende Instanzerstellung im nicht leeren Subnetz (2. Instanz, 3. Instanz usw.)|Änderung der Größe eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von 2,5 Stunden abgeschlossen.|
| | | 

<sup>1</sup> Der virtuelle Cluster wird pro Hardwaregeneration erstellt.

**Kategorie: Aktualisieren**

|Vorgang  |Segment mit langer Ausführungsdauer  |Geschätzte Dauer  |
|---------|---------|---------|
|Änderung der Instanzeigenschaft (Administratorkennwort, Azure AD-Anmeldung, Azure-Hybridvorteil-Flag)|–|Bis zu 1 Minute|
|Zentrales Hoch-/Herunterskalieren des Instanzspeichers (Dienstebene „Universell“)|Anfügung von Datenbankdateien|90 % der Vorgänge werden innerhalb von fünf Minuten abgeschlossen.|
|Zentrales Hoch-/Herunterskalieren des Instanzspeichers (Tarif „Unternehmenskritisch“)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen.|
|Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Universell)|- Änderung der Größe eines virtuellen Clusters<br>- Anfügung von Datenbankdateien|90 % der Vorgänge werden innerhalb von 2,5 Stunden abgeschlossen.|
|Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Unternehmenskritisch)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen.|
|Änderung der Instanzdienstebene („Universell“ in „Unternehmenskritisch“ und umgekehrt)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen.|
| | | 

**Kategorie: Löschen**

|Vorgang  |Segment mit langer Ausführungsdauer  |Geschätzte Dauer  |
|---------|---------|---------|
|Instanzlöschung|Log Tail-Sicherung für alle Datenbanken|90 % der Vorgänge werden innerhalb einer Minute abgeschlossen.<br>Hinweis: Wenn die letzte Instanz im Subnetz gelöscht wird, wird bei diesem Vorgang das Löschen des virtuellen Clusters nach Ablauf von zwölf Stunden geplant.<sup>1</sup>|
|Löschung eines virtuellen Clusters (als vom Benutzer initiierter Vorgang)|Löschung eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von 1,5 Stunden abgeschlossen.|
| | | 

<sup>1</sup> Die aktuelle Konfiguration ist auf 12 Stunden festgelegt. Dies wird jedoch ggf. noch geändert. Falls Sie einen virtuellen Cluster früher löschen müssen (z. B. um das Subnetz freizugeben), helfen Ihnen die Informationen unter [Löschen eines Subnetzes nach Löschen einer verwalteten Instanz](virtual-cluster-delete.md) weiter.

## <a name="instance-availability"></a>Instanzverfügbarkeit

SQL Managed Instance **ist während Aktualisierungsvorgängen verfügbar**. Aufgrund des Failovers am Ende der Aktualisierung kommt es jedoch zu einer kurzen Downtime. Dank der [beschleunigten Datenbankwiederherstellung](../accelerated-database-recovery.md) dauert diese in der Regel bis zu zehn Sekunden. Das gilt auch bei unterbrochenen zeitintensiven Transaktionen.

SQL Managed Instance ist während Bereitstellungs- und Löschvorgängen für Clientanwendungen nicht verfügbar.

> [!IMPORTANT]
> Es wird nicht empfohlen, eine Skalierung der Compute- oder Speicherkapazität von Azure SQL Managed Instance oder eine Änderung der Dienstebene parallel zu zeitintensiven Transaktionen (Datenimport, Datenverarbeitungsaufträge, Neuerstellung des Index usw.) vorzunehmen. Durch das Failover der Datenbank am Ende des Vorgangs werden alle laufenden Transaktionen abgebrochen. 

## <a name="management-operations-steps"></a>Schritte von Verwaltungsvorgängen

Verwaltungsvorgänge bestehen aus mehreren Schritten. Mit der [Einführung der Vorgangs-API](management-operations-monitor.md) werden diese Schritte für eine Teilmenge von Vorgängen (Bereitstellung und Aktualisierung) verfügbar gemacht. Der Bereitstellungsvorgang umfasst drei Schritte, der Aktualisierungsvorgang sechs. Ausführliche Informationen zur Dauer von Vorgängen finden Sie im Abschnitt [Dauer von Verwaltungsvorgängen](#duration). Die Schritte sind in der Ausführungsreihenfolge aufgeführt.

### <a name="managed-instance-deployment-steps"></a>Schritte für die Bereitstellung einer verwalteten Instanz

|Schrittname  |Beschreibung des Schritts  |
|----|---------|
|Anforderungsvalidierung |Die übermittelten Parameter werden überprüft. Im Falle einer falschen Konfiguration tritt ein Fehler auf, und der Vorgang ist nicht erfolgreich. |
|Änderung der Größe/Erstellung eines virtuellen Clusters |Je nach Zustand des Subnetzes wird der virtuelle Cluster entweder erstellt, oder seine Größe wird geändert. |
|Start der neuen SQL-Instanz |Der SQL-Prozess wird im bereitgestellten virtuellen Cluster gestartet. |

### <a name="managed-instance-update-steps"></a>Schritte für die Aktualisierung einer verwalteten Instanz

|Schrittname  |Beschreibung des Schritts  |
|----|---------|
|Anforderungsvalidierung | Die übermittelten Parameter werden überprüft. Im Falle einer falschen Konfiguration tritt ein Fehler auf, und der Vorgang ist nicht erfolgreich. |
|Änderung der Größe/Erstellung eines virtuellen Clusters |Je nach Zustand des Subnetzes wird der virtuelle Cluster entweder erstellt, oder seine Größe wird geändert. |
|Start der neuen SQL-Instanz | Der SQL-Prozess wird im bereitgestellten virtuellen Cluster gestartet. |
|Seeding/Anfügen von Datenbankdateien |Abhängig von der Art des Aktualisierungsvorgangs wird entweder das Seeding der Datenbank oder das Anfügen von Datenbankdateien durchgeführt. |
|Failovervorbereitung und -durchführung |Nach dem Seeding von Daten oder dem erneuten Anfügen von Datenbankdateien wird das System für das Failover vorbereitet. Anschließend wird das Failover durchgeführt. **Dabei kommt es zu einer kurzen Downtime.** |
|Bereinigung der alte SQL-Instanz |Der alte SQL-Prozess wird aus dem virtuellen Cluster entfernt. |

> [!NOTE]
> Infolge der Skalierung von Instanzen durchläuft der zugrunde liegende virtuelle Cluster den Prozess der Freigabe nicht genutzter Kapazität und ggf. eine Kapazitätsdefragmentierung. Dies kann sich auf Instanzen auswirken, die nicht an den Erstellungs-/Skalierungsvorgängen beteiligt waren. 


## <a name="management-operations-cross-impact"></a>Wechselseitige Auswirkungen von Verwaltungsvorgängen

Verwaltungsvorgänge für verwaltete Instanzen können sich auf andere Verwaltungsvorgänge der Instanzen auswirken, die sich in demselben virtuellen Cluster befinden:

- **Zeitintensive Wiederherstellungsvorgänge** in einem virtuellen Cluster stellen Instanzerstellungs- oder Skalierungsvorgänge anderer Instanzen im gleichen Subnetz zurück.<br/>**Beispiel:** Wenn ein zeitintensiver Wiederherstellungsvorgang ausgeführt wird und eine Anforderung zum Erstellen oder Skalieren im gleichen Subnetz vorhanden ist, dauert es länger, bis diese Anforderung abgeschlossen wird, da auf die Beendigung des Wiederherstellungsvorgangs gewartet wird.

- **Nachfolgende Instanzerstellungs- oder Skalierungsvorgänge** werden durch zuvor initiierte Instanzerstellungen oder -skalierungen zurückgestellt, durch die eine Größenänderung des virtuellen Clusters initiiert wurde.<br/>**Beispiel:** Wenn im gleichen Subnetz unter dem gleichen virtuellen Cluster mehrere Anforderungen zum Erstellen und/oder Skalieren vorhanden sind und eine davon eine Größenänderung des virtuellen Clusters initiiert, dauern alle Anforderungen, die mehr als fünf Minuten nach der ursprünglichen Vorgangsanforderung übermittelt wurden, länger als erwartet, da bei diesen Anforderungen auf den Abschluss der Größenänderung gewartet werden muss, bevor sie fortgesetzt werden können.

- **Erstellungs-/Skalierungsvorgänge, die im fünfminütigen Fenster übermittelt werden**, werden in einem Batch zusammengefasst und parallel ausgeführt.<br/>**Beispiel:** Für alle Vorgänge, die in einem fünfminütigen Fenster übermittelt werden, wird nur eine Größenänderung des virtuellen Clusters ausgeführt (gemessen ab dem Zeitpunkt der Ausführung der ersten Vorgangsanforderung). Wenn eine andere Anforderung mehr als fünf Minuten nach der ersten Anforderung übermittelt wird, wartet sie, bis die Größenänderung des virtuellen Clusters abgeschlossen ist, bevor die Ausführung beginnt.

> [!IMPORTANT]
> Verwaltungsvorgänge, die aufgrund eines anderen aktuell ausgeführten Vorgangs zurückgestellt werden, werden automatisch fortgesetzt, sobald die Bedingungen für eine Fortsetzung erfüllt sind. Es ist keine Benutzeraktion erforderlich, um den vorübergehend angehaltenen Verwaltungsvorgang fortzusetzen.

## <a name="monitoring-management-operations"></a>Überwachen von Verwaltungsvorgängen

Informationen zum Überwachen des Fortschritts und Status von Verwaltungsvorgängen finden Sie unter [Überwachen der Verwaltungsvorgänge für Azure SQL Managed Instance](management-operations-monitor.md).

## <a name="canceling-management-operations"></a>Abbrechen von Verwaltungsvorgängen

Informationen zum Abbrechen von Verwaltungsvorgängen finden Sie unter [Abbrechen von Verwaltungsvorgängen für Azure SQL Managed Instance](management-operations-cancel.md).


## <a name="next-steps"></a>Nächste Schritte

- Im [Schnellstarthandbuch](instance-create-quickstart.md) erfahren Sie, wie Sie Ihre erste verwaltete Instanz erstellen.
- Eine Liste der Features und einen Funktionsvergleich finden Sie unter [Allgemeine SQL-Features](../database/features-comparison.md).
- Weitere Informationen zur VNET-Konfiguration finden Sie unter [Konfigurieren eines VNET für SQL Managed Instance](connectivity-architecture-overview.md).
- Eine Schnellstartanleitung, in der eine verwaltete Instanz erstellt und eine Datenbank aus einer Sicherungsdatei wiederhergestellt wird, finden Sie unter [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](instance-create-quickstart.md).
- Ein Tutorial zur Verwendung des Azure Database Migration Service für die Migration finden Sie unter [SQL Managed Instance-Migration mithilfe des Database Migration Service](../../dms/tutorial-sql-server-to-managed-instance.md).
