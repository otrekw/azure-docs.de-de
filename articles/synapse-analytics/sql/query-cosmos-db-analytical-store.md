---
title: Abfragen von Azure Cosmos DB-Daten mithilfe von SQL Serverless in Azure Synapse Link (Vorschau)
description: In diesem Artikel erfahren Sie, wie Sie Azure Cosmos DB mit SQL Serverless in Azure Synapse Link abfragen (Vorschau).
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 09/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: c326aed172bb8159185829f80d66e8e00496aad2
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92057806"
---
# <a name="query-azure-cosmos-db-data-using-sql-serverless-in-azure-synapse-link-preview"></a>Abfragen von Azure Cosmos DB-Daten mithilfe von SQL Serverless in Azure Synapse Link (Vorschau)

Mit Synapse SQL Serverless (ehemals SQL On-Demand) können Sie Daten in Ihren Azure Cosmos DB-Containern mit [Azure Synapse Link](../../cosmos-db/synapse-link.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)-Aktivierung nahezu in Echtzeit analysieren, ohne dass sich dies auf die Leistung Ihrer Transaktionsworkloads auswirkt. SQL Serverless bietet eine vertraute T-SQL-Syntax zum Abfragen von Daten aus dem [Analysespeicher](../../cosmos-db/analytical-store-introduction.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) sowie integrierte Konnektivität mit einer Vielzahl von BI-Tools und Ad-hoc-Abfragetools über die T-SQL-Schnittstelle.

