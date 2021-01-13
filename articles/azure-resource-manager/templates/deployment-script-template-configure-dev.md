---
title: Konfigurieren der Entwicklungsumgebung für Bereitstellungsskripts in Vorlagen | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über das Konfigurieren der Entwicklungsumgebung für Bereitstellungsskripts in Azure Resource Manager-Vorlagen.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 12/14/2020
ms.author: jgao
ms.openlocfilehash: d12ec5e3fef45429741fff1665f435d68e6c83f6
ms.sourcegitcommit: f7084d3d80c4bc8e69b9eb05dfd30e8e195994d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2020
ms.locfileid: "97734180"
---
# <a name="configure-development-environment-for-deployment-scripts-in-templates"></a>Konfigurieren der Entwicklungsumgebung für Bereitstellungsskripts in Vorlagen

Hier erfahren Sie, wie Sie eine Bereitstellungsumgebung für das Erstellen und Testen von Bereitstellungsskripts mit einem Bereitstellungsskriptimage erstellen. Sie können entweder eine [Azure-Containerinstanz](../../container-instances/container-instances-overview.md) erstellen oder [Docker](https://docs.docker.com/get-docker/) verwenden. Beide Varianten werden in diesem Artikel beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie über kein Bereitstellungsskript verfügen, können Sie eine **hello.ps1**-Datei mit dem folgenden Inhalt erstellen:

```powershell
param([string] $name)
$output = 'Hello {0}' -f $name
Write-Output $output
$DeploymentScriptOutputs = @{}
$DeploymentScriptOutputs['text'] = $output
```

## <a name="use-azure-container-instance"></a>Verwenden einer Azure-Containerinstanz

Sie müssen ein Speicherkonto erstellen und dieses in die Containerinstanz einbinden, um die Skripts auf Ihrem Computer zu erstellen. Auf diese Weise können Sie Ihr Skript in das Speicherkonto hochladen und in der Containerinstanz ausführen.

> [!NOTE]
> Das Speicherkonto, das Sie zum Testen Ihres Skripts erstellen, ist nicht das gleiche Speicherkonto, das der Bereitstellungsskriptdienst zum Ausführen des Skripts verwendet. Der Bereitstellungsskriptdienst erstellt bei jeder Ausführung einen eindeutigen Namen als Dateifreigabe.

### <a name="create-an-azure-container-instance"></a>Erstellen einer Azure-Containerinstanz

Mit der folgenden ARM-Vorlage wird eine Containerinstanz und eine Dateifreigabe erstellt. Letztere wird dann in das Containerimage eingebunden.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "projectName": {
      "type": "string",
      "metadata": {
        "description": "Specify a project name that is used for generating resource names."
      }
    },
    "containerImage": {
      "type": "string",
      "defaultValue": "mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3",
      "metadata": {
        "description": "Specify the container image."
      }
    },
    "mountPath": {
      "type": "string",
      "defaultValue": "deploymentScript",
      "metadata": {
        "description": "Specify the mount path."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(parameters('projectName'), 'store')]",
    "fileShareName": "[concat(parameters('projectName'), 'share')]",
    "containerGroupName": "[concat(parameters('projectName'), 'cg')]",
    "containerName": "[concat(parameters('projectName'), 'container')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "name": "[variables('storageAccountName')]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "StorageV2",
      "properties": {
        "accessTier": "Hot"
      }
    },
    {
      "type": "Microsoft.Storage/storageAccounts/fileServices/shares",
      "apiVersion": "2019-06-01",
      "name": "[concat(variables('storageAccountName'), '/default/', variables('fileShareName'))]",
      "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
      ]
    },
    {
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2019-12-01",
      "name": "[variables('containerGroupName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
      ],
      "properties": {
        "containers": [
          {
            "name": "[variables('containerName')]",
            "properties": {
              "image": "[parameters('containerImage')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "protocol": "TCP",
                  "port": 80
                }
              ],
              "volumeMounts": [
                {
                  "name": "filesharevolume",
                  "mountPath": "[parameters('mountPath')]"
                }
              ],
              "command": [
                "/bin/sh",
                "-c",
                "pwsh -c 'Start-Sleep -Seconds 1800'"
              ]
            }
          }
        ],
        "osType": "Linux",
        "volumes": [
          {
            "name": "filesharevolume",
            "azureFile": {
              "readOnly": false,
              "shareName": "[variables('fileShareName')]",
              "storageAccountName": "[variables('storageAccountName')]",
              "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), '2019-06-01').keys[0].value]"
            }
          }
        ]
      }
    }
  ]
}
```
Der Standardwert für den Einbindungspfad lautet **deploymentScript**.  Dies ist der Pfad in der Containerinstanz, in dem er in die Dateifreigabe eingebunden wird.

Das in der Vorlage angegebene Standardcontainerimage ist **mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3"** .   Eine Liste mit den unterstützten Azure PowerShell-Versionen finden Sie [hier](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list). Eine Liste mit den unterstützten Azure CLI-Versionen finden Sie [hier](https://mcr.microsoft.com/v2/azure-cli/tags/list).

  >[!IMPORTANT]
  > Das Bereitstellungsskript verwendet die verfügbaren CLI-Images von Microsoft Container Registry (MCR). Das Zertifizieren eines CLI-Images für das Bereitstellungsskript dauert ungefähr einen Monat. Verwenden Sie nicht die CLI-Versionen, die innerhalb von 30 Tagen veröffentlicht wurden. Die Veröffentlichungsdaten für die Images finden Sie unter [Versionshinweise für die Azure CLI](/cli/azure/release-notes-azure-cli?view=azure-cli-latest&preserve-view=true). Wenn eine nicht unterstützte Version verwendet wird, werden in der Fehlermeldung die unterstützten Versionen aufgelistet.

Die Vorlage hält die Containerinstanz 1800 Sekunden an. Sie haben 30 Minuten Zeit, bis die Containerinstanz in den Terminalstatus wechselt und die Sitzung beendet wird.

So stellen Sie die Vorlage bereit:

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateFile = Read-Host -Prompt "Enter the template file path and file name"
$resourceGroupName = "${projectName}rg"

New-azResourceGroup -Location $location -name $resourceGroupName
New-AzResourceGroupDeployment -resourceGroupName $resourceGroupName -TemplateFile $templatefile -projectName $projectName
```

