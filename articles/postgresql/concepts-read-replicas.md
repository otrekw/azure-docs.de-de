---
title: Lesereplikate – Azure Database for PostgreSQL – Einzelserver
description: In diesem Artikel wird das Feature für Lesereplikate in Azure Database for PostgreSQL (Einzelserver) beschrieben.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/10/2020
ms.openlocfilehash: 608740ea52cf82485bae073d9679107ac52baa28
ms.sourcegitcommit: cd0a1ae644b95dbd3aac4be295eb4ef811be9aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88611125"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>Lesereplikate in Azure Database for PostgreSQL – Einzelserver

Mit dem Feature für Lesereplikate können Sie Daten von einem Azure Database for PostgreSQL-Server auf einen schreibgeschützten Server replizieren. Sie können vom Masterserver bis zu fünf Replikate replizieren. Replikate werden mithilfe der nativen Replikationstechnologie der PostgreSQL-Engine asynchron aktualisiert.

Replikate sind neue Server, die ähnlich wie reguläre Azure Database for PostgreSQL-Server verwaltet werden. Für jedes Lesereplikat werden Ihnen die bereitgestellten Computeressourcen in Form von virtuellen Kernen sowie der Speicher in GB/Monat in Rechnung gestellt.

Erfahren Sie, wie Sie [Replikate erstellen und verwalten](howto-read-replicas-portal.md).

## <a name="when-to-use-a-read-replica"></a>Einsatzmöglichkeiten von Lesereplikaten
Das Feature für Lesereplikate kann die Leistung und Skalierung von leseintensiven Workloads verbessern. Leseworkloads können in den Replikaten isoliert werden, während Schreibworkloads an den Masterserver weitergeleitet werden können.

In einem häufig anzutreffenden Szenario verwenden BI- und Analyseworkloads das Lesereplikat als Datenquelle für die Berichterstellung.

Da Replikate schreibgeschützt sind, führen sie nicht direkt zu einer verringerten Auslastung der Schreibkapazität auf dem Master. Dieses Feature ist nicht auf schreibintensive Workloads ausgerichtet.

Das Feature für Lesereplikate verwendet die asynchrone Replikation von PostgreSQL. Das Feature ist nicht für synchrone Replikationsszenarien vorgesehen. Zwischen dem Masterserver und dem Replikat entsteht eine messbare Verzögerung. Letztendlich sind die Daten auf dem Replikat mit den Daten auf dem Masterserver konsistent. Verwenden Sie das Feature für Workloads, für die diese Verzögerung akzeptabel ist.

## <a name="cross-region-replication"></a>Regionsübergreifende Replikation
Sie können über Ihren Masterserver ein Lesereplikat in einer anderen Region erstellen. Die regionsübergreifende Replikation kann beispielsweise hilfreich sein, um die Notfallwiederherstellung zu planen oder Daten näher beim Benutzer bereitzustellen.

>[!NOTE]
> Server im Tarif „Basic“ unterstützen nur die Replikation in derselben Region.

