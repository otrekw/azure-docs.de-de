---
title: Verwenden von Azure Pipelines zum Erstellen und Bereitstellen von HPC-Lösungen
description: Erfahren Sie, wie Sie eine Build-/Releaseepipeline für eine in Azure Batch ausgeführte HPC-Anwendung bereitstellen.
author: chrisreddington
ms.author: chredd
ms.date: 03/04/2021
ms.topic: how-to
ms.openlocfilehash: 7170044af58a508ff5a43751cc376f8b8d498444
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102435544"
---
# <a name="use-azure-pipelines-to-build-and-deploy-hpc-solutions"></a>Verwenden von Azure Pipelines zum Erstellen und Bereitstellen von HPC-Lösungen

Tools von Azure DevOps dienen zum Übersetzen in das automatische Erstellen und Testen von Hochleistungscomputinglösungen (HPC). [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines) bietet eine Reihe moderner Continuous Integration- (CI) und Continuous Deployment-Prozesse (CD) zum Erstellen, Bereitstellen, Testen und Überwachen von Software. Diese Prozesse beschleunigen die Auslieferung Ihrer Software, sodass Sie sich auf den Code konzentrieren können, anstatt sich um die Infrastruktur und sonstige Abläufe kümmern zu müssen.

In diesem Artikel wird erklärt, wie Sie CI/CD-Prozesse mit [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines) für HPC-Lösungen einrichten, die auf Azure Batch bereitgestellt werden.

## <a name="prerequisites"></a>Voraussetzungen

Um den Schritten in diesem Artikel folgen zu können, benötigen Sie eine [Azure DevOps-Organisation](/azure/devops/organizations/accounts/create-organization). Sie müssen auch ein [Projekt in Azure DevOps erstellen](/azure/devops/organizations/projects/create-project).

Ein grundlegendes Verständnis von [Quellcodeverwaltung](/azure/devops/user-guide/source-control) und [Azure Resource Manager-Vorlagesyntax](../azure-resource-manager/templates/template-syntax.md) ist hilfreich, bevor Sie beginnen.

## <a name="create-an-azure-pipeline"></a>Erstellen einer Azure Pipeline

In diesem Beispiel erstellen Sie eine Build- und Releaseepipeline, um eine Azure Batch-Infrastruktur bereitzustellen und ein Anwendungspaket herauszugeben. Vorausgesetzt, dass der Code lokal entwickelt wird, ist dies der Ablauf der allgemeinen Bereitstellung:

![Diagramm des Bereitstellungsablaufs in der Pipeline](media/batch-ci-cd/DeploymentFlow.png)

In diesem Beispiel werden mehrere Azure Resource Manager-Vorlagen und vorhandene Binärdateien verwendet. Sie können diese Beispiele in das Repository kopieren und an Azure DevOps pushen.

### <a name="understand-the-azure-resource-manager-templates"></a>Grundlegendes zu Azure Resource Manager-Vorlagen

In diesem Beispiel werden mehrere Azure Resource Manager-Vorlagen verwendet, um die Lösung bereitzustellen. Drei Funktionsvorlagen (ähnlich Einheiten oder Modulen) werden verwendet, um eine bestimmte Funktion zu implementieren. Eine End-to-End-Lösungsvorlage (deployment.json) wird dann verwendet, um diese zugrunde liegenden Funktionsvorlagen bereitzustellen. Mit dieser [verknüpften Vorlagenstruktur](../azure-resource-manager/templates/deployment-tutorial-linked-template.md) kann jede Funktionsvorlage einzeln getestet und in allen Lösungen wieder verwendet werden.

![Diagramm einer verknüpften Vorlagenstruktur unter Verwendung von Azure Resource Manager-Vorlagen.](media/batch-ci-cd/ARMTemplateHierarchy.png)

