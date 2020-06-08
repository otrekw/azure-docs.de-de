---
title: 'Tutorial: Vorbereiten von Daten zum Ausführen von Clustering in R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In Teil 1 dieser dreiteiligen Reihe von Tutorials bereiten Sie die Daten aus einer Azure SQL-Datenbank für die Durchführung von Clustering in R mit Machine Learning Services (Vorschauversion) für Azure SQL-Datenbank vor.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 07/29/2019
ROBOTS: NOINDEX
ms.openlocfilehash: c06e1b13f87972cbcd50e888edf55158b77881d8
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84024097"
---
# <a name="tutorial-prepare-data-to-perform-clustering-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Tutorial: Vorbereiten von Daten zum Durchführen von Clustering in R mit Machine Learning Services (Vorschauversion) für Azure SQL-Datenbank
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

In Teil 1 dieser dreiteiligen Tutorialreihe importieren Sie Daten aus einer Azure SQL-Datenbank in R und bereiten diese Daten vor. Später in dieser Reihe verwenden Sie die Daten zum Trainieren und Bereitstellen eines Clusteringmodells in R mit Machine Learning Services für Azure SQL-Datenbank (Vorschauversion).

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

*Clustering* kann als Organisieren von Daten in Gruppen beschrieben werden, in denen Mitglieder einer Gruppe in irgendeiner Weise ähnlich sind.
Sie verwenden den **K-Means**-Algorithmus zum Durchführen des Clusterings von Kunden in einem Dataset von Produktkäufen und -rückgaben. Durch das Clustern von Kunden können Sie Ihre Marketingmaßnahmen effektiver auf bestimmte Gruppen ausrichten.
K-Means-Clustering ist ein *nicht überwachter Lernalgorithmus*, der auf der Grundlage von Ähnlichkeiten nach Mustern in Daten sucht.

In den Teilen 1 und 2 dieser Reihe entwickeln Sie einige R-Skripts in RStudio, um Ihre Daten vorzubereiten und ein Machine Learning-Modell zu trainieren. In Teil 3 führen Sie dann diese R-Skripts in einer SQL-Datenbank mithilfe gespeicherter Prozeduren aus.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Importieren einer Beispieldatenbank in Azure SQL-Datenbank
> * Trennen von Kunden anhand verschiedener Dimensionen mithilfe von R
> * Laden der Daten aus der Azure SQL-Datenbank in einen R-Datenrahmen

In [Teil 2](clustering-model-build-tutorial.md) erfahren Sie, wie Sie ein K-Means-Clusteringmodell in R erstellen und trainieren.

In [Teil 3](clustering-model-deploy-tutorial.md) erlernen Sie das Erstellen einer gespeicherten Prozedur in einer Azure SQL-Datenbank, die Clustering in R basierend auf neuen Daten ausführen kann.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: Falls Sie nicht über ein Azure-Abonnement verfügen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/), bevor Sie beginnen.

* [Azure SQL-Datenbank mit Machine Learning Services (mit R)](machine-learning-services-overview.md) muss aktiviert sein.

