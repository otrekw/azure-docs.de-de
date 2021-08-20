---
title: 'Tutorial: Verwenden von FSSPEC zum Lesen/Schreiben von ADLS-Daten in den Spark-Pool von Synapse Analytics'
description: Tutorial zur Verwendung von FSSPEC für Machine Learning-Workloads zum Lesen/Schreiben von ADLS-Daten in dedizierten Spark-Pools
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 06/07/2021
author: AjAgr
ms.author: ajagarw
ms.openlocfilehash: ecd0b923939ab1da724b46272dbaf60c44389d3d
ms.sourcegitcommit: f9e368733d7fca2877d9013ae73a8a63911cb88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111904305"
---
# <a name="tutorial-use-fsspec-to-readwrite-adls-data-in-synapse-analytics-spark-pool"></a>Tutorial: Verwenden von FSSPEC zum Lesen/Schreiben von ADLS-Daten in den Spark-Pool von Synapse Analytics

Hier erfahren Sie, wie Sie die Dateisystemspezifikation (Filesystem Spec, FSSPEC) zum Lesen/Schreiben von Daten in Azure Data Lake Storage (ADLS) mithilfe eines verknüpften Diensts in einem dedizierten Spark-Pool in Azure Synapse Analytics verwenden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> - Lesen/Schreiben von ADLS-Daten in einer dedizierten Spark-Sitzung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- [Azure Synapse Analytics-Arbeitsbereich](../get-started-create-workspace.md) mit einem als Standardspeicher konfigurierten Azure Data Lake Storage Gen2-Speicherkonto. Für das hier verwendete Data Lake Storage Gen2-Dateisystem müssen Sie über die Rolle *Mitwirkender an Storage-Blobdaten* verfügen.
- Spark-Pool in Ihrem Azure Synapse Analytics-Arbeitsbereich. Ausführliche Informationen finden Sie unter [Erstellen eines Spark-Pools in Azure Synapse](../get-started-analyze-spark.md).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten

In Azure Synapse Analytics definieren Sie in einem verknüpften Dienst Ihre Verbindungsinformationen für andere Dienste. In diesem Abschnitt fügen Sie Azure Synapse Analytics und Azure Data Lake Storage Gen 2 als verknüpfte Dienste hinzu.

1. Öffnen Sie Azure Synapse Studio, und wechseln Sie zur Registerkarte **Verwalten**.
1. Wählen Sie unter **Externe Verbindungen** die Option **Verknüpfte Dienste** aus.
1. Klicken Sie auf **Neu**, um einen verknüpften Dienst hinzuzufügen.
1. Wählen Sie in der Liste die Kachel „Azure Data Lake Storage Gen2“ und dann **Weiter** aus.
1. Geben Sie Ihre Anmeldeinformationen für die Authentifizierung ein. Der Kontoschlüssel wird derzeit als Authentifizierungstyp unterstützt. Klicken Sie auf **Verbindung testen**, um Ihre Anmeldeinformationen zu überprüfen. Klicken Sie auf **Erstellen**.

   :::image type="content" source="media/tutorial-spark-pool-filesystem-spec/create-adls-linked-service.png" alt-text="Erstellen eines verknüpften Diensts mithilfe des ADLS Gen2-Speicherzugriffsschlüssels":::


## <a name="readwrite-data-using-storage-account-name-and-key"></a>Lesen/Schreiben von Daten mithilfe des Speicherkontonamens und -schlüssels

FSSPEC kann ADLS-Daten lesen/schreiben, indem der Speicherkontoname und der Schlüssel direkt angegeben werden.

1. Öffnen Sie in Synapse Studio **Data** > **Linked** > **Azure Data Lake Storage Gen2** (Daten > Verknüpft > Azure Data Lake Storage Gen2). Laden Sie Daten in das Standardspeicherkonto hoch.

1. Führen Sie den folgenden Code aus.

   > [!NOTE]
   > Aktualisieren Sie die Datei-URL, den ADLS Gen2-Speichernamen und den -schlüssel in diesem Skript, bevor Sie sie ausführen.

   ```PYSPARK
   # To read data
   import fsspec
   import pandas

   adls_account_name = '' #Provide exact ADLS account name
   adls_account_key = '' #Provide exact ADLS account key

   fsspec_handle = fsspec.open('abfs://<container>/<path-to-file>', account_name=adls_account_name, account_key=adls_account_key)

   with fsspec_handle.open() as f:
       df = pandas.read_csv(f)

   # To write data
   import fsspec
   import pandas

   adls_account_name = '' #Provide exact ADLS account name 
   adls_account_key = '' #Provide exact ADLS account key 
   
   data = pandas.DataFrame({'Name':['Tom', 'nick', 'krish', 'jack'], 'Age':[20, 21, 19, 18]})
   
   fsspec_handle = fsspec.open('abfs://<container>/<path-to-file>', account_name=adls_account_name,    account_key=adls_account_key, mode="wt")
   
   with fsspec_handle.open() as f:
    data.to_csv(f)
   ```

## <a name="readwrite-data-using-linked-service"></a>Lesen/Schreiben von Daten mithilfe des verknüpften Diensts

FSSPEC kann ADLS-Daten lesen/schreiben, indem der Name des verknüpften Diensts angegeben wird.


1. Öffnen Sie in Synapse Studio **Data** > **Linked** > **Azure Data Lake Storage Gen2** (Daten > Verknüpft > Azure Data Lake Storage Gen2). Laden Sie Daten in das Standardspeicherkonto hoch.

1. Führen Sie den folgenden Code aus.

   > [!NOTE]
   > Aktualisieren Sie die Datei-URL, den Namen des verknüpften Diensts und den ADLS Gen2-Speichernamen in diesem Skript, bevor Sie sie ausführen.

   ```PYSPARK
   # To read data
   import fsspec
   import pandas
   
   adls_account_name = '' #Provide exact ADLS account name
   sas_key = TokenLibrary.getConnectionString(<LinkedServiceName>)
   
   fsspec_handle = fsspec.open('abfs://<container>/<path-to-file>', account_name =    adls_account_name, sas_token=sas_key)
   
   with fsspec_handle.open() as f:
       df = pandas.read_csv(f)

   # To write data
   import fsspec
   import pandas
   
   adls_account_name = '' #Provide exact ADLS account name
   
   data = pandas.DataFrame({'Name':['Tom', 'nick', 'krish', 'jack'], 'Age':[20, 21, 19, 18]})
   sas_key = TokenLibrary.getConnectionString(<LinkedServiceName>) 
   
   fsspec_handle = fsspec.open('abfs://<container>/<path-to-file>', account_name =    adls_account_name, sas_token=sas_key, mode="wt") 
   
   with fsspec_handle.open() as f:
       data.to_csv(f) 
   ```

## <a name="next-steps"></a>Nächste Schritte

- [Machine Learning-Funktionen in Azure Synapse Analytics](what-is-machine-learning.md)