Diese Vorlage definiert ein Azure-Speicherkonto, das erforderlich ist, um die Anwendung auf dem Batch-Konto bereitzustellen. Ausführliche Informationen finden Sie im [Referenzhandbuch für Resource Manager-Vorlagen für Microsoft.Storage-Ressourcentypen](/azure/templates/microsoft.storage/allversions).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Storage Account"
             }
         }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('accountName')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "apiVersion": "2018-02-01",
            "location": "[resourceGroup().location]",
            "properties": {}
        }
    ],
    "outputs": {
        "blobEndpoint": {
          "type": "string",
          "value": "[reference(resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))).primaryEndpoints.blob]"
        },
        "resourceId": {
          "type": "string",
          "value": "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
        }
    }
}
```

In der nächsten Vorlage wird ein [Azure Batch-Konto](accounts.md)definiert. Das Azure Batch-Konto fungiert als Plattform zum Ausführen zahlreicher Anwendungen über [Pools](nodes-and-pools.md#pools) hinweg. Ausführliche Informationen finden Sie im [Referenzhandbuch für Resource Manager-Vorlagen für Microsoft.Batch-Ressourcentypen](/azure/templates/microsoft.batch/allversions).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "batchAccountName": {
           "type": "string",
           "metadata": {
                "description": "Name of the Azure Batch Account"
            }
        },
        "storageAccountId": {
           "type": "string",
           "metadata": {
                "description": "ID of the Azure Storage Account"
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "name": "[parameters('batchAccountName')]",
            "type": "Microsoft.Batch/batchAccounts",
            "apiVersion": "2017-09-01",
            "location": "[resourceGroup().location]",
            "properties": {
              "poolAllocationMode": "BatchService",
              "autoStorage": {
                  "storageAccountId": "[parameters('storageAccountId')]"
              }
            }
          }
    ],
    "outputs": {}
}
```

Mit der nächsten Vorlage wird im Azure Batch-Konto ein Azure Batch-Pool erstellt. Ausführliche Informationen finden Sie im [Referenzhandbuch für Resource Manager-Vorlagen für Microsoft.Batch-Ressourcentypen](/azure/templates/microsoft.batch/allversions).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "batchAccountName": {
           "type": "string",
           "metadata": {
                "description": "Name of the Azure Batch Account"
           }
        },
        "batchAccountPoolName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Batch Account Pool"
             }
         }
    },
    "variables": {},
    "resources": [
        {
            "name": "[concat(parameters('batchAccountName'),'/', parameters('batchAccountPoolName'))]",
            "type": "Microsoft.Batch/batchAccounts/pools",
            "apiVersion": "2017-09-01",
            "properties": {
                "deploymentConfiguration": {
                    "virtualMachineConfiguration": {
                        "imageReference": {
                            "publisher": "Canonical",
                            "offer": "UbuntuServer",
                            "sku": "18.04-LTS",
                            "version": "latest"
                        },
                        "nodeAgentSkuId": "batch.node.ubuntu 18.04"
                    }
                },
                "vmSize": "Standard_D1_v2"
            }
          }
    ],
    "outputs": {}
}
```

Die letzte Vorlage fungiert als Orchestrator, der die drei zugrunde liegenden Funktionsvorlagen bereitstellt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "templateContainerUri": {
           "type": "string",
           "metadata": {
                "description": "URI of the Blob Storage Container containing the Azure Resource Manager templates"
            }
        },
        "templateContainerSasToken": {
           "type": "string",
           "metadata": {
                "description": "The SAS token of the container containing the Azure Resource Manager templates"
            }
        },
        "applicationStorageAccountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Storage Account"
            }
         },
        "batchAccountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Batch Account"
            }
         },
         "batchAccountPoolName": {
             "type": "string",
             "metadata": {
                  "description": "Name of the Azure Batch Account Pool"
              }
          }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "storageAccountDeployment",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/storageAccount.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "accountName": {"value": "[parameters('applicationStorageAccountName')]"}
                }
            }
        },  
        {
            "apiVersion": "2017-05-10",
            "name": "batchAccountDeployment",
            "type": "Microsoft.Resources/deployments",
            "dependsOn": [
                "storageAccountDeployment"
            ],
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/batchAccount.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "batchAccountName": {"value": "[parameters('batchAccountName')]"},
                    "storageAccountId": {"value": "[reference('storageAccountDeployment').outputs.resourceId.value]"}
                }
            }
        },
        {
            "apiVersion": "2017-05-10",
            "name": "poolDeployment",
            "type": "Microsoft.Resources/deployments",
            "dependsOn": [
                "batchAccountDeployment"
            ],
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/batchAccountPool.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "batchAccountName": {"value": "[parameters('batchAccountName')]"},
                    "batchAccountPoolName": {"value": "[parameters('batchAccountPoolName')]"}
                }
            }
        }
    ],
    "outputs": {}
}
```

