---
title: 'Grenzwerte und Einschränkungen: Hyperscale (Citus) – Azure Database for PostgreSQL'
description: Aktuelle Grenzwerte für Hyperscale (Citus)-Servergruppen
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: cfd57c60c4bfd60976eb28822c696412cabda212
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107023813"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-limits-and-limitations"></a>Azure Database for PostgreSQL: Einschränkungen von Hyperscale (Citus)

In den folgenden Abschnitten werden die Kapazitäts- und funktionalen Beschränkungen in Hyperscale (Citus) beschrieben.

## <a name="maximum-connections"></a>Maximale Anzahl der Verbindungen

Jede PostgreSQL-Verbindung (auch inaktive Verbindungen) verwendet mindestens 10 MB Arbeitsspeicher, daher ist es wichtig, gleichzeitige Verbindungen einzuschränken. Dies sind die Grenzwerte, die wir ausgewählt haben, um Knoten fehlerfrei zu halten:

* Koordinatorknoten
   * Maximale Anzahl von Verbindungen: 300
   * Maximale Anzahl Benutzerverbindungen: 297
* Workerknoten
   * Maximale Anzahl von Verbindungen: 600
   * Maximale Anzahl Benutzerverbindungen: 597

> [!NOTE]
> In einer Servergruppe mit aktivierten [Previewfunktionen](hyperscale-preview-features.md) unterscheiden sich die Verbindungsgrenzwerte für den Koordinator geringfügig:
>
> * Maximale Anzahl von Verbindungen des Koordinatorknotens
>    * 300 für 0 – 3 virtuelle Kerne
>    * 500 für 4 – 15 virtuelle Kerne
>    * 1\.000 für 16 und mehr virtuelle Kerne

Verbindungsversuche über diese Grenzwerte hinaus führen zu einem Fehler. Das System reserviert drei Verbindungen für die Überwachung von Knoten. Aus diesem Grund sind für Benutzerabfragen drei Verbindungen weniger verfügbar als Verbindungen insgesamt.

### <a name="connection-pooling"></a>Verbindungspooling

Das Einrichten neuer Verbindungen nimmt Zeit in Anspruch. Dies spricht gegen die meisten Anwendungen, die viele kurzlebige Verbindungen anfordern. Wir empfehlen die Verwendung eines Verbindungspoolers, um Transaktionen im Leerlauf zu verringern und vorhandene Verbindungen wiederzuverwenden. Weitere Informationen finden Sie in unserem [Blogbeitrag](https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/not-all-postgres-connection-pooling-is-equal/ba-p/825717).

Sie können Ihre eigene Verbindungspoolfunktion ausführen oder die von Azure verwaltete Lösung PgBouncer verwenden.

#### <a name="managed-pgbouncer-preview"></a>Verwaltete PgBouncer-Instanz (Vorschauversion)

> [!IMPORTANT]
> Die verwaltete Verbindungspoollösung PgBouncer in Hyperscale (Citus) ist derzeit als Vorschauversion verfügbar. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
>
> Eine vollständige Liste der anderen neuen Features finden Sie unter [Previewfunktionen für PostgreSQL – Hyperscale (Citus)](hyperscale-preview-features.md).

Verbindungspoollösungen wie PgBouncer ermöglichen es einer größeren Anzahl von Clients, gleichzeitig eine Verbindung mit dem Koordinatorknoten herzustellen. Anwendungen stellen eine Verbindung mit der Poollösung her, und die Poollösung leitet Befehle an die Zieldatenbank weiter.

Die Anzahl von Verbindungen, die aktiv in der Datenbank ausgeführt werden können, ändert sich nicht, wenn Clients eine Verbindung über PgBouncer herstellen. Stattdessen reiht PgBouncer über den Grenzwert hinausgehende Verbindungen in die Warteschlange ein und führt sie aus, wenn die Datenbank bereit ist.

Hyperscale (Citus) bietet jetzt eine verwaltete Instanz von PgBouncer für Servergruppen (Vorschauversion). Sie unterstützt bis zu 2.000 gleichzeitige Clientverbindungen.
Führen Sie die folgenden Schritte aus, um eine Verbindung über PgBouncer herzustellen:

1. Wechseln Sie im Azure-Portal zur Seite **Verbindungszeichenfolgen** für Ihre Servergruppe.
2. Aktivieren Sie das Kontrollkästchen **PgBouncer connection strings** (PgBouncer-Verbindungszeichenfolgen). (Die aufgelisteten Verbindungszeichenfolgen ändern sich.)
3. Aktualisieren Sie Clientanwendungen, sodass sie die neue Zeichenfolge zum Herstellen einer Verbindung verwenden.

## <a name="storage-scaling"></a>Speicherskalierung

Der Speicher auf Koordinator- und Workerknoten kann hochskaliert (vergrößert) werden, er kann aber nicht herunterskaliert werden.

## <a name="storage-size"></a>Speichergröße

Für Koordinator- und Workerknoten wird eine Speicherkapazität von bis zu 2 TiB unterstützt. Weitere Informationen finden Sie in den verfügbaren Speicheroptionen und der IOPS-Berechnung [oben](concepts-hyperscale-configuration-options.md#compute-and-storage) für Knoten- und Clustergrößen.

## <a name="database-creation"></a>Datenbankerstellung

Im Azure-Portal werden Anmeldeinformationen bereitgestellt, um eine Verbindung mit genau einer Datenbank pro Hyperscale (Citus)-Servergruppe herzustellen, der `citus`-Datenbank. Das Erstellen einer anderen Datenbank ist zurzeit nicht zulässig, und der CREATE DATABASE-Befehl verursacht einen Fehler.

## <a name="columnar-storage"></a>Spaltenbasierter Speicher

In Hyperscale (Citus) gelten derzeit die folgenden Einschränkungen für [spaltenbasierte Tabellen](concepts-hyperscale-columnar.md):

* Komprimierung auf dem Datenträger, nicht im Arbeitsspeicher
* Nur Anfügen (keine Unterstützung für UPDATE/DELETE)
* Keine Speicherplatzrückgewinnung (Transaktionen, für die ein Rollback ausgeführt wurde, können z. B. weiterhin Speicherplatz belegen)
* Keine Indexunterstützung, Indexscans oder Bitmapindexscans
* Keine TID-Scans
* Keine Stichprobenscans
* Keine TOAST-Unterstützung (große Werte werden inline unterstützt)
* Keine Unterstützung für ON CONFLICT-Anweisungen (mit Ausnahme von DO NOTHING-Aktionen ohne Angabe eines Ziels)
* Keine Unterstützung für Tupelsperren (SELECT... FOR SHARE, SELECT... FOR UPDATE)
* Keine Unterstützung für serialisierbare Isolationsstufen
* Unterstützung nur für PostgreSQL-Serverversionen ab Version 12
* Keine Unterstützung für Fremdschlüssel, UNIQUE-Einschränkungen oder Ausschlusseinschränkungen
* Keine Unterstützung für logische Decodierung
* Keine Unterstützung für knoteninterne parallele Scans
* Keine Unterstützung für AFTER... FOR EACH ROW-Trigger
* Keine nicht protokollierten spaltenbasierten Tabellen
* Keine temporären Spaltentabellen

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [eine Hyperscale (Citus)-Servergruppe im Portal erstellen](quickstart-create-hyperscale-portal.md).
