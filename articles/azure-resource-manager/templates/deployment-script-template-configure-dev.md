---
title: Konfigurieren der Entwicklungsumgebung für Bereitstellungsskripts in Vorlagen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Entwicklungsumgebung für Bereitstellungsskripts in Azure Resource Manager-Vorlagen (ARM-Vorlagen) konfigurieren.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 12/14/2020
ms.author: jgao
ms.openlocfilehash: f731236b235883f019c74ef0b32f5066ca5b7514
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2021
ms.locfileid: "98179366"
---
# <a name="configure-development-environment-for-deployment-scripts-in-arm-templates"></a>Konfigurieren der Entwicklungsumgebung für Bereitstellungsskripts in ARM-Vorlagen

Hier erfahren Sie, wie Sie eine Bereitstellungsumgebung für das Erstellen und Testen von Bereitstellungsskripts der ARM-Vorlage anhand eines Bereitstellungsskriptimage erstellen. Sie können entweder eine [Azure-Containerinstanz](../../container-instances/container-instances-overview.md) erstellen oder [Docker](https://docs.docker.com/get-docker/) verwenden. Beide Optionen werden in diesem Artikel beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-powershell-container"></a>Azure PowerShell-Container

Wenn Sie über kein Azure PowerShell-Bereitstellungsskript verfügen, können Sie eine Datei *hello.ps1* mit dem folgenden Inhalt erstellen:

```powershell
param([string] $name)
$output = 'Hello {0}' -f $name
Write-Output $output
$DeploymentScriptOutputs = @{}
$DeploymentScriptOutputs['text'] = $output
```

### <a name="azure-cli-container"></a>Azure CLI-Container

Wenn Sie ein Azure CLI-Containerimage verwenden möchten, können Sie eine Datei *hello.sh* mit dem folgenden Inhalt erstellen:

```bash
firstname=$1
lastname=$2
output="{\"name\":{\"displayName\":\"$firstname $lastname\",\"firstName\":\"$firstname\",\"lastName\":\"$lastname\"}}"
echo -n "Hello "
echo $output | jq -r '.name.displayName'
```

> [!NOTE]
> Wenn Sie ein Azure CLI-Bereitstellungsskript ausführen, wird der Speicherort der Skriptausgabedatei in einer Umgebungsvariablen namens `AZ_SCRIPTS_OUTPUT_PATH` gespeichert. Die Umgebungsvariable ist im Container der Entwicklungsumgebung nicht verfügbar. Weitere Informationen zum Arbeiten mit Azure CLI-Ausgaben finden Sie unter [Arbeiten mit Ausgaben von CLI-Skripts](deployment-script-template.md#work-with-outputs-from-cli-script).

## <a name="use-azure-powershell-container-instance"></a>Verwenden einer Azure PowerShell-Containerinstanz

Sie müssen ein Speicherkonto erstellen und dieses in die Containerinstanz einbinden, um die Skripts auf Ihrem Computer zu erstellen. Auf diese Weise können Sie Ihr Skript in das Speicherkonto hochladen und in der Containerinstanz ausführen.

> [!NOTE]
> Das Speicherkonto, das Sie zum Testen Ihres Skripts erstellen, ist nicht das gleiche Speicherkonto, das der Bereitstellungsskriptdienst zum Ausführen des Skripts verwendet. Der Bereitstellungsskriptdienst erstellt bei jeder Ausführung einen eindeutigen Namen als Dateifreigabe.

### <a name="create-an-azure-powershell-container-instance"></a>Erstellen einer Azure PowerShell-Containerinstanz

Die folgende Azure Resource Manager-Vorlage (ARM-Vorlage) erstellt eine Containerinstanz sowie eine Dateifreigabe und bindet die Freigabe in das Containerimage ein.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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
      "defaultValue": "mcr.microsoft.com/azuredeploymentscripts-powershell:az5.2",
      "metadata": {
        "description": "Specify the container image."
      }
    },
    "mountPath": {
      "type": "string",
      "defaultValue": "/mnt/azscripts/azscriptinput",
      "metadata": {
        "description": "Specify the mount path."
      }
    }
  },
  "variables": {
    "storageAccountName": "[tolower(concat(parameters('projectName'), 'store'))]",
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

Der Standardwert für den Einbindungspfad lautet `/mnt/azscripts/azscriptinput`. Dies ist der Pfad in der Containerinstanz, unter dem sie in die Dateifreigabe eingebunden wird.

Das in der Vorlage angegebene Standardcontainerimage ist **mcr.microsoft.com/azuredeploymentscripts-powershell:az5.2**. Eine Liste aller unterstützter Azure PowerShell-Versionen finden Sie [hier](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list).

Die Vorlage hält die Containerinstanz nach 1.800 Sekunden an. Sie haben 30 Minuten Zeit, bis die Containerinstanz in einen Beendigungsstatus wechselt und die Sitzung beendet wird.

So stellen Sie die Vorlage bereit:

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateFile = Read-Host -Prompt "Enter the template file path and file name"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Location $location -name $resourceGroupName
New-AzResourceGroupDeployment -resourceGroupName $resourceGroupName -TemplateFile $templatefile -projectName $projectName
```

### <a name="upload-the-deployment-script"></a>Hochladen des Bereitstellungsskripts

Laden Sie Ihr Bereitstellungsskript in das Speicherkonto hoch. Nachfolgend sehen Sie ein Beispiel für ein PowerShell-Skript:

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
2. Öffnen Sie die Containergruppe. Der Name der Standardcontainergruppe entspricht dem Projektnamen mit dem Zusatz *cg*. Die Containerinstanz weist den Status **Wird ausgeführt** auf.
3. Wählen Sie im Ressourcenmenü die Option **Container** aus. Der Name der Containerinstanz entspricht dem Projektnamen mit dem Zusatz *container*.

    ![Screenshot der Verbindungscontainerinstanz des Bereitstellungsskripts im Azure-Portal.](./media/deployment-script-template-configure-dev/deployment-script-container-instance-connect.png)

4. Klicken Sie auf **Verbinden** und dann auf **Verbinden**. Wenn Sie keine Verbindung mit der Containerinstanz herstellen können, starten Sie die Containergruppe neu, und versuchen Sie es noch mal.
5. Führen Sie im Konsolenbereich die folgenden Befehle aus:

    ```console
    cd /mnt/azscripts/azscriptinput
    ls
    pwsh ./hello.ps1 "John Dole"
    ```

    Die Ausgabe ist **Hello John Dole**.

    ![Screenshot der Testausgabe der Verbindungscontainerinstanz des Bereitstellungsskripts im Azure-Portal.](./media/deployment-script-template-configure-dev/deployment-script-container-instance-test.png)

## <a name="use-an-azure-cli-container-instance"></a>Verwenden einer Azure CLI-Containerinstanz

Sie müssen ein Speicherkonto erstellen und in die Containerinstanz einbinden, um Skripts auf Ihrem Computer zu erstellen. Anschließend können Sie Ihr Skript in das Speicherkonto hochladen und in der Containerinstanz ausführen.

> [!NOTE]
> Das Speicherkonto, das Sie zum Testen Ihres Skripts erstellen, ist nicht das gleiche Speicherkonto, das der Bereitstellungsskriptdienst zum Ausführen des Skripts verwendet. Der Bereitstellungsskriptdienst erstellt bei jeder Ausführung einen eindeutigen Namen als Dateifreigabe.

### <a name="create-an-azure-cli-container-instance"></a>Verwenden einer Azure CLI-Containerinstanz

Mit der folgenden ARM-Vorlage werden eine Containerinstanz und eine Dateifreigabe erstellt. Letztere wird dann in das Containerimage eingebunden:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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
      "defaultValue": "mcr.microsoft.com/azure-cli:2.9.1",
      "metadata": {
        "description": "Specify the container image."
      }
    },
    "mountPath": {
      "type": "string",
      "defaultValue": "/mnt/azscripts/azscriptinput",
      "metadata": {
        "description": "Specify the mount path."
      }
    }
  },
  "variables": {
    "storageAccountName": "[tolower(concat(parameters('projectName'), 'store'))]",
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
                "/bin/bash",
                "-c",
                "echo hello; sleep 1800"
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

Der Standardwert für den Einbindungspfad lautet `/mnt/azscripts/azscriptinput`. Dies ist der Pfad in der Containerinstanz, unter dem sie in die Dateifreigabe eingebunden wird.

Das in der Vorlage angegebene Standardcontainerimage ist **mcr.microsoft.com/azure-cli:2.9.1**. Eine Liste mit den unterstützten Azure CLI-Versionen finden Sie [hier](https://mcr.microsoft.com/v2/azure-cli/tags/list).

> [!IMPORTANT]
> Das Bereitstellungsskript verwendet die verfügbaren CLI-Images von Microsoft Container Registry (MCR). Das Zertifizieren eines CLI-Images für ein Bereitstellungsskript dauert ungefähr einen Monat. Verwenden Sie nicht die CLI-Versionen, die innerhalb von 30 Tagen veröffentlicht wurden. Die Veröffentlichungsdaten für die Images finden Sie unter [Versionshinweise für die Azure CLI](/cli/azure/release-notes-azure-cli?view=azure-cli-latest&preserve-view=true). Wenn Sie eine nicht unterstützte Version verwenden, werden in der Fehlermeldung die unterstützten Versionen aufgelistet.

Die Vorlage hält die Containerinstanz nach 1.800 Sekunden an. Sie haben 30 Minuten Zeit, bis die Containerinstanz in einen Beendigungsstatus wechselt und die Sitzung beendet wird.

So stellen Sie die Vorlage bereit:

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateFile = Read-Host -Prompt "Enter the template file path and file name"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Location $location -name $resourceGroupName
New-AzResourceGroupDeployment -resourceGroupName $resourceGroupName -TemplateFile $templatefile -projectName $projectName
```

### <a name="upload-the-deployment-script"></a>Hochladen des Bereitstellungsskripts

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
1. Öffnen Sie die Containergruppe. Der Name der Standardcontainergruppe entspricht dem Projektnamen mit dem Zusatz *cg*. Die Containerinstanz wird mit dem Status **Wird ausgeführt** angezeigt.
1. Wählen Sie im Ressourcenmenü die Option **Container** aus. Der Name der Containerinstanz entspricht dem Projektnamen mit dem Zusatz *container*.

    ![Bereitstellungsskript: Verbinden mit der Containerinstanz](./media/deployment-script-template-configure-dev/deployment-script-container-instance-connect.png)

1. Klicken Sie auf **Verbinden** und dann auf **Verbinden**. Wenn Sie keine Verbindung mit der Containerinstanz herstellen können, starten Sie die Containergruppe neu, und versuchen Sie es noch mal.
1. Führen Sie im Konsolenbereich die folgenden Befehle aus:

    ```console
    cd /mnt/azscripts/azscriptinput
    ls
    ./hello.sh John Dole
    ```

    Die Ausgabe ist **Hello John Dole**.

    ![Bereitstellungsskript: Testen der Containerinstanz](./media/deployment-script-template-configure-dev/deployment-script-container-instance-test-cli.png)

## <a name="use-docker"></a>Verwenden von Docker

Sie können ein vorkonfiguriertes Docker-Containerimage als Entwicklungsumgebung für Ihr Bereitstellungsskript verwenden. Informationen zum Installieren von Docker finden Sie [hier](https://docs.docker.com/get-docker/).
Darüber hinaus muss die Dateifreigabe so konfiguriert werden, dass das Verzeichnis mit den Bereitstellungsskripts in den Docker-Container eingebunden wird.

1. Pullen Sie das Containerimage für das Bereitstellungsskript auf den lokalen Computer:

    ```command
    docker pull mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    In diesem Beispiel wird die PowerShell-Version 4.3.0 verwendet.

    So rufen Sie ein CLI-Image per Pull aus MCR ab

    ```command
    docker pull mcr.microsoft.com/azure-cli:2.0.80
    ```

    In diesem Beispiel wird Version 2.0.80 der CLI verwendet. Das Bereitstellungsskript verwendet die standardmäßigen CLI-Containerimages, die Sie [hier](https://hub.docker.com/_/microsoft-azure-cli) finden.

1. Führen Sie das Docker-Image lokal aus.

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    Ersetzen Sie **&lt;host drive letter>** und **&lt;host directory name>** durch einen vorhandenen Ordner auf dem freigegebenen Laufwerk. Der Ordner wird dem Ordner _/data_ im Container zugeordnet. Bei der Zuordnung von _D:\docker_ gilt beispielsweise:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az4.3
    ```

    **-it** bedeutet, dass das Containerimage aktiv bleibt.

    Ein CLI-Beispiel:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azure-cli:2.0.80
    ```

1. Der folgende Screenshot zeigt die Ausführung eines PowerShell-Skripts, wenn sich auf dem freigegebenen Laufwerk eine Datei namens *helloworld.ps1* befindet.

    ![Resource Manager-Vorlage: Bereitstellungsskript, Docker-Befehle](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

Nachdem das Skript erfolgreich getestet wurde, können Sie es als Bereitstellungsskript in Ihren Vorlagen verwenden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Bereitstellungsskripts verwenden. Ein Tutorial zu Bereitstellungsskripts finden Sie unter:

> [!div class="nextstepaction"]
> [Tutorial: Verwenden von Bereitstellungsskripts in ARM-Vorlagen](./template-tutorial-deployment-script.md)