* RevoScaleR-Paket: Unter [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler?view=sql-server-2017#versions-and-platforms) finden Sie Optionen zum lokalen Installieren dieses Pakets.

* R-IDE: In diesem Tutorial wird [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) verwendet.

* SQL-Abfragetool: In diesem Tutorial wird davon ausgegangen, dass Sie [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) oder [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) verwenden.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="import-the-sample-database"></a>Importieren der Beispieldatenbank

Das in diesem Tutorial verwendete Beispieldataset wurde in einer **BACPAC**-Datenbank-Sicherungsdatei gespeichert, die Sie herunterladen und verwenden können. Dieses Dataset wird aus dem [tpcx-bb](http://www.tpc.org/tpcx-bb/default.asp)-Dataset abgeleitet, das von [TPC (Transaction Processing Performance Council)](http://www.tpc.org/default.asp) bereitgestellt wird.

1. Laden Sie die Datei [tpcxbb_1gb.bacpac](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bacpac) herunter.

1. Befolgen Sie die Anweisungen in [Importieren einer BACPAC-Datei in eine Datenbank in Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-import), und verwenden Sie dazu diese Details:

   * Führen Sie einen Import aus der heruntergeladenen Datei **tpcxbb_1gb.bacpac** durch.
   * Wählen Sie während der öffentlichen Vorschau die Konfiguration **Gen5/vCore** für die neue Datenbank
   * Nennen Sie die neue Datenbank „tpcxbb_1gb“.

## <a name="separate-customers"></a>Aufteilen von Kunden

Erstellen Sie eine neue RScript-Datei in RStudio, und führen Sie das folgende Skript aus.
In der SQL-Abfrage trennen Sie Kunden anhand der folgenden Dimensionen:

* **orderRatio** = Retourenquote für Bestellungen (Gesamtzahl der teilweise oder vollständig zurückgegebenen Bestellungen im Vergleich zur Gesamtzahl der Bestellungen)
* **itemsRatio** = Retourenquote für Artikel (Gesamtzahl der zurückgegebenen Artikel im Vergleich zur Anzahl der gekauften Artikel)
* **monetaryRatio** = Rückzahlungsquote (Monetärer Gesamtbetrag der zurückgegebenen Artikel im Vergleich zum erworbenen Betrag)
* **frequency** = Häufigkeit der Retouren

In der **paste**-Funktion ersetzen Sie **Server**, **UID** und **PWD** durch Ihre eigenen Verbindungsinformationen.

```r
# Define the connection string to connect to the tpcxbb_1gb database
connStr <- paste("Driver=SQL Server",
               "; Server=", "<Logical SQL server>",
               "; Database=tpcxbb_1gb",
               "; UID=", "<user>",
               "; PWD=", "<password>",
                  sep = "");


#Define the query to select data from SQL Server
input_query <- "
SELECT ss_customer_sk AS customer
    ,round(CASE
            WHEN (
                       (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio
    ,round(CASE
            WHEN (
                     (orders_items = 0)
                  OR (returns_items IS NULL)
                  OR (orders_items IS NULL)
                  OR ((returns_items / orders_items) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio
    ,round(CASE
            WHEN (
                     (orders_money = 0)
                  OR (returns_money IS NULL)
                  OR (orders_money IS NULL)
                  OR ((returns_money / orders_money) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio
    ,round(CASE
            WHEN (returns_count IS NULL)
            THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk
"
```

## <a name="load-the-data-into-a-data-frame"></a>Laden der Daten in einem neuen Datenrahmen

Jetzt verwenden Sie das folgende Skript, um die Ergebnisse der Abfrage mithilfe der Funktion **rxSqlServerData** an einen R-Datenrahmen zurückzugeben.
Im Zuge des Prozesses definieren Sie den Typ für die ausgewählten Spalten (mit colClasses), um sicherzustellen, dass die Typen ordnungsgemäß an R übertragen werden.

```r
# Query SQL Server using input_query and get the results back
# to data frame customer_returns
# Define the types for selected columns (using colClasses),
# to make sure that the types are correctly transferred to R
customer_returns <- rxSqlServerData(
                     sqlQuery=input_query,
                     colClasses=c(customer ="numeric",
                                  orderRatio="numeric",
                                  itemsRatio="numeric",
                                  monetaryRatio="numeric",
                                  frequency="numeric" ),
                     connectionString=connStr);

# Transform the data from an input dataset to an output dataset
customer_data <- rxDataStep(customer_returns);

# Take a look at the data just loaded from SQL Server
head(customer_data, n = 5);
```

Das Ergebnis sollte etwa folgendermaßen aussehen:

```results
  customer orderRatio itemsRatio monetaryRatio frequency
1    29727          0          0      0.000000         0
2    26429          0          0      0.041979         1
3    60053          0          0      0.065762         3
4    97643          0          0      0.037034         3
5    32549          0          0      0.031281         4
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

***Wenn Sie nicht mit diesem Tutorial fortfahren möchten***, löschen Sie die Datenbank „tpcxbb_1gb“ von Ihrem Server.

Führen Sie im Azure-Portal die folgenden Schritte aus:

1. Wählen Sie im Menü auf der linken Seite im Azure-Portal **Alle Ressourcen** oder **SQL-Datenbanken** aus.
1. Geben Sie im Feld **Nach Name filtern** die Zeichenfolge **tpcxbb_1gb** ein, und wählen Sie Ihr Abonnement aus.
1. Wählen Sie die Datenbank **tpcxbb_1gb** aus.
1. Wählen Sie auf der Seite **Übersicht** die Option **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

Im ersten Teil dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Importieren einer Beispieldatenbank in eine Azure SQL-Datenbank
* Trennen von Kunden anhand verschiedener Dimensionen mithilfe von R
* Laden der Daten aus der Azure SQL-Datenbank in einen R-Datenrahmen

Um ein Machine Learning-Modell zu erstellen, das diese Kundendaten verwendet, führen Sie Teil 2 dieser Tutorialreihe durch:

> [!div class="nextstepaction"]
> [Tutorial: Erstellen eines Vorhersagemodells in R mit Machine Learning Services (Vorschauversion) für Azure SQL-Datenbank](clustering-model-build-tutorial.md)
