---
title: Hinzufügen von benutzerdefinierten Attributen zu Self-Service-Registrierungsflows – Azure AD
description: Hier erfahren Sie mehr über das Anpassen der Attribute für Ihre Benutzerflows für die Self-Service-Registrierung.
services: active-directory
author: msmimart
manager: celestedg
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 06/16/2020
ms.author: mimart
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e64ab70fed13d4ca907b2bfb3aa448acdedc39e9
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92441452"
---
# <a name="define-custom-attributes-for-user-flows-preview"></a>Definieren benutzerdefinierter Attribute für Benutzerflows (Vorschau)

> [!NOTE]
> Das Feature für benutzerdefinierte Attribute ist eine öffentliche Previewfunktion von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie haben unter Umständen für jede Anwendung andere Anforderungen an die Informationen, die Sie während der Registrierung sammeln möchten. Azure AD umfasst einen integrierten Satz von in Attributen gespeicherten Informationen, z. B. Vorname, Nachname, Ort und Postleitzahl. Mit Azure AD können Sie die verschiedenen Attribute erweitern, die in einem Gastkonto gespeichert werden, wenn sich der externe Benutzer über einen Benutzerflow anmeldet.

Im Azure-Portal können Sie benutzerdefinierte Attribute erstellen und in den Benutzerflows für die Self-Service-Registrierung verwenden. Außerdem können Sie diese Attribute mit der [Microsoft Graph-API](../../active-directory-b2c/manage-user-accounts-graph-api.md) lesen und schreiben. Die Microsoft Graph-API unterstützt das Erstellen und Aktualisieren eines Benutzers mit Erweiterungsattributen. Erweiterungsattribute in der Graph-API werden mithilfe der Konvention `extension_<extensions-app-id>_attributename` benannt. Beispiel:

```JSON
"extension_831374b3bd5041bfaa54263ec9e050fc_loyaltyNumber": "212342"
```

Die `<extensions-app-id>` gilt nur für Ihren Mandanten. Um diese ID zu suchen, navigieren Sie zu „Azure Active Directory > App-Registrierungen > Alle Anwendungen“. Suchen Sie nach der APP, die mit „aad-extensions-app“ beginnt, und wählen Sie sie aus. Notieren Sie sich auf der Übersichtsseite der App die Anwendungs(client)-ID.

## <a name="create-a-custom-attribute"></a>Erstellen eines benutzerdefinierten Attributs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Azure AD-Administrator an.
2. Wählen Sie unter **Azure-Dienste** die Option **Azure Active Directory** aus.
3. Wählen Sie im Menü auf der linken Seite **Externe Identitäten** aus.
4. Wählen Sie **Benutzerdefinierte Benutzerattribute (Vorschau)** aus. Die verfügbaren Benutzerattribute werden aufgelistet.

   ![Auswählen von Benutzerattributen für die Registrierung](media/user-flow-add-custom-attributes/user-attributes.png)

5. Wählen Sie zum Hinzufügen eines Attributs **Hinzufügen** aus.
6. Geben Sie im Bereich **Attribut hinzufügen** die folgenden Werte ein:

   - **Name** : Geben Sie einen Namen für das benutzerdefinierte Attribut an (z. B. „Shoesize“).
   - **Datentyp** : Wählen Sie einen Datentyp ( **Zeichenfolge** , **Boolesch** oder **Ganze Zahl** ) aus.
   - **Beschreibung** : Geben Sie optional eine Beschreibung des benutzerdefinierten Attributs zur internen Verwendung ein. Diese Beschreibung ist für den Benutzer nicht sichtbar.

   ![Hinzufügen eines Attributs](media/user-flow-add-custom-attributes/add-an-attribute.png)

7. Klicken Sie auf **Erstellen** .

Das benutzerdefinierte Attribut steht jetzt in der Liste der Benutzerattribute zur Verfügung und kann in den Benutzerflows verwendet werden. Ein benutzerdefiniertes Attribut wird nur erstellt, wenn es zum ersten Mal in einem Benutzerflow verwendet wird, und nicht, wenn Sie es der Liste der Benutzerattribute hinzufügen.

Nach dem Erstellen eines neuen Benutzers über einen Benutzerflow, der das neu erstellte benutzerdefinierte Attribut verwendet, kann das Objekt im [Microsoft Graph-Explorer](https://developer.microsoft.com/graph/graph-explorer) abgefragt werden. **ShoeSize** sollte jetzt in der Liste der Attribute, die während des Registrierungsvorgangs für das Benutzerobjekt erfasst werden, angezeigt werden. Sie können aus Ihrer Anwendung die Graph-API aufrufen, um die Daten aus diesem Attribut abzurufen, nachdem es dem Benutzerobjekt hinzugefügt wurde.

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen eines Benutzerflows für die Self-Service-Registrierung zu einer App](self-service-sign-up-user-flow.md)