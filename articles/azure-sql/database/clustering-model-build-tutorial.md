---
title: 'Tutorial: Erstellen eines Clusteringmodells in R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Im zweiten Teil dieser dreiteiligen Tutorialreihe erstellen Sie ein K-Means-Modell für das Clustering in R mit Machine Learning Services (Vorschau) in Azure SQL-Datenbank.
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
ms.openlocfilehash: a7d0bc787f1ae4444849a1483d6323f8a098ae1e
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84024130"
---
# <a name="tutorial-build-a-clustering-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Tutorial: Erstellen eines Clusteringmodells in R mit Machine Learning Services (Vorschauversion) in Azure SQL-Datenbank
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Im zweiten Teil dieser dreiteiligen Tutorialreihe erstellen Sie ein K-Means-Modell in R für das Clustering. Im nächsten Teil dieser Reihe stellen Sie dieses Modell in einer SQL-Datenbank mit Machine Learning Services für Azure SQL-Datenbank (Vorschauversion) bereit.

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Definieren der Anzahl der Cluster für einen K-Means-Algorithmus
> * Durchführen des Clustering
> * Analysieren der Ergebnisse

Im [ersten Teil](clustering-model-prepare-data-tutorial.md) haben Sie gelernt, wie Sie die Daten aus einer Azure SQL-Datenbank für das Clustering vorbereiten.

In [Teil 3](clustering-model-deploy-tutorial.md) erlernen Sie das Erstellen einer gespeicherten Prozedur in einer Azure SQL-Datenbank, die Clustering in R basierend auf neuen Daten ausführen kann.

## <a name="prerequisites"></a>Voraussetzungen

* In Teil zwei dieser Reihe von Tutorials wird angenommen, dass Sie [**Teil eins**](clustering-model-prepare-data-tutorial.md) und seine Voraussetzungen abgeschlossen haben.

## <a name="define-the-number-of-clusters"></a>Definieren der Anzahl der Cluster

