---
title: Benutzeroberflächenelement „TextBox“
description: Hier wird das Benutzeroberflächenelement Microsoft.Common.TextBox für das Azure-Portal beschrieben. Es wird zum Hinzufügen von unformatiertem Text verwendet.
author: tfitzmac
ms.topic: conceptual
ms.date: 03/03/2021
ms.author: tomfitz
ms.openlocfilehash: 8e4cfcc7fe46c19bf1e58ee5eadf1e0bb8c7bd8e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102124328"
---
# <a name="microsoftcommontextbox-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.TextBox UI“

Ein Benutzeroberflächenelement (UI), das zum Hinzufügen von unformatiertem Text verwendet werden kann. Das `Microsoft.Common.TextBox`-Element ist ein einzeilige Eingabefeld, unterstützt jedoch mehrzeilige Eingaben mit der `multiLine`-Eigenschaft.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

Das `TextBox`-Element verwendet ein einzeiliges oder mehrzeiliges Textfeld.

:::image type="content" source="media/managed-application-elements/microsoft-common-textbox.png" alt-text="Das einzeilige Microsoft.Common.TextBox-Elementtextfeld.":::

:::image type="content" source="media/managed-application-elements/microsoft-common-textbox-multi-line.png" alt-text="Das mehrzeilige Microsoft.Common.TextBox-Elementtextfeld.":::

## <a name="schema"></a>Schema

```json
{
  "name": "nameInstance",
  "type": "Microsoft.Common.TextBox",
  "label": "Name",
  "defaultValue": "contoso123",
  "toolTip": "Use only allowed characters",
  "placeholder": "",
  "multiLine": false,
  "constraints": {
    "required": true,
    "validations": [
      {
        "regex": "^[a-z0-9A-Z]{1,30}$",
        "message": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
      },
      {
        "isValid": "[startsWith(steps('resourceConfig').nameInstance, 'contoso')]",
        "message": "Must start with 'contoso'."
      }
    ]
  },
  "visible": true
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
"contoso123"
```

## <a name="remarks"></a>Bemerkungen

- Verwenden Sie die `toolTip`-Eigenschaft, um Text zum Element anzuzeigen, wenn sich der Mauszeiger über dem Informationssymbol befindet.
- Die `placeholder`-Eigenschaft ist Hilfetext, der ausgeblendet wird, wenn der Benutzer mit der Bearbeitung beginnt. Wenn sowohl `placeholder` als auch `defaultValue` definiert ist, hat `defaultValue` Vorrang und wird angezeigt.
- Die `multiLine`-Eigenschaft ist ein boolescher Wert, `true` oder `false`. Um ein mehrzeiliges Textfeld zu verwenden, legen Sie die Eigenschaft auf `true` fest. Wenn kein mehrzeiliges Textfeld benötigt wird, legen Sie die Eigenschaft auf `false` fest, oder schließen Sie die Eigenschaft aus. Bei neuen Zeilen zeigt die JSON-Ausgabe `\n` für den Zeilenvorschub an. Das mehrzeilige Textfeld akzeptiert `\r` für einen Wagenrücklauf (CR) und `\n` für einen Zeilenvorschub (LF). Ein Standardwert kann z. B. `\r\n` zum Angeben von CRLF enthalten.
- Wenn `constraints.required` auf `true` festgelegt ist, muss das Textfeld für eine erfolgreiche Überprüfung einen Wert enthalten. Standardwert: `false`.
- Die `validations`-Eigenschaft ist ein Array, in dem Sie Bedingungen zum Überprüfen des im Textfeld angegebenen Werts hinzufügen.
- Die Eigenschaft `regex` ist ein reguläres JavaScript-Ausdrucksmuster. Wenn der Wert des Textfelds angegeben wird, muss er für eine erfolgreiche Überprüfung dem Muster entsprechen. Standardwert: `null`. Weitere Informationen zur RegEx-Syntax finden Sie unter [Sprachelemente für reguläre Ausdrücke – Kurzübersicht](/dotnet/standard/base-types/regular-expression-language-quick-reference).
- Die `isValid`-Eigenschaft enthält einen Ausdruck, dessen Auswertung `true` oder `false` ergibt. Innerhalb des Ausdrucks definieren Sie die Bedingung, die bestimmt, ob das Textfeld gültig ist.
- Die Eigenschaft `message` ist eine Zeichenfolge, die angezeigt wird, wenn bei der Überprüfung des Werts des Textfelds ein Fehler auftritt.
- Für `regex` kann ein Wert angegeben werden, wenn `required` auf `false` festgelegt ist. In diesem Szenario ist für eine erfolgreiche Überprüfung des Textfelds kein Wert erforderlich. Wird ein Wert angegeben, muss er dem Muster des regulären Ausdrucks entsprechen.

## <a name="examples"></a>Beispiele

In den Beispielen wird gezeigt, wie ein einzeiliges Textfeld und ein mehrzeiliges Textfeld verwendet werden.

### <a name="single-line"></a>Einzeilig

Im folgenden Beispiel wird ein Textfeld mit dem [Microsoft.Solutions.ArmApiControl](microsoft-solutions-armapicontrol.md)-Steuerelement verwendet, um die Verfügbarkeit eines Ressourcennamens zu überprüfen.

```json
"basics": [
  {
    "name": "nameApi",
    "type": "Microsoft.Solutions.ArmApiControl",
    "request": {
      "method": "POST",
      "path": "[concat(subscription().id, '/providers/Microsoft.Storage/checkNameAvailability?api-version=2019-06-01')]",
      "body": "[parse(concat('{\"name\": \"', basics('txtStorageName'), '\", \"type\": \"Microsoft.Storage/storageAccounts\"}'))]"
    }
  },
  {
    "name": "txtStorageName",
    "type": "Microsoft.Common.TextBox",
    "label": "Storage account name",
    "constraints": {
      "validations": [
        {
          "isValid": "[not(equals(basics('nameApi').nameAvailable, false))]",
          "message": "[concat('Name unavailable: ', basics('txtStorageName'))]"
        }
      ]
    }
  }
]
```

### <a name="multi-line"></a>Mehrzeilig

In diesem Beispiel wird die `multiLine`-Eigenschaft verwendet, um ein mehrzeiliges Textfeld mit Platzhaltertext zu erstellen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {
        "name": "demoTextBox",
        "type": "Microsoft.Common.TextBox",
        "label": "Multi-line text box",
        "defaultValue": "",
        "toolTip": "Use 1-60 alphanumeric characters, hyphens, spaces, periods, and colons.",
        "placeholder": "This is a multi-line text box:\nLine 1.\nLine 2.\nLine 3.",
        "multiLine": true,
        "constraints": {
          "required": true,
          "validations": [
            {
              "regex": "^[a-z0-9A-Z -.:\n]{1,60}$",
              "message": "Only 1-60 alphanumeric characters, hyphens, spaces, periods, and colons are allowed."
            }
          ]
        },
        "visible": true
      }
    ],
    "steps": [],
    "outputs": {
      "textBox": "[basics('demoTextBox')]"
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung in das Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Die Datei „CreateUiDefinition.json“ für die Benutzeroberfläche zum Erstellen verwalteter Azure-Anwendungen](create-uidefinition-overview.md).
- Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
