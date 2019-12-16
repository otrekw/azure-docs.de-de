---
title: Sichere Artefakte in Vorlagen
description: Es wird beschrieben, wie Sie die Artefakte schützen, die in Ihren Azure Resource Manager-Vorlagen verwendet werden.
author: mumian
ms.date: 12/09/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 1a9d209e843d8e9a1735a3c6907b00d85be6580b
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74971729"
---
# <a name="tutorial-secure-artifacts-in-azure-resource-manager-template-deployments"></a>Tutorial: Schützen von Artefakten in Bereitstellungen per Azure Resource Manager-Vorlage

Hier wird beschrieben, wie Sie die in Ihren Azure Resource Manager-Vorlagen verwendeten Artefakte mit dem Azure Storage-Konto anhand von Shared Access Signatures (SAS) schützen. Bereitstellungsartefakte sind sämtliche Dateien, die zusätzlich zur Hauptvorlagendatei für eine Bereitstellung benötigt werden. Unter [Tutorial: Importieren von SQL-BACPAC-Dateien mit Azure Resource Manager-Vorlagen](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md) wird mit der Hauptvorlage beispielsweise eine Azure SQL-Datenbank erstellt. Außerdem wird eine BACPAC-Datei aufgerufen, um Tabellen zu erstellen und Daten einzufügen. Bei einer BACPAC-Datei handelt es sich um ein in einem Azure-Speicherkonto gespeichertes Artefakt. Der Speicherkontoschlüssel wurde für den Zugriff auf das Artefakt verwendet. In diesem Tutorial verwenden Sie SAS, um in Ihrem eigenen Azure Storage-Konto begrenzten Zugriff auf die BACPAC-Datei zu gewähren. Weitere Informationen zu SAS finden Sie unter [Verwenden von Shared Access Signatures (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Informationen zum Schützen einer verknüpften Vorlage finden Sie unter [Tutorial: Erstellen verknüpfter Azure Resource Manager-Vorlagen](./resource-manager-tutorial-create-linked-templates.md).

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Vorbereiten einer BACPAC-Datei
> * Öffnen einer vorhandenen Vorlage
> * Bearbeiten der Vorlage
> * Bereitstellen der Vorlage
> * Überprüfen der Bereitstellung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* Visual Studio Code mit der Erweiterung „Azure Resource Manager-Tools“. Informationen finden Sie unter [Verwenden von Visual Studio Code für die Erstellung von Azure Resource Manager-Vorlagen](./resource-manager-tools-vs-code.md).
* Sehen Sie sich [Tutorial: Importieren von SQL-BACPAC-Dateien mit Azure Resource Manager-Vorlagen](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md) an. Die hier verwendete Vorlage wird in diesem Tutorial entwickelt. Dieser Artikel enthält einen Link zum Herunterladen der fertigen Vorlage.
* Verwenden Sie aus Sicherheitsgründen ein generiertes Kennwort für das SQL Server-Administratorkonto. Hier sehen Sie ein Beispiel für die Kennwortgenerierung:

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault dient zum Schützen von kryptografischen Schlüsseln und anderen Geheimnissen. Weitere Informationen finden Sie unter [Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung](./resource-manager-tutorial-use-key-vault.md). Wir empfehlen Ihnen auch, Ihr Kennwort alle drei Monate zu aktualisieren.

## <a name="prepare-a-bacpac-file"></a>Vorbereiten einer BACPAC-Datei

In diesem Abschnitt bereiten Sie die BACPAC-Datei vor, damit der sichere Zugriff auf die Datei möglich ist, wenn Sie die Resource Manager-Vorlage bereitstellen. Dieser Abschnitt enthält fünf Verfahren:

* Herunterladen der BACPAC-Datei
* Erstellen eines Azure-Speicherkontos.
* Erstellen eines Speicherkonto-Blobcontainers
* Hochladen der BACPAC-Datei in den Container
* Abrufen des SAS-Tokens der BACPAC-Datei

1. Wählen Sie zum Öffnen der Cloudshell **Jetzt testen** aus, und fügen Sie anschließend das folgende PowerShell-Skript in das Shellfenster ein.

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name"   # This name is used to generate names for Azure resources, such as storage account name.
    $location = Read-Host -Prompt "Enter a location (i.e. centralus)"

    $resourceGroupName = $projectName + "rg"
    $storageAccountName = $projectName + "store"
    $containerName = "bacpacfile" # The name of the Blob container to be created.

    $bacpacURL = "https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac"
    $bacpacFileName = "SQLDatabaseExtension.bacpac" # A file name used for downloading and uploading the BACPAC file.

    # Download the bacpac file
    Invoke-WebRequest -Uri $bacpacURL -OutFile "$home/$bacpacFileName"

    # Create a resource group
    New-AzResourceGroup -Name $resourceGroupName -Location $location

    # Create a storage account
    $storageAccount = New-AzStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageAccountName `
        -Location $location `
        -SkuName "Standard_LRS"

    $context = $storageAccount.Context

    # Create a container
    New-AzStorageContainer -Name $containerName -Context $context

    # Upload the bacpac file
    Set-AzStorageBlobContent `
        -Container $containerName `
        -File "$home/$bacpacFileName" `
        -Blob $bacpacFileName `
        -Context $context

    # Generate a SAS token
    $bacpacURI = New-AzStorageBlobSASToken `
        -Context $context `
        -Container $containerName `
        -Blob $bacpacFileName `
        -Permission r `
        -ExpiryTime (Get-Date).AddHours(8.0) `
        -FullUri

    $str = $bacpacURI.split("?")

    Write-Host "You need the blob url and the SAS token later in the tutorial:"
    Write-Host $str[0]
    Write-Host (-join ("?", $str[1]))

    Write-Host "Press [ENTER] to continue ..."
    ```

1. Notieren Sie sich die URL zur BACPAC-Datei sowie das SAS-Token. Sie benötigen diese Werte beim Bereitstellen der Vorlage.

## <a name="open-an-existing-template"></a>Öffnen einer vorhandenen Vorlage

In dieser Sitzung ändern Sie die Vorlage, die Sie unter [Tutorial: Importieren von SQL-BACPAC-Dateien mit Azure Resource Manager-Vorlagen](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md) erstellt haben, um die BACPAC-Datei mit einem SAS-Token aufzurufen.  Die im Tutorial zur SQL-Erweiterung entwickelte Vorlage ist auf [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json) verfügbar.

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy2.json
    ```

3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.

    Es gibt vier Ressourcen, die in der Vorlage definiert werden:

   * `Microsoft.Sql/servers`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers).
   * `Microsoft.SQL/servers/firewallRules`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers/firewallrules).
   * `Microsoft.SQL/servers/databases`.  Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).
   * `Microsoft.SQL/server/databases/extensions`.  Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions).

     Bevor Sie die Vorlage anpassen, sollten Sie sich zunächst grundlegend damit vertraut machen.
