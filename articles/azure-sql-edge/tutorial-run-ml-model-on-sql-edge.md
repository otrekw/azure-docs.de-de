---
title: Bereitstellen eines ML-Modells in Azure SQL Edge mithilfe von ONNX
description: Im dritten Teil dieses dreiteiligen Azure SQL Edge-Tutorials zum Vorhersagen von Eisenerzverunreinigungen führen Sie die ONNX Machine Learning-Modelle in SQL Edge aus.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: tutorial
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: f38a973611cb1ab18eead4ec51e6be91ada2cc40
ms.sourcegitcommit: 01cd19edb099d654198a6930cebd61cae9cb685b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85318640"
---
# <a name="deploy-ml-model-on-azure-sql-edge-using-onnx"></a>Bereitstellen eines ML-Modells in Azure SQL Edge mithilfe von ONNX 

Im dritten Teil dieses dreiteiligen Tutorials zum Vorhersagen von Eisenerzverunreinigungen in Azure SQL Edge führen Sie die folgenden Schritte aus:

1. Sie verwenden Azure Data Studio, um eine Verbindung mit SQL-Datenbank in der Azure SQL Edge-Instanz herzustellen.
2. Sie sagen Eisenerzverunreinigungen mit ONNX in Azure SQL Edge vorher.

## <a name="connect-to-the-sql-database-in-the-azure-sql-edge-instance"></a>Sie stellen eine Verbindung mit der SQL-Datenbank in der Azure SQL Edge-Instanz her.

1. Öffnen Sie Azure Data Studio.

2. Starten Sie auf der Registerkarte **Willkommen** eine neue Verbindung mit den folgenden Details:

   |_Feld_|_Wert_|
   |-------|-------|
   |Verbindungstyp| Microsoft SQL Server|
   |Server|Öffentliche IP-Adresse in der VM, die für diese Demo erstellt wurde|
   |Username|sa|
   |Kennwort|Das starke Kennwort, das bei der Erstellung der Azure SQL Edge-Instanz verwendet wurde|
   |Datenbank|Standard|
   |Servergruppe|Standard|
   |Name (optional)|Geben Sie einen optionalen Namen an.|

3. Klicken Sie auf das Menü **Verbinden**

4. Öffnen Sie im Abschnitt **Datei** ein neues Notebook, oder drücken Sie die Tastenkombination ALT+Windows+N. 

5. Legen Sie den Kernel auf „Python 3“ fest.

## <a name="predict-iron-ore-impurities-with-onnx"></a>Vorhersagen von Eisenerzverunreinigungen mit ONNX

Geben Sie den folgenden Python-Code im Azure Data Studio-Notebook ein, und führen Sie ihn aus.

1. Installieren und importieren Sie zuerst die erforderlichen Pakete.

   ```python
   !pip install azureml.core -q
   !pip install azureml.train.automl -q
   !pip install matplotlib -q
   !pip install pyodbc -q
   !pip install spicy -q
   
   import logging
   from matplotlib import pyplot as plt
   import numpy as np
   import pandas as pd
   import pyodbc
   
   from scipy import stats
   from scipy.stats import skew #for some statistics
   
   import azureml.core
   from azureml.core.experiment import Experiment
   from azureml.core.workspace import Workspace
   from azureml.train.automl import AutoMLConfig
   from azureml.train.automl import constants
   ```

1. Definieren Sie den Azure AutoML-Arbeitsbereich und die AutoML-Experimentkonfiguration für das Regressionsexperiment.

   ```python
   ws = Workspace(subscription_id="<Azure Subscription ID>",
                  resource_group="<resource group name>",
                  workspace_name="<ML workspace name>")
   # Choose a name for the experiment.
   experiment_name = 'silic_percent2-Forecasting-onnx'
   experiment = Experiment(ws, experiment_name)
   ```

