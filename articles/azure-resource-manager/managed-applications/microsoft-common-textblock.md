---
title: Benutzeroberflächenelement „TextBlock“
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.InfoBox“ für das Azure-Portal beschrieben. Es wird zum Hinzufügen von Text zur Benutzeroberfläche verwendet.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: ba2c4d410891910c29ee1fda1065f8230ab171bf
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87063853"
---
# <a name="microsoftcommontextblock-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.TextBlock“

Ein Steuerelement, mit dem Text zur Portaloberfläche hinzugefügt werden kann

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft-common-textblock.png)

## <a name="schema"></a>Schema

```json
{
  "name": "text1",
  "type": "Microsoft.Common.TextBlock",
  "visible": true,
  "options": {
    "text": "Please provide the configuration values for your application.",
    "link": {
      "label": "Learn more",
      "uri": "https://www.microsoft.com"
    }
  }
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
"Please provide the configuration values for your application. Learn more"
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
