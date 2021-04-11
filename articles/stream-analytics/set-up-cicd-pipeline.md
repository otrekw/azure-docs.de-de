---
title: Erstellen einer CI/CD-Pipeline für einen Stream Analytics-Auftrag mit Azure DevOps
description: In diesem Artikel wird beschrieben, wie Sie eine CI/CD-Pipeline (Continuous Integration und Continuous Deployment) für einen Azure Stream Analytics-Auftrag in Azure DevOps einrichten.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.service: stream-analytics
ms.topic: how-to
ms.date: 09/10/2020
ms.openlocfilehash: 82a2c3047f851c9fbc273cd13e730572c38b6bcd
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105640384"
---
# <a name="use-azure-devops-to-create-a-cicd-pipeline-for-a-stream-analytics-job"></a>Erstellen einer CI/CD-Pipeline für einen Stream Analytics-Auftrag mit Azure DevOps

In diesem Artikel erfahren Sie, wie Sie [Build](/azure/devops/pipelines/get-started/pipelines-get-started)- und [Release](/azure/devops/pipelines/release/define-multistage-release-process)-Pipelines in Azure DevOps mit den CI/CD-Tools in Azure Stream Analytics erstellen.

## <a name="commit-your-stream-analytics-project"></a>Ausführen eines Commits für ein Stream Analytics-Projekt

