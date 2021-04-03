---
title: Untersuchen von Daten auf einem virtuellen SQL Server-Computer – Team Data Science-Prozess
description: Erfahren Sie, wie Sie mithilfe von SQL oder einer Programmiersprache wie Python Daten untersuchen, die in Azure auf einem virtuellen SQL Server-Computer gespeichert sind.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 33b55afb7796b197f7130ec9288abb01cc115651
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "86085649"
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Durchsuchen von Daten auf einer SQL Server-VM in Azure

In diesem Artikel wird beschrieben, wie Sie Daten durchsuchen, die in einer SQL Server-VM in Azure gespeichert sind. Verwenden Sie SQL oder Python, um die Daten zu untersuchen.

Dieser Task ist ein Schritt im [Team Data Science-Prozess](overview.md).

> [!NOTE]
> Bei den SQL-Beispielanweisungen in diesem Dokument wird davon ausgegangen, dass die Daten auf einem SQL Server gespeichert sind. Wenn dies nicht der Fall ist, finden Sie im Cloud Data Science-Prozess Anweisungen zum Verschieben der Daten zu einer SQL Server-Instanz.
> 
> 

## <a name="explore-sql-data-with-sql-scripts"></a><a name="sql-dataexploration"></a>Durchsuchen von SQL-Daten mit SQL-Skripts
Hier finden Sie einige SQL-Beispielskripts, die zum Durchsuchen von Daten auf einem SQL Server verwendet werden können.

1. Abrufen der Anzahl der Vorkommen pro Tag:
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Abrufen der Ebenen in einer Kategoriespalte:
   
    `select  distinct <column_name> from <databasename>`
3. Abrufen der Anzahl von Ebenen in Kombination zweier kategorischer Spalten: 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Abrufen der Verteilung der numerischen Spalten:
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Für ein praktisches Beispiel können Sie das [NYC Taxi-Dataset](https://www.andresmh.com/nyctaxitrips/) verwenden. Eine vollständige exemplarische Vorgehensweise finden Sie im IPython Notebook-Beispiel unter [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb).
> 
> 

## <a name="explore-sql-data-with-python"></a><a name="python"></a>Durchsuchen von SQL-Daten mit Python
Die Verwendung von Python zum Durchsuchen von Daten und zum Generieren von Funktionen mit Daten in SQL Server ähnelt der Datenverarbeitung in Azure-Blobs mit Python, die unter [Verarbeiten von Azure-Blobdaten in Ihrer Data Science-Umgebung](data-blob.md)beschrieben ist. Laden Sie die Daten in einen Pandas-Datenrahmen, damit sie weiterverarbeitet werden können. In diesem Abschnitt werden das Herstellen einer Verbindung mit der Datenbank und das Laden der Daten in den DataFrame beschrieben.

Das folgende Format für die Verbindungszeichenfolge kann verwendet werden, um aus Python mit „pyodbc“ eine Verbindung mit einer SQL Server-Datenbank herzustellen (ersetzen Sie „servername“, „dbname“, „username“ und „password“ durch Ihre Daten):

```python
#Set up the SQL Azure connection
import pyodbc    
conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')
```

Die [Pandas-Bibliothek](https://pandas.pydata.org/) in Python bietet eine Vielzahl an Datenstrukturen und Datenanalysetools für die Datenbearbeitung zur Python-Programmierung. Der folgende Code liest die aus einer SQL Server-Datenbank zurückgegebenen Ergebnisse in ein Pandas-DataFrame ein:

```python
# Query database and load the returned results in pandas data frame
data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)
```

Sie können nun mit dem Pandas-DataFrame arbeiten. Die Vorgehensweise wird im Thema [Verarbeiten von Azure-Blobdaten mit erweiterter Analyse](data-blob.md) beschrieben.

## <a name="the-team-data-science-process-in-action-example"></a>Der Team Data Science-Prozess in Aktion: Beispiel
Eine lückenlose exemplarische Vorgehensweise zur Verwendung des Team Data Science-Prozesses mit einem öffentlichen Dataset finden Sie unter [Der Team Data Science-Prozess in Aktion: Verwenden von SQL Server](sql-walkthrough.md).

