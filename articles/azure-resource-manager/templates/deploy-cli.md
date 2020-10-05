---
title: Bereitstellen von Ressourcen mit Azure-CLI und Vorlagen
description: Verwenden Sie Azure Resource Manager und Azure CLI, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert.
ms.topic: conceptual
ms.date: 09/08/2020
ms.openlocfilehash: 7e8ae7e8c568f5f0ebb85f434e33f142b5fe94e8
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2020
ms.locfileid: "89566159"
---
# <a name="deploy-resources-with-arm-templates-and-azure-cli"></a>Bereitstellen von Ressourcen mit ARM-Vorlagen und der Azure CLI

In diesem Artikel wird erläutert, wie Sie Ihre Ressourcen mithilfe der Azure CLI und Azure Resource Manager-Vorlagen (ARM) in Azure bereitstellen. Wenn Sie nicht mit den Konzepten der Bereitstellung und Verwaltung Ihrer Azure-Lösungen vertraut sind, informieren Sie sich unter [Übersicht über die Vorlagenbereitstellung](overview.md).

Die Bereitstellungsbefehle wurden in Version 2.2.0 der Azure CLI geändert. Die Beispiele in diesem Artikel erfordern Version 2.2.0 oder höher der Azure CLI.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

Wenn die Azure CLI nicht installiert ist, können Sie [Cloud Shell](#deploy-template-from-cloud-shell) verwenden.

## <a name="deployment-scope"></a>Bereitstellungsumfang

Sie können als Ziel für Ihre Bereitstellung eine Ressourcengruppe, ein Abonnement, eine Verwaltungsgruppe oder einen Mandanten verwenden. In den meisten Fällen wählen Sie die Bereitstellung in einer Ressourcengruppe aus. Verwenden Sie zum Anwenden von Richtlinien und Rollenzuweisungen in einem größeren Rahmen Abonnement-, Verwaltungsgruppen- oder Mandantenbereitstellungen. Bei Bereitstellungen in einem Abonnement können Sie eine Ressourcengruppe erstellen und ihr Ressourcen bereitstellen.

Abhängig vom Umfang der Bereitstellung verwenden Sie unterschiedliche Befehle.

* Für die Bereitstellung in einer **Ressourcengruppe** verwenden Sie [az deployment group create](/cli/azure/deployment/group#az-deployment-group-create):

  ```azurecli-interactive
  az deployment group create --resource-group <resource-group-name> --template-file <path-to-template>
  ```

* Für die Bereitstellung in einem **Abonnement** verwenden Sie [az deployment sub create](/cli/azure/deployment/sub#az-deployment-sub-create):

  ```azurecli-interactive
  az deployment sub create --location <location> --template-file <path-to-template>
  ```

  Weitere Informationen zu Bereitstellungen auf Abonnementebene finden Sie unter [Erstellen von Ressourcengruppen und Ressourcen auf Abonnementebene](deploy-to-subscription.md).

* Für die Bereitstellung in einer **Verwaltungsgruppe** verwenden Sie [az deployment mg create](/cli/azure/deployment/mg#az-deployment-mg-create):

  ```azurecli-interactive
  az deployment mg create --location <location> --template-file <path-to-template>
  ```

  Weitere Informationen zu Bereitstellungen auf Verwaltungsgruppenebene finden Sie unter [Erstellen von Ressourcen auf der Verwaltungsgruppenebene](deploy-to-management-group.md).

* Für die Bereitstellung in einem **Mandanten** verwenden Sie [az deployment tenant create](/cli/azure/deployment/tenant#az-deployment-tenant-create):

  ```azurecli-interactive
  az deployment tenant create --location <location> --template-file <path-to-template>
  ```

  Weitere Informationen zu Bereitstellungen auf Mandantenebene finden Sie unter [Erstellen von Ressourcen auf der Mandantenebene](deploy-to-tenant.md).

Die Beispiele in diesem Artikel verwenden Ressourcengruppenbereitstellungen.

## <a name="deploy-local-template"></a>Bereitstellen einer lokalen Vorlage

Beim Bereitstellen von Ressourcen in Azure gehen Sie folgendermaßen vor:

1. Anmelden bei Ihrem Azure-Konto
2. Erstellen Sie eine Ressourcengruppe, die als Container für die bereitgestellten Ressourcen fungiert. Der Name einer Ressourcengruppe darf nur alphanumerische Zeichen, Punkte, Unterstriche, Bindestriche und Klammern enthalten. Der Name kann bis zu 90 Zeichen umfassen. Der Name darf nicht mit einem Punkt enden.
3. Stellen Sie die Vorlage, die die zu erstellenden Ressourcen definiert, für die Ressourcengruppe bereit.

Eine Vorlage kann Parameter enthalten, mit denen Sie die Bereitstellung anpassen können. Beispielsweise können Sie Werte angeben, die einer bestimmten Umgebung (z.B. Entwicklung, Test und Produktion) angepasst sind. Die Beispielvorlage definiert einen Parameter für die Speicherkonto-SKU.

Im folgenden Beispiel wird eine Ressourcengruppe erstellt und eine Vorlage von Ihrem lokalen Computer aus bereitgestellt:

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Die Bereitstellung kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Nachricht mit dem Ergebnis angezeigt:

```output
"provisioningState": "Succeeded",
```

## <a name="deployment-name"></a>„Deployment name“ (Bereitstellungsname)

Im vorherigen Beispiel haben Sie der Bereitstellung den Namen `ExampleDeployment` gegeben. Wenn Sie keinen Namen für die Bereitstellung angeben, wird der Name der Vorlagendatei verwendet. Wenn Sie beispielsweise eine Vorlage mit dem Namen `azuredeploy.json` bereitstellen und keinen Bereitstellungsnamen angeben, erhält die Bereitstellung den Namen `azuredeploy`.

Bei jedem Ausführen einer Bereitstellung wird dem Bereitstellungsverlauf der Ressourcengruppe ein Eintrag mit dem Bereitstellungsnamen hinzugefügt. Wenn Sie eine andere Bereitstellung ausführen und denselben Namen vergeben, wird der vorherige Eintrag durch die aktuelle Bereitstellung ersetzt. Wenn Sie eindeutige Einträge im Bereitstellungsverlauf beibehalten möchten, müssen Sie jeder Bereitstellung einen eindeutigen Namen geben.

Um einen eindeutigen Namen zu erstellen, können Sie eine Zufallszahl zuweisen.

```azurecli-interactive
deploymentName='ExampleDeployment'$RANDOM
```

Sie können auch einen Datumswert hinzufügen.

```azurecli-interactive
deploymentName='ExampleDeployment'$(date +"%d-%b-%Y")
```

Wenn Sie gleichzeitige Bereitstellungen in derselben Ressourcengruppe mit dem gleichen Bereitstellungsnamen ausführen, wird nur die letzte Bereitstellung abgeschlossen. Alle Bereitstellungen mit dem gleichen Namen, die noch nicht abgeschlossen wurden, werden durch die letzte Bereitstellung ersetzt. Wenn Sie z. B. eine Bereitstellung mit dem Namen `newStorage` ausführen, die ein Speicherkonto mit dem Namen `storage1` bereitstellt, und gleichzeitig eine andere Bereitstellung mit dem Namen `newStorage` ausführen, die ein Speicherkonto mit dem Namen `storage2` bereitstellt, wird nur ein Speicherkonto bereitgestellt. Das resultierende Speicherkonto hat den Namen `storage2`.

Führen Sie jedoch eine Bereitstellung mit dem Namen `newStorage` aus, die ein Speicherkonto mit dem Namen `storage1` bereitstellt, und führen Sie direkt nach dem Abschluss eine andere Bereitstellung mit dem Namen `newStorage` aus, die ein Speicherkonto mit dem Namen `storage2` bereitstellt, erhalten Sie zwei Speicherkonten. Eines hat den Namen `storage1` und das andere den Namen `storage2`. Es ist jedoch nur ein Eintrag im Bereitstellungsverlauf vorhanden.

Wenn Sie für jede Bereitstellung einen eindeutigen Namen angeben, können Sie diese ohne Konflikt gleichzeitig ausführen. Wenn Sie eine Bereitstellung namens `newStorage1` ausführen, die ein Speicherkonto namens `storage1` bereitstellt, und gleichzeitig eine andere Bereitstellung namens `newStorage2` ausführen, die ein Speicherkonto namens `storage2` bereitstellt, erhalten Sie zwei Speicherkonten und zwei Einträge im Bereitstellungsverlauf.

Um Konflikte mit gleichzeitigen Bereitstellungen zu vermeiden und eindeutige Einträge im Bereitstellungsverlauf zu gewährleisten, geben Sie jeder Bereitstellung einen eindeutigen Namen.

## <a name="deploy-remote-template"></a>Bereitstellen einer Remotevorlage

Anstatt ARM-Vorlagen auf dem lokalen Computer zu speichern, könnten Sie sie auch an einem externen Speicherort speichern. Sie können Vorlagen in einem Quellcodeverwaltungs-Repository (z.B. GitHub) speichern. Für den gemeinsamen Zugriff in Ihrer Organisation können Sie sie auch in einem Azure-Speicherkonto speichern.

Verwenden Sie zum Bereitstellen einer externen Vorlage den **template-uri**-Parameter. Verwenden Sie den URI im Beispiel, um die Beispielvorlage aus GitHub bereitzustellen.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

Das obige Beispiel erfordert einen URI mit öffentlichem Zugriff für die Vorlage, was in den meisten Szenarien funktioniert, da die Vorlage keine vertraulichen Daten enthalten sollte. Wenn Sie vertrauliche Daten (z.B. ein Administratorkennwort) angeben müssen, übergeben Sie diesen Wert als sicheren Parameter. Wenn Sie jedoch keinen öffentlichen Zugriff auf Ihre Vorlage wünschen, können Sie sie schützen, indem Sie sie in einem privaten Speichercontainer speichern. Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token (Shared Access Signature) erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](secure-template-with-sas-token.md).

## <a name="deploy-template-spec"></a>Bereitstellen der Vorlagenspezifikationen

Anstatt eine lokale oder Remotevorlage bereitzustellen, können Sie eine [Vorlagenspezifikation](template-specs.md) erstellen. Bei der Vorlagenspezifikation handelt es sich um eine Ressource im Azure-Abonnement, die eine ARM-Vorlage enthält. Sie vereinfacht die sichere Freigabe der Vorlage für Benutzer in Ihrer Organisation. Mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) können Sie Zugriff auf die Vorlagenspezifikation gewähren. Diese Funktion steht derzeit als Vorschau zur Verfügung.

In den folgenden Beispielen wird das Erstellen und Bereitstellen einer Vorlagenspezifikation veranschaulicht. Diese Befehle sind nur verfügbar, wenn Sie sich für die [Vorschau registriert haben](https://aka.ms/templateSpecOnboarding).

Als Erstes erstellen Sie die Vorlagenspezifikation, indem Sie die ARM-Vorlage bereitstellen.

```azurecli
az ts create \
  --name storageSpec \
  --version "1.0" \
  --resource-group templateSpecRG \
  --location "westus2" \
  --template-file "./mainTemplate.json"
```

Anschließend erhalten Sie die ID für die Vorlagenspezifikation und stellen sie bereit.

```azurecli
id = $(az ts show --name storageSpec --resource-group templateSpecRG --version "1.0" --query "id")

az deployment group create \
  --resource-group demoRG \
  --template-spec $id
```

Weitere Informationen finden Sie unter [Azure Resource Manager-Vorlagenspezifikationen (Vorschau)](template-specs.md).

## <a name="preview-changes"></a>Vorschau der Änderungen

Vor dem Bereitstellen der Vorlage können Sie die Änderungen, die von der Vorlage an Ihrer Umgebung vorgenommen werden, in der Vorschau anzeigen. Überprüfen Sie anhand des [„Was-wäre-wenn“-Vorgangs](template-deploy-what-if.md), ob die Vorlage die erwarteten Änderungen vornimmt. „Was-wäre-wenn“ überprüft auch die Vorlage auf Fehler.

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../../includes/resource-manager-cloud-shell-deploy.md)]

Geben Sie in der Cloud Shell-Instanz folgende Befehle ein:

```azurecli-interactive
az group create --name examplegroup --location "South Central US"
az deployment group create --resource-group examplegroup \
  --template-uri <copied URL> \
  --parameters storageAccountType=Standard_GRS
```

## <a name="parameters"></a>Parameter

Zum Übergeben von Parameterwerten können Sie entweder Inlineparameter oder eine Parameterdatei verwenden.

### <a name="inline-parameters"></a>Inlineparameter

Wenn Sie Inlineparameter übergeben möchten, geben Sie die Werte in `parameters` an. Wenn Sie beispielsweise eine Zeichenfolge und ein Array an eine Vorlage in einer Bash-Shell übergeben möchten, verwenden Sie Folgendes:

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString='inline string' exampleArray='("value1", "value2")'
```

Wenn Sie die Azure-Befehlszeilenschnittstelle mit der Windows-Eingabeaufforderung (CMD) oder PowerShell verwenden, übergeben Sie das Array in folgendem Format: `exampleArray="['value1','value2']"`.

Sie können auch den Inhalt einer Datei abrufen und als Inlineparameter übergeben.

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString=@stringContent.txt exampleArray=@arrayContent.json
```

Das Abrufen eines Parameterwerts aus einer Datei ist praktisch, wenn Sie Konfigurationswerte angeben müssen. Sie können beispielsweise [cloud-init-Werte für einen virtuellen Linux-Computer](../../virtual-machines/linux/using-cloud-init.md) angeben.

Das Format „arrayContent.json“ sieht wie folgt aus:

```json
[
    "value1",
    "value2"
]
```

Verwenden Sie JSON, um ein Objekt zu übergeben, z. B. zum Festlegen von Tags. Ihre Vorlage kann z. B. einen Parameter wie den folgenden enthalten:

```json
    "resourceTags": {
      "type": "object",
      "defaultValue": {
        "Cost Center": "IT Department"
      }
    }
```

In diesem Fall können Sie eine JSON-Zeichenfolge übergeben, um den Parameter festzulegen, wie im folgenden Bash-Skript gezeigt:

```bash
tags='{"Owner":"Contoso","Cost Center":"2345-324"}'
az deployment group create --name addstorage  --resource-group myResourceGroup \
--template-file $templateFile \
--parameters resourceName=abcdef4556 resourceTags="$tags"
```

Schließen Sie den JSON-Code, der an das Objekt übergeben werden soll, in doppelte Anführungszeichen ein.

### <a name="parameter-files"></a>Parameterdateien

Anstatt Parameter als Inlinewerte in Ihrem Skript zu übergeben, ist es wohl einfacher, eine JSON-Datei zu verwenden, die die Parameterwerte enthält. Die Parameterdatei muss eine lokale Datei sein. Externe Parameterdateien werden mit der Azure-Befehlszeilenschnittstelle nicht unterstützt.

Weitere Informationen zur Parameterdatei finden Sie unter [Erstellen einer Resource Manager-Parameterdatei](parameter-files.md).

Um eine lokale Parameterdatei zu übergeben, verwenden Sie `@` zur Angabe einer lokalen Datei mit dem Namen „storage.parameters.json“ anzugeben.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

## <a name="handle-extended-json-format"></a>Verarbeiten des erweiterten JSON-Formats

Zum Bereitstellen einer Vorlage mit mehrzeiligen Zeichenfolgen oder Kommentaren mithilfe von Azure CLI, Version 2.3.0 oder niedriger, müssen Sie den Switch `--handle-extended-json-format` verwenden.  Beispiel:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2018-10-01",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[
    parameters('location')
    ]", //defaults to resource group location
  /*
    storage account and network interface
    must be deployed first
  */
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Rollback zu einer erfolgreiche Bereitstellung, wenn ein Fehler auftritt, finden Sie unter [Rollback bei Fehler zu erfolgreicher Bereitstellung](rollback-on-error.md).
- Wenn Sie angeben möchten, wie Ressourcen behandelt werden sollen, die in der Ressourcengruppe enthalten sind, aber nicht in der Vorlage definiert wurden, lesen Sie die Informationen unter [Azure Resource Manager-Bereitstellungsmodi](deployment-modes.md).
- Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Verstehen der Struktur und Syntax von ARM-Vorlagen](template-syntax.md).
- Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](common-deployment-errors.md).
- Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](secure-template-with-sas-token.md).
