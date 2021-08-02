---
title: Definieren mehrerer Instanzen eines Ausgabewerts in Bicep
description: Verwenden Sie eine Bicep-Ausgabeschleife, um Bereitstellungswerte zu durchlaufen und zurückzugeben.
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: cc931b7e0d65804892176a2965f87022a2becb7b
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111026143"
---
# <a name="output-iteration-in-bicep"></a>Ausgabeiteration in Bicep

In diesem Artikel wird gezeigt, wie Sie mehr als einen Wert für eine Ausgabe in Ihrer Bicep-Datei erstellen. Sie können dem Abschnitt `output` der Datei eine Schleife hinzufügen und während der Bereitstellung dynamisch mehrere Elemente zurückgeben.

Sie können eine Schleife auch mit [Ressourcen](loop-resources.md), [Eigenschaften in einer Ressource](loop-properties.md) und [Variablen](loop-variables.md) verwenden.

## <a name="syntax"></a>Syntax

Mit Schleifen können zahlreiche Elemente während der Bereitstellung zurückgegeben werden:

- Durchlaufen eines Arrays

  ```bicep
  output <output-name> array = [for <item> in <collection>: {
    <properties>
  }]

  ```

- Durchlaufen der Elemente eines Arrays

  ```bicep
  output <output-name> array = [for <item>, <index> in <collection>: {
    <properties>
  }]
  ```

- Verwenden eines Schleifenindexes

  ```bicep
  output <output-name> array = [for <index> in range(<start>, <stop>): {
    <properties>
  }]
  ```

## <a name="loop-limits"></a>Schleifengrenzwerte

Die Schleifeniterationen der Bicep-Datei dürfen keine negative Zahl sein oder 800 Iterationen überschreiten. Installieren Sie zum Bereitstellen von Bicep-Dateien die neueste Version der [Bicep-Tools](install.md).

## <a name="output-iteration"></a>Ausgabeiteration

Im folgenden Beispiel wird eine variable Anzahl von Speicherkonten erstellt und für jedes Speicherkonto ein Endpunkt zurückgegeben:

```bicep
param rgLocation string = resourceGroup().location
param storageCount int = 2

var baseNameVar = 'storage${uniqueString(resourceGroup().id)}'

resource baseName 'Microsoft.Storage/storageAccounts@2021-02-01' = [for i in range(0, storageCount): {
  name: '${i}${baseNameVar}'
  location: rgLocation
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'Storage'
}]

output storageEndpoints array = [for i in range(0, storageCount): reference('${i}${baseNameVar}').primaryEndpoints.blob]
```

Die Ausgabe gibt ein Array mit den folgenden Elementen zurück:

```json
[
  "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
  "https://1storagecfrbqnnmpeudi.blob.core.windows.net/"
]
```

In diesem Beispiel werden drei Eigenschaften aus den neuen Speicherkonten zurückgegeben:

```bicep
param rgLocation string = resourceGroup().location
param storageCount int = 2

var baseNameVar = 'storage${uniqueString(resourceGroup().id)}'

resource baseName 'Microsoft.Storage/storageAccounts@2021-02-01' = [for i in range(0, storageCount): {
  name: '${i}${baseNameVar}'
  location: rgLocation
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'Storage'
}]

output storageInfo array = [for i in range(0, storageCount): {
  id: reference('${i}${baseNameVar}', '2021-02-01', 'Full').resourceId
  blobEndpoint: reference('${i}${baseNameVar}').primaryEndpoints.blob
  status: reference('${i}${baseNameVar}').statusOfPrimary
}]
```

Die Ausgabe gibt ein Array mit den folgenden Elementen zurück:

```json
[
  {
    "id": "Microsoft.Storage/storageAccounts/0storagecfrbqnnmpeudi",
    "blobEndpoint": "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
    "status": "available"
  },
  {
    "id": "Microsoft.Storage/storageAccounts/1storagecfrbqnnmpeudi",
    "blobEndpoint": "https://1storagecfrbqnnmpeudi.blob.core.windows.net/",
    "status": "available"
  }
]
```

In diesem Beispiel wird ein Arrayindex verwendet, da direkte Verweise auf ein Ressourcenmodul oder eine Modulsammlung in Ausgabeschleifen nicht unterstützt werden:

```bicep
param rgLocation string = resourceGroup().location

var stgNames = [
  'demostg1'
  'demostg2'
  'demostg3'
]

resource stg 'Microsoft.Storage/storageAccounts@2021-02-01' = [for name in stgNames: {
  name: name
  location: rgLocation
  kind: 'Storage'
  sku: {
    name: 'Standard_LRS'
    tier: 'Standard'
  }
}]

output stgOutput array = [for (name, i) in stgNames: {
  name: stg[i].name
  resourceId: stg[i].id
}]
```

Die Ausgabe gibt ein Array mit den folgenden Elementen zurück:

```json
[
  {
    "name": "demostg1",
    "resourceId": "/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/demostg1"
  },
  {
    "name": "demostg2",
    "resourceId": "/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/demostg2"
  },
  {
    "name": "demostg3",
    "resourceId": "/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/demostg3"
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu anderen Verwendungsmöglichkeiten von Schleifen finden Sie unter folgenden Links:
  - [Ressourceniteration in Bicep-Dateien](loop-resources.md)
  - [Eigenschafteniteration in Bicep-Dateien](loop-properties.md)
  - [Variableniteration in Bicep-Dateien](loop-variables.md)
- Weitere Informationen zu den Abschnitten in einer Bicep-Datei finden Sie unter [Verstehen der Struktur und Syntax von Bicep-Dateien](file.md).
- Informationen zum Bereitstellen mehrerer Ressourcen finden Sie unter [Verwenden von Bicep-Modulen](modules.md).
- Informationen zum Festlegen von Abhängigkeiten von Ressourcen, die in einer Schleife erstellt werden, finden Sie unter [Festlegen von Ressourcenabhängigkeiten](./resource-declaration.md#set-resource-dependencies).
- Informationen zum Bereitstellen mit PowerShell finden Sie unter [Bereitstellen von Ressourcen mit Bicep und Azure PowerShell](deploy-powershell.md).
- Informationen zum Bereitstellen mit der Azure CLI finden Sie unter [Bereitstellen von Ressourcen mit Bicep und der Azure CLI](deploy-cli.md).