Ein Masterserver kann in jeder beliebigen [Azure Database for PostgreSQL-Region](https://azure.microsoft.com/global-infrastructure/services/?products=postgresql) vorhanden sein. Ein Masterserver kann ein Replikat in der gekoppelten Region oder den universellen Replikatregionen besitzen. Die folgende Abbildung zeigt, welche Replikatregionen entsprechend Ihrer Masterregion verfügbar sind.

[ ![Lesereplikatregionen](media/concepts-read-replica/read-replica-regions.png)](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>Universelle Replikatregionen
Sie können jederzeit ein Lesereplikat in einer der folgenden Regionen erstellen, unabhängig davon, wo sich der Masterserver befindet. Dies sind die universellen Replikatregionen:

„Australien, Osten“, „Australien, Südosten“, „USA, Mitte“, „Asien, Osten“, „USA, Osten“, „USA, Osten 2“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „Europa, Westen“, „USA, Westen“, „USA, Westen 2“, USA, Westen-Mitte

### <a name="paired-regions"></a>Regionspaare
Zusätzlich zu den universellen Replikatregionen können Sie ein Lesereplikat in der gekoppelten Azure-Region Ihres Masterservers erstellen. Sollte Ihnen Ihr Regionspaar nicht bekannt sein, lesen Sie den Artikel [Gekoppelte Azure-Regionen](../best-practices-availability-paired-regions.md).

Wenn Sie für die Notfallwiederherstellungsplanung regionsübergreifende Replikate verwenden, empfehlen wir Ihnen, das Replikat in der gekoppelten Region und nicht in einer der anderen Regionen zu erstellen. Regionspaare vermeiden gleichzeitige Aktualisierungen und priorisieren die physische Isolation und die Datenresidenz.  

Es gibt Einschränkungen: 

* Regionale Verfügbarkeit: Azure Database for PostgreSQL ist in den Regionen „Frankreich, Mitte“ und „VAE, Norden“ und „Deutschland, Mitte“ verfügbar. Die entsprechenden gekoppelten Regionen sind allerdings nicht verfügbar.
    
* Unidirektionale Paare: Einige Azure-Regionen werden nur in eine Richtung gekoppelt. Zu diesen Regionen zählen „Indien, Westen“ und „Brasilien, Süden“. 
   Das bedeutet, dass ein Masterserver in der Region „Indien, Westen“ ein Replikat in „Indien, Süden“ erstellen kann. Ein Masterserver in der Region „Indien, Süden“ kann jedoch kein Replikat in „Indien, Westen“ erstellen. Der Grund: Die sekundäre Region von „Indien, Westen“ ist zwar „Indien, Süden“, die sekundäre Region von „Indien, Süden“ ist jedoch nicht „Indien, Westen“.


## <a name="create-a-replica"></a>Erstellen eines Replikats
Wenn Sie den Workflow zum Erstellen eines Replikats starten, wird ein leerer Azure Database for PostgreSQL-Server erstellt. Der neue Server wird mit den Daten gefüllt, die auf dem Masterserver vorhanden waren. Die Erstellungszeit hängt von der Datenmenge auf dem Masterserver und der verstrichenen Zeit seit der letzten wöchentlichen vollständigen Sicherung ab. Dieser Zeitraum kann wenige Minuten bis zu mehrere Stunden umfassen.

Jedes Replikat ist für die [automatische Vergrößerung](concepts-pricing-tiers.md#storage-auto-grow) von Speicher aktiviert. Das Feature der automatischen Vergrößerung sorgt dafür, dass im Replikat genügend Speicherplatz für die replizierten Daten vorhanden ist, und verhindert damit Unterbrechungen bei der Replikation durch Fehler aufgrund von unzureichendem Speicher.

Das Feature für Lesereplikate verwendet die physische Replikation von PostgreSQL, nicht die logische Replikation. Die Streamingreplikation anhand von Replikationsslots ist der Standardbetriebsmodus. Bei Bedarf wird zum Aufholen der Protokollversand verwendet.

[Erfahren Sie, wie Sie ein Lesereplikat im Azure-Portal erstellen](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Herstellen einer Verbindung mit einem Replikat
Wenn Sie ein Replikat erstellen, erbt dieses weder die Firewallregeln noch den VNET-Dienstendpunkt des Masterservers. Diese Regeln müssen separat für das Replikat eingerichtet werden.

Das Replikat erbt das Administratorkonto vom Masterserver. Alle Benutzerkonten auf dem Masterserver werden auf die Lesereplikate repliziert. Sie können nur mit denjenigen Benutzerkonten eine Verbindung mit einem Lesereplikat herstellen, die auf dem Masterserver verfügbar sind.

Sie können wie bei einem normalen Azure Database for PostgreSQL-Server anhand des Hostnamens und eines gültigen Benutzerkontos eine Verbindung mit dem Replikat herstellen. Für einen Server namens **my replica** mit dem Administratorbenutzernamen **myadmin** können Sie mit psql eine Verbindung mit dem Replikat herstellen:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

Geben Sie an der Eingabeaufforderung das Kennwort für das Benutzerkonto ein.

## <a name="monitor-replication"></a>Überwachen der Replikation
Azure Database for PostgreSQL stellt zwei Metriken zum Überwachen der Replikation bereit. Die beiden Metriken sind **Maximale Verzögerung zwischen Replikaten** und **Replikatverzögerung**. Informationen zum Anzeigen dieser Metriken finden Sie im Abschnitt **Monitor a replica** (Überwachen eines Replikats) im Artikel [Howto read replicas](howto-read-replicas-portal.md) (Gewusst wie: Lesereplikate).

Die Metrik **Maximale Verzögerung zwischen Replikaten** zeigt die Verzögerung in Bytes zwischen dem Master und dem Replikat mit der größten Verzögerung an. Diese Metrik steht nur auf dem Masterserver zur Verfügung.

Die Metrik **Replikatverzögerung** zeigt die Zeit seit der letzten wiedergegebenen Transaktion an. Wenn auf dem Masterserver keine Transaktionen stattfinden, gibt die Metrik diese Verzögerungszeit wieder. Diese Metrik steht nur für Replikatserver zur Verfügung. Die Metrik „Replikatverzögerung“ wird aus der Ansicht `pg_stat_wal_receiver` berechnet:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp());
```

Richten Sie eine Benachrichtigung ein, die Sie informiert, wenn die Replikatverzögerung einen für Ihre Workload nicht akzeptablen Wert erreicht. 

Um weitere Erkenntnisse zu erhalten, können Sie die Replikationsverzögerung in Bytes auf allen Replikaten direkt vom Masterserver abfragen.

In PostgreSQL, Version 10:

```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

In PostgreSQL, Version 9.6 und früher:

```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Wenn ein Masterserver oder ein Lesereplikat neu gestartet wird, spiegelt die Metrik „Replikatverzögerung“ die benötigte Zeit zum Neustarten und anschließenden Aufholen wider.

## <a name="stop-replication"></a>Beenden der Replikation
Sie können die Replikation zwischen einem Masterserver und einem Replikat beenden. Durch die Beendigungsaktion wird das Replikat neu gestartet, und die zugehörigen Replikationseinstellungen werden entfernt. Sobald die Replikation zwischen einem Masterserver und einem Lesereplikat beendet wurde, wird das Replikat zu einem eigenständigen Server. Bei den Daten auf diesem eigenständigen Server handelt es sich um die Daten, die zu dem Zeitpunkt, als der Befehl zum Beenden der Replikation gestartet wurde, auf dem Replikatserver vorhanden waren. Der eigenständige Server wird nicht zusammen mit dem Masterserver aktualisiert.

> [!IMPORTANT]
> Der eigenständige Server kann nicht wieder in ein Replikat umgewandelt werden.
> Stellen Sie vor dem Beenden der Replikation auf einem Lesereplikat sicher, dass das Replikat alle erforderlichen Daten enthält.

Wenn Sie die Replikation beenden, verliert das Replikat alle Links zu seinem vorherigen Master und zu anderen Replikaten.

Erfahren Sie, wie Sie die [Replikation auf ein Replikat beenden](howto-read-replicas-portal.md).

## <a name="failover"></a>Failover
Zwischen Master- und Replikatservern erfolgt kein automatisiertes Failover. 

Da die Replikation asynchron erfolgt, gibt es eine Verzögerung zwischen dem Master und dem Replikat. Die Verzögerungsdauer kann durch eine Reihe von Faktoren beeinflusst werden, z.B. durch den Umfang der Workload auf dem Masterserver und die Latenzzeit zwischen Rechenzentren. In den meisten Fällen beträgt die Replikatverzögerung einige Sekunden bis zu einigen Minuten. Sie können die tatsächliche Replikatverzögerung mithilfe der Metrik *Replikatverzögerung* nachverfolgen, die für jedes Replikat verfügbar ist. Diese Metrik zeigt die seit der letzten wiedergegebenen Transaktion verstrichene Zeit an. Es wird empfohlen, die durchschnittliche Verzögerung zu ermitteln, indem Sie die Replikatverzögerung über einen bestimmten Zeitraum hinweg beobachten. Sie können eine Warnung für die Replikatverzögerung festlegen, sodass Sie Maßnahmen ergreifen können, wenn sie sich außerhalb des erwarteten Bereichs befindet.

> [!Tip]
> Wenn Sie ein Failover auf das Replikat durchführen, zeigt die Verzögerung zum Zeitpunkt der Trennung des Replikats vom Master an, wie viel Daten verloren gehen.

Gehen Sie folgendermaßen vor, nachdem Sie entschieden haben, ein Failover auf ein Replikat durchzuführen: 

1. Beenden der Replikation auf das Replikat<br/>
   Dieser Schritt ist erforderlich, damit der Replikatserver Schreibvorgänge akzeptieren kann. Im Rahmen dieses Vorgangs wird der Replikatserver neu gestartet und vom Master getrennt. Nachdem Sie die das Beenden der Replikation gestartet haben, dauert der Abschluss des Back-End-Prozesse in der Regel etwa 2 Minuten. Informationen zu den Auswirkungen dieser Aktion finden Sie im Abschnitt [Beenden der Replikation](#stop-replication) in diesem Artikel.
    
2. Verweisen der Anwendung auf das (ehemalige) Replikat<br/>
   Jeder Server verfügt über eine eindeutige Verbindungszeichenfolge. Aktualisieren Sie Ihre Anwendung so, dass Sie auf das (ehemalige) Replikat und nicht auf den Master verweist.
    
Wenn die Anwendung erfolgreich Lese- und Schreibvorgänge verarbeitet, haben Sie das Failover abgeschlossen. Die Ausfallzeit Ihrer Anwendung hängt davon ab, ob Sie ein Problem erkennen und die oben beschriebenen Schritte 1 und 2 ausführen.


## <a name="considerations"></a>Überlegungen

In diesem Abschnitt werden Aspekte des Features für Lesereplikate zusammengefasst.

### <a name="prerequisites"></a>Voraussetzungen
Lesereplikate und [logische Decodierung](concepts-logical.md) sind beide vom Write-Ahead-Protokoll von Postgres abhängig. Diese beiden Features erfordern unterschiedliche Ebenen der Protokollierung durch Postgres. Die logische Decodierung erfordert einen höheren Grad an Protokollierung als Lesereplikate.

Um den richtigen Grad an Protokollierung zu konfigurieren, verwenden Sie den Parameter zur Unterstützung der Azure-Replikation. Für die Unterstützung der Azure-Replikation gibt es drei Einstellungsoptionen:

* **Off**: legt die wenigsten Informationen im Write-Ahead-Protokoll ab. Diese Einstellung ist für die meisten Azure Database for PostgreSQL-Server nicht verfügbar.  
* **Replica**: ausführlichere Informationen als bei Wahl von **Off**. Dies ist der erforderlich Mindestgrad an Protokollierung, damit [Lesereplikate](concepts-read-replicas.md) funktionieren. Das ist die Standardeinstellung auf den meisten Servern.
* **Logical**: noch ausführlichere Informationen als bei Wahl von **Replica**. Dies ist der erforderlich Mindestgrad an Protokollierung, damit die logische Decodierung funktioniert. Lesereplikate funktionieren auch bei dieser Einstellung.

Der Server muss nach einer Änderung dieses Parameters neu gestartet werden. Intern legt dieser Parameter die Postgres-Parameter `wal_level`, `max_replication_slots` und `max_wal_senders` fest.

### <a name="new-replicas"></a>Neue Replikate
Ein Lesereplikat wird als neuer Azure Database for PostgreSQL-Server erstellt. Ein vorhandener Server kann nicht in ein Replikat umgewandelt werden. Es kann kein Replikat eines anderen Lesereplikats erstellt werden.

### <a name="replica-configuration"></a>Replikatkonfiguration
Ein Replikat wird mit den gleichen Compute- und Speichereinstellungen erstellt wie der Master. Nach der Erstellung eines Replikats können einige Einstellungen geändert werden, einschließlich des Aufbewahrungszeitraums für Speicher und Sicherungen.

Firewallregeln, VNET-Regeln und Parametereinstellungen werden beim Erstellen eines Replikats oder danach nicht vom Masterserver geerbt.

### <a name="scaling"></a>Skalierung
Skalieren virtueller Kerne zwischen „Universell“ und „Arbeitsspeicheroptimiert“:
* PostgreSQL erfordert, dass die Einstellung `max_connections` auf einem sekundären Server [größer oder gleich der Einstellung auf dem primären Server](https://www.postgresql.org/docs/current/hot-standby.html) ist. Andernfalls wird der sekundäre Server nicht gestartet.
* In Azure Database for PostgreSQL ist die maximal zulässige Anzahl von Verbindungen für jeden Server an die Compute-SKU gebunden, da Verbindungen Speicher belegen. Weitere Informationen zur [Zuordnung zwischen „max_connections“ und Compute-SKUs](concepts-limits.md)
* **Hochskalieren**: Skalieren Sie zuerst die Computekapazität eines Replikats und dann den primären Server hoch. Diese Reihenfolge verhindert, dass die `max_connections`-Anforderung durch Fehler beeinträchtigt wird.
* **Herunterskalieren**: Skalieren Sie zuerst die Computekapazität des primären Servers und dann das Replikat herunter. Wenn Sie auf das Replikat eine geringere Skalierung anwenden als auf den primären Server, tritt ein Fehler auf, weil die `max_connections`-Anforderung verletzt wird.

Skalieren des Speichers:
* Für alle Replikate ist die automatische Speichervergrößerung aktiviert. Dies soll verhindern, dass aufgrund eines vollen Replikatspeichers Replikationsprobleme auftreten. Diese Einstellung kann nicht deaktiviert werden.
* Sie können den Speicher wie bei jedem anderen Server auch manuell hochskalieren.


### <a name="basic-tier"></a>Basic-Tarif
Server im Tarif „Basic“ unterstützen nur die Replikation in derselben Region.

### <a name="max_prepared_transactions"></a>max_prepared_transactions
Für PostgreSQL [muss](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAX-PREPARED-TRANSACTIONS) der Wert des Parameters `max_prepared_transactions` auf dem Lesereplikat mindestens so groß sein wie der Wert auf dem Masterserver. Andernfalls wird das Replikat nicht gestartet. Wenn Sie `max_prepared_transactions` auf dem Masterserver ändern möchten, ändern Sie den Wert zunächst auf den Replikaten.

### <a name="stopped-replicas"></a>Beendete Replikate
Wenn Sie die Replikation zwischen einem Masterserver und einem Lesereplikat beenden, wird das Replikat neu gestartet, um diese Änderung anzuwenden. Das beendete Replikat wird zu einem eigenständigen Server, der sowohl Lese- als auch Schreibvorgänge akzeptiert. Der eigenständige Server kann nicht wieder in ein Replikat umgewandelt werden.

### <a name="deleted-master-and-standalone-servers"></a>Gelöschte Masterserver und eigenständige Server
Wenn ein Masterserver gelöscht wird, werden alle zugehörigen Lesereplikate zu eigenständigen Servern. Die Replikate werden neu gestartet, um diese Änderung anzuwenden.

## <a name="next-steps"></a>Nächste Schritte
* Erhalten Sie Informationen zum [Erstellen und Verwalten von Lesereplikaten im Azure-Portal](howto-read-replicas-portal.md).
* Erfahren Sie, wie Sie [Lesereplikate in der Azure-Befehlszeilenschnittstelle (Azure CLI) und der REST-API erstellen und verwalten](howto-read-replicas-cli.md).