Bevor Sie beginnen, führen Sie einen Commit für alle Stream Analytics-Projekte als Quelldateien in ein [Azure DevOps](/azure/devops/user-guide/source-control)-Repository aus. Sie können dabei auf dieses [Beispielrepository](https://dev.azure.com/ASA-CICD-sample/azure-streamanalytics-cicd-demo) und den [Stream Analytics-Projektquellcode ](https://dev.azure.com/ASA-CICD-sample/_git/azure-streamanalytics-cicd-demo) in Azure Pipelines zurückgreifen.

In den Schritten in diesem Artikel wird ein Stream Analytics Visual Studio Code-Projekt verwendet. Wenn Sie ein Visual Studio-Projekt verwenden, führen Sie die Schritte in [Automate builds, tests, and deployments of an Azure Stream Analytics job using CI/CD tools](cicd-tools.md) (Automatisieren von Builds, Tests und Bereitstellungen eines Azure Stream Analytics-Auftrags mithilfe von CI/CD-Tools) aus.

## <a name="create-a-build-pipeline"></a>Erstellen einer Buildpipeline

In diesem Abschnitt erfahren Sie, wie Sie eine Buildpipeline erstellen. 

1. Öffnen Sie einen Webbrowser, und navigieren Sie zum gewünschten Projekt in Azure DevOps.  

2. Wählen Sie unter **Pipelines** im linken Navigationsmenü **Builds** aus. Wählen Sie dann **Neue Pipeline** aus.

   :::image type="content" source="media/set-up-cicd-pipeline/new-pipeline.png" alt-text="Erstellen einer neue Azure-Pipeline":::

3. Wählen Sie **Klassischen Editor verwenden** aus, um eine Pipeline ohne YAML zu erstellen.

4. Wählen Sie Ihren Quelltyp, das Teamprojekt und das Repository aus. Wählen Sie anschließend **Weiter** aus.

   :::image type="content" source="media/set-up-cicd-pipeline/select-repo.png" alt-text="Auswählen eines Azure Stream Analytics-Projekts":::

5. Wählen Sie auf der Seite **Eine Vorlage auswählen** den Eintrag **Leerer Auftrag** aus.

## <a name="install-npm-package"></a>Installieren des npm-Pakets

1. Wählen Sie auf der Registerkarte **Aufgaben** das Pluszeichen neben **Agent-Auftrag 1** aus. Geben Sie in die Aufgabensuche **npm** ein, und wählen Sie *npm* aus.

   :::image type="content" source="media/set-up-cicd-pipeline/search-npm.png" alt-text="npm-Aufgabe auswählen":::

2. Weisen Sie der Aufgabe einen **Anzeigenamen** zu. Ändern Sie die Option **Befehl** in *benutzerdefiniert*, und geben Sie den folgenden Befehl in **Befehl und Argumente** ein. Belassen Sie die übrigen Optionen in ihrer Standardeinstellung.

   ```bash
   install -g azure-streamanalytics-cicd
   ```

   :::image type="content" source="media/set-up-cicd-pipeline/npm-config.png" alt-text="Konfiguration für npm-Aufgabe eingeben":::

Führen Sie die folgenden Schritte aus, wenn Sie den gehosteten Linux-Agent verwenden müssen:
1.  Wählen Sie Ihre **Agentspezifikation** aus.
   
    :::image type="content" source="media/set-up-cicd-pipeline/select-linux-agent.png" alt-text="Screenshot der ausgewählten Agentspezifikation":::

2.  Wählen Sie auf der Registerkarte **Aufgaben** das Pluszeichen neben **Agent-Auftrag 1** aus. Geben Sie in der Aufgabensuche *Befehlszeile* ein, und wählen Sie die **Befehlszeile** aus.
   
    :::image type="content" source="media/set-up-cicd-pipeline/cmd-search.png" alt-text="Screenshot der Suche nach der Aufgabe „Befehlszeile“":::

3.  Weisen Sie der Aufgabe einen **Anzeigenamen** zu. Geben Sie den folgenden Befehl in **Skript** ein. Belassen Sie die übrigen Optionen in ihrer Standardeinstellung.

      ```bash
      sudo npm install -g azure-streamanalytics-cicd --unsafe-perm=true --allow-root
      ```
      :::image type="content" source="media/set-up-cicd-pipeline/cmd-scripts.png" alt-text="Screenshot der Eingabe eines Skripts für die Befehlszeilenaufgabe":::

## <a name="add-a-build-task"></a>Hinzufügen einer Buildaufgabe

1. Wählen Sie auf der Seite **Variablen** unter **Pipelinevariablen** die Option **+Hinzufügen** aus. Fügen Sie die folgenden Variablen hinzu: Legen Sie die folgenden Werte wie gewünscht fest:

   |Variablenname|Wert|
   |-|-|
   |projectRootPath|[IhrProjektname]|
   |outputPath|Ausgabe|
   |deployPath|Bereitstellen|

2. Wählen Sie auf der Registerkarte **Aufgaben** das Pluszeichen neben **Agent-Auftrag 1** aus. Suchen Sie nach **Befehlszeile**.

3. Geben Sie der Aufgabe einen **Anzeigenamen**, und geben Sie das folgende Skript ein. Ändern Sie das Skript mit dem Namen Ihres Repositorys und Ihrem Projektnamen.

   ```bash
   azure-streamanalytics-cicd build -project $(projectRootPath)/asaproj.json -outputpath $(projectRootPath)/$(outputPath)/$(deployPath)
   ```

   In der folgenden Abbildung wird ein Stream Analytics Visual Studio Code-Projekt als Beispiel verwendet.

   :::image type="content" source="media/set-up-cicd-pipeline/command-line-config-build.png" alt-text="Konfigurationen in Visual Studio-Code für Befehlszeilenaufgaben eingeben":::

## <a name="add-a-test-task"></a>Hinzufügen einer Testaufgabe

1. Wählen Sie auf der Seite **Variablen** unter **Pipelinevariablen** die Option **+Hinzufügen** aus. Fügen Sie die folgenden Variablen hinzu: Ändern Sie die Werte, und geben Sie Ihren Ausgabepfad und Repositorynamen ein.

   |Variablenname|Wert|
   |-|-|
   |testPath|Test|

   :::image type="content" source="media/set-up-cicd-pipeline/pipeline-variables-test.png" alt-text="Pipelinevariablen hinzufügen":::

2. Wählen Sie auf der Registerkarte **Aufgaben** das Pluszeichen neben **Agent-Auftrag 1** aus. Suchen Sie nach **Befehlszeile**.

3. Geben Sie der Aufgabe einen **Anzeigenamen**, und geben Sie das folgende Skript ein. Ändern Sie das Skript, und geben Sie Ihren Projektdateinamen und den Pfad zur Testkonfigurationsdatei ein. 

   Ausführliche Informationen zum Hinzufügen und Konfigurieren von Testfällen finden Sie in der [Anleitung zu automatisierten Tests](cicd-tools.md#automated-test).

   ```bash
   azure-streamanalytics-cicd test -project $(projectRootPath)/asaproj.json -outputpath $(projectRootPath)/$(outputPath)/$(testPath) -testConfigPath $(projectRootPath)/test/testConfig.json 
   ```

   :::image type="content" source="media/set-up-cicd-pipeline/command-line-config-test.png" alt-text="Konfigurationen für Befehlszeilenaufgaben eingeben":::

## <a name="add-a-copy-files-task"></a>Hinzufügen einer Aufgabe zum Kopieren von Dateien

Sie müssen eine Aufgabe zum Kopieren von Dateien hinzufügen, um die Testzusammenfassungsdatei und die Azure Resource Manager-Vorlagendateien in den Artefaktordner zu kopieren. 

1. Wählen Sie auf der Registerkarte **Aufgaben** neben **Agent-Auftrag 1** **+** aus. Suchen Sie nach **Dateien kopieren**. Geben Sie dann die folgenden Konfigurationen ein. Indem Sie `**` **Inhalt** zuweisen, werden alle Dateien der Testergebnisse kopiert.

   |Parameter|Eingabe|
   |-|-|
   |Angezeigter Name|Dateien in „:$(build.artifactstagingdirectory)“ kopieren|
   |Quellordner|`$(system.defaultworkingdirectory)/$(outputPath)/`|
   |Inhalte| `**` |
   |Zielordner| `$(build.artifactstagingdirectory)`|

2. Erweitern Sie **Steuerungsoptionen**. Wählen Sie **Auch wenn eine vorherige Aufgabe fehlgeschlagen ist, außer wenn der Build abgebrochen wurde** unter **Aufgabe ausführen** aus.

   :::image type="content" source="media/set-up-cicd-pipeline/copy-config.png" alt-text="Konfiguration für Kopieraufgabe eingeben":::

## <a name="add-a-publish-build-artifacts-task"></a>Hinzufügen einer Aufgabe zum Veröffentlichen von Buildartefakten

1. Wählen Sie auf der Registerkarte **Aufgaben** das Pluszeichen neben **Agent-Auftrag 1** aus. Suchen Sie nach **Buildartefakte veröffentlichen**, und wählen Sie die Option mit dem schwarzen Pfeilsymbol aus.

2. Erweitern Sie **Steuerungsoptionen**. Wählen Sie **Auch wenn eine vorherige Aufgabe fehlgeschlagen ist, außer wenn der Build abgebrochen wurde** unter **Aufgabe ausführen** aus.

   :::image type="content" source="media/set-up-cicd-pipeline/publish-config.png" alt-text="Konfiguration für Veröffentlichungsaufgabe eingeben":::

## <a name="save-and-run"></a>Speichern und ausführen

Wenn Sie mit dem Hinzufügen der Aufgaben für npm-Paket, Befehlszeile, „Dateien kopieren“ und „Buildartefakte veröffentlichen“ fertig sind, wählen Sie **Speichern und in Warteschlangen einreihen** aus. Wenn Sie dazu aufgefordert werden, geben Sie einen Kommentar zum Speichern ein, und wählen Sie **Speichern und ausführen** aus. Sie können die Testergebnisse auf der Seite **Zusammenfassung** der Pipeline herunterladen.

## <a name="check-the-build-and-test-results"></a>Überprüfen des Builds und der Testergebnisse

Die Testzusammenfassungsdatei und Azure Resource Manager-Vorlagendateien finden Sie im Ordner **Published**.

   :::image type="content" source="media/set-up-cicd-pipeline/check-build-test-result.png" alt-text="Build und Testergebnisse überprüfen":::

   :::image type="content" source="media/set-up-cicd-pipeline/check-drop-folder.png" alt-text="Artefakte überprüfen":::

## <a name="release-with-azure-pipelines"></a>Release mit Azure Pipelines

In diesem Abschnitt erfahren Sie, wie Sie eine Releasepipeline erstellen. 

Öffnen Sie einen Webbrowser, und navigieren Sie zu Ihrem Azure Stream Analytics Visual Studio Code-Projekt.

1. Wählen Sie unter **Pipelines** im linken Navigationsmenü **Releases** aus. Wählen Sie dann **Neue Pipeline** aus.

2. Wählen Sie **Mit leerem Auftrag beginnen** aus.

3. Wählen Sie im Feld **Artefakte** den Befehl **+ Artefakt hinzufügen** aus. Wählen Sie unter **Quelle** die Buildpipeline aus, die Sie gerade erstellt haben, und wählen Sie **Hinzufügen** aus.

   :::image type="content" source="media/set-up-cicd-pipeline/build-artifact.png" alt-text="Buildpipelineartefakt eingeben":::

4. Ändern Sie den Namen von **Stufe 1** in **Auftrag in Testumgebung bereitstellen**.

5. Fügen Sie eine neue Stufe hinzu, und nennen Sie sie **Auftrag in Produktionsumgebung bereitstellen**.

### <a name="add-deploy-tasks"></a>Hinzufügen von Aufgaben zum Bereitstellen

1. Wählen Sie im Aufgabendropdown **Auftrag in Testumgebung bereitstellen** aus.

2. Wählen Sie das **+** neben **Agent-Auftrag** aus, und suchen Sie nach **ARM-Vorlagenbereitstellung**. Legen Sie die folgenden Parameter fest:

   |Parameter|Wert|
   |-|-|
   |Angezeigter Name| *Deploy myASAProject*|
   |Azure-Abonnement| Wählen Sie Ihr Abonnement aus.|
   |Aktion| *Erstellen oder Aktualisieren einer Ressourcengruppe*|
   |Resource group| Auswählen eines Namens für die Testressourcengruppe, die ihren Stream Analytics-Auftrag enthalten soll.|
   |Ort|Auswählen des Speicherorts Ihrer Testressourcengruppe.|
   |Speicherort der Vorlage| Verknüpftes Artefakt|
   |Vorlage| $(System.DefaultWorkingDirectory)/_azure-streamanalytics-cicd-demo-CI-Deploy/drop/myASAProject.JobTemplate.json |
   |Vorlagenparameter|$(System.DefaultWorkingDirectory)/_azure-streamanalytics-cicd-demo-CI-Deploy/drop/myASAProject.JobTemplate.parameters.json |
   |Vorlagenparameter überschreiben|-<arm_template_parameter> "Ihr Wert". Sie können die Parameter mithilfe von **Variablen** definieren.|
   |Bereitstellungsmodus|Inkrementell|

3. Wählen Sie im Aufgabendropdown **Auftrag in Produktionsumgebung bereitstellen** aus.

4. Wählen Sie das **+** neben **Agent-Auftrag** aus, und suchen Sie nach *ARM-Vorlagenbereitstellung*. Legen Sie die folgenden Parameter fest:

   |Parameter|Wert|
   |-|-|
   |Angezeigter Name| *Deploy myASAProject*|
   |Azure-Abonnement| Wählen Sie Ihr Abonnement aus.|
   |Aktion| *Erstellen oder Aktualisieren einer Ressourcengruppe*|
   |Resource group| Auswählen eines Namens für die Produktionsressourcengruppe, die ihren Stream Analytics-Auftrag enthalten soll.|
   |Ort|Wählen Sie den Speicherort Ihrer Produktionsressourcengruppe aus.|
   |Speicherort der Vorlage| *Verknüpftes Artefakt*|
   |Vorlage| $(System.DefaultWorkingDirectory)/_azure-streamanalytics-cicd-demo-CI-Deploy/drop/myASAProject.JobTemplate.json |
   |Vorlagenparameter|$(System.DefaultWorkingDirectory)/_azure-streamanalytics-cicd-demo-CI-Deploy/drop/myASAProject.JobTemplate.parameters.json |
   |Vorlagenparameter überschreiben|-<arm_template_parameter> "Ihr Wert"|
   |Bereitstellungsmodus|Inkrementell|

### <a name="create-a-release"></a>Erstellen eines Release

Um ein Release zu erstellen, wählen Sie **Release erstellen** in der oberen rechten Ecke aus.

:::image type="content" source="media/set-up-cicd-pipeline/create-release.png" alt-text="Erstellen eines Release mittels Azure Pipelines":::

## <a name="next-steps"></a>Nächste Schritte

* [Continuous Integration und Continuous Deployment für Azure Stream Analytics](cicd-overview.md)
* [Automatisieren von Builds, Tests und Bereitstellungen eines Azure Stream Analytics-Auftrags mit CI/CD-Tools](cicd-tools.md)