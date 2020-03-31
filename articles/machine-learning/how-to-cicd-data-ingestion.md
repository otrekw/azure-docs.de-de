---
title: DevOps für eine Datenerfassungspipeline
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie DevOps-Praktiken auf eine Implementierung einer Datenerfassungspipeline anwenden, die zum Vorbereiten von Daten für ein Modelltraining verwendet wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: iefedore
author: eedorenko
manager: davete
ms.reviewer: larryfr
ms.date: 01/30/2020
ms.openlocfilehash: d987171d41bd6d80bab4cce91ef9ecec1f0dc7a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247179"
---
# <a name="devops-for-a-data-ingestion-pipeline"></a>DevOps für eine Datenerfassungspipeline

In den meisten Szenarien ist eine Datenerfassungslösung eine Kombination aus Skripts, Dienstaufrufen und einer Pipeline, die alle Aktivitäten orchestriert. In diesem Artikel erfahren Sie, wie Sie DevOps-Praktiken auf den Entwicklungslebenszyklus einer allgemeinen Datenerfassungspipeline anwenden. Die Pipeline bereitet die Daten für das Machine Learning-Modelltraining vor.

## <a name="the-solution"></a>Die Lösung

Betrachten Sie den folgenden Datenerfassungsworkflow:

![data-ingestion-pipeline](media/how-to-cicd-data-ingestion/data-ingestion-pipeline.png)

Bei diesem Ansatz werden die Trainingsdaten in einem Azure-Blobspeicher gespeichert. Eine Azure Data Factory-Pipeline ruft die Daten aus einem Eingabeblobcontainer ab, wandelt sie um und speichert die Daten dann im Ausgabeblobcontainer. Dieser Container dient als [Datenspeicher](concept-data.md) für Azure Machine Learning Service. Wenn die Daten vorbereitet sind, ruft die Data Factory eine Machine Learning-Trainingspipeline auf, um ein Modell zu trainieren. In diesem speziellen Beispiel wird die Datentransformation von einem Python-Notebook ausgeführt, das in einem Azure Databricks-Cluster ausgeführt wird. 

## <a name="what-we-are-building"></a>Was wir erstellen

Wie bei jeder Softwarelösung ist ein Team (z. B. Data Engineers) daran beteiligt. 

![cicd-data-ingestion](media/how-to-cicd-data-ingestion/cicd-data-ingestion.png)

Die Mitglieder des Teams arbeiten zusammen und nutzen dieselben Azure-Ressourcen, z. B. Azure Data Factory, Azure Databricks, Azure Storage-Konto usw. Die Sammlung dieser Ressourcen ist eine Entwicklungsumgebung. Die Data Engineers tragen zur gleichen Quellcodebasis bei. Der Continuous Integration-Prozess stellt den Code zusammen, prüft ihn mit Codequalitätstests sowie Komponententests und generiert Artefakte wie getesteten Code und Azure Resource Manager-Vorlagen. Der Continuous Delivery-Prozess stellt die Artefakte in den Downstreamumgebungen bereit. In diesem Artikel wird veranschaulicht, wie die CI- und CD-Prozesse mit [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) automatisiert werden.

## <a name="source-control-management"></a>Quellcodeverwaltung

