---
title: ArmApiControl-Benutzeroberflächenelement
description: Hier wird das Benutzeroberflächenelement „Microsoft.Solutions.ArmApiControl“ für das Azure-Portal beschrieben. Wird zum Aufrufen von API-Vorgängen verwendet.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: tomfitz
ms.openlocfilehash: bbe36e072d10b81c421331b2212d8b161afd2693
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87094437"
---
# <a name="microsoftcommonarmapicontrol-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.ArmApiControl“

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
- Die `request.path`-Eigenschaft gibt den relativen Pfad der URL an. Dies kann ein statischer Pfad sein, oder er kann dynamisch erstellt werden, indem auf Ausgabewerte der anderen Steuerelemente verwiesen wird.
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

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
