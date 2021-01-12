---
title: Einrichten eines Profilbearbeitungsflows
titleSuffix: Azure AD B2C
description: Hier erfahren Sie, wie Sie in Azure Active Directory B2C einen Profilbearbeitungsflow einrichten.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/16/2020
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 1cf66918d8b5211dcc03be944dbcb92cdf6e0773
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618610"
---
# <a name="set-up-a-profile-editing-flow-in-azure-active-directory-b2c"></a>Einrichten eines Profilbearbeitungsflows in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

## <a name="profile-editing-flow"></a>Profilbearbeitungsflow

Mithilfe der Richtlinie für die Profilbearbeitung können Benutzer ihre Profilattribute wie Anzeigename, Nachname, Vorname, Stadt und andere Informationen verwalten. Der Profilbearbeitungsflow umfasst die folgenden Schritte: 

1. Registrieren Sie sich, oder melden Sie sich mit einem lokalen Konto oder einem Social-Media-Konto an. Wenn die Sitzung noch aktiv ist, autorisiert Azure AD B2C den Benutzer und fährt mit dem nächsten Schritt fort.
1. Azure AD B2C liest das Benutzerprofil aus dem Verzeichnis und ermöglicht dem Benutzer das Bearbeiten der Attribute.

![Profilbearbeitungsflow](./media/add-profile-editing-policy/profile-editing-flow.png)


## <a name="prerequisites"></a>Voraussetzungen

Wenn dies noch nicht erfolgt ist, [registrieren Sie eine Webanwendung in Azure Active Directory B2C](tutorial-register-applications.md).

::: zone pivot="b2c-user-flow"

## <a name="create-a-profile-editing-user-flow"></a>Erstellen eines Benutzerflows für die Profilbearbeitung

Wenn Sie Benutzern die Profilbearbeitung in Ihrer Anwendung ermöglichen möchten, verwenden Sie einen Benutzerflow für die Profilbearbeitung.

1. Wählen Sie auf der Übersichtsseite des Azure AD B2C-Mandanten im Menü die Option **Benutzerflows** und dann **Neuer Benutzerflow** aus.
1. Wählen Sie auf der Seite **Benutzerflow erstellen** den Benutzerflow **Profilbearbeitung** aus. 
1. Wählen Sie unter **Version auswählen** die Option **Empfohlen** und dann **Erstellen** aus.
1. Geben Sie unter **Name** einen Namen für den Benutzerflow ein. Beispiel: *profileediting1*.
1. Klicken Sie unter **Identitätsanbieter** auf **Email sign-in** (E-Mail-Anmeldung).
1. Wählen Sie für **Benutzerattribute** die Attribute aus, die der Kunde in seinem Profil bearbeiten darf. Wählen Sie z.B. **Mehr anzeigen** und dann sowohl Attribute als auch Ansprüche für **Anzeigename** und **Position** aus. Klicken Sie auf **OK**.
1. Klicken Sie auf **Erstellen**, um den Benutzerflow hinzuzufügen. Dem Namen wird automatisch das Präfix *B2C_1* vorangestellt.

### <a name="test-the-user-flow"></a>Testen des Benutzerflows

1. Wählen Sie den von Ihnen erstellten Benutzerflow aus, um die entsprechende Übersichtsseite zu öffnen, und wählen Sie dann **Benutzerflow ausführen** aus.
1. Wählen Sie für **Anwendung** die Webanwendung *webapp1* aus, die Sie zuvor registriert haben. Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Klicken Sie auf **Benutzerflow ausführen**, und melden Sie sich mit dem zuvor erstellten Konto an.
1. Sie haben jetzt die Möglichkeit, den Anzeigenamen und den Stellentitel für den Benutzer zu ändern. Klicken Sie auf **Weiter**. Das Token wird an `https://jwt.ms` zurückgegeben und sollte Ihnen angezeigt werden.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-profile-editing-policy"></a>Erstellen einer Richtlinie für die Profilbearbeitung

Benutzerdefinierte Richtlinien bestehen aus mehreren XML-Dateien, die Sie in den Azure AD B2C-Mandanten hochladen, um User Journeys zu definieren. Es sind Starter Packs mit mehreren integrierten Richtlinien verfügbar, z. B. für die Registrierung und Anmeldung, für die Kennwortzurücksetzung und für die Profilbearbeitung. Weitere Informationen finden Sie unter [Erste Schritte für benutzerdefinierte Richtlinien in Azure Active Directory B2C](custom-policy-get-started.md).

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

* [Hinzufügen eines Identitätsanbieters zu Ihrem Azure Active Directory B2C-Mandanten](add-identity-provider.md)