> [!NOTE]
> Die Unterstützung für das Abfragen des Azure Cosmos DB-Analysespeichers mit Synapse SQL Serverless befindet sich derzeit in der geschlossenen Vorschau. Die Public Preview wird auf der Seite für [Azure-Dienstupdates](https://azure.microsoft.com/updates/?status=nowavailable&category=databases) angekündigt.

Zum Abfragen von Azure Cosmos DB wird die vollständige [SELECT](/sql/t-sql/queries/select-transact-sql?view=sql-server-ver15)-Oberfläche durch die [OPENROWSET](develop-openrowset.md)-Funktion unterstützt, einschließlich eines Großteils der [SQL-Funktionen und -Operatoren](overview-features.md). Mit [CREATE EXTERNAL TABLE AS SELECT](develop-tables-cetas.md#cetas-in-sql-on-demand) können Sie die Ergebnisse der Abfrage, bei der Daten aus Azure Cosmos DB gelesen werden, auch zusammen mit Daten in Azure Blob Storage oder Azure Data Lake Storage speichern. Das Speichern von SQL Serverless-Abfrageergebnissen in Azure Cosmos DB mit [CETAS](develop-tables-cetas.md#cetas-in-sql-on-demand) ist derzeit nicht möglich.

In diesem Artikel erfahren Sie, wie Sie eine Abfrage mit SQL Serverless erstellen, die Daten aus Azure Cosmos DB-Containern mit Synapse Link-Aktivierung abfragt. Anschließend erfahren Sie in [diesem Tutorial](./tutorial-data-analyst.md) mehr über das Erstellen von SQL Serverless-Ansichten über Azure Cosmos DB-Container und das Verbinden dieser Ansichten mit Power BI-Modellen. 

## <a name="overview"></a>Übersicht

SQL Serverless verwendet die folgende `OPENROWSET`-Syntax, um das Abfragen und Analysieren von Daten im Azure Cosmos DB-Analysespeicher zu unterstützen:

```sql
OPENROWSET( 
       'CosmosDB',
       '<Azure Cosmos DB connection string>',
       <Container name>
    )  [ < with clause > ] AS alias
```

Die Azure Cosmos DB-Verbindungszeichenfolge übergibt den Namen des Azure Cosmos DB-Kontos, den Datenbanknamen, den Hauptschlüssel des Datenbankkontos sowie optional einen Regionsnamen an die `OPENROWSET`-Funktion. 

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie einen Alias nach `OPENROWSET` verwenden. Es gibt ein [bekanntes Problem](#known-issues), das Verbindungsprobleme mit dem Synapse SQL Serverless-Endpunkt verursacht, wenn Sie den Alias nach der `OPENROWSET`-Funktion nicht angeben.

Die Verbindungszeichenfolge weist das folgende Format auf:
```sql
'account=<database account name>;database=<database name>;region=<region name>;key=<database account master key>'
```

Der Name des Azure Cosmos DB-Containers wird in der `OPENROWSET`-Syntax ohne Anführungszeichen angegeben. Wenn der Containername Sonderzeichen (z. B. einen Bindestrich, „-“) enthält, sollte er in der `OPENROWSET`-Syntax von `[]` (eckigen Klammern) umschlossen werden.

> [!NOTE]
> Abfragen des Azure Cosmos DB-Transaktionsspeichers werden in SQL Serverless nicht unterstützt.

## <a name="sample-data-set"></a>Beispieldataset

Die Beispiele in diesem Artikel basieren auf Daten aus den Datasets [European Centre for Disease Prevention and Control (ECDC) COVID-19 Cases](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) und [COVID-19 Open Research Dataset (CORD-19), doi:10.5281/zenodo.3715505](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/). 

Auf den verlinkten Seiten erhalten Sie Informationen zur Lizenz sowie zur Struktur der Daten und können Beispieldaten für das [ECDC](https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json)- und das [CORD-19](https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json)-Dataset herunterladen.

Stellen Sie sicher, dass Sie die folgenden Ressourcen erstellen, um die in diesem Artikel beschriebenen Schritte zur Abfrage von Cosmos DB-Daten mit SQL Serverless ausführen zu können:
* Ein Azure Cosmos DB-Datenbankkonto mit [Synapse Link-Aktivierung](../../cosmos-db/configure-synapse-link.md)
* Eine Azure Cosmos DB-Datenbank namens `covid`
* Zwei Azure Cosmos DB-Container namens `EcdcCases` und `Cord19`, in die die obigen Beispieldatasets geladen wurden

## <a name="explore-azure-cosmos-db-data-with-automatic-schema-inference"></a>Untersuchen von Azure Cosmos DB-Daten mit automatischem Schemarückschluss

Die einfachste Möglichkeit der Untersuchung von Daten in Azure Cosmos DB ist die Verwendung des automatischen Schemarückschlusses. Wenn Sie die `OPENROWSET`-Anweisung ohne die `WITH`-Klausel verwenden, können Sie SQL Serverless anweisen, das Schema des Analysespeichers des Azure Cosmos DB-Containers automatisch zu erkennen (Rückschlüsse zu ziehen).

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases) as documents
```
Im obigen Beispiel weisen Sie SQL Serverless an, eine Verbindung mit der `covid`-Datenbank im Azure Cosmos DB-Konto `MyCosmosDbAccount` mit Authentifizierung durch den Azure Cosmos DB-Schlüssel (Platzhalterwert im obigen Beispiel) herzustellen. Anschließend greifen Sie auf den Analysespeicher des Containers `EcdcCases` in der Region `West US 2` zu. Da keine Projektion bestimmter Eigenschaften vorgenommen wurde, gibt die `OPENROWSET`-Funktion alle Eigenschaften aus den Azure Cosmos DB-Elementen zurück.

Wenn Sie Daten aus dem anderen Container in derselben Azure Cosmos DB-Datenbank untersuchen müssen, können Sie die gleiche Verbindungszeichenfolge verwenden und auf den erforderlichen Container als dritten Parameter verweisen:

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19) as cord19
```

## <a name="explicitly-specify-schema"></a>Explizites Angeben des Schemas

Der automatische Schemarückschluss in `OPENROWSET` ermöglicht zwar eine einfache und leicht zu verwendende Abfrage, in Ihren Geschäftsszenarios müssen Sie jedoch möglicherweise das Schema für schreibgeschützte relevante Eigenschaften aus den Azure Cosmos DB-Daten explizit angeben.

In `OPENROWSET` können Sie die Eigenschaften, die Sie aus den Daten im Container lesen möchten, sowie deren Datentypen explizit angeben. Angenommen, Sie haben einige Daten aus dem [ECDC-COVID-Dataset](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) mit der folgenden Struktur in Azure Cosmos DB importiert:

```json
{"date_rep":"2020-08-13","cases":254,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-12","cases":235,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-11","cases":163,"countries_and_territories":"Serbia","geo_id":"RS"}
```

Diese flachen JSON-Dokumente in Azure Cosmos DB können in Synapse SQL als Gruppe von Zeilen und Spalten dargestellt werden. In der `WITH`-Klausel der `OPENROWSET`-Funktion können Sie eine Teilmenge von Eigenschaften, die gelesen werden sollen, sowie die genauen Spaltentypen angeben:

```sql
SELECT TOP 10 *
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) with ( date_rep varchar(20), cases bigint, geo_id varchar(6) ) as rows
```

Mögliches Ergebnis dieser Abfrage:

| date_rep | cases | geo_id |
| --- | --- | --- |
| 2020-08-13 | 254 | RS |
| 2020-08-12 | 235 | RS |
| 2020-08-11 | 163 | RS |

Weitere Informationen zu den SQL-Typen, die für Azure Cosmos DB-Werte verwendet werden sollten, finden Sie in den [Regeln für die SQL-Typzuordnung](#azure-cosmos-db-to-sql-type-mappings) am Ende dieses Artikels.

## <a name="querying-nested-objects-and-arrays"></a>Abfragen von geschachtelten Objekten und Arrays

Mit Azure Cosmos DB können Sie komplexere Datenmodelle in Form von geschachtelten Objekten oder Arrays darstellen. Die automatische Synchronisierungsfunktion von Synapse Link für Azure Cosmos DB verwaltet standardmäßig die Schemadarstellung im Analysespeicher. Diese umfasst auch die Verarbeitung von geschachtelten Datentypen, was eine umfangreiche Abfrage über SQL Serverless ermöglicht.

Beispielsweise enthält das [CORD-19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/)-Dataset JSON-Dokumente mit folgender Struktur:

```json
{
    "paper_id": <str>,                   # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": <array of objects>    # list of author dicts, in order
        ...
     }
     ...
}
```

Die geschachtelten Objekte und Arrays in Azure Cosmos DB werden im Abfrageergebnis als JSON-Zeichenfolgen dargestellt, wenn sie von der `OPENROWSET`-Funktion gelesen werden. Eine Möglichkeit zum Lesen der Werte aus diesen komplexen Typen als SQL-Spalten ist die Verwendung von SQL-JSON-Funktionen:

```sql
SELECT
    title = JSON_VALUE(metadata, '$.title'),
    authors = JSON_QUERY(metadata, '$.authors'),
    first_author_name = JSON_VALUE(metadata, '$.authors[0].first')
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    WITH ( metadata varchar(MAX) ) AS docs;
```

Mögliches Ergebnis dieser Abfrage:

| title | authors | first_autor_name |
| --- | --- | --- |
| Supplementary Information An eco-epidemi… |   `[{"first":"Julien","last":"Mélade","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Julien |  

Alternativ können Sie bei Verwendung der `WITH`-Klausel die Pfade zu geschachtelten Werten in den Objekten angeben:

```sql
SELECT
    *
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    WITH ( title varchar(1000) '$.metadata.title',
           authors varchar(max) '$.metadata.authors'
    ) AS docs;
```

Hier erfahren Sie mehr über das Analysieren von [komplexen Datentypen in Synapse Link](../how-to-analyze-complex-schema.md) und [geschachtelten Strukturen in SQL Serverless](query-parquet-nested-types.md).

> [!IMPORTANT]
> Wenn in Ihrem Text unerwartete Zeichen angezeigt werden, z. B. `MÃƒÂ©lade` statt `Mélade`, ist die Datenbanksortierung nicht auf [UTF8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8)-Sortierung festgelegt. 
> [Ändern Sie die Sortierung der Datenbank](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation) in eine UTF8-Sortierung mithilfe einer SQL-Anweisung wie `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8`.


## <a name="flattening-nested-arrays"></a>Vereinfachen von geschachtelten Arrays

Azure Cosmos DB-Daten verfügen möglicherweise über geschachtelte untergeordnete Arrays wie das Array des Autors im [CORD-19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/)-Dataset:

```json
{
    "paper_id": <str>,                      # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": [                        # list of author dicts, in order
            {
                "first": <str>,
                "middle": <list of str>,
                "last": <str>,
                "suffix": <str>,
                "affiliation": <dict>,
                "email": <str>
            },
            ...
        ],
        ...
}
```

In einigen Fällen müssen Sie möglicherweise die Eigenschaften aus dem obersten Element (metadata) mit allen Elementen des Arrays (authors) verknüpfen. SQL Serverless ermöglicht Ihnen das Vereinfachen von geschachtelten Strukturen durch Anwenden der `OPENJSON`-Funktion auf das geschachtelte Array:

```sql
SELECT
    *
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    ) WITH ( title varchar(1000) '$.metadata.title',
             authors varchar(max) '$.metadata.authors' ) AS docs
      CROSS APPLY OPENJSON ( authors )
                  WITH (
                       first varchar(50),
                       last varchar(50),
                       affiliation nvarchar(max) as json
                  ) AS a
```

Mögliches Ergebnis dieser Abfrage:

| title | authors | first | last | affiliation |
| --- | --- | --- | --- | --- |
| Supplementary Information An eco-epidemi… |   `[{"first":"Julien","last":"Mélade","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Julien | Mélade | `   {"laboratory":"Centre de Recher…` |
Supplementary Information An eco-epidemi… | `[{"first":"Nicolas","last":"4#","suffix":"","affiliation":{"laboratory":"","institution":"U…` | Nicolas | 4# |`{"laboratory":"","institution":"U…` | 
| Supplementary Information An eco-epidemi… |   `[{"first":"Beza","last":"Ramazindrazana","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Beza | Ramazindrazana | `{"laboratory":"Centre de Recher…` |
| Supplementary Information An eco-epidemi… |   `[{"first":"Olivier","last":"Flores","suffix":"","affiliation":{"laboratory":"UMR C53 CIRAD, …` | Olivier | Flores |`{"laboratory":"UMR C53 CIRAD, …` |     

> [!IMPORTANT]
> Wenn in Ihrem Text unerwartete Zeichen angezeigt werden, z. B. `MÃƒÂ©lade` statt `Mélade`, ist die Datenbanksortierung nicht auf [UTF8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8)-Sortierung festgelegt. 
> [Ändern Sie die Sortierung der Datenbank](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation) in eine UTF8-Sortierung mithilfe einer SQL-Anweisung wie `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8`.

## <a name="azure-cosmos-db-to-sql-type-mappings"></a>Zuordnung von SQL-Typen zu Azure Cosmos DB

Beachten Sie unbedingt Folgendes: Während der Azure Cosmos DB-Transaktionsspeicher schemaagnostisch ist, ist der Analysespeicher zur Optimierung für die Leistung von analytischen Abfragen schematisiert. Mit der automatischen Synchronisierungsfunktion von Synapse Link verwaltet Azure Cosmos DB standardmäßig die Schemadarstellung im Analysespeicher. Diese umfasst auch die Verarbeitung von geschachtelten Datentypen. Da SQL Serverless den Analysespeicher abfragt, ist es wichtig zu verstehen, wie Azure Cosmos DB-Eingabedatentypen SQL-Datentypen zugeordnet werden.

Azure Cosmos DB-Konten für die SQL (Core)-API unterstützen als JSON-Eigenschaftstypen Zahlen, Zeichenfolgen, boolesche Werte, NULL, geschachtelte Objekte und Arrays. Wenn Sie die `WITH`-Klausel in `OPENROWSET` verwenden, müssen Sie SQL-Typen auswählen, die zu diesen JSON-Typen passen. Im Folgenden sind die SQL-Spaltentypen aufgeführt, die für verschiedene Eigenschaftstypen in Azure Cosmos DB verwendet werden sollen.

| Azure Cosmos DB-Eigenschaftstyp | SQL-Spaltentyp |
| --- | --- |
| Boolean | bit |
| Integer | BIGINT |
| Decimal | float |
| String | varchar (UTF8-Datenbanksortierung) |
| Date Time (ISO-formatierte Zeichenfolge) | varchar(30) |
| Datum und Uhrzeit (UNIX-Zeitstempel) | BIGINT |
| Null | `any SQL type` 
| Geschachteltes Objekt oder Array | varchar(max) (UTF8-Datenbanksortierung), als JSON-Text serialisiert |

Wenn Sie Azure Cosmos DB-Konten über die Mongo DB-API abfragen möchten, finden Sie [hier](../../cosmos-db/analytical-store-introduction.md#analytical-schema) weitere Informationen zur Schemadarstellung mit vollständiger Genauigkeit im Analysespeicher und den dabei zu verwendenden Namen für erweiterte Eigenschaften.

## <a name="known-issues"></a>Bekannte Probleme

- Der Alias **MUSS** nach der `OPENROWSET`-Funktion angegeben werden (z. B. `OPENROWSET (...) AS function_alias`). Das Weglassen des Alias kann zu Verbindungsproblemen führen, und der Synapse SQL Serverless-Endpunkt ist möglicherweise vorübergehend nicht verfügbar. Dieses Problem wird im November 2020 gelöst.
- Die [Azure Cosmos DB-Schemadarstellung mit vollständiger Genauigkeit](../../cosmos-db/analytical-store-introduction.md#schema-representation) wird derzeit nicht von Synapse SQL Serverless unterstützt. Verwenden Sie Synapse SQL Serverless nur für den Zugriff auf das genau definierte Cosmos DB-Schema.

Auf der [Azure Synapse-Feedbackseite](https://feedback.azure.com/forums/307516-azure-synapse-analytics?category_id=387862) können Sie Vorschläge übermitteln und Probleme melden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Erstellen und Verwenden von Ansichten in SQL On-Demand (Vorschauversion) mithilfe von Azure Synapse Analytics](create-use-views.md) 
- [Tutorial: Erstellen von SQL Serverless-Ansichten über Azure Cosmos DB und Verbinden dieser Ansichten mit Power BI-Modellen über DirectQuery](./tutorial-data-analyst.md)
