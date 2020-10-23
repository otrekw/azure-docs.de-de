---
title: Benutzeroberflächenelement ServicePrincipalSelector
description: Hier wird das Benutzeroberflächenelement Microsoft.Common.ServicePrincipalSelector für das Azure-Portal beschrieben. Es enthält eine Dropdownliste zum Auswählen eines Anwendungsbezeichners und ein Textfeld zum Eingeben eines Kennworts oder Zertifikatfingerabdrucks.
author: tfitzmac
ms.topic: conceptual
ms.date: 09/29/2020
ms.author: tomfitz
ms.openlocfilehash: 73b242754bfae53b6df5abd9c2c8dee33b973dad
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91575995"
---
# <a name="microsoftcommonserviceprincipalselector-ui-element"></a>Benutzeroberflächenelement Microsoft.Common.ServicePrincipalSelector

Ein Steuerelement, mit dem Benutzer einen vorhandenen Dienstprinzipal auswählen oder einen neuen registrieren können. Wenn Sie **Neu erstellen** auswählen, durchlaufen Sie die Schritte zum Registrieren einer neuen Anwendung. Wenn Sie eine vorhandene Anwendung auswählen, stellt das Steuerelement ein Textfeld zum Eingeben eines Kennworts oder Zertifikatfingerabdrucks bereit.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

Die Standardansicht wird durch die Werte in der `defaultValue`-Eigenschaft bestimmt. Wenn die `principalId`-Eigenschaft einen gültigen Globally Unique Identifier (GUID) enthält, sucht das Steuerelement nach der Objekt-ID der Anwendung. Der Standardwert ist gültig, wenn der Benutzer keine Auswahl in der Dropdownliste trifft.

:::image type="content" source="./media/managed-application-elements/microsoft-common-serviceprincipal-initial.png" alt-text="Microsoft.Common.ServicePrincipalSelector – anfängliche Ansicht":::

Wenn Sie in der Dropdownliste **Neu erstellen** oder einen vorhandenen Anwendungsbezeichner auswählen, wird der **Authentifizierungstyp** zur Eingabe eines Kennworts oder Zertifikatsfingerabdrucks in das Textfeld angezeigt.

:::image type="content" source="./media/managed-application-elements/microsoft-common-serviceprincipal-selection.png" alt-text="Microsoft.Common.ServicePrincipalSelector – anfängliche Ansicht":::

## <a name="schema"></a>Schema

```json
{
  "name": "ServicePrincipal",
  "type": "Microsoft.Common.ServicePrincipalSelector",
  "label": {
    "principalId": "App Id",
    "password": "Password",
    "certificateThumbprint": "Certificate thumbprint",
    "authenticationType": "Authentication Type",
    "sectionHeader": "Service Principal"
  },
  "toolTip": {
    "principalId": "App Id",
    "password": "Password",
    "certificateThumbprint": "Certificate thumbprint",
    "authenticationType": "Authentication Type"
  },
  "defaultValue": {
    "principalId": "<default guid>",
    "name": "(New) default App Id"
  },
  "constraints": {
    "required": true,
    "regex": "^[a-zA-Z0-9]{8,}$",
    "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
  },
  "options": {
    "hideCertificate": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Bemerkungen

- Die erforderlichen Eigenschaften sind:
  - `name`
  - `type`
  - `label`
  - `defaultValue`: Gibt die Standardwerte `principalId` und `name` an.

- Die optionalen Eigenschaften sind:
  - `toolTip`: Fügt jeder Bezeichnung eine QuickInfo `infoBalloon` an.
  - `visible`: Ausblenden oder Anzeigen des Steuerelements.
  - `options`: Gibt an, ob die Zertifikatsfingerabdruck-Option zur Verfügung gestellt werden soll.
  - `constraints`: RegEx-Einschränkungen für die Kennwortüberprüfung.

## <a name="example"></a>Beispiel

Das Folgende ist ein Beispiel für das `Microsoft.Common.ServicePrincipalSelector`-Steuerelement. Mit der `defaultValue`-Eigenschaft wird `principalId` als Platzhalter für eine standardmäßige Anwendungsbezeichner-GUID auf `<default guid>` festgelegt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [],
    "steps": [
      {
        "name": "SPNcontrol",
        "label": "SPNcontrol",
        "elements": [
          {
            "name": "ServicePrincipal",
            "type": "Microsoft.Common.ServicePrincipalSelector",
            "label": {
              "principalId": "App Id",
              "password": "Password",
              "certificateThumbprint": "Certificate thumbprint",
              "authenticationType": "Authentication Type",
              "sectionHeader": "Service Principal"
            },
            "toolTip": {
              "principalId": "App Id",
              "password": "Password",
              "certificateThumbprint": "Certificate thumbprint",
              "authenticationType": "Authentication Type"
            },
            "defaultValue": {
              "principalId": "<default guid>",
              "name": "(New) default App Id"
            },
            "constraints": {
              "required": true,
              "regex": "^[a-zA-Z0-9]{8,}$",
              "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
            },
            "options": {
              "hideCertificate": false
            },
            "visible": true
          }
        ]
      }
    ],
    "outputs": {
      "appId": "[steps('SPNcontrol').ServicePrincipal.appId]",
      "objectId": "[steps('SPNcontrol').ServicePrincipal.objectId]",
      "password": "[steps('SPNcontrol').ServicePrincipal.password]",
      "certificateThumbprint": "[steps('SPNcontrol').ServicePrincipal.certificateThumbprint]",
      "newOrExisting": "[steps('SPNcontrol').ServicePrincipal.newOrExisting]",
      "authenticationType": "[steps('SPNcontrol').ServicePrincipal.authenticationType]"
    }
  }
}
```

## <a name="example-output"></a>Beispielausgabe

Die `appId` ist die ID der Anwendungsregistrierung, die Sie ausgewählt oder erstellt haben. Die `objectId` ist ein Array von „objectIds“ für die Dienstprinzipale, die für die ausgewählte Anwendungsregistrierung konfiguriert sind.

Wenn in der Dropdownliste keine Auswahl getroffen wird, ist **new** der `newOrExisting`-Eigenschaftswert:

```json
{
  "appId": {
    "value": "<default guid>"
  },
  "objectId": {
    "value": ["<default guid>"]
  },
  "password": {
    "value": "<password>"
  },
  "certificateThumbprint": {
    "value": ""
  },
  "newOrExisting": {
    "value": "new"
  },
  "authenticationType": {
    "value": "password"
  }
}
```

Wenn **Neu erstellen** oder ein vorhandener Anwendungsbezeichner in der Dropdownliste ausgewählt wird, ist **existing** der `newOrExisting`-Eigenschaftswert:

```json
{
  "appId": {
    "value": "<guid>"
  },
  "objectId": {
    "value": ["<guid>"]
  },
  "password": {
    "value": "<password>"
  },
  "certificateThumbprint": {
    "value": ""
  },
  "newOrExisting": {
    "value": "existing"
  },
  "authenticationType": {
    "value": "password"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
- Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
