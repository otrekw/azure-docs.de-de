---
title: Bereitstellen mehrerer Instanzen von Ressourcen
description: Verwenden des copy-Vorgangs und von Arrays in einer Azure Resource Manager-Vorlage, um einen Ressourcentyp mehrere Male bereitzustellen.
ms.topic: conceptual
ms.date: 09/27/2019
ms.openlocfilehash: 38b5bcd38e0dc8ba8c758e9aa8371857541ba55e
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2020
ms.locfileid: "77210659"
---
# <a name="resource-iteration-in-azure-resource-manager-templates"></a>Ressourceniteration in Azure Resource Manager-Vorlagen

In diesem Artikel erfahren Sie, wie mehr als eine Instanz einer Ressource in Ihrer Azure Resource Manager-Vorlage erstellen. Durch das Hinzufügen des **copy**-Elements zum Ressourcenabschnitt Ihrer Vorlage können Sie die Anzahl der bereitzustellenden Ressourcen dynamisch festlegen. Sie vermeiden außerdem die Wiederholung von Vorlagensyntax.

Sie können „copy“ auch mit [Eigenschaften](copy-properties.md) und [Variablen](copy-variables.md) verwenden.

Wenn Sie angeben müssen, ob eine Ressource überhaupt bereitgestellt wird, finden Sie die erforderlichen Informationen unter [Element „condition“](conditional-resource-deployment.md).

## <a name="resource-iteration"></a>Ressourceniteration

Das „copy“-Element weist das folgende allgemeine Format auf:

```json
"copy": {
  "name": "<name-of-loop>",
  "count": <number-of-iterations>,
  "mode": "serial" <or> "parallel",
  "batchSize": <number-to-deploy-serially>
}
```

Die Eigenschaft **name** ist ein beliebiger Wert, der die Schleife identifiziert. Die Eigenschaft **count** gibt die für den Ressourcentyp gewünschte Anzahl von Iterationen an.

Verwenden Sie die Eigenschaften **mode** und **batchSize**, um anzugeben, ob die Ressourcen parallel oder nacheinander bereitgestellt werden. Diese Eigenschaften werden unter [Seriell oder parallel](#serial-or-parallel) beschrieben.

Im folgenden Beispiel wird die Anzahl von Speicherkonten erstellt, die im Parameter **storageCount** angegeben ist.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {}
}
```

Beachten Sie, dass der Name der einzelnen Ressourcen die `copyIndex()`-Funktion enthält, die die aktuelle Iteration in der Schleife zurückgibt. `copyIndex()` ist nullbasiert. Im folgenden Beispiel werden die unten aufgeführten Namen erstellt:

```json
"name": "[concat('storage', copyIndex())]",
```

Namen:

* storage0
* storage1
* storage2

Zum Versetzen des Indexwerts können Sie einen Wert in der copyIndex()-Funktion übergeben. Die Anzahl von Iterationen wird weiterhin im copy-Element angegeben, aber der Wert von „copyIndex“ wird um den angegebenen Wert versetzt. Im folgenden Beispiel werden die unten aufgeführten Namen erstellt:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Namen:

* storage1
* storage2
* storage3

Der „copy“-Vorgang ist besonders bei Verwendung von Arrays hilfreich, weil Sie jedes Element im Array durchlaufen können. Verwenden Sie die Funktion `length` für das Array, um die Anzahl von Iterationen anzugeben, und `copyIndex`, um den aktuellen Index im Array abzurufen.

Im folgenden Beispiel wird ein Speicherkonto für jeden im Parameter angegebenen Namen erstellt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageNames": {
          "type": "array",
          "defaultValue": [
            "contoso",
            "fabrikam",
            "coho"
          ]
      }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(parameters('storageNames')[copyIndex()], uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": "[length(parameters('storageNames'))]"
      }
    }
  ],
  "outputs": {}
}
```