Für das Clustering Ihrer Kundendaten verwenden Sie den **K-Means**-Clusteringalgorithmus. Dies ist eine der einfachsten und bekanntesten Methoden zum Gruppieren von Daten.
Weitere Informationen zum K-Means-Algorithmus finden Sie im [Umfassenden Leitfaden zu K-Means-Clusteringalgorithmus](https://www.kdnuggets.com/2019/05/guide-k-means-clustering-algorithm.html).

Der Algorithmus akzeptiert zwei Eingaben: Die Daten selbst und eine vordefinierte Zahl „*k*“, die die Anzahl der zu generierenden Cluster darstellt.
Die Ausgabe entspricht *k* Clustern mit den partitionierten Eingabedaten.

Verwenden Sie einen Plot aus der Abweichungsquadratsumme der enthaltenen Gruppen nach der Anzahl der extrahierten Cluster, um die Anzahl der zu verwendenden Cluster für den Algorithmus zu bestimmen. Die angemessene Anzahl der zu verwendenden Cluster befindet sich an der Knickstelle bzw. am „Ellenbogen“ des Plots.

```r
# Determine number of clusters by using a plot of the within groups sum of squares,
# by number of clusters extracted.
wss <- (nrow(customer_data) - 1) * sum(apply(customer_data, 2, var))
for (i in 2:20)
    wss[i] <- sum(kmeans(customer_data, centers = i)$withinss)
plot(1:20, wss, type = "b", xlab = "Number of Clusters", ylab = "Within groups sum of squares")
```

![Ellenbogengraph](./media/clustering-model-build-tutorial/elbow-graph.png)

Aus diesem Graph geht *k = 4* als geeigneter Wert hervor. Mit diesem *k*-Wert werden die Kunden in vier Cluster gruppiert.

## <a name="perform-clustering"></a>Durchführen des Clustering

Im folgenden R-Skript wird die Funktion **rxKmeans** verwendet. Hierbei handelt es sich um die K-Means-Funktion im RevoScaleR-Paket.

```r
# Output table to hold the customer group mappings.
# This is a table where the cluster mappings will be saved in the database.
return_cluster = RxSqlServerData(table = "return_cluster", connectionString = connStr);
# Set the seed for the random number generator for predictability
set.seed(10);
# Generate clusters using rxKmeans and output key / cluster to a table in a database in SQL Database
# called return_cluster
clust <- rxKmeans( ~ orderRatio + itemsRatio + monetaryRatio + frequency,
                   customer_returns,
                   numClusters=4,
                   outFile=return_cluster,
                   outColName="cluster",
                   extraVarsToWrite=c("customer"),
                   overwrite=TRUE);

# Read the customer returns cluster table from the database
customer_cluster <- rxDataStep(return_cluster);
```

## <a name="analyze-the-results"></a>Analysieren der Ergebnisse

Nach Abschluss des K-Means-Clusterings muss das Ergebnis analysiert werden, um zu ermitteln, ob verwertbare Informationen vorliegen.

Das Objekt **clust** enthält die Ergebnisse des K-Means-Clusterings.

```r
#Look at the clustering details to analyze results
clust
```

```results
Call:
rxKmeans(formula = ~orderRatio + itemsRatio + monetaryRatio +
    frequency, data = customer_returns, outFile = return_cluster,
    outColName = "cluster", extraVarsToWrite = c("customer"),
    overwrite = TRUE, numClusters = 4)
Data: customer_returns
Number of valid observations: 37336
Number of missing observations: 0
Clustering algorithm:  

K-means clustering with 4 clusters of sizes 31675, 671, 2851, 2139
Cluster means:
   orderRatio   itemsRatio monetaryRatio frequency
1 0.000000000 0.0000000000    0.00000000  0.000000
2 0.007451565 0.0000000000    0.04449653  4.271237
3 1.008067345 0.2707821817    0.49515232  1.031568
4 0.000000000 0.0004675082    0.10858272  1.186068
Within cluster sum of squares by cluster:
         1          2          3          4
    0.0000  1329.0160 18561.3157   363.2188
```

Die vier Clusterdurchschnittswerte werden mittels der Variablen angegeben, die Sie in [Teil 1](clustering-model-prepare-data-tutorial.md#separate-customers) des Tutorials definiert haben:

* *orderRatio* = Retourenquote für Bestellungen (Gesamtzahl der teilweise oder vollständig zurückgegebenen Bestellungen im Vergleich zur Gesamtzahl der Bestellungen)
* *itemsRatio* = Retourenquote für Artikel (Gesamtzahl der zurückgegebenen Artikel im Vergleich zur Anzahl der gekauften Artikel)
* *monetaryRatio* = Rückzahlungsquote (Monetärer Gesamtbetrag der zurückgegebenen Artikel im Vergleich zum erworbenen Betrag)
* *frequency* = Häufigkeit der Retouren

Data Mining mit K-Means erfordert häufig weitere Analysen der Ergebnisse sowie weitere Schritte, um die einzelnen Cluster besser zu verstehen, kann jedoch gute Leads erzielen.
Im Folgenden finden Sie Beispiele für die Interpretation dieser Ergebnisse:

* Beim ersten Cluster (dem größten Cluster) scheint es sich um eine Gruppe von inaktiven Kunden zu handeln. (Alle Werte sind null.)
* Cluster 3 scheint eine Gruppe mit erhöhtem Retournierverhalten zu sein.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

***Wenn Sie nicht mit diesem Tutorial fortfahren möchten***, löschen Sie die Datenbank „tpcxbb_1gb“ von Ihrem Server.

Führen Sie im Azure-Portal die folgenden Schritte aus:

1. Wählen Sie im Menü auf der linken Seite im Azure-Portal **Alle Ressourcen** oder **SQL-Datenbanken** aus.
1. Geben Sie im Feld **Nach Name filtern** die Zeichenfolge **tpcxbb_1gb** ein, und wählen Sie Ihr Abonnement aus.
1. Wählen Sie die Datenbank **tpcxbb_1gb** aus.
1. Wählen Sie auf der Seite **Übersicht** die Option **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In Teil 2 dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Definieren der Anzahl der Cluster für einen K-Means-Algorithmus
* Durchführen des Clustering
* Analysieren der Ergebnisse

Um das Machine Learning-Modell bereitzustellen, das Sie erstellt haben, fahren Sie mit Teil drei dieser Tutorialreihe fort:

> [!div class="nextstepaction"]
> [Tutorial: Deploy a clustering model in R with Azure SQL Database Machine Learning Services (preview)](clustering-model-deploy-tutorial.md) (Tutorial: Bereitstellen eines Clusteringmodells in R mit Machine Learning Services (Vorschauversion) in Azure SQL-Datenbank)
