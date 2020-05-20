---
title: JavaScript-Beispiele
titleSuffix: Azure AD B2C
description: Hier finden Sie Informationen zur Verwendung von JavaScript in Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a26f6c5e69ca083335580a0368459e062de3941e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187660"
---
# <a name="javascript-samples-for-use-in-azure-active-directory-b2c"></a>JavaScript-Beispiele für die Verwendung in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Sie können Ihren Azure Active Directory B2C-Anwendungen (Azure AD B2C) Ihren eigenen clientseitigen JavaScript-Code hinzufügen.

So aktivieren Sie JavaScript für Ihre Anwendungen

* Hinzufügen eines Elements zu Ihrer [benutzerdefinierten Richtlinie](custom-policy-overview.md)
* Wählen Sie ein [Seitenlayout](page-layout.md) aus.
* Verwenden Sie [b2clogin.com](b2clogin.md) in Ihren Anforderungen.

In diesem Artikel wird beschrieben, wie Sie Ihre benutzerdefinierte Richtlinie ändern können, um die Skriptausführung zu aktivieren.

> [!NOTE]
> Wenn Sie JavaScript für Benutzerflows aktivieren möchten, finden Sie Informationen unter [JavaScript und Seitenlayoutversionen in Azure Active Directory B2C](user-flow-javascript-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

### <a name="select-a-page-layout"></a>Auswählen eines Seitenlayouts

* Wählen Sie ein [Seitenlayout](contentdefinitions.md#select-a-page-layout) für die Benutzeroberflächenelemente Ihrer Anwendung aus.

    Wenn Sie JavaScript verwenden möchten, müssen Sie eine [Seitenlayoutversion](contentdefinitions.md#migrating-to-page-layout) mit der `contract`-Version der Seite für *alle* Inhaltsdefinitionen in Ihrer benutzerdefinierten Richtlinie definieren.

## <a name="add-the-scriptexecution-element"></a>Hinzufügen des ScriptExecution-Elements

Sie aktivieren die Skriptausführung, indem Sie dem [RelyingParty](relyingparty.md)-Element das **ScriptExecution**-Element hinzufügen.

1. Öffnen Sie Ihre benutzerdefinierte Richtliniendatei. Beispiel: *SignUpOrSignin.xml*.
2. Fügen Sie das **ScriptExecution**-Element zum **UserJourneyBehaviors**-Element von **RelyingParty** hinzu:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="B2CSignUpOrSignInWithPassword" />
      <UserJourneyBehaviors>
        <ScriptExecution>Allow</ScriptExecution>
      </UserJourneyBehaviors>
      ...
    </RelyingParty>
    ```
3. Speichern Sie die Datei, und laden Sie sie hoch.

[!INCLUDE [active-directory-b2c-javascript-guidelines](../../includes/active-directory-b2c-javascript-guidelines.md)]

## <a name="javascript-samples"></a>JavaScript-Beispiele

### <a name="show-or-hide-a-password"></a>Anzeigen oder Ausblenden eines Kennworts

Eine gängige Methode zur Unterstützung Ihrer Kunden bei der erfolgreichen Registrierung besteht darin, zuzulassen, dass sie sehen können, was sie als Kennwort eingegeben haben. Diese Option unterstützt die Benutzer bei der Registrierung, indem die Benutzer auf einfache Weise ihr Kennwort sehen und bei Bedarf korrigieren können. Jedes Feld des Typs Kennwort verfügt über ein Kontrollkästchen mit der Bezeichnung **Kennwort anzeigen**.  Dadurch kann der Benutzer das Kennwort als Nur-Text anzeigen. Fügen Sie den folgenden Codeausschnitt in Ihre Registrierungs- oder Anmeldevorlage für eine selbstbestätigte Seite ein:

```Javascript
function makePwdToggler(pwd){
  // Create show-password checkbox
  var checkbox = document.createElement('input');
  checkbox.setAttribute('type', 'checkbox');
  var id = pwd.id + 'toggler';
  checkbox.setAttribute('id', id);

  var label = document.createElement('label');
  label.setAttribute('for', id);
  label.appendChild(document.createTextNode('show password'));

  var div = document.createElement('div');
  div.appendChild(checkbox);
  div.appendChild(label);

  // Add show-password checkbox under password input
  pwd.insertAdjacentElement('afterend', div);

  // Add toggle password callback
  function toggle(){
    if(pwd.type === 'password'){
      pwd.type = 'text';
    } else {
      pwd.type = 'password';
    }
  }
  checkbox.onclick = toggle;
  // For non-mouse usage
  checkbox.onkeydown = toggle;
}

function setupPwdTogglers(){
  var pwdInputs = document.querySelectorAll('input[type=password]');
  for (var i = 0; i < pwdInputs.length; i++) {
    makePwdToggler(pwdInputs[i]);
  }
}

setupPwdTogglers();
```

### <a name="add-terms-of-use"></a>Hinzufügen von Nutzungsbedingungen

Fügen Sie den folgenden Code in die Seite ein, auf der Sie ein Kontrollkästchen für **Nutzungsbedingungen** hinzufügen möchten. Dieses Kontrollkästchen ist in der Regel auf Ihren Seiten für die Registrierung über ein lokales Konto und die Registrierung über ein Konto eines sozialen Netzwerks erforderlich.

```Javascript
function addTermsOfUseLink() {
    // find the terms of use label element
    var termsOfUseLabel = document.querySelector('#api label[for="termsOfUse"]');
    if (!termsOfUseLabel) {
        return;
    }

    // get the label text
    var termsLabelText = termsOfUseLabel.innerHTML;

    // create a new <a> element with the same inner text
    var termsOfUseUrl = 'https://docs.microsoft.com/legal/termsofuse';
    var termsOfUseLink = document.createElement('a');
    termsOfUseLink.setAttribute('href', termsOfUseUrl);
    termsOfUseLink.setAttribute('target', '_blank');
    termsOfUseLink.appendChild(document.createTextNode(termsLabelText));

    // replace the label text with the new element
    termsOfUseLabel.replaceChild(termsOfUseLink, termsOfUseLabel.firstChild);
}
```

Ersetzen Sie im Code den Platzhalter `termsOfUseUrl` durch den Link zu Ihren Nutzungsbedingungen. Erstellen Sie für Ihr Verzeichnis ein neues Benutzerattribut namens **termsOfUse**, und fügen Sie dann **termsOfUse** als Benutzerattribut ein.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie die Benutzeroberfläche Ihrer Anwendungen anpassen können, finden Sie unter [Anpassen der Benutzeroberfläche einer Anwendung mithilfe einer benutzerdefinierten Richtlinie in Azure Active Directory B2C](custom-policy-ui-customization.md).