## <a name="serial-or-parallel"></a>Seriell oder parallel

Resource Manager erstellt die Ressourcen standardmäßig gleichzeitig. Es gilt keine Beschränkung für die Anzahl der parallel bereitgestellten Ressourcen, mit Ausnahme der Begrenzung der Gesamtanzahl auf 800 Ressourcen in der Vorlage. Die Reihenfolge, in der sie erstellt werden, ist nicht garantiert.

Es ist aber möglicherweise sinnvoll, anzugeben, dass die Ressource sequenziell bereitgestellt werden. Wenn Sie z.B. eine Produktionsumgebung aktualisieren, möchten Sie die Updates möglicherweise staffeln, sodass nur eine bestimmte Anzahl von Ressourcen gleichzeitig aktualisiert wird. Legen Sie zum seriellen Bereitstellen mehrerer Instanzen einer Ressource `mode` auf **serial** und `batchSize` auf die Anzahl der Instanzen fest, die zu einem Zeitpunkt bereitgestellt werden sollen. Im seriellen Modus erstellt Resource Manager eine Abhängigkeit von früheren Instanzen in der Schleife, sodass ein Batch erst dann gestartet wird, wenn der vorherige Batch abgeschlossen wurde.

Zum seriellen Bereitstellen von zwei Speicherkonten gleichzeitig verwenden Sie beispielsweise:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "copy": {
        "name": "storagecopy",
        "count": 4,
        "mode": "serial",
        "batchSize": 2
      },
      "properties": {}
    }
  ],
  "outputs": {}
}
```

Die mode-Eigenschaft akzeptiert auch **parallel**, wobei es sich um den Standardwert handelt.

## <a name="depend-on-resources-in-a-loop"></a>Abhängigkeit von Ressourcen in einer Schleife

Sie geben an, dass eine Ressource nach einer anderen Ressource bereitgestellt wird, indem Sie das `dependsOn`-Element verwenden. Um eine Ressource bereitzustellen, die von der Sammlung von Ressourcen in einer Schleife abhängt, geben Sie den Namen der Kopierschleife im dependsOn-Element an. Das folgende Beispiel zeigt, wie drei Speicherkonten vor dem Bereitstellen des virtuellen Computers bereitgestellt werden. Die vollständige Definition des virtuellen Computers ist dabei nicht angegeben. Beachten Sie, dass „name“ für das „copy“-Element auf `storagecopy` und auch das „dependsOn“-Element für die virtuellen Computer auf `storagecopy` festgelegt ist.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "copy": {
        "name": "storagecopy",
        "count": 3
      },
      "properties": {}
    },
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2015-06-15",
      "name": "[concat('VM', uniqueString(resourceGroup().id))]",
      "dependsOn": ["storagecopy"],
      ...
    }
  ],
  "outputs": {}
}
```

## <a name="iteration-for-a-child-resource"></a>Iteration für eine untergeordnete Ressource

Für eine untergeordnete Ressource kann keine Kopierschleife verwendet werden. Um mehrere Instanzen einer Ressource zu erstellen, die Sie in der Regel als innerhalb einer anderen Ressource geschachtelt definieren, müssen Sie diese Ressource stattdessen als Ressource oberster Ebene erstellen. Sie definieren die Beziehung zur übergeordneten Ressource mithilfe der Eigenschaften „type“ und „name“.

Angenommen, Sie definieren ein Dataset als untergeordnete Ressource innerhalb einer Data Factory.

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
  "resources": [
    {
      "type": "datasets",
      "name": "exampleDataSet",
      "dependsOn": [
        "exampleDataFactory"
      ],
      ...
    }
  ]