1. Importieren Sie das Dataset in einen Panda-Frame. Verwenden Sie zum Modelltraining das Trainingsdataset [Quality Prediction in a Mining Process](https://www.kaggle.com/edumagalhaes/quality-prediction-in-a-mining-process) (Vorsagequalität in einem Abbauverfahren) von Kaggle. Laden Sie die Datendatei herunter, und speichern Sie sie lokal auf Ihrem Entwicklungscomputer. Anhand dieser Daten können Sie vorhersagen, wieviel Unreinheiten das Eisenerzkonzentrat enthält.

   ```python
   df = pd.read_csv("<local path where you have saved the data file>",decimal=",",parse_dates=["date"],infer_datetime_format=True)
   df = df.drop(['date'],axis=1)
   df.describe()
   ```

1. Analysieren Sie die Daten, um eine beliebige Schiefe zu identifizieren. Sehen Sie sich während dieses Vorgangs die Verteilung und die Informationen zur Schiefe für die einzelnen Spalten im Datenrahmen an.

   ```python
   ## We can use a histogram chart to view the data distribution for the Dataset. In this example, we are looking at the histogram for the "% Silica Concentrate" 
   ## and the "% Iron Feed". From the histogram, you'll notice the data distribution is skewed for most of the features in the dataset. 
   
   f, (ax1,ax2,ax3) = plt.subplots(1,3)
   ax1.hist(df['% Iron Feed'], bins='auto')
   #ax1.title = 'Iron Feed'
   ax2.hist(df['% Silica Concentrate'], bins='auto')
   #ax2.title = 'Silica Concentrate'
   ax3.hist(df['% Silica Feed'], bins='auto')
   #ax3.title = 'Silica Feed'
   ```

1. Überprüfen und korrigieren Sie die Ebene der Schiefe in den Daten.

   ```python
   ##Check data skewness with the skew or the kurtosis function in spicy.stats
   ##Skewness using the spicy.stats skew function
   for i in list(df):
           print('Skew value for column "{0}" is: {1}'.format(i,skew(df[i])))
   
   #Fix the Skew using Box Cox Transform
   from scipy.special import boxcox1p
   for i in list(df):
       if(abs(skew(df[i])) >= 0.20):
           #print('found skew in column - {0}'.format(i))
           df[i] = boxcox1p(df[i], 0.10)
           print('Skew value for column "{0}" is: {1}'.format(i,skew(df[i])))
   ```

1. Überprüfen Sie die Korrelation anderer Funktionen mit der Vorhersagefunktion. Wenn die Korrelation nicht hoch ist, entfernen Sie diese Funktionen.

   ```python
   silic_corr = df.corr()['% Silica Concentrate']
   silic_corr = abs(silic_corr).sort_values()
   drop_index= silic_corr.index[:8].tolist()
   df = df.drop(drop_index, axis=1)
   df.describe()
   ```

1. Starten Sie das AzureML-Experiment, um den besten Algorithmus zu suchen und zu trainieren. In diesem Fall testen Sie alle Regressionsalgorithmen mit einer primären Metrik der normalisierten Wurzel aus dem mittleren quadratischen Fehler (Normalized Root Mean Squared Error, NRMSE). Weitere Informationen finden Sie unter [Primäre Metrik für Azure ML-Experimente](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#primary-metric). Mit dem folgenden Code wird eine lokale Durchführung des ML-Experiments gestartet.

   ```python
   ## Define the X_train and the y_train data sets for the AutoML experiments. X_Train are the inputs or the features, while y_train is the outcome or the prediction result. 
   
   y_train = df['% Silica Concentrate']
   x_train = df.iloc[:,0:-1]
   automl_config = AutoMLConfig(task = 'regression',
                                primary_metric = 'normalized_root_mean_squared_error',
                                iteration_timeout_minutes = 60,
                                iterations = 10,                        
                                X = x_train, 
                                y = y_train,
                                featurization = 'off',
                                enable_onnx_compatible_models=True)
   
   local_run = experiment.submit(automl_config, show_output = True)
   best_run, onnx_mdl = local_run.get_output(return_onnx_model=True)
   ```

1. Laden Sie das Modell zur lokalen Bewertung in Azure SQL Edge Database.

   ```python
   ## Load the Model into a SQL Database.
   ## Define the Connection string parameters. These connection strings will be used later also in the demo.
   server = '<SQL Server IP address>'
   username = 'sa' # SQL Server username
   password = '<SQL Server password>'
   database = 'IronOreSilicaPrediction'
   db_connection_string = "Driver={ODBC Driver 17 for SQL Server};Server=" + server + ";Database=" + database + ";UID=" + username + ";PWD=" + password + ";"
   conn = pyodbc.connect(db_connection_string, autocommit=True)
   cursor = conn.cursor()
   
   # Insert the ONNX model into the models table
   query = f"insert into models ([description], [data]) values ('Silica_Percentage_Predict_Regression_NRMSE_New1',?)"
   model_bits = onnx_mdl.SerializeToString()
   insert_params  = (pyodbc.Binary(model_bits))
   cursor.execute(query, insert_params)
   conn.commit()
   cursor.close()
   conn.close()
   ```

1. Verwenden Sie abschließend das Azure SQL Edge-Modell, um Vorhersagen mithilfe des trainierten Modells auszuführen.

   ```python
   ## Define the Connection string parameters. These connection strings will be used later also in the demo.
   server = '<SQL Server IP address>'
   username = 'sa' # SQL Server username
   password = '<SQL Server password>'
   database = 'IronOreSilicaPrediction'
   db_connection_string = "Driver={ODBC Driver 17 for SQL Server};Server=" + server + ";Database=" + database + ";UID=" + username + ";PWD=" + password + ";"
   conn = pyodbc.connect(db_connection_string, autocommit=True)
   #cursor = conn.cursor()
   query = \
           f'declare @model varbinary(max) = (Select [data] from [dbo].[Models] where [id] = 1);' \
           f' with d as ( SELECT  [timestamp] ,cast([cur_Iron_Feed] as real) [__Iron_Feed] ,cast([cur_Silica_Feed]  as real) [__Silica_Feed]' \
           f',cast([cur_Starch_Flow] as real) [Starch_Flow],cast([cur_Amina_Flow] as real) [Amina_Flow]' \
           f' ,cast([cur_Ore_Pulp_pH] as real) [Ore_Pulp_pH] ,cast([cur_Flotation_Column_01_Air_Flow] as real) [Flotation_Column_01_Air_Flow]' \
           f' ,cast([cur_Flotation_Column_02_Air_Flow] as real) [Flotation_Column_02_Air_Flow]' \
           f' ,cast([cur_Flotation_Column_03_Air_Flow] as real) [Flotation_Column_03_Air_Flow]' \
           f' ,cast([cur_Flotation_Column_07_Air_Flow] as real) [Flotation_Column_07_Air_Flow]' \
           f' ,cast([cur_Flotation_Column_04_Level] as real) [Flotation_Column_04_Level]' \
           f' ,cast([cur_Flotation_Column_05_Level] as real) [Flotation_Column_05_Level]' \
           f' ,cast([cur_Flotation_Column_06_Level] as real) [Flotation_Column_06_Level]' \
           f' ,cast([cur_Flotation_Column_07_Level] as real) [Flotation_Column_07_Level]' \
           f' ,cast([cur_Iron_Concentrate] as real) [__Iron_Concentrate]' \
           f' FROM [dbo].[IronOreMeasurements1]' \
           f' where timestamp between dateadd(hour,-1,getdate()) and getdate()) ' \
           f' SELECT d.*, p.variable_out1' \
           f' FROM PREDICT(MODEL = @model, DATA = d) WITH(variable_out1 numeric(25,17)) as p;' 
     
   df_result = pd.read_sql(query,conn)
   df_result.describe()
   ```

1. Erstellen Sie mithilfe von Python ein Diagramm des vorhergesagten Prozentsatzes von Kieselsäure im Vergleich zur Eisenzufuhr, zu DateTime und zur Kieselsäurezufuhr.

   ```python
   import plotly.graph_objects as go
   fig = go.Figure()
   fig.add_trace(go.Scatter(x=df_result['timestamp'],y=df_result['__Iron_Feed'],mode='lines+markers',name='Iron Feed',line=dict(color='firebrick', width=2)))
   fig.add_trace(go.Scatter(x=df_result['timestamp'],y=df_result['__Silica_Feed'],mode='lines+markers',name='Silica Feed',line=dict(color='green', width=2)))
   fig.add_trace(go.Scatter(x=df_result['timestamp'],y=df_result['variable_out1'],mode='lines+markers',name='Silica Percent',line=dict(color='royalblue', width=3)))
   fig.update_layout(height= 600, width=1500,xaxis_title='Time')
   fig.show()
   ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von ONNX-Modellen in Azure SQL Edge finden Sie unter [Machine Learning und KI mit ONNX in SQL Edge (Vorschau)](onnx-overview.md).
