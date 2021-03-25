---
title: Benutzeroberflächenelement „TagsByResource“
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.TagsByResource“ für das Azure-Portal beschrieben. Verwenden Sie dies, um während der Bereitstellung Tags auf eine Ressource anzuwenden.
author: tfitzmac
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: tomfitz
ms.openlocfilehash: e730201812005a9b469a964e4acd081ebe86b100
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87063926"
---
# <a name="microsoftcommontagsbyresource-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.TagsByResource“

Ein Steuerelement, um den Ressourcen in einer Bereitstellung [Tags](../management/tag-resources.md) zuzuordnen.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft-common-tagsbyresource.png)

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TagsByResource",
  "resources": [
    "Microsoft.Storage/storageAccounts",
    "Microsoft.Compute/virtualMachines"
  ]
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
{
  "Microsoft.Storage/storageAccounts": {
    "Dept": "Finance",
    "Environment": "Production"
  },
  "Microsoft.Compute/virtualMachines": {
    "Dept": "Finance"
  }
}
```

## <a name="remarks"></a>Bemerkungen

- Mindestens ein Element im `resources`-Array muss angegeben sein.
- Jedes Element in `resources` muss ein vollqualifizierter Ressourcentyp sein. Diese Elemente werden in der Dropdownliste **Ressourcen** angezeigt und können vom Benutzer mit Tags versehen werden.
- Die Ausgabe des Steuerelements wird für die einfache Zuweisung von Tagwerten in einer Azure Resource Manager-Vorlage formatiert. Um die Ausgabe des Steuerelements in einer Vorlage zu empfangen, fügen Sie einen Parameter in Ihre Vorlage ein, wie im folgenden Beispiel gezeigt:

  ```json
  "parameters": {
    "tagsByResource": { "type": "object", "defaultValue": {} }
  }
  ```

  Weisen Sie für jede markierbare Ressource die Tags-Eigenschaft dem Parameterwert für diesen Ressourcentyp zu:

  ```json
  {
    "name": "saName1",
    "type": "Microsoft.Storage/storageAccounts",
    "tags": "[ if(contains(parameters('tagsByResource'), 'Microsoft.Storage/storageAccounts'), parameters('tagsByResource')['Microsoft.Storage/storageAccounts'], json('{}')) ]",
    ...
  ```

- Verwenden Sie die [if](../templates/template-functions-logical.md#if)-Funktion, wenn Sie auf den „tagsByResource“-Parameter zugreifen. Dies ermöglicht Ihnen, ein leeres Objekt zuzuweisen, wenn dem angegebenen Ressourcentyp keine Tags zugewiesen sind.

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
- Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
