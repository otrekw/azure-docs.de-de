---
title: ArmApiControl-Benutzeroberflächenelement
description: Hier wird das Benutzeroberflächenelement „Microsoft.Solutions.ArmApiControl“ für das Azure-Portal beschrieben. Wird zum Aufrufen von API-Vorgängen verwendet.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: tomfitz
ms.openlocfilehash: fdc299ef1945e3ee0810f1c314fc07edfb4f4873
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107313299"
---
# <a name="microsoftsolutionsarmapicontrol-ui-element"></a>Benutzeroberflächenelement „Microsoft.Solutions.ArmApiControl“

Mit „ArmApiControl“ können Sie Ergebnisse aus einem Azure Resource Manager-API-Vorgang abrufen. Mit den Ergebnissen können Sie dynamische Inhalte in anderen Steuerelementen auffüllen.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

Es gibt keine Benutzeroberfläche für dieses Steuerelement.

## <a name="schema"></a>Schema

Das folgende Beispiel zeigt das Schema für dieses Steuerelement:

```json
{
    "name": "testApi",
    "type": "Microsoft.Solutions.ArmApiControl",
    "request": {
        "method": "{HTTP-method}",
        "path": "{path-for-the-URL}",
        "body": {
            "key1": "val1",
            "key2": "val2"
        }
    }
}
```

## <a name="sample-output"></a>Beispielausgabe

Die Ausgabe des Steuerelements wird dem Benutzer nicht angezeigt. Stattdessen wird das Ergebnis des Vorgangs in anderen Steuerelementen verwendet.

## <a name="remarks"></a>Bemerkungen

- Die `request.method`-Eigenschaft gibt die HTTP-Methode an. Nur `GET` und `POST` sind zulässig.
- Durch die Eigenschaft `request.path` wird eine URL angegeben, bei der es sich um einen relativen Pfad zu einem ARM-Endpunkt handeln muss. Dies kann ein statischer Pfad sein, oder er kann dynamisch erstellt werden, indem auf Ausgabewerte der anderen Steuerelemente verwiesen wird.

  Beispielsweise ein ARM-Aufruf für den Ressourcenanbieter `Microsoft.Network/expressRouteCircuits`:

  ```json
  "path": "<subid>/resourceGroup/<resourceGroupName>/providers/Microsoft.Network/expressRouteCircuits/<routecircuitName>/?api-version=2020-05-01"
  ```

- Die `request.body`-Eigenschaft ist optional. Verwenden Sie ihn, um einen JSON-Text anzugeben, der mit der Anforderung gesendet wird. Der Text kann statischer Inhalt sein, oder er kann dynamisch erstellt werden, indem auf Ausgabewerte anderer Steuerelemente verwiesen wird.

## <a name="example"></a>Beispiel

Im folgenden Beispiel ruft das `providersApi`-Element eine API auf, um ein Array von Anbieterobjekten zu erhalten.

Die `allowedValues`-Eigenschaft des `providersDropDown`-Elements ist für das Abrufen der Namen der Anbieter konfiguriert. Diese werden in der Dropdownliste angezeigt.

```json
{
    "name": "providersApi",
    "type": "Microsoft.Solutions.ArmApiControl",
    "request": {
        "method": "GET",
        "path": "[concat(subscription().id, '/providers/Microsoft.Network/expressRouteServiceProviders?api-version=2019-02-01')]"
    }
},
{
    "name": "providerDropDown",
    "type": "Microsoft.Common.DropDown",
    "label": "Provider",
    "toolTip": "The provider that offers the express route connection.",
    "constraints": {
        "allowedValues": "[map(steps('settings').providersApi.value, (item) => parse(concat('{\"label\":\"', item.name, '\",\"value\":\"', item.name, '\"}')))]",
        "required": true
    },
    "visible": true
}
```

Ein Beispiel für die Verwendung von „ArmApiControl“ zum Überprüfen der Verfügbarkeit eines Ressourcennamens finden Sie unter [Microsoft.Common.TextBox](microsoft-common-textbox.md).

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
- Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