### <a name="understand-the-hpc-solution"></a>Grundlegendes zur HPC-Lösung

Wie bereits erwähnt, werden in diesem Beispiel mehrere Azure Resource Manager-Vorlagen und vorhandene Binärdateien verwendet. Sie können diese Beispiele in das Repository kopieren und an Azure DevOps pushen.

Für diese Lösung wird „ffmpeg“ als Anwendungspaket verwendet. Sie können das [ffmpeg-Paket herunterladen](https://github.com/GyanD/codexffmpeg/releases/tag/4.3.1-2020-11-08), wenn es noch nicht vorhanden ist.

![Screenshot der Repositorystruktur.](media/batch-ci-cd/git-repository.jpg)

Dieses Repository hat vier Hauptabschnitte:

- Ein **arm-templates**-Ordner, der die Azure Resource Manager-Vorlagen enthält
- Ein **hpc-application**-Ordner, der die Windows-64-Bit-Version von [ffmpeg 4.3.1](https://github.com/GyanD/codexffmpeg/releases/tag/4.3.1-2020-11-08) enthält.
- Ein **pipelines**-Ordner, der die YAML-Datei enthält, die den Buildpipeline-Prozess definiert.
- Optional: Ein **client-application**-Ordner, der eine Kopie des Beispiels [Azure Batch .NET File Processing with ffmpeg](https://github.com/Azure-Samples/batch-dotnet-ffmpeg-tutorial) (Azure Batch .NET-Dateiverarbeitung mit ffmpeg) ist. Diese Anwendung ist für diesen Artikel nicht erforderlich.


> [!NOTE]
> Dies ist nur ein Beispiel einer Struktur einer Codebasis. Dieser Ansatz soll veranschaulichen, dass Anwendungs-, Infrastruktur- und Pipelinecode im selben Repository gespeichert werden.

Nun ist der Quellcode eingerichtet, und Sie können mit dem ersten Build beginnen.

## <a name="continuous-integration"></a>Continuous Integration

Mit [Azure Pipelines](/azure/devops/pipelines/get-started/) innerhalb von Azure DevOps Services können Sie eine Erstellungs-, Test- und Bereitstellungspipeline für Ihre Anwendungen implementieren.

In dieser Phase der Pipeline werden Tests in der Regel zum Überprüfen von Code und Erstellen der entsprechenden Teile der Software ausgeführt. Anzahl und Typen der Tests sowie weitere Aufgaben, die Sie ausführen, hängen von Ihrer übergeordneten Build- und Releasestrategie ab.

## <a name="prepare-the-hpc-application"></a>Vorbereiten der HPC-Anwendung

In diesem Abschnitt verwenden Sie dem Ordner **hpc-application**. Dieser Ordner enthält die Software (ffmpeg), die innerhalb des Azure Batch-Kontos ausgeführt wird.

1. Navigieren Sie zum Abschnitt „Builds“ von Azure Pipelines in Ihrer Azure DevOps-Organisation. Erstellen Sie eine **Neue Pipeline**.

    ![Screenshot des Bildschirms „Neue Pipeline“.](media/batch-ci-cd/new-build-pipeline.jpg)

1. Eine Buildpipeline kann auf zwei Arten erstellt werden:

    a. [Verwenden Sie den visuellen Designer](/azure/devops/pipelines/get-started-designer). Wählen Sie dazu auf der Seite **Neue Pipeline** die Option „Visuellen Designer verwenden“ aus.

    b. [Verwenden Sie YAML-Builds](/azure/devops/pipelines/get-started-yaml). Sie können eine neue YAML-Pipeline erstellen, indem Sie auf der Seite **Neue Pipeline** auf die Option für Azure Repos oder GitHub klicken. Alternativ können Sie das folgende Beispiel in Ihrer Quellcodeverwaltung speichern und auf eine vorhandene YAML-Datei verweisen, indem Sie den visuellen Designer auswählen und dann die YAML-Vorlage verwenden.

    ```yml
    # To publish an application into Azure Batch, we need to
    # first zip the file, and then publish an artifact, so that
    # we can take the necessary steps in our release pipeline.
    steps:
    # First, we Zip up the files required in the Batch Account
    # For this instance, those are the ffmpeg files
    - task: ArchiveFiles@2
      displayName: 'Archive applications'
      inputs:
        rootFolderOrFile: hpc-application
        includeRootFolder: false
        archiveFile: '$(Build.ArtifactStagingDirectory)/package/$(Build.BuildId).zip'
    # Publish that zip file, so that we can use it as part
    # of our Release Pipeline later
    - task: PublishPipelineArtifact@0
      inputs:
        artifactName: 'hpc-application'
        targetPath: '$(Build.ArtifactStagingDirectory)/package'
    ```

1. Sobald der Build wie erforderlich konfiguriert ist, wählen Sie **Speichern und in Warteschlange einreihen** aus. Wenn Sie (im **Trigger**-Abschnitt) fortlaufende Integration aktiviert haben, wird der Build automatisch ausgelöst, wenn gemäß der im Build festgelegten Bedingungen ein neuer Commit an das Repository vorgenommen wird.

    ![Screenshot einer vorhandenen Buildpipeline.](media/batch-ci-cd/existing-build-pipeline.jpg)

1. Um Liveupdates über den Fortschritt des Builds in Azure DevOps anzuzeigen, navigieren Sie zum **Build**-Abschnitt von Azure Pipelines. Wählen Sie den entsprechenden Build aus der Builddefinition aus.

    ![Screenshot der Liveausgaben des Builds in Azure DevOps.](media/batch-ci-cd/Build-1.jpg)

> [!NOTE]
> Wenn Sie eine Clientanwendung zum Ausführen der HPC-Lösung verwenden, müssen Sie eine separate Builddefinition für diese Anwendung erstellen. Sie finden eine Reihe von Gewusst-wie-Anleitungen in der Dokumentation zu [Azure Pipelines](/azure/devops/pipelines/get-started/index).

## <a name="continuous-deployment"></a>Kontinuierliche Bereitstellung

Azure Pipelines wird auch zum Bereitstellen Ihrer Anwendung und der zugrunde liegenden Infrastruktur verwenden. [Releasepipelines](/azure/devops/pipelines/release) ermöglichen die kontinuierliche Bereitstellung und automatisiert Ihren Herausgabeprozess.

### <a name="deploy-your-application-and-underlying-infrastructure"></a>Bereitstellen der Anwendung und der zugrunde liegenden Infrastruktur

Das Bereitstellen der Infrastruktur ist mit einer Reihe von Schritten verbunden. Da diese Lösung [verknüpfte Vorlagen](../azure-resource-manager/templates/linked-templates.md) verwendet, muss der Zugriff auf diese Vorlagen von einem öffentlichen Endpunkt (HTTP oder HTTPS) möglich sein. Dies kann ein Repository auf GitHub, ein Azure Blob Storage-Konto oder ein anderer Speicherort sein. Die hochgeladenen Vorlagenartefakte können sicher bleiben, weil sie in einem privaten Modus gehalten werden können, der Zugriff auf sie aber mithilfe einer Form des Shared Access Signature-Tokens (SAS) erfolgt.

Das folgende Beispiel veranschaulicht die Bereitstellung einer Infrastruktur mit Vorlagen aus einem Azure Storage-Blob.

1. Erstellen Sie eine **Neue Releasedefinition**, und wählen Sie eine leere Definition aus. Benennen Sie die neu erstellte Umgebung Ihrer Pipeline gemäß um.

    ![Screenshot der ursprünglichen Releasepipeline.](media/batch-ci-cd/Release-0.jpg)

1. Erstellen Sie eine Abhängigkeit von der Buildpipeline, um die Ausgabe für die HPC-Anwendung zu erhalten.

    > [!NOTE]
    > Beachten Sie den **Quellalias**, da dieser benötigt wird, wenn Aufgaben innerhalb der Releasedefinition erstellt werden.

    ![Screenshot eines Artefaktlinks zum HPCApplicationPackage in der entsprechenden Buildpipeline.](media/batch-ci-cd/Release-1.jpg)

1. Erstellen Sie einen Link zu einem anderen Artefakt, dieses Mal einem Azure-Repository. Dies ist erforderlich, um auf die in Ihrem Repository gespeicherten Resource Manager-Vorlagen zuzugreifen. Da Resource Manager-Vorlagen nicht kompiliert werden müssen, müssen Sie sie nicht über eine Buildpipeline pushen.

    > [!NOTE]
    > Beachten Sie auch hier den **Quellalias**, da dieser später benötigt wird.

    ![Screenshot eines Artefaktlinks zu den Azure Repos.](media/batch-ci-cd/Release-2.jpg)

1. Navigieren Sie zum Abschnitt **Variablen**. Sie sollten eine Reihe von Variablen in Ihrer Pipeline erstellen, damit Sie dieselben Informationen nicht in mehreren Tasks erneut eingeben müssen. In diesem Beispiel werden die folgenden Variablen verwendet:

   - **applicationStorageAccountName**: Name des Speicherkontos, das die HPC-Anwendungsbinärdateien enthält
   - **batchAccountApplicationName**: Name der Anwendung im Azure Batch-Konto
   - **batchAccountName**: Name des Azure Batch-Kontos
   - **batchAccountPoolName**: Der Name des Pools von virtuellen Computern, die die Verarbeitung ausführen
   - **batchApplicationId**: Eindeutige ID für die Azure Batch-Anwendung
   - **batchApplicationVersion**: Semantische Version Ihrer Azure Batch-Anwendung(d. h. ffmpeg-Binärdateien)
   - **location**: Speicherort für die bereitzustellenden Azure-Ressourcen
   - **resourceGroupName**: Name der zu erstellenden Ressourcengruppe, und wo Ihre Ressourcen bereitgestellt werden
   - **storageAccountName**: Name des Speicherkontos, das die verknüpften Resource Manager-Vorlagen enthält

   ![Screenshot der Variablen, die für das Azure Pipelines-Release festgelegt werden.](media/batch-ci-cd/Release-4.jpg)

1. Navigieren Sie zu den Aufgaben für die Entwicklungsumgebung. In der unten stehenden Momentaufnahme werden sechs Aufgaben angezeigt. Diese Aufgaben bewirken Folgendes: Herunterladen der gezippten ffmpeg-Dateien, Bereitstellen eines Speicherkontos zum Hosten der geschachtelten Resource Manager-Vorlagen, Kopieren dieser Resource Manager-Vorlagen auf das Speicherkonto, Bereitstellen des Batch-Kontos und der erforderlichen Abhängigkeiten, Erstellen einer Anwendung im Azure Batch-Konto und Hochladen des Anwendungspakets auf das Azure Batch-Konto.

    ![Screenshot der Aufgaben, die zum Herausgeben der HPC-Anwendung für Azure Batch verwendet werden.](media/batch-ci-cd/Release-3.jpg)

1. Fügen Sie die Aufgabe **Pipelineartefakt herunterladen (Vorschau)** hinzu, und legen Sie die folgenden Eigenschaften fest:
    - **Anzeigename**: Herunterladen von ApplicationPackage auf den Agent
    - **Der Name des herunterzuladenden Artefakts:** hpc-application
    - **Pfad, der als Downloadziel verwendet werden soll**: $(System.DefaultWorkingDirectory)

1. Erstellen Sie ein Speicherkonto für Ihre Azure Resource Manager-Vorlagen. Ein bestehendes Speicherkonto aus der Lösung könnte verwendet werden, aber um diese eigenständige Stichprobe und die Isolierung der Inhalte zu unterstützen, wird ein dediziertes Speicherkonto erstellt.

    Fügen Sie die Aufgabe **Bereitstellung einer Azure-Ressourcengruppe** hinzu, und legen Sie die folgenden Eigenschaften fest:
    - **Anzeigename**: Bereitstellen des Speicherkontos für Resource Manager-Vorlagen
    - **Azure-Abonnement:** Auswählen des entsprechenden Azure-Abonnements
    - **Aktion:** Erstellen oder aktualisieren Sie eine Ressourcengruppe.
    - **Ressourcengruppe**: $(resourceGroupName)
    - **Speicherort**: $(location)
    - **Vorlage**: $(System.ArtifactsDirectory)/ **{YourAzureRepoArtifactSourceAlias}** /arm-templates/storageAccount.json
    - **Vorlagenparameter überschreiben**: -accountName $(storageAccountName)

1. Laden Sie die Artefakte aus der Quellcodeverwaltung mithilfe von Azure Pipelines in das Speicherkonto hoch. Als Teil dieser Azure Pipelines-Task können der Speicherkontocontainer-URI und das SAS-Token in eine Variable in Azure Pipelines ausgegeben werden, sodass sie in dieser Agentphase jederzeit wiederverwendet werden können.

    Fügen Sie die **Azure-Dateikopiervorgang**-Aufgabe hinzu, und legen Sie die folgenden Eigenschaften fest:
    - **Quelle:** $(System.ArtifactsDirectory)/ **{YourAzureRepoArtifactSourceAlias}** /arm-templates/
    - **Azure-Verbindungstyp**: Azure Resource Manager
    - **Azure-Abonnement:** Auswählen des entsprechenden Azure-Abonnements
    - **Zieltyp**: Azure Blob
    - **RM-Speicherkonto**: $(storageAccountName)
    - **Containername**: templates
    - **Speichercontainer-URI**: templateContainerUri
    - **SAS-Token des Speichercontainers**: templateContainerSasToken

1. Stellen Sie die Orchestratorvorlage bereit. Diese Vorlage enthält Parameter für den Speicherkontocontainer-URI und das SAS-Token. Die in der Resource Manager-Vorlage erforderlichen Variablen werden entweder im Variablenabschnitt der Releasedefinition gespeichert oder wurden von einer anderen Azure Pipelines-Aufgabe (z. B. als Teil der Azure-Blobkopiervorgang-Aufgabe) festgelegt.

    Fügen Sie die Aufgabe **Bereitstellung einer Azure-Ressourcengruppe** hinzu, und legen Sie die folgenden Eigenschaften fest:
    - **Anzeigename**: Bereitstellen von Azure Batch
    - **Azure-Abonnement:** Auswählen des entsprechenden Azure-Abonnements
    - **Aktion:** Erstellen oder aktualisieren Sie eine Ressourcengruppe.
    - **Ressourcengruppe**: $(resourceGroupName)
    - **Speicherort**: $(location)
    - **Vorlage**: $(System.ArtifactsDirectory)/ **{YourAzureRepoArtifactSourceAlias}** /arm-templates/deployment.json
    - **Vorlagenparameter überschreiben**: `-templateContainerUri $(templateContainerUri) -templateContainerSasToken $(templateContainerSasToken) -batchAccountName $(batchAccountName) -batchAccountPoolName $(batchAccountPoolName) -applicationStorageAccountName $(applicationStorageAccountName)`

   Eine gängige Praxis ist die Verwendung von Azure Key Vault-Aufgaben. Wenn für den mit Ihrem Azure-Abonnement verknüpften Dienstprinzipal entsprechende Zugriffsrichtlinien festgelegt sind, kann er Geheimnisse aus einer Azure Key Vault-Instanz herunterladen und als Variablen in Ihrer Pipeline verwendet werden. Der Name des Geheimnisses wird mit den zugeordneten Wert festgelegt. Beispielsweise könnte auf ein Geheimnis sshPassword in der Releasedefinition mit „$(sshPassword)“ verwiesen werden.

1. Die nächsten Schritten rufen die Azure-Befehlszeilenschnittstelle auf. Im ersten wird eine Anwendung in Azure Batch erstellt und werden die entsprechenden Pakete hochgeladen.

    Fügen Sie die **Azure CLI**-Aufgabe hinzu, und legen Sie die folgenden Eigenschaften fest:
    - **Anzeigename**: Erstellen einer Anwendung im Azure Batch-Konto
    - **Azure-Abonnement:** Auswählen des entsprechenden Azure-Abonnements
    - **Skriptspeicherort**: Inlineskript
    - **Inlineskript**: `az batch application create --application-id $(batchApplicationId) --name $(batchAccountName) --resource-group $(resourceGroupName)`

1. Im zweiten Schritt werden zugehörige Pakete in die Anwendung hochgeladen (in diesem Fall die ffmpeg-Dateien).

    Fügen Sie die **Azure CLI**-Aufgabe hinzu, und legen Sie die folgenden Eigenschaften fest:
    - **Anzeigename**: Hochladen des Pakets in das Azure Batch-Konto
    - **Azure-Abonnement:** Auswählen des entsprechenden Azure-Abonnements
    - **Skriptspeicherort**: Inlineskript
    - **Inlineskript**: `az batch application package create --application-id $(batchApplicationId)  --name $(batchAccountName)  --resource-group $(resourceGroupName) --version $(batchApplicationVersion) --package-file=$(System.DefaultWorkingDirectory)/$(Release.Artifacts.{YourBuildArtifactSourceAlias}.BuildId).zip`

    > [!NOTE]
    > Die Versionsnummer des Anwendungspakets wird auf eine Variable festgelegt. Dies ermöglicht, frühere Versionen des Pakets außer Kraft zu setzen, und lässt Sie die Versionsnummer des Pakets, das an Azure Batch übertragen wird, manuell steuern.

1. Erstellen Sie ein neues Release durch Auswahl von **Release > Neues Release erstellen**. Wählen Sie nach dem Auslösenden den Link zu Ihrem neuen Release aus, um den Status anzuzeigen.

1. Zeigen Sie die Liveausgabe vom Agent aus an, indem Sie die **Protokolle**-Schaltfläche unterhalb Ihrer Umgebung auswählen.

    ![Screenshot des Releasestatus.](media/batch-ci-cd/Release-5.jpg)

## <a name="test-the-environment"></a>Testen der Umgebung

Nachdem die Umgebung eingerichtet ist, vergewissern Sie sich, dass die folgenden Tests erfolgreich abgeschlossen werden können.

Stellen Sie mithilfe der Azure CLI über eine PowerShell-Eingabeaufforderung eine Verbindung mit dem neuen Azure Batch-Konto her.

- Melden Sie sich mit `az login` bei Ihrem Azure-Konto an, und führen Sie die Anweisungen aus, um sich zu authentifizieren.
- Authentifizieren Sie jetzt das Batch-Konto: `az batch account login -g <resourceGroup> -n <batchAccount>`

#### <a name="list-the-available-applications"></a>Auflisten der verfügbaren Anwendungen

```azurecli
az batch application list -g <resourcegroup> -n <batchaccountname>
```

#### <a name="check-the-pool-is-valid"></a>Überprüfen der Gültigkeit des Pools

```azurecli
az batch pool list
```

Beachten Sie den Wert von `currentDedicatedNodes` aus der Ausgabe dieses Befehls. Dieser Wert wird im nächsten Test angepasst.

#### <a name="resize-the-pool"></a>Ändern der Größe des Pools

Ändern Sie die Größe des Pools, sodass Serverknoten für den Auftrag und das Testen der Aufgabe verfügbar sind, und überprüfen Sie mit dem Befehl zur Poolauflistung den aktuellen Status, bis die Größenänderung abgeschlossen ist und Knoten verfügbar sind.

```azurecli
az batch pool resize --pool-id <poolname> --target-dedicated-nodes 4
```

## <a name="next-steps"></a>Nächste Schritte

In diesen Tutorials finden Sie weitere Informationen zur Interaktion mit einem Batch-Konto über eine einfache Anwendung.

- [Ausführen einer parallelen Workload mit Azure Batch über die Python-API](tutorial-parallel-python.md)
- [Ausführen einer parallelen Workload mit Azure Batch über die .NET-API](tutorial-parallel-dotnet.md)
