---
title: Konvertieren von Azure Resource Manager-Vorlagen zwischen JSON und Bicep
description: Vergleicht Azure Resource Manager-Vorlagen, die mit JSON und Bicep erstellt wurden.
ms.topic: conceptual
ms.date: 02/19/2021
ms.openlocfilehash: 9388ed50f13d6885d0a0668b61a9141dae375244
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743304"
---
# <a name="comparing-json-and-bicep-for-templates"></a>Vergleichen von JSON- und Bicep für Vorlagen

In diesem Artikel wird die Bicep-Syntax mit der JSON-Syntax für Azure Resource Manager-Vorlagen (ARM-Vorlagen) verglichen. In den meisten Fällen bietet Bicep eine Syntax, die weniger ausführlich ist als die entsprechende in JSON.

## <a name="syntax-equivalents"></a>Syntaxentsprechungen

Wenn Sie mit der Verwendung von JSON zum Entwickeln von ARM-Vorlagen vertraut sind, verwenden Sie die folgende Tabelle, um mehr über die entsprechende Syntax für Bicep zu erfahren.

| Szenario | ARM-Vorlage | Bicep |
| -------- | ------------ | ----- |
| Ausdruck verfassen | `"[func()]"` | `func()` |
| Parameterwert abrufen | `[parameters('exampleParameter'))]` | `exampleParameter` |
| Variablenwert abrufen | `[variables('exampleVar'))]` | `exampleVar` |
| Verketten von Zeichenfolgen | `[concat(parameters('namePrefix'), '-vm')]` | `'${namePrefix}-vm'` |
| Ressourceneigenschaft festlegen | `"sku": "2016-Datacenter",` | `sku: '2016-Datacenter'` |
| Logisches UND zurückgeben | `[and(parameter('isMonday'), parameter('isNovember'))]` | `isMonday && isNovember` |
| Ressourcen-ID der Ressource in der Vorlage abrufen | `[resourceId('Microsoft.Network/networkInterfaces', variables('nic1Name'))]` | `nic1.id` |
| Eigenschaft aus Ressource in der Vorlage abrufen | `[reference(resourceId('Microsoft.Storage/storageAccounts', variables('diagStorageAccountName'))).primaryEndpoints.blob]` | `diagsAccount.properties.primaryEndpoints.blob` |
| Einen Wert bedingt festlegen | `[if(parameters('isMonday'), 'valueIfTrue', 'valueIfFalse')]` | `isMonday ? 'valueIfTrue' : 'valueIfFalse'` |
| Eine Lösung in mehrere Dateien aufteilen | Verknüpfte Vorlagen verwenden | Module verwenden |
| Zielumfang der Bereitstellung festlegen | `"$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#"` | `targetScope = 'subscription'` |
| Abhängigkeit festlegen | `"dependsOn": ["[resourceId('Microsoft.Storage/storageAccounts', 'parameters('storageAccountName'))]"]` | Entweder Sie verlassen sich auf die automatische Erkennung von Abhängigkeiten, oder Sie legen Abhängigkeiten manuell mit `dependsOn: [ stg ]` fest. |

Verwenden Sie zum Deklarieren des Typs und der Version für eine Ressource Folgendes in Bicep:

```bicep
resource vm 'Microsoft.Compute/virtualMachines@2020-06-01' = {
  ...
}
```

Anstelle der entsprechenden Syntax in JSON:

```json
"resources": [
  {
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2020-06-01",
    ...
  }
]
```

## <a name="recommendations"></a>Empfehlungen

* Vermeiden Sie nach Möglichkeit die Verwendung der Funktionen [reference](template-functions-resource.md#reference) und [resourceId](template-functions-resource.md#resourceid) in Ihrer Bicep-Datei. Wenn Sie in derselben Bicep-Bereitstellung auf eine Ressource verweisen, verwenden Sie stattdessen den Ressourcenbezeichner. Wenn Sie z. B. eine Ressource in Ihrer Bicep-Datei mit `stg` als Ressourcenbezeichner bereitgestellt haben, verwenden Sie eine Syntax wie `stg.id` oder `stg.properties.primaryEndpoints.blob`, um Eigenschaftswerte abzurufen. Wenn Sie den Ressourcenbezeichner verwenden, erstellen Sie eine implizite Abhängigkeit zwischen Ressourcen. Sie müssen die Abhängigkeit nicht explizit mit der dependsOn-Eigenschaft festlegen.
* Verwenden Sie konsistente Schreibweisen für Bezeichner. Wenn Sie nicht sicher sind, welche Schreibweise Sie verwenden sollen, versuchen Sie es mit gemischter Groß-/Kleinschreibung (CamelCase). Beispiel: `param myCamelCasedParameter string`.
* Fügen Sie einem Parameter nur dann eine Beschreibung hinzu, wenn sie dem Benutzer wichtige Informationen vermittelt. Sie können für einige Informationen `//`-Kommentare verwenden.

## <a name="decompile-json-to-bicep"></a>Dekompilieren von JSON in Bicep

Die Bicep CLI bietet einen Befehl zum Dekompilieren jeglicher vorhandener ARM-Vorlage in eine Bicep-Datei. Verwenden Sie zum Dekompilieren einer JSON-Datei `bicep decompile "path/to/file.json"`.

Dieser Befehl bietet einen Ausgangspunkt für die Bicep-Erstellung, aber er funktioniert nicht für alle Vorlagen. Der Befehl kann fehlschlagen, oder Sie müssen möglicherweise nach der Dekompilierung Probleme beheben. Zurzeit weist der Befehl die folgenden Einschränkungen auf:

* Vorlagen mit Kopierschleifen können nicht dekompiliert werden.
* Geschachtelte Vorlagen können nur dekompiliert werden, wenn sie den Auswertungsbereich „inner“ für Ausdrücke verwenden.

Sie können die Vorlage für eine Ressourcengruppe exportieren und diese dann direkt an den Bicep-Befehl zum Dekompilieren übergeben. Im folgenden Beispiel wird gezeigt, wie Sie eine exportierte Vorlage dekompilieren.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az group export --name "your_resource_group_name" > main.json
bicep decompile main.json
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Export-AzResourceGroup -ResourceGroupName "your_resource_group_name" -Path ./main.json
bicep decompile main.json
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Exportieren Sie die Vorlage](export-template-portal.md) über das Portal. Verwenden Sie `bicep decompile <filename>` mit der heruntergeladenen Datei.

---

## <a name="build-json-from-bicep"></a>Erstellen von JSON aus Bicep

Die Bicep CLI bietet auch einen Befehl zum Konvertieren von Bicep in JSON. Verwenden Sie zum Erstellen einer JSON-Datei `bicep build "path/to/file.json"`.

## <a name="side-by-side-view"></a>Parallele Ansicht

Mit einem [Bicep Playground](https://aka.ms/bicepdemo) können Sie sich entsprechende JSON- und Bicep-Dateien nebeneinander anzeigen. Sie können eine Beispielvorlage auswählen, um beide Versionen anzuzeigen. Oder Sie wählen `Decompile` aus, um Ihre eigene JSON-Vorlage hochzuladen und die entsprechende Bicep-Datei anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Bicep-Projekt finden Sie unter [Projekt Bicep](https://github.com/Azure/bicep).
