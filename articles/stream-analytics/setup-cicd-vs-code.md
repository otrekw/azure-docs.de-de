---
title: Bereitstellen eines Azure Stream Analytics-Auftrags mit einem CI/CD npm-Paket
description: In diesem Artikel wird beschrieben, wie Sie Continuous Integration und Continuous Deployment mithilfe des Azure Stream Analytics CI/CD npm-Pakets einrichten.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: deb6c2439cc84f196b7f42fd9f49d3ebfd057cbb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76962182"
---
# <a name="deploy-an-azure-stream-analytics-job-using-cicd-npm-package"></a>Bereitstellen eines Azure Stream Analytics-Auftrags mit einem CI/CD npm-Paket 

Sie können einen Continuous Integration- und Continuous Deployment-Prozess mithilfe des Azure Stream Analytics CI/CD npm-Pakets für Ihre Stream Analytics-Aufträge einrichten. In diesem Artikel wird beschrieben, wie Sie das npm-Paket generell mit einem beliebigen CI/CD-System verwenden, sowie bestimmte Anweisungen für die Bereitstellung mit Azure Pipelines.

Weitere Informationen zum Bereitstellen mit PowerShell finden Sie unter [Bereitstellen mit einer Resource Manager-Vorlagendatei und Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Weitere Informationen finden Sie außerdem unter [Use an object as a parameter in an Azure Resource Manager template](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters) (Verwenden eines Objekts als Parameter in einer Azure Resource Manager-Vorlage).

## <a name="build-the-vs-code-project"></a>Erstellen des VS CodeProjekts

Sie können Continuous Integration und Continuous Deployment für Azure Stream Analytics-Aufträge mithilfe des npm-Pakets **asa-streamanalytics-cicd** aktivieren. Das npm-Paket stellt die Tools für die Generierung von Azure Resource Manager-Vorlagen von [Stream Analytics-Projekten in Visual Studio Code](quick-create-vs-code.md) bereit. Es kann ohne die Installation von Visual Studio Code unter Windows, macOS und Linux verwendet werden.

Sie können das [Paket direkt herunterladen](https://www.npmjs.com/package/azure-streamanalytics-cicd) oder es über den `npm install -g azure-streamanalytics-cicd`-Befehl [global](https://docs.npmjs.com/downloading-and-installing-packages-globally) installieren. Dies ist die empfohlene Vorgehensweise, die auch in einem PowerShell- oder Azure CLI Skripttask einer Buildpipeline in **Azure Pipelines** verwendet werden kann.

Nachdem Sie das Paket installiert haben, verwenden Sie den folgenden Befehl, um die Azure Resource Manager-Vorlagen auszugeben. Das **scriptPath**-Argument ist der absolute Pfad zu der **asaql**-Datei in Ihrem Projekt. Stellen Sie sicher, dass sich die Dateien „asaproj.json“ und „JobConfig.json“ im selben Ordner wie die Skriptdatei befinden. Wenn **outputPath** nicht angegeben ist, werden die Vorlagen in den Ordner **Bereitstellen** im **bin**-Ordner des Projekts platziert.

```powershell
azure-streamanalytics-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```
Beispiel (unter macOS)
```powershell
azure-streamanalytics-cicd build -scriptPath "/Users/roger/projects/samplejob/script.asaql" 
```

Wenn ein Stream Analytics Visual Studio Code-Projekt erfolgreich erstellt wurde, generiert es die folgenden beiden Azure Resource Manager-Vorlagendateien im Ordner **bin/[Debug/Retail]/Deploy**: 

*  Resource Manager-Vorlagendatei

       [ProjectName].JobTemplate.json 

*  Resource Manager-Parameterdatei

       [ProjectName].JobTemplate.parameters.json   

Die Standardparameter in der „parameters.json“-Datei werden aus den Einstellungen im Visual Studio Code-Projekt entnommen. Wenn Sie die Bereitstellung in einer anderen Umgebung ausführen möchten, ersetzen Sie die Parameter entsprechend.

> [!NOTE]
> Für alle Anmeldeinformationen sind die Standardwerte auf NULL festgelegt. Sie **müssen** die Werte festlegen, bevor Sie eine Bereitstellung in der Cloud ausführen.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```

## <a name="deploy-with-azure-pipelines"></a>Bereitstellen mit Azure Pipelines

In diesem Abschnitt erfahren Sie, wie Sie [Build](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav)- und [Release](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)pipelines mithilfe von npm in Azure Pipelines erstellen.

Öffnen Sie einen Webbrowser, und navigieren Sie zu Ihrem Azure Stream Analytics Visual Studio Code-Projekt.

1. Wählen Sie unter **Pipelines** im linken Navigationsmenü **Builds** aus. Wählen Sie dann **Neue Pipeline** aus.

   ![Erstellen einer neue Azure-Pipeline](./media/setup-cicd-vs-code/new-pipeline.png)

2. Wählen Sie **Klassischen Editor verwenden** aus, um eine Pipeline ohne YAML zu erstellen.

3. Wählen Sie Ihren Quelltyp, das Teamprojekt und das Repository aus. Klicken Sie anschließend auf **Weiter**.

   ![Auswählen eines Azure Stream Analytics-Projekts](./media/setup-cicd-vs-code/select-repo.png)

4. Wählen Sie auf der Seite **Eine Vorlage auswählen** den Eintrag **Leerer Auftrag** aus.

### <a name="add-npm-task"></a>npm-Aufgabe hinzufügen

1. Wählen Sie auf der Registerkarte **Aufgaben** das Pluszeichen neben **Agent-Auftrag 1** aus. Geben Sie in die Aufgabensuche „npm“ ein, und wählen Sie **npm** aus.

   ![npm-Aufgabe auswählen](./media/setup-cicd-vs-code/search-npm.png)

2. Weisen Sie der Aufgabe einen **Anzeigenamen** zu. Ändern Sie die Option **Befehl** in *benutzerdefiniert*, und geben Sie den folgenden Befehl in **Befehl und Argumente** ein. Belassen Sie die übrigen Optionen in ihrer Standardeinstellung.

   ```cmd
   install -g azure-streamanalytics-cicd
   ```

   ![Konfiguration für npm-Aufgabe eingeben](./media/setup-cicd-vs-code/npm-config.png)

### <a name="add-command-line-task"></a>Befehlszeilenaufgabe hinzufügen

1. Wählen Sie auf der Registerkarte **Aufgaben** das Pluszeichen neben **Agent-Auftrag 1** aus. Suchen Sie nach **Befehlszeile**.

2. Geben Sie der Aufgabe einen **Anzeigenamen**, und geben Sie das folgende Skript ein. Ändern Sie das Skript mit dem Namen Ihres Repositorys und Ihrem Projektnamen.

   ```cmd
   azure-streamanalytics-cicd build -scriptPath $(Build.SourcesDirectory)/myASAProject/myASAProj.asaql
   ```

   ![Konfigurationen für Befehlszeilenaufgaben eingeben](./media/setup-cicd-vs-code/commandline-config.png)

### <a name="add-copy-files-task"></a>Aufgabe zum Kopieren von Dateien hinzufügen

1. Wählen Sie auf der Registerkarte **Aufgaben** das Pluszeichen neben **Agent-Auftrag 1** aus. Suchen Sie nach **Dateien kopieren**. Geben Sie dann die folgenden Konfigurationen ein.

   |Parameter|Eingabe|
   |-|-|
   |`Display name`|Dateien in „:$(build.artifactstagingdirectory)“ kopieren|
   |Quellordner|`$(system.defaultworkingdirectory)`| 
   |Contents| `**\Deploy\**` |
   |Zielordner| `$(build.artifactstagingdirectory)`|

   ![Konfiguration für Kopieraufgabe eingeben](./media/setup-cicd-vs-code/copy-config.png)

### <a name="add-publish-build-artifacts-task"></a>Aufgabe zum Veröffentlichen von Buildartefakten hinzufügen

1. Wählen Sie auf der Registerkarte **Aufgaben** das Pluszeichen neben **Agent-Auftrag 1** aus. Suchen Sie nach **Buildartefakte veröffentlichen**, und wählen Sie die Option mit dem schwarzen Pfeilsymbol aus. 

2. Ändern Sie keine der Standardkonfigurationen.

### <a name="save-and-run"></a>Speichern und ausführen

Wenn Sie mit dem Hinzufügen der npm-, Befehlszeilen-, „Dateien kopieren“- und „Buildartefakte veröffentlichen“-Aufgaben fertig sind, wählen Sie **Speichern und in Warteschlangen einreihen** aus. Wenn Sie dazu aufgefordert werden, geben Sie einen Kommentar zum Speichern ein, und wählen Sie **Speichern und ausführen** aus.

## <a name="release-with-azure-pipelines"></a>Release mit Azure Pipelines

Öffnen Sie einen Webbrowser, und navigieren Sie zu Ihrem Azure Stream Analytics Visual Studio Code-Projekt.

1. Wählen Sie unter **Pipelines** im linken Navigationsmenü **Releases** aus. Wählen Sie dann **Neue Pipeline** aus.

2. Wählen Sie **Mit leerem Auftrag beginnen** aus.

3. Wählen Sie im Feld **Artefakte** den Befehl **+ Artefakt hinzufügen** aus. Wählen Sie unter **Quelle** die Buildpipeline aus, die Sie gerade erstellt haben, und wählen Sie **Hinzufügen** aus.

   ![Buildpipelineartefakt eingeben](./media/setup-cicd-vs-code/build-artifact.png)

4. Ändern Sie den Namen von **Stufe 1** in **Auftrag in Testumgebung bereitstellen**.

5. Fügen Sie eine neue Stufe hinzu, und nennen Sie sie **Auftrag in Produktionsumgebung bereitstellen**.

### <a name="add-tasks"></a>Hinzufügen von Aufgaben

1. Wählen Sie im Aufgabendropdown **Auftrag in Testumgebung bereitstellen** aus. 

2. Wählen Sie das **+** neben **Agent-Auftrag** aus, und suchen Sie nach *Bereitstellung einer Azure-Ressourcengruppe*. Legen Sie die folgenden Parameter fest:

   |Einstellung|Wert|
   |-|-|
   |`Display name`| *myASAJob bereitstellen*|
   |Azure-Abonnement| Wählen Sie Ihr Abonnement aus.|
   |Aktion| *Erstellen oder Aktualisieren einer Ressourcengruppe*|
   |Resource group| Auswählen eines Namens für die Testressourcengruppe, die ihren Stream Analytics-Auftrag enthalten soll.|
   |Position|Auswählen des Speicherorts Ihrer Testressourcengruppe.|
   |Speicherort der Vorlage| *Verknüpftes Artefakt*|
   |Vorlage| $(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.json |
   |Vorlagenparameter|($(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.parameters.json|
   |Vorlagenparameter überschreiben|-Input_IoTHub1_iotHubNamespace $(test_eventhubname)|
   |Bereitstellungsmodus|Inkrementell|

3. Wählen Sie im Aufgabendropdown **Auftrag in Produktionsumgebung bereitstellen** aus.

4. Wählen Sie das **+** neben **Agent-Auftrag** aus, und suchen Sie nach *Bereitstellung einer Azure-Ressourcengruppe*. Legen Sie die folgenden Parameter fest:

   |Einstellung|Wert|
   |-|-|
   |`Display name`| *myASAJob bereitstellen*|
   |Azure-Abonnement| Wählen Sie Ihr Abonnement aus.|
   |Aktion| *Erstellen oder Aktualisieren einer Ressourcengruppe*|
   |Resource group| Auswählen eines Namens für die Produktionsressourcengruppe, die ihren Stream Analytics-Auftrag enthalten soll.|
   |Position|Wählen Sie den Speicherort Ihrer Produktionsressourcengruppe aus.|
   |Speicherort der Vorlage| *Verknüpftes Artefakt*|
   |Vorlage| $(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.json |
   |Vorlagenparameter|($(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.parameters.json|
   |Vorlagenparameter überschreiben|-Input_IoTHub1_iotHubNamespace $(eventhubname)|
   |Bereitstellungsmodus|Inkrementell|

### <a name="create-release"></a>Release erstellen

Um ein Release zu erstellen, wählen Sie **Release erstellen** in der oberen rechten Ecke aus.

![Erstellen eines Release mittels Azure Pipelines](./media/setup-cicd-vs-code/create-release.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Wenn Sie die verwaltete Identität für Azure Data Lake Store Gen1 als Ausgabesenke verwenden möchten, müssen Sie vor der Bereitstellung in Azure per PowerShell den Zugriff auf den Dienstprinzipal ermöglichen. Weitere Informationen hierzu finden Sie im Artikel zum Thema [Bereitstellen von ADLS Gen1 mit verwalteter Identität per Resource Manager-Vorlage](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment).


## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Erstellen eines Azure Stream Analytics-Cloudauftrags in Visual Studio Code (Vorschauversion)](quick-create-vs-code.md)
* [Lokales Testen von Stream Analytics-Abfragen mit Visual Studio Code (Vorschauversion)](visual-studio-code-local-run.md)
* [Erkunden von Azure Stream Analytics mit Visual Studio Code (Vorschauversion)](visual-studio-code-explore-jobs.md)
