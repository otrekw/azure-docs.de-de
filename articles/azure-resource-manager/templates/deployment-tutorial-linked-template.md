---
title: 'Tutorial: Bereitstellen einer verknüpften Vorlage'
description: Anleitung zum Bereitstellen einer verknüpften Vorlage
ms.date: 03/13/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: ''
ms.openlocfilehash: 81a8b6cd5044b31ee80f50448d6610ba6a57281e
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97585648"
---
# <a name="tutorial-deploy-a-linked-template"></a>Tutorial: Bereitstellen einer verknüpften Vorlage

In den [vorherigen Tutorials](./deployment-tutorial-local-template.md) haben Sie erfahren, wie Sie eine Vorlage bereitstellen, die auf dem lokalen Computer gespeichert ist. Zum Bereitstellen komplexer Lösungen können Sie eine Vorlage in mehrere Vorlagen unterteilen und diese über eine Hauptvorlage gemeinsam bereitstellen. In diesem Tutorial erfahren Sie, wie Sie eine Hauptvorlage bereitstellen, die den Verweis auf eine verknüpfte Vorlage enthält. Beim Bereitstellen der Hauptvorlage wird die Bereitstellung der verknüpften Vorlage ausgelöst. Außerdem erfahren Sie, wie Sie die verknüpfte Vorlage mit einem SAS-Token speichern und sichern. Dieser Schritt dauert ungefähr **12 Minuten**.

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das vorherige Tutorial durchzuführen. Dies ist jedoch keine Voraussetzung.

## <a name="review-template"></a>Überprüfen der Vorlage

In den vorherigen Tutorials haben Sie eine Vorlage bereitgestellt, mit der ein Speicherkonto, ein App Service-Plan und eine Web-App erstellt werden. Dazu wurde folgende Vorlage verwendet:

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/local-template/azuredeploy.json":::

## <a name="create-a-linked-template"></a>Erstellen einer verknüpften Vorlage

Sie können die Speicherkontoressource in einer verknüpften Vorlage aufteilen:

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/linked-template/linkedStorageAccount.json":::

Die folgende Vorlage ist die Hauptvorlage. Das hervorgehobene Objekt `Microsoft.Resources/deployments` zeigt, wie eine verknüpfte Vorlage aufgerufen wird. Die verknüpfte Vorlage kann nicht als lokale Datei oder Datei gespeichert werden, die nur in Ihrem lokalen Netzwerk verfügbar ist. Sie können nur einen URI-Wert bereitstellen, der HTTP oder HTTPS enthält. Resource Manager muss auf die Vorlage zugreifen können. Eine Option besteht darin, die verknüpfte Vorlage in einem Speicherkonto zu platzieren und den URI für dieses Element zu verwenden. Der URI wird mithilfe eines Parameters an die Vorlage übergeben. Beachten Sie die hervorgehobene Parameterdefinition.

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/linked-template/azuredeploy.json" highlight="27-32,40-58":::

Speichern Sie eine Kopie der Hauptvorlage auf dem lokalen Computer mit der Erweiterung _JSON_, z. B. _azuredeploy.json_. Sie müssen keine Kopie der verknüpften Vorlage speichern. Die verknüpfte Vorlage wird aus einem GitHub-Repository in ein Speicherkonto kopiert.

## <a name="store-the-linked-template"></a>Speichern der verknüpften Vorlage

Mit dem folgenden PowerShell-Skript werden ein Speicherkonto und ein Container erstellt und die verknüpfte Vorlage aus einem GitHub-Repository in den Container kopiert. Eine Kopie der verknüpften Vorlage wird in [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/get-started-deployment/linked-template/linkedStorageAccount.json) gespeichert.

Wählen Sie **Try-it** aus, um die Cloud Shell zu öffnen, wählen Sie **Kopieren** aus, um das PowerShell-Skript zu kopieren, und klicken Sie mit der rechten Maustaste auf den Shellbereich, um das Skript einzufügen:

> [!IMPORTANT]
> Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Der Name muss eindeutig sein. In der Vorlage ist der Name des Speicherkontos der Projektname, an den **store** angefügt wird. Der Projektname muss zwischen 3 und 11 Zeichen lang sein. Daher muss der Projektname die Anforderungen des Speicherkontonamens erfüllen und weniger als 11 Zeichen enthalten.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name:"   # This name is used to generate names for Azure resources, such as storage account name.
$location = Read-Host -Prompt "Enter a location (i.e. centralus)"

