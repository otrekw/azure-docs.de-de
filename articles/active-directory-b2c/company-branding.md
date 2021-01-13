---
title: Hinzufügen von Branding zur Anmeldeseite Ihrer Organisation
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie den Azure Active Directory B2C-Seiten das Branding Ihrer Organisation hinzufügen.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/10/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: bee81876b066b9fc1ea69c418ee4d0839db27b3c
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "97111050"
---
# <a name="add-branding-to-your-organizations-azure-active-directory-b2c-pages"></a>Hinzufügen von Branding zu den Azure Active Directory B2C-Seiten Ihrer Organisation

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

In Azure Active Directory können Sie mithilfe des Features [Unternehmensbranding](../active-directory/fundamentals/customize-branding.md) Ihre Azure AD B2C-Seiten mit einem Bannerlogo, einem Hintergrundbild und einer Hintergrundfarbe anpassen. Dieses Unternehmensbranding umfasst die Funktionen zum Registrieren, Anmelden und Bearbeiten von Profilen und zum Zurücksetzen von Kennwörtern. 

> [!TIP]
> Informationen zum Anpassen anderer Aspekte Ihrer Benutzerflowseiten über das Bannerlogo, das Hintergrundbild oder die Hintergrundfarbe hinaus finden Sie unter [Anpassen der Benutzeroberfläche mit einer HTML-Vorlage](customize-ui-with-html.md).

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]


Zum Anpassen Ihrer Benutzerflowseiten konfigurieren Sie zuerst das Unternehmensbranding in Azure Active Directory, und dann aktivieren Sie es in den Seitenlayouts Ihrer Benutzerflows in Azure AD B2C.

## <a name="configure-company-branding"></a>Konfigurieren des Unternehmensbrandings

Legen Sie zuerst das Bannerlogo, das Hintergrundbild und die Hintergrundfarbe in **Unternehmensbranding** fest.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie unter **Verwalten** die Option **Unternehmensbranding** aus.
1. Führen Sie die unter [Hinzufügen von Branding zur Azure Active Directory-Anmeldeseite Ihrer Organisation](../active-directory/fundamentals/customize-branding.md) aufgeführten Schritte aus.

Beachten Sie beim Konfigurieren des Unternehmensbrandings in Azure AD B2C die folgenden Punkte:

* Das Unternehmensbranding in Azure AD B2C ist derzeit auf die Anpassung des **Hintergrundbilds**, des **Bannerlogos** und der **Hintergrundfarbe** beschränkt. Die anderen Eigenschaften im Bereich „Unternehmensbranding“ (z. B. die unter **Erweiterte Einstellungen**) werden *nicht unterstützt*.
* Auf Ihren Benutzerflowseiten wird die Hintergrundfarbe angezeigt, bevor das Hintergrundbild geladen wird. Es wird empfohlen, eine Hintergrundfarbe auszuwählen, die weitestgehend den Farben in Ihrem Hintergrundbild entspricht, um ein reibungsloseres Ladeerlebnis zu ermöglichen.
* Das Bannerlogo wird in den Überprüfungs-E-Mails angezeigt, die an Ihre Benutzer gesendet werden, wenn sie einen Benutzerflow für die Registrierung initiieren.

::: zone pivot="b2c-user-flow"

## <a name="enable-branding-in-user-flow-pages"></a>Aktivieren von Branding auf Benutzerflowseiten

Nachdem Sie das Unternehmensbranding konfiguriert haben, können Sie es in Ihren Benutzerflows aktivieren.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Azure AD B2C** aus.
1. Wählen Sie unter **Richtlinien** die Option **Benutzerflows (Richtlinien)** aus.
1. Wählen Sie den Benutzerflow aus, für den Sie das Unternehmensbranding aktivieren möchten. Für die Standard-Benutzerflowtypen *Anmeldung* und *Profilbearbeitung* wird das Unternehmensbranding **nicht unterstützt**.
1. Wählen Sie unter **Anpassen** die Option **Seitenlayouts** aus, und wählen Sie dann das Layout aus, das Sie mit Branding versehen möchten. Wählen Sie z. B. **Einheitliche Seite für Registrierung oder Anmeldung** aus.
1. Wählen Sie für **Seitenlayoutversion (Vorschau)** die Version **1.2.0** oder eine höhere Version aus.
1. Wählen Sie **Speichern** aus.

Wenn Sie alle Seiten im Benutzerflow mit Branding versehen möchten, legen Sie die Seitenlayoutversion für jedes Seitenlayout im Benutzerflow fest.

![Seitenlayoutauswahl in Azure AD B2C im Azure-Portal](media/company-branding/portal-02-page-layout-select.png)

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="select-a-page-layout"></a>Auswählen eines Seitenlayouts

Wenn Sie Unternehmensbranding verwenden möchten, müssen Sie eine [Seitenlayoutversion](contentdefinitions.md#migrating-to-page-layout) mit der `contract`-Version der Seite für *alle* Inhaltsdefinitionen in Ihrer benutzerdefinierten Richtlinie definieren. Das Format des Werts muss das Wort `contract` enthalten: _urn:com:microsoft:aad:b2c:elements:**contract**:page-name:version_. Sie können das Seitenlayout in Ihren benutzerdefinierten Richtlinien angeben, die einen alten **DataUri**-Wert verwenden.

Erfahren Sie, wie Sie mit der Seitenversion eine [Migration zum Seitenlayout](contentdefinitions.md#migrating-to-page-layout) durchführen.

Im folgenden Beispiel werden die Inhaltsdefinitions-IDs mit dem zugehörigen **DataUri** für den Seitenvertrag gezeigt: 

```xml
<ContentDefinitions>
  <ContentDefinition Id="api.error">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections.signup">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.signuporsignin">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted.profileupdate">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountsignup">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountpasswordreset">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.phonefactor">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.2.0</DataUri>
  </ContentDefinition>
</ContentDefinitions>
```

::: zone-end

Im folgenden Beispiel sehen Sie ein benutzerdefiniertes Bannerlogo und Hintergrundbild auf einer Benutzerflowseite für die *Registrierung und Anmeldung*, für die die Vorlage „Ozeanblau“ verwendet wird:

![Von Azure AD B2C bereitgestellte Registrierungs-/Anmeldeseite mit Branding](media/company-branding/template-ocean-blue-branded.png)

## <a name="use-company-branding-assets-in-custom-html"></a>Verwenden von Unternehmensbrandingobjekten in benutzerdefiniertem HTML

Wenn Sie Ihre Unternehmensbrandingobjekte in [benutzerdefiniertem HTML-Code](customize-ui-with-html.md) verwenden möchten, fügen Sie die folgenden Tags außerhalb des Tags `<div id="api">` hinzu:

```HTML
<img data-tenant-branding-background="true" />
<img data-tenant-branding-logo="true" alt="Company Logo" />
```

Die Bildquelle wird durch die des Hintergrundbilds und Bannerlogos ersetzt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie die Benutzeroberfläche Ihrer Anwendungen anpassen können, finden Sie unter [Anpassen der Benutzeroberfläche einer Anwendung in Azure Active Directory B2C](customize-ui-with-html.md).