Die Teammitglieder arbeiten auf eine etwas andere Art und Weise an dem Python-Notebookquellcode und dem Azure Data Factory-Quellcode zusammen. In beiden Fällen wird der Code jedoch in einem Quellcoderepository (z. B. Azure DevOps, GitHub, GitLab) gespeichert, und die Zusammenarbeit basiert normalerweise auf einem Branchingmodell (z. B. [GitFlow](https://datasift.github.io/gitflow/IntroducingGitFlow.html)).

### <a name="python-notebook-source-code"></a>Python-Notebookquellcode

Die Data Engineers arbeiten mit dem Python-Notebookquellcode entweder lokal in einer IDE (z. B. [Visual Studio Code](https://code.visualstudio.com)) oder direkt im Databricks-Arbeitsbereich. Letzteres bietet die Möglichkeit, den Code in der Entwicklungsumgebung zu debuggen. In jedem Fall wird der Code gemäß einer Branchingrichtlinie in das Repository gemergt. Es wird dringend empfohlen, den Code nicht im `.py`-Format von Jupyter-Notebooks, sondern in `.ipynb`-Dateien zu speichern. Dadurch wird die Lesbarkeit des Codes verbessert, und es werden automatische Codequalitätsprüfungen im CI-Prozess ermöglicht.

### <a name="azure-data-factory-source-code"></a>Azure Data Factory-Quellcode

Der Quellcode von Azure Data Factory-Pipelines ist eine Sammlung von JSON-Dateien, die von einem Arbeitsbereich generiert werden. Normalerweise arbeiten die Data Engineers mit einem visuellen Designer im Azure Data Factory-Arbeitsbereich anstatt direkt mit den Quellcodedateien. Konfigurieren Sie den Arbeitsbereich mit einem Quellcodeverwaltungsrepository, wie in der [Dokumentation zu Azure Data Factory](https://docs.microsoft.com/azure/data-factory/source-control#author-with-azure-repos-git-integration) beschrieben. Wenn diese Konfiguration eingerichtet ist, können die Data Engineers am Quellcode gemäß einem bevorzugten Branchingworkflow zusammenarbeiten.    

## <a name="continuous-integration-ci"></a>Continuous Integration (CI)

Das ultimative Ziel des Continuous Integration-Prozesses besteht darin, die gemeinsame Teamarbeit aus dem Quellcode zu erfassen und diese für die Bereitstellung in den Downstreamumgebungen vorzubereiten. Wie bei der Quellcodeverwaltung unterscheidet sich dieser Prozess für die Python-Notebooks und Azure Data Factory-Pipelines. 

### <a name="python-notebook-ci"></a>Python-Notebook-CI

Der CI-Prozess für die Python-Notebooks ruft den Code aus dem Zusammenarbeitsbranch ab (z. B. ***master*** oder ***develop***) und führt die folgenden Aktivitäten aus:
* Codelinting
* Komponententests
* Speichern des Codes als Artefakt

Der folgende Codeausschnitt veranschaulicht die Implementierung dieser Schritte in einer Azure DevOps-***YAML***-Pipeline:

```yaml
steps:
- script: |
   flake8 --output-file=$(Build.BinariesDirectory)/lint-testresults.xml --format junit-xml  
  workingDirectory: '$(Build.SourcesDirectory)'
  displayName: 'Run flake8 (code style analysis)'  
  
- script: |
   python -m pytest --junitxml=$(Build.BinariesDirectory)/unit-testresults.xml $(Build.SourcesDirectory)
  displayName: 'Run unit tests'

- task: PublishTestResults@2
  condition: succeededOrFailed()
  inputs:
    testResultsFiles: '$(Build.BinariesDirectory)/*-testresults.xml'
    testRunTitle: 'Linting & Unit tests'
    failTaskOnFailedTests: true
  displayName: 'Publish linting and unit test results'

- publish: $(Build.SourcesDirectory)
    artifact: di-notebooks

```

Die Pipeline verwendet ***flake8***, um das Python-Codelinting auszuführen. Sie führt die im Quellcode definierten Komponententests aus und veröffentlicht die Linting- und Testergebnisse, sodass sie auf dem Ausführungsbildschirm der Azure-Pipeline verfügbar sind:

![linting-unit-tests](media/how-to-cicd-data-ingestion/linting-unit-tests.png)

Wenn die Linting- und Komponententests erfolgreich ausgeführt werden, kopiert die Pipeline den Quellcode in das Artefaktrepository, damit er von den nachfolgenden Bereitstellungsschritten verwendet werden kann.

### <a name="azure-data-factory-ci"></a>Azure Data Factory-CI

Der CI-Prozess für eine Azure Data Factory-Pipeline ist ein Engpass in der gesamten CI/CD-Story für eine Datenerfassungspipeline. Es gibt keine ***Continuous*** Integration. Ein bereitstellbares Artefakt für Azure Data Factory ist eine Sammlung von Azure Resource Manager-Vorlagen. Diese Vorlagen können Sie nur erstellen, indem Sie im Azure Data Factory-Arbeitsbereich auf die Schaltfläche ***Veröffentlichen*** klicken. Hier gibt es keine Automatisierung.
Der Quellcode wird von den Data Engineers aus ihren Funktionsbranches in den Zusammenarbeitsbranch gemergt, z. B. in ***master*** oder ***develop***. Dann klickt ein Benutzer mit den gewährten Berechtigungen auf die Schaltfläche ***Veröffentlichen***, um Azure Resource Manager-Vorlagen aus dem Quellcode im Zusammenarbeitsbranch zu generieren. Wenn auf die Schaltfläche geklickt wird, überprüft der Arbeitsbereich die Pipelines (etwa Linting und Komponententests), generiert Azure Resource Manager-Vorlagen (ähnlich einem Buildvorgang) und speichert die generierten Vorlagen in einem technischen Branch ***adf_publish*** im gleichen Coderepository (gleicht der Veröffentlichung von Artefakten). Dieser Branch wird automatisch vom Azure Data Factory-Arbeitsbereich erstellt. Dieser Prozess wird in der [Dokumentation zu Azure Data Factory](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment) ausführlich beschrieben.

Es ist wichtig sicherzustellen, dass die generierten Azure Resource Manager-Vorlagen umgebungsagnostisch sind. Dies bedeutet, dass alle Werte, die sich zwischen Umgebungen unterscheiden können, parametrisiert werden. Azure Data Factory ist intelligent genug, um die Mehrzahl dieser Werte als Parameter bereitzustellen. In der folgenden Vorlage werden z. B. die Verbindungseigenschaften für einen Azure Machine Learning-Arbeitsbereich als Parameter zur Verfügung gestellt:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "factoryName": {
            "value": "devops-ds-adf"
        },
        "AzureMLService_servicePrincipalKey": {
            "value": ""
        },
        "AzureMLService_properties_typeProperties_subscriptionId": {
            "value": "0fe1c235-5cfa-4152-17d7-5dff45a8d4ba"
        },
        "AzureMLService_properties_typeProperties_resourceGroupName": {
            "value": "devops-ds-rg"
        },
        "AzureMLService_properties_typeProperties_servicePrincipalId": {
            "value": "6e35e589-3b22-4edb-89d0-2ab7fc08d488"
        },
        "AzureMLService_properties_typeProperties_tenant": {
            "value": "72f988bf-86f1-41af-912b-2d7cd611db47"
        }
    }
}
```

Möglicherweise möchten Sie jedoch Ihre benutzerdefinierten Eigenschaften verfügbar machen, die nicht standardmäßig vom Azure Data Factory-Arbeitsbereich verarbeitet werden. Im Szenario dieses Artikels ruft eine Azure Data Factory-Pipeline ein Python-Notebook auf, das die Daten verarbeitet. Das Notebook akzeptiert einen Parameter mit dem Namen einer Eingabedatendatei.

```Python
import pandas as pd
import numpy as np

data_file_name = getArgument("data_file_name")
data = pd.read_csv(data_file_name)

labels = np.array(data['target'])
...
```

Dieser Name unterscheidet sich für ***Dev***- (Entwicklung), ***QA***- (Qualitätssicherung), ***UAT***- (Akzeptanztests) und ***Prod***-Umgebungen (Produktion). In einer komplexen Pipeline mit mehreren Aktivitäten können mehrere benutzerdefinierte Eigenschaften vorhanden sein. Es empfiehlt sich, alle diese Werte an einem Ort zu sammeln und als ***Variablen*** der Pipeline zu definieren:

![adf-variables](media/how-to-cicd-data-ingestion/adf-variables.png)

Die Pipelineaktivitäten können auf die Pipelinevariablen verweisen, während sie tatsächlich verwendet werden:

![adf-notebook-parameters](media/how-to-cicd-data-ingestion/adf-notebook-parameters.png)

Der Azure Data Factory-Arbeitsbereich stellt Pipelinevariablen ***nicht*** standardmäßig als Azure Resource Manager-Vorlagenparameter bereit. Der Arbeitsbereich verwendet die [Standardparameterisierungsvorlage](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#default-parameterization-template), die vorgibt, welche Pipelineeigenschaften als Azure Resource Manager-Vorlagenparameter verfügbar gemacht werden sollen. Um der Liste Pipelinevariablen hinzuzufügen, aktualisieren Sie den Abschnitt „Microsoft.DataFactory/factories/pipelines“ der [Standardparameterisierungsvorlage](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#default-parameterization-template) mit dem folgenden Codeausschnitt, und platzieren Sie die JSON-Ergebnisdatei im Stammverzeichnis des Quellordners:

```json
"Microsoft.DataFactory/factories/pipelines": {
        "properties": {
            "variables": {
                "*": {
                    "defaultValue": "="
                }
            }
        }
    }
```

Dadurch wird der Azure Data Factory-Arbeitsbereich gezwungen, die Variablen der Parameterliste hinzuzufügen, wenn auf die Schaltfläche ***Veröffentlichen*** geklickt wird:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "factoryName": {
            "value": "devops-ds-adf"
        },
        ...
        "data-ingestion-pipeline_properties_variables_data_file_name_defaultValue": {
            "value": "driver_prediction_train.csv"
        }        
    }
}
```

Bei den Werten in der JSON-Datei handelt es sich um in der Pipelinedefinition konfigurierte Standardwerte. Beim Bereitstellen der Azure Resource Manager-Vorlage wird erwartet, dass sie mit den Werten der Zielumgebung überschrieben werden.

## <a name="continuous-delivery-cd"></a>Continuous Delivery (CD)

Der Continuous Delivery-Prozess verwendet die Artefakte und stellt sie in der ersten Zielumgebung bereit. Er stellt sicher, dass die Lösung funktioniert, indem Tests ausgeführt werden. Bei Erfolg wird der Vorgang in der nächsten Umgebung fortgesetzt. Die Azure-CD-Pipeline besteht aus mehreren Stufen, die die Umgebungen darstellen. Jede Stufe enthält [Bereitstellungen](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) und [Aufträge](https://docs.microsoft.com/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml), mit denen die folgenden Schritte ausgeführt werden:
* Bereitstellen eines Python-Notebooks im Azure Databricks-Arbeitsbereich
* Bereitstellen einer Azure Data Factory-Pipeline 
* Führen Sie die Pipeline aus.
* Überprüfen des Ergebnisses der Datenerfassung

Die Pipelinestufen können mit [Genehmigungen](https://docs.microsoft.com/azure/devops/pipelines/process/approvals?view=azure-devops&tabs=check-pass) und [Gates](https://docs.microsoft.com/azure/devops/pipelines/release/approvals/gates?view=azure-devops) konfiguriert werden, die zusätzliche Kontrolle über die Weiterentwicklung des Bereitstellungsprozesses in der Kette der Umgebungen bieten.

### <a name="deploy-a-python-notebook"></a>Bereitstellen eines Python-Notebooks

Mit dem folgenden Codeausschnitt wird die [Bereitstellungs](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) einer Azure-Pipeline definiert, die ein Python-Notebook in einen Databricks-Cluster kopiert:

```yaml
- stage: 'Deploy_to_QA'
  displayName: 'Deploy to QA'
  variables:
  - group: devops-ds-qa-vg
  jobs:
  - deployment: "Deploy_to_Databricks"
    displayName: 'Deploy to Databricks'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: UsePythonVersion@0
              inputs:
                versionSpec: '3.x'
                addToPath: true
                architecture: 'x64'
              displayName: 'Use Python3'

            - task: configuredatabricks@0
              inputs:
                url: '$(DATABRICKS_URL)'
                token: '$(DATABRICKS_TOKEN)'
              displayName: 'Configure Databricks CLI'    

            - task: deploynotebooks@0
              inputs:
                notebooksFolderPath: '$(Pipeline.Workspace)/di-notebooks'
                workspaceFolder: '/Shared/devops-ds'
              displayName: 'Deploy (copy) data processing notebook to the Databricks cluster'       
```            

Die von CI erstellten Artefakte werden automatisch in den Bereitstellungs-Agent kopiert und sind im Ordner ***$(Pipeline.Workspace)*** verfügbar. In diesem Fall verweist die Bereitstellungsaufgabe auf das ***di-notebooks***-Artefakt, das das Python-Notebook enthält. Diese [Bereitstellungs](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) verwendet die [Databricks Azure DevOps-Erweiterung](https://marketplace.visualstudio.com/items?itemName=riserrad.azdo-databricks), um die Notebookdateien in den Databricks-Arbeitsbereich zu kopieren.
Die Stufe ***Deploy_to_QA*** enthält einen Verweis auf die im Azure DevOps-Projekt definierte Variablenguppe ***devops-ds-qa-vg***. Die Schritte in dieser Stufe verweisen auf die Variablen aus dieser Variablengruppe (z. B. $(DATABRICKS_URL), $(DATABRICKS_TOKEN)). Der Gedanke dabei ist, dass die nächste Stufe (z. B. ***Deploy_to_UAT***) mit denselben Variablennamen ausgeführt wird, die in der eigenen Variablengruppe mit dem Bereich „UAT“ definiert sind.

### <a name="deploy-an-azure-data-factory-pipeline"></a>Bereitstellen einer Azure Data Factory-Pipeline

Ein bereitstellbares Artefakt für Azure Data Factory ist eine Azure Resource Manager-Vorlage. Daher wird sie mit der Aufgabe ***Azure-Ressourcengruppenbereitstellung*** bereitgestellt, wie im folgenden Codeausschnitt veranschaulicht wird:

```yaml
  - deployment: "Deploy_to_ADF"
    displayName: 'Deploy to ADF'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: AzureResourceGroupDeployment@2
              displayName: 'Deploy ADF resources'
              inputs:
                azureSubscription: $(AZURE_RM_CONNECTION)
                resourceGroupName: $(RESOURCE_GROUP)
                location: $(LOCATION)
                csmFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateForFactory.json'
                csmParametersFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateParametersForFactory.json'
                overrideParameters: -data-ingestion-pipeline_properties_variables_data_file_name_defaultValue "$(DATA_FILE_NAME)"
```
Der Wert des Parameters „data filename“ stammt von der Variablen $(DATA_FILE_NAME), die in einer Variablengruppe der QA-Stufe definiert ist. Auf ähnliche Weise können alle Parameter, die in ***ARMTemplateForFactory.json*** definiert sind, überschrieben werden. Wenn dies nicht der Fall ist, werden die Standardwerte verwendet.

### <a name="run-the-pipeline-and-check-the-data-ingestion-result"></a>Ausführen der Pipeline und Überprüfen des Ergebnisses der Datenerfassung

Im nächsten Schritt müssen Sie sicherstellen, dass die bereitgestellte Lösung funktioniert. Die folgende Auftragsdefinition führt eine Azure Data Factory-Pipeline mit einem [PowerShell-Skript](https://github.com/microsoft/DataOps/tree/master/adf/utils) aus und führt ein Python-Notebook in einem Azure Databricks-Cluster aus. Das Notebook überprüft, ob die Daten ordnungsgemäß erfasst wurden, und validiert die Ergebnisdatendatei mit dem Namen $(bin_FILE_NAME).

```yaml
  - job: "Integration_test_job"
    displayName: "Integration test job"
    dependsOn: [Deploy_to_Databricks, Deploy_to_ADF]
    pool:
      vmImage: 'ubuntu-latest'
    timeoutInMinutes: 0
    steps:
    - task: AzurePowerShell@4
      displayName: 'Execute ADF Pipeline'
      inputs:
        azureSubscription: $(AZURE_RM_CONNECTION)
        ScriptPath: '$(Build.SourcesDirectory)/adf/utils/Invoke-ADFPipeline.ps1'
        ScriptArguments: '-ResourceGroupName $(RESOURCE_GROUP) -DataFactoryName $(DATA_FACTORY_NAME) -PipelineName $(PIPELINE_NAME)'
        azurePowerShellVersion: LatestVersion
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.x'
        addToPath: true
        architecture: 'x64'
      displayName: 'Use Python3'

    - task: configuredatabricks@0
      inputs:
        url: '$(DATABRICKS_URL)'
        token: '$(DATABRICKS_TOKEN)'
      displayName: 'Configure Databricks CLI'    

    - task: executenotebook@0
      inputs:
        notebookPath: '/Shared/devops-ds/test-data-ingestion'
        existingClusterId: '$(DATABRICKS_CLUSTER_ID)'
        executionParams: '{"bin_file_name":"$(bin_FILE_NAME)"}'
      displayName: 'Test data ingestion'

    - task: waitexecution@0
      displayName: 'Wait until the testing is done'
```

Die letzte Aufgabe im Auftrag überprüft das Ergebnis der Notebookausführung. Wenn ein Fehler zurückgegeben wird, wird der Status der Pipelineausführung auf „fehlerhaft“ festgelegt.

## <a name="putting-pieces-together"></a>Zusammenführen der einzelnen Elemente

Das Ergebnis dieses Artikels ist eine Azure-CI/CD-Pipeline, die aus den folgenden Stufen besteht:
* CI
* Bereitstellung für QA
    * Bereitstellung für Databricks und Bereitstellung in ADF
    * Integrationstest

Sie enthält eine Reihe von ***Bereitstellungsstufen***, die der Anzahl der Zielumgebungen entsprechen. Jede ***Bereitstellungsstufe*** enthält zwei [Bereitstellungen](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops), die parallel ausgeführt werden, sowie einen [Auftrag](https://docs.microsoft.com/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml), der nach den Bereitstellungen ausgeführt wird, um die Lösung für die Umgebung zu testen.

Eine Beispielimplementierung der Pipeline wird im folgenden ***yaml***-Codeausschnitt zusammengestellt:

```yaml
variables:
- group: devops-ds-vg

stages:
- stage: 'CI'
  displayName: 'CI'
  jobs:
  - job: "CI_Job"
    displayName: "CI Job"
    pool:
      vmImage: 'ubuntu-latest'
    timeoutInMinutes: 0
    steps:
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.x'
        addToPath: true
        architecture: 'x64'
      displayName: 'Use Python3'
    - script: pip install --upgrade flake8 flake8_formatter_junit_xml
      displayName: 'Install flake8'
    - checkout: self
    - script: |
       flake8 --output-file=$(Build.BinariesDirectory)/lint-testresults.xml --format junit-xml  
    workingDirectory: '$(Build.SourcesDirectory)'
    displayName: 'Run flake8 (code style analysis)'  
    - script: |
       python -m pytest --junitxml=$(Build.BinariesDirectory)/unit-testresults.xml $(Build.SourcesDirectory)
    displayName: 'Run unit tests'
    - task: PublishTestResults@2
    condition: succeededOrFailed()
    inputs:
        testResultsFiles: '$(Build.BinariesDirectory)/*-testresults.xml'
        testRunTitle: 'Linting & Unit tests'
        failTaskOnFailedTests: true
    displayName: 'Publish linting and unit test results'    

    # The CI stage produces two artifacts (notebooks and ADF pipelines).
    # The pipelines Azure Resource Manager templates are stored in a technical branch "adf_publish"
    - publish: $(Build.SourcesDirectory)/$(Build.Repository.Name)/code/dataingestion
      artifact: di-notebooks
    - checkout: git://${{variables['System.TeamProject']}}@adf_publish    
    - publish: $(Build.SourcesDirectory)/$(Build.Repository.Name)/devops-ds-adf
      artifact: adf-pipelines

- stage: 'Deploy_to_QA'
  displayName: 'Deploy to QA'
  variables:
  - group: devops-ds-qa-vg
  jobs:
  - deployment: "Deploy_to_Databricks"
    displayName: 'Deploy to Databricks'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: UsePythonVersion@0
              inputs:
                versionSpec: '3.x'
                addToPath: true
                architecture: 'x64'
              displayName: 'Use Python3'

            - task: configuredatabricks@0
              inputs:
                url: '$(DATABRICKS_URL)'
                token: '$(DATABRICKS_TOKEN)'
              displayName: 'Configure Databricks CLI'    

            - task: deploynotebooks@0
              inputs:
                notebooksFolderPath: '$(Pipeline.Workspace)/di-notebooks'
                workspaceFolder: '/Shared/devops-ds'
              displayName: 'Deploy (copy) data processing notebook to the Databricks cluster'             
  - deployment: "Deploy_to_ADF"
    displayName: 'Deploy to ADF'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: AzureResourceGroupDeployment@2
              displayName: 'Deploy ADF resources'
              inputs:
                azureSubscription: $(AZURE_RM_CONNECTION)
                resourceGroupName: $(RESOURCE_GROUP)
                location: $(LOCATION)
                csmFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateForFactory.json'
                csmParametersFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateParametersForFactory.json'
                overrideParameters: -data-ingestion-pipeline_properties_variables_data_file_name_defaultValue "$(DATA_FILE_NAME)"
  - job: "Integration_test_job"
    displayName: "Integration test job"
    dependsOn: [Deploy_to_Databricks, Deploy_to_ADF]
    pool:
      vmImage: 'ubuntu-latest'
    timeoutInMinutes: 0
    steps:
    - task: AzurePowerShell@4
      displayName: 'Execute ADF Pipeline'
      inputs:
        azureSubscription: $(AZURE_RM_CONNECTION)
        ScriptPath: '$(Build.SourcesDirectory)/adf/utils/Invoke-ADFPipeline.ps1'
        ScriptArguments: '-ResourceGroupName $(RESOURCE_GROUP) -DataFactoryName $(DATA_FACTORY_NAME) -PipelineName $(PIPELINE_NAME)'
        azurePowerShellVersion: LatestVersion
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.x'
        addToPath: true
        architecture: 'x64'
      displayName: 'Use Python3'

    - task: configuredatabricks@0
      inputs:
        url: '$(DATABRICKS_URL)'
        token: '$(DATABRICKS_TOKEN)'
      displayName: 'Configure Databricks CLI'    

    - task: executenotebook@0
      inputs:
        notebookPath: '/Shared/devops-ds/test-data-ingestion'
        existingClusterId: '$(DATABRICKS_CLUSTER_ID)'
        executionParams: '{"bin_file_name":"$(bin_FILE_NAME)"}'
      displayName: 'Test data ingestion'

    - task: waitexecution@0
      displayName: 'Wait until the testing is done'                

```

## <a name="next-steps"></a>Nächste Schritte

* [Quellcodeverwaltung in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/source-control)
* [Continuous Integration und Continuous Delivery in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment)
* [Connectors für Azure Databricks](https://marketplace.visualstudio.com/items?itemName=riserrad.azdo-databricks)
