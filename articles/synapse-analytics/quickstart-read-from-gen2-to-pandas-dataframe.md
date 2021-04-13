---
title: 'Schnellstart: Einlesen von Daten aus ADLS Gen2 in einen Pandas-Datenrahmen'
description: Hier erfahren Sie, wie Sie mithilfe von Python in Synapse Studio in Azure Synapse Analytics Daten aus einem Azure Data Lake Storage Gen2-Konto in einen Pandas-Datenrahmen einlesen.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: quickstart
ms.reviewer: jrasnick, garye, negust
ms.date: 03/23/2021
author: garyericson
ms.author: garye
ms.openlocfilehash: b7358c522cf12e7856496ad71fda393394e7ceab
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105969571"
---
# <a name="quickstart-read-data-from-adls-gen2-to-pandas-dataframe-in-azure-synapse-analytics"></a>Schnellstart: Einlesen von Daten aus ADLS Gen2 in einen Pandas-Datenrahmen in Azure Synapse Analytics

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Python Daten aus Azure Data Lake Storage (ADLS) Gen2 in einen Pandas-Datenrahmen in Azure Synapse Analytics einlesen.

In einem Synapse Studio-Notebook führen Sie folgende Schritte aus:

- Herstellen einer Verbindung mit einem Container in Data Lake Storage Gen2, der mit Ihrem Azure Synapse Analytics-Arbeitsbereich verknüpft ist
- Einlesen der Daten aus einem PySpark-Notebook mithilfe von `spark.read.load`
- Konvertieren der Daten in einen Pandas-Datenrahmen mithilfe von `.toPandas()`

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
- Synapse Analytics-Arbeitsbereich mit einer Data Lake Storage Gen2-Instanz, die als Standardspeicher konfiguriert ist: Sie müssen über die Rolle **Mitwirkender an Storage-Blobdaten** für das von Ihnen verwendete Data Lake Storage Gen2-Dateisystem verfügen. Ausführliche Informationen zum Erstellen eines Arbeitsbereichs finden Sie unter [Erstellen eines Synapse-Arbeitsbereichs](get-started-create-workspace.md).
- Apache Spark-Pool im Arbeitsbereich: siehe [Erstellen eines serverlosen Apache Spark-Pools](get-started-analyze-spark.md#create-a-serverless-apache-spark-pool).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="upload-sample-data-to-adls-gen2"></a>Hochladen von Beispieldaten in ADLS Gen2

1. Erstellen Sie im Azure-Portal einen Container in der gleichen Data Lake Storage Gen2-Instanz, die auch von Synapse Studio verwendet wird. Sie können diesen Schritt überspringen, wenn Sie das verknüpfte Standardspeicherkonto in Ihrem Azure Synapse Analytics-Arbeitsbereich verwenden möchten.

1. Klicken Sie in Synapse Studio auf **Daten**, und wählen Sie die Registerkarte **Verknüpft** und unter **Azure Data Lake Storage Gen2** den Container aus.

1. Laden Sie die Beispieldatei [RetailSales.csv](https://github.com/Azure-Samples/Synapse/blob/main/Notebooks/PySpark/Synapse%20Link%20for%20Cosmos%20DB%20samples/Retail/RetailData/RetailSales.csv) herunter, und laden Sie sie in den Container hoch.

1. Wählen Sie die hochgeladene Datei aus, klicken Sie auf **Eigenschaften**, und kopieren Sie den Wert für den **ABFSS-Pfad**.

## <a name="read-data-from-adls-gen2-into-a-pandas-dataframe"></a>Einlesen von Daten aus ADLS Gen2 in einen Pandas-Datenrahmen

1. Klicken Sie im linken Bereich auf **Entwickeln**.

1. Klicken Sie auf **+** , und wählen Sie „Notebook“ aus, um ein neues Notebook zu erstellen.

1. Wählen Sie unter **Anfügen an** den Apache Spark-Pool aus. Besitzen Sie keinen, klicken Sie auf **Apache Spark-Pool erstellen**.

1. Fügen Sie in der Notebook-Codezelle den folgenden Python-Code und den zuvor kopierten ABFSS-Pfad ein:

   ```python
   %%pyspark
   data_path = spark.read.load('<ABFSS Path to RetailSales.csv>', format='csv', header=True)
   data_path.show(10)
   
   print('Converting to Pandas.')
   
   pdf = data_path.toPandas()
   print(pdf)
   ```

1. Führen Sie die Zelle aus.

Nach einigen Minuten sollte der angezeigte Text etwa wie folgt aussehen:

```text
Command executed in 25s 324ms by gary on 03-23-2021 17:40:23.481 -07:00

Job execution Succeeded Spark 2 executors 8 cores

+-------+-----------+--------+-----------+-----------+-----+------------+--------------------+
|storeId|productCode|quantity|logQuantity|advertising|price|weekStarting|                  id|
+-------+-----------+--------+-----------+-----------+-----+------------+--------------------+
|      2| surface.go|     105|9.264828557|          1|  159|   6/15/2017|d6bd47a7-2ad6-4f0...|
|      2| surface.go|      80|8.987196821|          0|  269|   7/27/2017|64cc74c2-c7da-4e1...|
|      2| surface.go|      68|8.831711918|          1|  209|    8/3/2017|9a2d164b-5e44-44d...|
|      2| surface.go|      28|7.965545573|          0|  209|   8/10/2017|b8cd9987-1d5a-4f4...|
|      2| surface.go|      16|7.377758908|          0|  209|   8/24/2017|ac0ec099-e102-4bf...|
|      2| surface.go|     253| 10.1402973|          1|  189|   8/31/2017|3d22c002-b04c-409...|
|      2| surface.go|     107|9.282847063|          0|  189|    9/7/2017|b6e19699-d684-449...|
|      2| surface.go|      66|8.803273983|          0|  189|   9/14/2017|e89a5838-fb8f-413...|
|      2| surface.go|      65|8.793612072|          0|  179|   9/21/2017|c3278682-16c0-483...|
|      2| surface.go|      17|7.454719949|          0|  269|  10/12/2017|f40190c1-b2ed-46f...|
+-------+-----------+--------+-----------+-----------+-----+------------+--------------------+
only showing top 10 rows

Converting to Pandas.
      storeId  ...                                    id
0           2  ...  d6bd47a7-2ad6-4f0a-b8de-ed1386cae5ea
1           2  ...  64cc74c2-c7da-4e12-af64-c95bdf429934
2           2  ...  9a2d164b-5e44-44d7-9837-cf9ae6566c99
3           2  ...  b8cd9987-1d5a-4f4f-9346-719d73b1f7f0
4           2  ...  ac0ec099-e102-4bfc-9775-983b151dcd03
...       ...  ...                                   ...
28942     137  ...  6af00133-7015-415d-831b-ddf05bb5828c
28943     137  ...  1e0d3a21-ab43-49c4-89e2-49d202821807
28944     137  ...  5cc7e50a-6aa4-419b-a933-905a667aa2df
28945     137  ...  650ca506-7a4f-46f8-b2e1-e52ceffadf16
28946     137  ...  9bb216f6-04ec-4b61-9e68-34772b814c44

[28947 rows x 8 columns]
```

## <a name="next-steps"></a>Nächste Schritte

- [Was ist Azure Synapse Analytics?](overview-what-is.md)
- [Erste Schritte mit Azure Synapse Analytics](get-started.md)
- [Erstellen eines serverlosen Apache Spark-Pools](get-started-analyze-spark.md#create-a-serverless-apache-spark-pool)
