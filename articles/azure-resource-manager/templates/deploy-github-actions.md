---
title: Bereitstellen von Resource Manager-Vorlagen mithilfe von GitHub Actions
description: In diesem Artikel wird beschrieben, wie Sie Azure Resource Manager-Vorlagen mithilfe von GitHub Actions bereitstellen.
ms.topic: conceptual
ms.date: 07/02/2020
ms.openlocfilehash: 313354499901bc69ec6e00f0ba7c385065cae615
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85854739"
---
# <a name="deploy-azure-resource-manager-templates-by-using-github-actions"></a>Bereitstellen von Azure Resource Manager-Vorlagen mithilfe von GitHub Actions

Mit [GitHub Actions](https://help.github.com/en/actions) können Sie direkt in dem GitHub-Repository, in dem Ihre Azure Resource Manager-Vorlagen (ARM) gespeichert sind, benutzerdefinierte Workflows für den Lebenszyklus der Softwareentwicklung erstellen. Ein [Workflow](https://help.github.com/actions/reference/workflow-syntax-for-github-actions) ist in einer YAML-Datei definiert. Workflows bestehen aus mindestens einem Auftrag, von denen jeder Schritte enthält, in denen jeweils eigene Tasks ausgeführt werden. In Schritten können Befehle ausgeführt oder eine Aktion verwendet werden. Sie können eigene Aktionen erstellen oder die verwenden, die in der [GitHub-Community](https://github.com/marketplace?type=actions) geteilt werden, und sie nach Bedarf anpassen. In diesem Artikel erfahren Sie, wie Sie Resource Manager-Vorlagen mithilfe von [Azure CLI Action](https://github.com/marketplace/actions/azure-cli-action) bereitstellen.

Azure CLI Action verfügt über zwei abhängige Aktionen:

- **[Auschecken:](https://github.com/marketplace/actions/checkout)** Checkt Ihr Repository aus, damit der Workflow auf alle angegebenen Resource Manager-Vorlagen zugreifen kann
- **[Azure-Anmeldung:](https://github.com/marketplace/actions/azure-login)** Für die Anmeldung mit Ihren Azure-Anmeldeinformationen

Ein grundlegender Workflow für die Bereitstellung einer Resource Manager-Vorlage kann drei Schritte umfassen:

1. Auschecken einer Vorlagendatei
2. Melden Sie sich bei Azure an.
3. Bereitstellen einer Resource Manager-Vorlage

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein GitHub-Repository, in dem Sie Ihre Resource Manager-Vorlagen und Ihre Workflowdateien speichern können. Informationen zum Erstellen eines neuen Repositorys finden Sie [in diesem Hilfeartikel](https://help.github.com/en/enterprise/2.14/user/articles/creating-a-new-repository).

## <a name="configure-deployment-credentials"></a>Konfigurieren von Anmeldeinformationen für die Bereitstellung

Bei der Aktion „Azure-Anmeldung“ wird ein Dienstprinzipal für die Azure-Authentifizierung verwendet. Der Prinzipal eines CI/CD-Workflows benötigt in der Regel die integrierte Berechtigung „Mitwirkender“, um Azure-Ressourcen bereitzustellen.

Das folgende Azure CLI-Skript zeigt, wie Sie einen Azure-Dienstprinzipal mit Berechtigungen der Rolle „Mitwirkender“ für eine Azure-Ressourcengruppe generieren. In dieser Ressourcengruppe werden vom Workflow die in der Resource Manager-Vorlage definierten Ressourcen bereitgestellt.

```azurecli
$projectName="[EnterAProjectName]"
$location="centralus"
$resourceGroupName="${projectName}rg"
$appName="http://${projectName}"
$scope=$(az group create --name $resourceGroupName --location $location --query 'id')
az ad sp create-for-rbac --name $appName --role Contributor --scopes $scope --sdk-auth
```

Passen Sie im Skript die Werte von **$projectName** und **$location** an. Der Ressourcengruppenname ist der Projektname mit dem Zusatz **rg**. Sie müssen den Namen der Ressourcengruppe in Ihrem Workflow angeben.

Das Skript gibt ein JSON-Objekt aus, das dem folgenden ähnelt:

```json
{
   "clientId": "<GUID>",
   "clientSecret": "<GUID>",
   "subscriptionId": "<GUID>",
   "tenantId": "<GUID>",
   (...)
}
```

Kopieren Sie die JSON-Ausgabe, und speichern Sie sie als GitHub-Geheimnis in Ihrem GitHub-Repository. Falls Sie noch kein Repository haben, lesen Sie sich noch einmal den Abschnitt [Voraussetzungen](#prerequisites) durch.

1. Klicken Sie in Ihrem GitHub-Repository auf die Registerkarte **Settings** (Einstellungen).
1. Klicken Sie im linken Bereich auf **Secrets** (Geheimnisse).
1. Geben Sie die folgenden Werte ein:

    - **Name**: AZURE_CREDENTIALS
    - **Value**: (Fügen Sie die JSON-Ausgabe ein.)
1. Klicken Sie auf **Add secret** (Geheimnis hinzufügen).

Sie müssen den Namen des Geheimnisses im Workflow angeben.

## <a name="add-resource-manager-template"></a>Hinzufügen von Resource Manager-Vorlagen

Fügen Sie dem GitHub-Repository eine Resource Manager-Vorlage hinzu. Wenn Sie noch keine haben, können Sie die folgende Vorlage verwenden. Mithilfe der Vorlage wird ein Speicherkonto erstellt.

```url
https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Sie können die Datei an einer beliebigen Stelle im Repository ablegen. Im Workflowbeispiel im nächsten Abschnitt wird davon ausgegangen, dass die Vorlagendatei **azuredeploy.json** heißt und im Repositorystamm in einem Ordner namens **templates** gespeichert ist.

## <a name="create-workflow"></a>Erstellen des Workflows

Die Workflowdatei muss am Repositorystamm im Ordner **.github/workflows** gespeichert werden. Die Erweiterung der Workflowdatei kann entweder **.yml** oder **.yaml** lauten.

Sie können entweder eine Workflowdatei erstellen und diese dann in das Repository pushen/hochladen oder wie folgt vorgehen:

1. Klicken Sie in Ihrem GitHub-Repository im oberen Menü auf **Actions** (Aktionen).
1. Klicken Sie auf **New workflow** (Neuer Workflow).
1. Klicken Sie auf **set up a workflow yourself** (Workflow selbst einrichten).
1. Benennen Sie die Workflowdatei um, wenn Sie einen anderen Namen als **main.yml** bevorzugen. Beispiel: **deployStorageAccount.yml**
1. Ersetzen Sie den Inhalt dieser YML-Datei durch den folgenden Code:

    ```yml
    name: Deploy ARM Template

    on:
      push:
        branches:
          - master
        paths:
          - ".github/workflows/deployStorageAccount.yml"
          - "templates/azuredeploy.json"

    jobs:
      deploy-storage-account-template:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout source code
            uses: actions/checkout@master

          - name: Login to Azure
            uses: azure/login@v1
            with:
              creds: ${{ secrets.AZURE_CREDENTIALS }}


          - name: Deploy ARM Template
            uses: azure/CLI@v1
            with:
              inlineScript: |
                az deployment group create --resource-group myResourceGroup --template-file ./templates/azuredeploy.json
    ```

    Die Workflowdatei besteht aus drei Abschnitten:

    - **name:** Der Name des Workflows.
    - **on:** Der Name der GitHub-Ereignisse, die den Workflow auslösen. Der Workflow wird ausgelöst, wenn ein Pushereignis für den Masterbranch auftritt, durch das mindestens eine der beiden angegebenen Dateien geändert wird. Bei den beiden Dateien handelt es sich um die Workflow- und die Vorlagendatei.

        > [!IMPORTANT]
        > Überprüfen Sie, ob die beiden Dateien und ihre Pfade mit denen übereinstimmen, die Ihnen vorliegen.
    - **jobs:** Eine Workflowausführung besteht aus mindestens einem Auftrag. Es gibt nur einen Auftrag namens **deploy-storage-account-template**.  Dieser Auftrag umfasst drei Schritte:

        - **Checkout source code** (Auschecken des Quellcodes)
        - **Login to Azure** (Anmelden bei Azure)

            > [!IMPORTANT]
            > Überprüfen Sie, ob der Name des Geheimnisses mit dem übereinstimmt, den Sie im Repository gespeichert haben. Weitere Informationen finden Sie unter [Konfigurieren von Anmeldeinformationen für die Bereitstellung](#configure-deployment-credentials).
        - **Deploy ARM template** (ARM-Vorlage anzeigen): Ersetzen Sie den Wert von **resourceGroupName**.  Wenn Sie im Abschnitt [Konfigurieren der Anmelde Informationen für die Bereitstellung verwendet haben](#configure-deployment-credentials) das Azure CLI-Skript verwendet haben, besteht der Name der generierten Ressourcengruppe aus dem Projektnamen, dem das Suffix **rg** angefügt wurde. Überprüfen Sie den Wert von **templateLocation**.

1. Klicken Sie auf **Start commit** (Commit starten).
1. Klicken Sie auf **Commit directly to the master branch** (Direkt im Masterbranch committen).
1. Klicken Sie auf **Commit new file** (Neue Datei committen) (oder **Commit changes** [Änderungen committen]).

Da der Workflow so konfiguriert ist, dass er entweder von der Workflow- oder der Vorlagendatei ausgelöst wird, die gerade aktualisiert wird, startet der Workflow direkt nach dem Committen der Änderungen.

## <a name="check-workflow-status"></a>Überprüfen des Workflowstatus

1. Klicken Sie auf die Registerkarte **Actions** (Aktionen). Ihnen sollte der Workflow **Create deployStorageAccount.yml** (deployStorageAccount.yml erstellen) angezeigt werden. Die Ausführung des Workflows dauert 1–2 Minuten.
1. Klicken Sie auf den Workflow, um ihn zu öffnen.
1. Klicken Sie im Menü auf der linken Seite auf **deploy-storage-account-template** (Auftragsname). Der Auftragsname wird im Workflow definiert.
1. Klicken Sie auf **Deploy ARM Template** (ARM-Vorlage bereitstellen) (Schrittname), um den Schritt zu erweitern. Nun wird Ihnen die Antwort der Rest-API angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Ein Schritt-für-Schritt-Tutorial mit Anleitungen zum Erstellen einer Vorlage finden Sie unter [Tutorial: Erstellen und Bereitstellen Ihrer ersten ARM-Vorlage](template-tutorial-create-first-template.md).