$resourceGroupName = $projectName + "rg"
$storageAccountName = $projectName + "store"
$containerName = "templates" # The name of the Blob container to be created.

$linkedTemplateURL = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/get-started-deployment/linked-template/linkedStorageAccount.json" # A completed linked template used in this tutorial.
$fileName = "linkedStorageAccount.json" # A file name used for downloading and uploading the linked template.

# Download the template
Invoke-WebRequest -Uri $linkedTemplateURL -OutFile "$home/$fileName"

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
New-AzStorageContainer -Name $containerName -Context $context -Permission Container

# Upload the template
Set-AzStorageBlobContent `
    -Container $containerName `
    -File "$home/$fileName" `
    -Blob $fileName `
    -Context $context

Write-Host "Press [ENTER] to continue ..."
```

## <a name="deploy-template"></a>Bereitstellen der Vorlage

Generieren Sie zum Bereitstellen einer privaten Vorlage in einem Speicherkonto ein SAS-Token, und fügen Sie es dem URI für die Vorlage hinzu. Legen Sie die Ablaufzeit so fest, dass ausreichend Zeit für die Bereitstellung bleibt. Auf den Blob, der die Vorlage enthält, hat nur der Kontobesitzer Zugriff. Wenn Sie jedoch ein SAS-Token für das Blob erstellen, können andere Benutzer über diesen URI auf das Blob zugreifen. Wenn ein anderer Benutzer den URI abfängt, hat dieser Benutzer Zugriff auf die Vorlage. Ein SAS-Token ist eine gute Möglichkeit zum Einschränken des Zugriffs auf Ihre Vorlagen. Sie sollten allerdings Kennwörter auf keinen Fall direkt in die Vorlage einschließen.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](./deployment-tutorial-local-template.md#create-resource-group).

> [!NOTE]
> Im folgenden Azure CLI-Code ist der `date`-Parameter `-d` ein ungültiges Argument in macOS. Daher müssen macOS-Benutzer `-v+2H` verwenden, um im Terminal unter macOS zwei Stunden zur aktuellen Zeit zu addieren.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell

$projectName = Read-Host -Prompt "Enter a project name:"   # This name is used to generate names for Azure resources, such as storage account name.
$templateFile = Read-Host -Prompt "Enter the main template file and path"

$resourceGroupName="${projectName}rg"
$storageAccountName="${projectName}store"
$containerName = "templates"
$fileName = "linkedStorageAccount.json" # A file name used for downloading and uploading the linked template.

$key = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName).Value[0]
$context = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $key

# Generate a SAS token
$linkedTemplateUri = New-AzStorageBlobSASToken `
    -Context $context `
    -Container $containerName `
    -Blob $fileName `
    -Permission r `
    -ExpiryTime (Get-Date).AddHours(2.0) `
    -FullUri

# Deploy the template
New-AzResourceGroupDeployment `
  -Name DeployLinkedTemplate `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile $templateFile `
  -projectName $projectName `
  -linkedTemplateUri $linkedTemplateUri `
  -verbose
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli

echo "Enter a project name that is used to generate resource names:"
read projectName
echo "Enter the main template file:"
read templateFile

resourceGroupName="${projectName}rg"
storageAccountName="${projectName}store"
containerName="templates"
fileName="linkedStorageAccount.json"

key=$(az storage account keys list -g $resourceGroupName -n $storageAccountName --query [0].value -o tsv)

linkedTemplateUri=$(az storage blob generate-sas \
  --account-name $storageAccountName \
  --account-key $key \
  --container-name $containerName \
  --name $fileName \
  --permissions r \
  --expiry `date -u -d "120 minutes" '+%Y-%m-%dT%H:%MZ'` \
  --full-uri)

linkedTemplateUri=$(echo $linkedTemplateUri | sed 's/"//g')
az deployment group create \
  --name DeployLinkedTemplate \
  --resource-group $resourceGroupName \
  --template-file $templateFile \
  --parameters projectName=$projectName linkedTemplateUri=$linkedTemplateUri \
  --verbose
```

---

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, eine verknüpfte Vorlage bereitzustellen. Im nächsten Tutorial erfahren Sie, wie Sie eine DevOps-Pipeline zum Bereitstellen einer Vorlage erstellen.

> [!div class="nextstepaction"]
> [Erstellen einer Pipeline](./deployment-tutorial-pipeline.md)