4. Wählen Sie **Datei**>**Speichern unter** aus, um eine Kopie der Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="edit-the-template"></a>Bearbeiten der Vorlage

1. Ersetzen Sie die Parameterdefinition „storageAccountKey“ durch die folgende Parameterdefinition:

    ```json
    "_artifactsLocationSasToken": {
      "type": "securestring",
      "metadata": {
        "description": "Specifies the SAS token required to access the artifact location."
      }
    },
    ```

    ![Resource Manager-Tutorial: Schützen von Artefaktparametern](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-secure-artifacts-parameters.png)

2. Aktualisieren Sie den Wert der folgenden drei Elemente der SQL-Erweiterungsressource:

    ```json
    "storageKeyType": "SharedAccessKey",
    "storageKey": "[parameters('_artifactsLocationSasToken')]",
    "storageUri": "[parameters('bacpacUrl')]",
    ```

Die fertige Vorlage sieht folgendermaßen aus:

[!code-json[](~/resourcemanager-templates/tutorial-sql-extension/azuredeploy3.json?range=1-106&highlight=38-43,95-97)]

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Informationen zum Bereitstellungsverfahren finden Sie im Abschnitt [Bereitstellen der Vorlage](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template). Verwenden Sie stattdessen das folgende PowerShell-Bereitstellungsskript:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the project name that is used earlier"   # This name is used to generate names for Azure resources, such as storage account name.
$location = Read-Host -Prompt "Enter a location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the sql database admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$bacpacUrl = Read-Host -Prompt "Enter the BACPAC url"
$artifactsLocationSasToken = Read-Host -Prompt "Enter the artifacts location SAS token" -AsSecureString

$resourceGroupName = $projectName + "rg"

#New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -_artifactsLocationSasToken $artifactsLocationSasToken `
    -bacpacUrl $bacpacUrl `
    -TemplateFile "$HOME/azuredeploy.json"

Write-Host "Press [ENTER] to continue ..."
```

Verwenden Sie ein generiertes Kennwort. Siehe [Voraussetzungen](#prerequisites).
Die Werte von „_artifactsLocation“, „_artifactsLocationSasToken“ und „bacpacFileName“ finden Sie unter [Vorbereiten einer BACPAC-Datei](#prepare-a-bacpac-file).

## <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

Wählen Sie im Portal die SQL-Datenbank aus der neu bereitgestellten Ressourcengruppe aus. Wählen Sie **Abfrage-Editor (Vorschau)** aus, und geben Sie dann die Administratoranmeldeinformationen ein. Wie Sie sehen, wurden zwei Tabellen in die Datenbank importiert:

![Azure Resource Manager: Bereitstellen von SQL-Erweiterungen (BACPAC)](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine SQL Server-Instanz und eine SQL-Datenbank bereitgestellt und eine BACPAC-Datei mit einem SAS-Token importiert. Weitere Informationen zum Erstellen einer Azure Pipeline-Instanz zur kontinuierlichen Entwicklung und Bereitstellung von Resource Manager-Vorlagen finden Sie unter

> [!div class="nextstepaction"]
> [Continuous Integration mit Azure Pipelines](./resource-manager-tutorial-use-azure-pipelines.md)
