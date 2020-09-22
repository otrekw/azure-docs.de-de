---
title: 'Erweiterungen: Azure Database for PostgreSQL Flexible Server'
description: In diesem Artikel erhalten Sie Informationen zu den verfügbaren Postgres-Erweiterungen in Azure Database for PostgreSQL Flexible Server.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 1126c218f8e80b7d89183746890a3fae1357d29d
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90930573"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql---flexible-server"></a>PostgreSQL-Erweiterungen in Azure Database for PostgreSQL Flexible Server

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

PostgreSQL bietet die Möglichkeit, die Funktionalität Ihrer Datenbank mithilfe von Erweiterungen zu erweitern. Bei Erweiterungen werden mehrere zusammengehörige SQL-Objekte zu einem Paket gebündelt und mit nur einem Befehl in die Datenbank geladen oder daraus entfernt. Nach dem Laden in die Datenbank funktionieren Erweiterungen genauso wie integrierte Features.

## <a name="how-to-use-postgresql-extensions"></a>Wie werden PostgreSQL-Erweiterungen verwendet?
Bevor Sie PostgreSQL-Erweiterungen verwenden können, müssen diese in Ihrer Datenbank installiert werden. Zur Installation einer bestimmten Erweiterung führen Sie den Befehl [CREATE EXTENSION](https://www.postgresql.org/docs/current/sql-createextension.html) aus. Dieser Befehl lädt die Objektpakete in Ihre Datenbank.

Azure Database for PostgreSQL unterstützt einen Teil der wichtigsten Erweiterungen, die unten aufgeführt sind. Sie können diese Informationen auch abrufen, indem Sie `SHOW azure.extensions;` ausführen. Erweiterungen, die in diesem Artikel nicht aufgeführt sind, werden für Azure Database for PostgreSQL Flexible Server nicht unterstützt. Sie können in Azure Database for PostgreSQL keine eigenen Erweiterungen erstellen oder laden.


## <a name="postgres-12-extensions"></a>Postgres 12-Erweiterungen

Die folgenden Erweiterungen sind für Azure Database for PostgreSQL Flexible Server mit Postgres-Version 12 verfügbar. 

> [!div class="mx-tableFixed"]
> | **Erweiterung**| **Erweiterungsversion** | **Beschreibung** |
> |---|---|---|
> |[amcheck](https://www.postgresql.org/docs/12/amcheck.html)                    | 1.2             | Funktionen zum Überprüfen der Beziehungsintegrität|
> |[bloom](https://www.postgresql.org/docs/12/bloom.html)                    | 1.0             | Bloom-Zugriffsmethode: Index, der auf einer Signaturdatei basiert|
> |[btree_gin](https://www.postgresql.org/docs/12/btree-gin.html)                    | 1.3             | Unterstützung für die Indizierung häufiger Datentypen in GIN|
> |[btree_gist](https://www.postgresql.org/docs/12/btree-gist.html)                   | 1.5             | Unterstützung für die Indizierung häufiger Datentypen in GIST|
> |[citext](https://www.postgresql.org/docs/12/citext.html)                       | 1.6             | Datentyp für Zeichenfolgen ohne Berücksichtigung der Groß-/Kleinschreibung|
> |[cube](https://www.postgresql.org/docs/12/cube.html)                         | 1.4             | Datentyp für mehrdimensionale Cubes|
> |[dblink](https://www.postgresql.org/docs/12/dblink.html)                       | 1.2             | Verbindungsherstellung mit anderen PostgreSQL-Datenbanken aus einer Datenbank|
> |[dict_int](https://www.postgresql.org/docs/12/dict-int.html)                     | 1.0             | Textsuche-Wörterbuchvorlage für Integerwerte|
> |[dict_xsyn](https://www.postgresql.org/docs/12/dict-xsyn.html)                     | 1.0             | Wörterbuchvorlage für die Textsuche für die erweiterte Verarbeitung von Synonymen|
> |[earthdistance](https://www.postgresql.org/docs/12/earthdistance.html)                | 1.1             | Berechnung von Großkreisentfernungen auf der Erdoberfläche|
> |[fuzzystrmatch](https://www.postgresql.org/docs/12/fuzzystrmatch.html)                | 1.1             | Ermittlung von Ähnlichkeiten und der Entfernung zwischen Zeichenfolgen|
> |[hstore](https://www.postgresql.org/docs/12/hstore.html)                       | 1.6             | Datentyp zum Speichern von Schlüssel-Wert-Paaren|
> |[intagg](https://www.postgresql.org/docs/12/intagg.html)                     | 1.1             | Integeraggregator und -enumerator. (Veraltet)|
> |[intarray](https://www.postgresql.org/docs/12/intarray.html)                     | 1.2             | Funktionen, Operatoren und Indexunterstützung für 1-D-Arrays mit Integerwerten|
> |[isn](https://www.postgresql.org/docs/12/isn.html)                          | 1.2             | Datentypen für internationale Produktnummerierungsnormen|
> |[ltree](https://www.postgresql.org/docs/12/ltree.html)                        | 1.1             | Datentyp für hierarchische baumähnliche Strukturen|
> |[pageinspect](https://www.postgresql.org/docs/12/pageinspect.html)                        | 1.7             | Überprüft den Inhalt von Datenbankseiten im Detail|
> |[pg_buffercache](https://www.postgresql.org/docs/12/pgbuffercache.html)               | 1.3             | Untersuchung des freigegebenen Puffercaches|
> |[pg_freespacemap](https://www.postgresql.org/docs/12/pgfreespacemap.html)               | 1.2             | Untersucht Free Space Map (FSM)|
> |[pg_prewarm](https://www.postgresql.org/docs/12/pgprewarm.html)                   | 1.2             | „Vorwärmung“ von Beziehungsdaten|
> |[pg_stat_statements](https://www.postgresql.org/docs/12/pgstatstatements.html)           | 1.7             | Nachverfolgung von Ausführungsstatistiken aller ausgeführten SQL-Anweisungen|
> |[pg_trgm](https://www.postgresql.org/docs/12/pgtrgm.html)                      | 1.4             | Messung von Textähnlichkeit und Indexsuche basierend auf Trigrammen|
> |[pg_visibility](https://www.postgresql.org/docs/12/pgvisibility.html)                      | 1.2             | Überprüft die Informationen zur Zuordnung von Sichtbarkeiten (VM) und Sichtbarkeit auf Seitenebene|
> |[pgaudit](https://www.pgaudit.org/)                     | 1.4             | Überwachungsfunktionen|
> |[pgcrypto](https://www.postgresql.org/docs/12/pgcrypto.html)                     | 1.3             | Kryptografische Funktionen|
> |[pgrowlocks](https://www.postgresql.org/docs/12/pgrowlocks.html)                   | 1.2             | Anzeige von Sperrinformationen auf Zeilenebene|
> |[pgstattuple](https://www.postgresql.org/docs/12/pgstattuple.html)                  | 1.5             | Anzeige von Statistiken auf Tupelebene|
> |[plpgsql](https://www.postgresql.org/docs/12/plpgsql.html)                      | 1.0             | PL/pgSQL: Prozedurale Sprache|
> |[postgis](https://www.postgis.net/)                      | 3.0.0           | PostGIS: Geometrie, Geografie und räumliche Rastertypen und -funktionen|
> |[postgres_fdw](https://www.postgresql.org/docs/12/postgres-fdw.html)                 | 1.0             | Fremddaten-Wrapper für PostgreSQL-Remoteserver|
> |[sslinfo](https://www.postgresql.org/docs/12/sslinfo.html)                    | 1.2             | Informationen zu SSL-Zertifikaten|
> |[tsm_system_rows](https://www.postgresql.org/docs/12/tsm-system-rows.html)                    | 1.0             |  TABLESAMPLE-Methode, die die Anzahl von Zeilen als Grenzwert akzeptiert|
> |[tsm_system_time](https://www.postgresql.org/docs/12/tsm-system-time.html)                    | 1.0             |  TABLESAMPLE-Methode, die die Zeit in Millisekunden als Grenzwert akzeptiert|
> |[unaccent](https://www.postgresql.org/docs/12/unaccent.html)                     | 1.1             | Wörterbuch für Textsuche mit Entfernung von Akzenten|
> |[uuid-ossp](https://www.postgresql.org/docs/12/uuid-ossp.html)                    | 1.1             | Generierung von UUIDs (Universally Unique Identifiers)|

## <a name="postgres-11-extensions"></a>Postgres 11-Erweiterungen

Die folgenden Erweiterungen sind für Azure Database for PostgreSQL Flexible Server mit Postgres-Version 11 verfügbar. 

> [!div class="mx-tableFixed"]
> | **Erweiterung**| **Erweiterungsversion** | **Beschreibung** |
> |---|---|---|
> |[amcheck](https://www.postgresql.org/docs/11/amcheck.html)                    | 1.1             | Funktionen zum Überprüfen der Beziehungsintegrität|
> |[bloom](https://www.postgresql.org/docs/11/bloom.html)                    | 1.0             | Bloom-Zugriffsmethode: Index, der auf einer Signaturdatei basiert|
> |[btree_gin](https://www.postgresql.org/docs/11/btree-gin.html)                    | 1.3             | Unterstützung für die Indizierung häufiger Datentypen in GIN|
> |[btree_gist](https://www.postgresql.org/docs/11/btree-gist.html)                   | 1.5             | Unterstützung für die Indizierung häufiger Datentypen in GIST|
> |[citext](https://www.postgresql.org/docs/11/citext.html)                       | 1.5             | Datentyp für Zeichenfolgen ohne Berücksichtigung der Groß-/Kleinschreibung|
> |[cube](https://www.postgresql.org/docs/11/cube.html)                         | 1.4             | Datentyp für mehrdimensionale Cubes|
> |[dblink](https://www.postgresql.org/docs/11/dblink.html)                       | 1.2             | Verbindungsherstellung mit anderen PostgreSQL-Datenbanken aus einer Datenbank|
> |[dict_int](https://www.postgresql.org/docs/11/dict-int.html)                     | 1.0             | Textsuche-Wörterbuchvorlage für Integerwerte|
> |[dict_xsyn](https://www.postgresql.org/docs/11/dict-xsyn.html)                     | 1.0             | Wörterbuchvorlage für die Textsuche für die erweiterte Verarbeitung von Synonymen|
> |[earthdistance](https://www.postgresql.org/docs/11/earthdistance.html)                | 1.1             | Berechnung von Großkreisentfernungen auf der Erdoberfläche|
> |[fuzzystrmatch](https://www.postgresql.org/docs/11/fuzzystrmatch.html)                | 1.1             | Ermittlung von Ähnlichkeiten und der Entfernung zwischen Zeichenfolgen|
> |[hstore](https://www.postgresql.org/docs/11/hstore.html)                       | 1.5             | Datentyp zum Speichern von Schlüssel-Wert-Paaren|
> |[intagg](https://www.postgresql.org/docs/11/intagg.html)                     | 1.1             | Integeraggregator und -enumerator. (Veraltet)|
> |[intarray](https://www.postgresql.org/docs/11/intarray.html)                     | 1.2             | Funktionen, Operatoren und Indexunterstützung für 1-D-Arrays mit Integerwerten|
> |[isn](https://www.postgresql.org/docs/11/isn.html)                          | 1.2             | Datentypen für internationale Produktnummerierungsnormen|
> |[ltree](https://www.postgresql.org/docs/11/ltree.html)                        | 1.1             | Datentyp für hierarchische baumähnliche Strukturen|
> |[pageinspect](https://www.postgresql.org/docs/11/pageinspect.html)                        | 1.7             | Überprüft den Inhalt von Datenbankseiten im Detail|
> |[pg_buffercache](https://www.postgresql.org/docs/11/pgbuffercache.html)               | 1.3             | Untersuchung des freigegebenen Puffercaches|
> |[pg_freespacemap](https://www.postgresql.org/docs/11/pgfreespacemap.html)               | 1.2             | Untersucht Free Space Map (FSM)|
> |[pg_prewarm](https://www.postgresql.org/docs/11/pgprewarm.html)                   | 1.2             | „Vorwärmung“ von Beziehungsdaten|
> |[pg_stat_statements](https://www.postgresql.org/docs/11/pgstatstatements.html)           | 1.6             | Nachverfolgung von Ausführungsstatistiken aller ausgeführten SQL-Anweisungen|
> |[pg_trgm](https://www.postgresql.org/docs/11/pgtrgm.html)                      | 1.4             | Messung von Textähnlichkeit und Indexsuche basierend auf Trigrammen|
> |[pg_visibility](https://www.postgresql.org/docs/11/pgvisibility.html)                      | 1.2             | Überprüft die Informationen zur Zuordnung von Sichtbarkeiten (VM) und Sichtbarkeit auf Seitenebene|
> |[pgaudit](https://www.pgaudit.org/)                     | 1.3.1             | Überwachungsfunktionen|
> |[pgcrypto](https://www.postgresql.org/docs/11/pgcrypto.html)                     | 1.3             | Kryptografische Funktionen|
> |[pgrowlocks](https://www.postgresql.org/docs/11/pgrowlocks.html)                   | 1.2             | Anzeige von Sperrinformationen auf Zeilenebene|
> |[pgstattuple](https://www.postgresql.org/docs/11/pgstattuple.html)                  | 1.5             | Anzeige von Statistiken auf Tupelebene|
> |[plpgsql](https://www.postgresql.org/docs/11/plpgsql.html)                      | 1.0             | PL/pgSQL: Prozedurale Sprache|
> |[postgis](https://www.postgis.net/)                      | 2.5.1           | PostGIS: Geometrie, Geografie und räumliche Rastertypen und -funktionen|
> |[postgres_fdw](https://www.postgresql.org/docs/11/postgres-fdw.html)                 | 1.0             | Fremddaten-Wrapper für PostgreSQL-Remoteserver|
> |[sslinfo](https://www.postgresql.org/docs/11/sslinfo.html)                    | 1.2             | Informationen zu SSL-Zertifikaten|
> |[tablefunc](https://www.postgresql.org/docs/11/tablefunc.html)                    | 1.0             | Funktionen zum Bearbeiten gesamter Tabellen, einschließlich Kreuztabelle|
> |[tsm_system_rows](https://www.postgresql.org/docs/11/tsm-system-rows.html)                    | 1.0             |  TABLESAMPLE-Methode, die die Anzahl von Zeilen als Grenzwert akzeptiert|
> |[tsm_system_time](https://www.postgresql.org/docs/11/tsm-system-time.html)                    | 1.0             |  TABLESAMPLE-Methode, die die Zeit in Millisekunden als Grenzwert akzeptiert|
> |[unaccent](https://www.postgresql.org/docs/11/unaccent.html)                     | 1.1             | Wörterbuch für Textsuche mit Entfernung von Akzenten|
> |[uuid-ossp](https://www.postgresql.org/docs/11/uuid-ossp.html)                    | 1.1             | Generierung von UUIDs (Universally Unique Identifiers)|


## <a name="pg_prewarm"></a>pg_prewarm

Die pg_prewarm-Erweiterung lädt relationale Daten in den Cache. Durch das Vorwärmen der Caches weisen Ihre Abfragen bei der ersten Ausführung nach einem Neustart bessere Reaktionszeiten auf. Die automatische prewarm-Funktion ist in Azure Database for PostgreSQL Flexible Server aktuell nicht verfügbar.


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie eine Erweiterung fehlt, die Sie verwenden möchten, lassen Sie es uns wissen. Stimmen Sie in unserem [Feedbackforum](https://feedback.azure.com/forums/597976-azure-database-for-postgresql) für vorhandene Anfragen ab, oder geben Sie uns weiteres Feedback.