```

Um mehrere Datasets zu erstellen, verschieben Sie sie außerhalb der Data Factory. Das Dataset muss sich auf der gleichen Ebene wie die Data Factory befinden, ist aber immer noch eine untergeordnete Ressource der Data Factory. Sie erhalten die Beziehung zwischen Dataset und Data Factory mithilfe der Eigenschaften „type“ und „name“ bei. Da „type“ nicht mehr von seiner Position in der Vorlage abgeleitet werden kann, müssen Sie den vollqualifizierten Typ im folgenden Format angeben: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Um eine Über-/Unterordnungsbeziehung mit einer Instanz der Data Factory herzustellen, geben Sie einen Namen für das Dataset an, das den Namen der übergeordneten Ressource enthält. Verwenden Sie das folgende Format: `{parent-resource-name}/{child-resource-name}`.

Das folgende Beispiel zeigt die Implementierung:

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
},
{
  "type": "Microsoft.DataFactory/datafactories/datasets",
  "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
  "dependsOn": [
    "exampleDataFactory"
  ],
  "copy": {
    "name": "datasetcopy",
    "count": "3"
  },
  ...
}]
```

## <a name="copy-limits"></a>Einschränkungen für „copy“

Der Wert von „count“ darf 800 nicht überschreiten.

Der Wert von „count“ darf nicht negativ sein. Wenn Sie eine Vorlage mit Azure PowerShell 2.6 oder höher, Azure CLI 2.0.74 oder höher oder mit der REST-API-Version **2019-05-10** oder höher bereitstellen, können Sie „count“ auf „0“ (null) festlegen. Frühere Versionen von PowerShell, CLI und der REST-API unterstützen den Wert „0“ (null) für „count“ nicht.

Wenden Sie die [Bereitstellung im vollständigen Modus](deployment-modes.md) mit Kopieren mit Vorsicht an. Wenn Sie mit dem vollständigen Modus erneut in einer Ressourcengruppe bereitstellen, werden alle Ressourcen, die nicht in der Vorlage angegeben sind, nach dem Auflösen der Kopierschleife gelöscht.

## <a name="example-templates"></a>Beispielvorlagen

Die folgenden Beispiele zeigen allgemeine Szenarien für das Erstellen mehrerer Ressourcen oder Eigenschaften.

|Vorlage  |Beschreibung  |
|---------|---------|
|[Speicher kopieren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |Stellt mehrere Speicherkonten mit einer Indexnummer im Namen bereit. |
|[Speicher seriell kopieren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |Stellt mehrere Speicherkonten nacheinander bereit. Der Name enthält die Indexnummer. |
|[Speicher mit Array kopieren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |Stellt mehrere Speicherkonten bereit. Der Name enthält einen Wert aus einem Array. |
|[VM-Bereitstellung mit einer variablen Anzahl von Datenträgern](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Stellt mehrere Datenträger mit einem virtuellen Computer bereit. |
|[Mehrere Sicherheitsregeln](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Stellt mehrere Sicherheitsregeln in einer Netzwerksicherheitsgruppe bereit. Die Sicherheitsregeln werden aus einem Parameter generiert. Informationen zum Parameter finden Sie im Artikel zur [Datei mit mehreren NSG-Parametern](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json). |

## <a name="next-steps"></a>Nächste Schritte

* Ein Tutorial, das Sie durcharbeiten können, finden Sie unter [Tutorial: Erstellen mehrerer Ressourceninstanzen mit Resource Manager-Vorlagen](template-tutorial-create-multiple-instances.md).
* Informationen zu anderen Verwendungsmöglichkeiten für das „copy“-Element finden Sie unter [Eigenschafteniteration in Azure Resource Manager-Vorlagen](copy-properties.md) und [Variableniteration in Azure Resource Manager-Vorlagen](copy-variables.md).
* Informationen zur Verwendung des „copy“-Elementes mit geschachtelten Vorlagen finden Sie unter [Verwenden des „copy“-Elements](linked-templates.md#using-copy).
* Informationen zu den Abschnitten einer Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](template-syntax.md).
* Informationen zum Bereitstellen Ihrer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](deploy-powershell.md).