### <a name="upload-deployment-script"></a>Hochladen des Bereitstellungsskripts

Laden Sie Ihr Bereitstellungsskript in das Speicherkonto hoch. Dies ist ein PowerShell-Beispiel:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that you used earlier"
$fileName = Read-Host -Prompt "Enter the deployment script file name with the path"

$resourceGroupName = "${projectName}rg"
$storageAccountName = "${projectName}store"
$fileShareName = "${projectName}share"

$context = (Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName).Context
Set-AzStorageFileContent -Context $context -ShareName $fileShareName -Source $fileName -Force
```

Sie können die Datei auch mithilfe des Azure-Portals oder über die Azure CLI hochladen.

### <a name="test-the-deployment-script"></a>Testen des Bereitstellungsskripts

1. Öffnen Sie im Azure-Portal die Ressourcengruppe, in der Sie die Containerinstanz und das Speicherkonto bereitgestellt haben.
1. Öffnen Sie die Containergruppe. Der Standardcontainergruppenname entspricht dem Projektnamen mit dem Zusatz **cg**. Für die Containerinstanz sollte der Status **Wird ausgeführt** angezeigt werden.
1. Klicken Sie im Menü links auf **Container**. Es sollte eine Containerinstanz angezeigt werden.  Der Containerinstanzname entspricht dem Projektnamen mit dem Zusatz **container**.

    ![Bereitstellungsskript: Verbinden mit der Containerinstanz](./media/deployment-script-template-configure-dev/deployment-script-container-instance-connect.png)

1. Klicken Sie auf **Verbinden** und dann auf **Verbinden**. Wenn Sie keine Verbindung mit der Containerinstanz herstellen können, starten Sie die Containergruppe neu, und versuchen Sie es noch mal.
1. Führen Sie im Konsolenbereich die folgenden Befehle aus:

    ```console
    cd deploymentScript
    ls
    pwsh ./hello.ps1 "John Dole"
    ```

    Die Ausgabe ist **Hello John Dole**.

    ![Bereitstellungsskript: Testen der Containerinstanz](./media/deployment-script-template-configure-dev/deployment-script-container-instance-test.png)

1. Wenn Sie das Containerimage „AZ CLI“ verwenden, führen Sie den folgenden Code aus:

   ```console
   cd /mnt/azscripts/azscriptinput
   ls
   ./userscript.sh
   ```

## <a name="use-docker"></a>Verwenden von Docker

Sie können ein vorkonfiguriertes Docker-Containerimage als Entwicklungsumgebung für Ihr Bereitstellungsskript verwenden. Informationen zum Installieren von Docker finden Sie [hier](https://docs.docker.com/get-docker/).
Darüber hinaus muss die Dateifreigabe so konfiguriert werden, dass das Verzeichnis mit den Bereitstellungsskripts in den Docker-Container eingebunden wird.

1. Pullen Sie das Containerimage für das Bereitstellungsskript auf den lokalen Computer:

    ```command
    docker pull mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    In diesem Beispiel wird die PowerShell-Version 4.3.0 verwendet.

    So pullen Sie ein CLI-Image aus einer Microsoft Container Registry (MCR)

    ```command
    docker pull mcr.microsoft.com/azure-cli:2.0.80
    ```

    In diesem Beispiel wird Version 2.0.80 der CLI verwendet. Das Bereitstellungsskript verwendet die standardmäßigen CLI-Containerimages, die Sie [hier](https://hub.docker.com/_/microsoft-azure-cli) finden.

1. Führen Sie das Docker-Image lokal aus.

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    Ersetzen Sie **&lt;host drive letter>** und **&lt;host directory name>** durch einen vorhandenen Ordner auf dem freigegebenen Laufwerk.  Der Ordner wird dem Ordner **/data** im Container zugeordnet. So ordnen Sie z. B. „D:\docker“ zu

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    **-it** bedeutet, dass das Containerimage aktiv bleibt.

    Ein CLI-Beispiel:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azure-cli:2.0.80
    ```

1. Der folgende Screenshot zeigt die Ausführung eines PowerShell-Skripts, wenn das freigegebene Laufwerk eine Datei namens „helloworld.ps1“ enthält.

    ![Resource Manager-Vorlage: Bereitstellungsskript, Docker-Befehle](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

Nachdem das Skript erfolgreich getestet wurde, können Sie es als Bereitstellungsskript in Ihren Vorlagen verwenden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Bereitstellungsskripts verwenden. Ein Tutorial zu Bereitstellungsskripts finden Sie unter:

> [!div class="nextstepaction"]
> [Tutorial: Verwenden von Bereitstellungsskripts in Azure Resource Manager-Vorlagen](./template-tutorial-deployment-script.md).
