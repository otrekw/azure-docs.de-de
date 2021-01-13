---
title: Einrichten eines Kennwortzurücksetzungsflows
titleSuffix: Azure AD B2C
description: Hier erfahren Sie, wie Sie einen Kennwortzurücksetzungsflow in Azure Active Directory B2C einrichten.
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
ms.openlocfilehash: 6e6e4aa4ece781f415308b638323f8d4d4b34038
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618612"
---
# <a name="set-up-a-password-reset-flow-in-azure-active-directory-b2c"></a>Einrichten eines Kennwortzurücksetzungsflows in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

## <a name="password-rest-flow"></a>Flow zur Kennwortzurücksetzung

Die Richtlinie für die Kennwortzurücksetzung ermöglicht es Benutzern, ihr eigenes vergessenes Kennwort zurückzusetzen. Der Ablauf der Kennwortzurücksetzung umfasst die folgenden Schritte: 
1. Sie müssen auf der Registrierungs- und Anmeldeseite auf den Link „Forgot your password?“ (Kennwort vergessen?) klicken. Azure AD B2C gibt den Fehlercode AADB2C90118 an Ihre App zurück. Die App verarbeitet diesen Fehlercode durch Aufrufen der Richtlinie zur Kennwortzurücksetzung. 
1. Sie müssen Ihre E-Mail-Adresse mit einer zeitgesteuerten Einmalkennung verifizieren.
1. Geben Sie ein neues Kennwort ein.

![Kennwortzurücksetzungsflow](./media/add-password-reset-policy/password-reset-flow.png)

## <a name="prerequisites"></a>Voraussetzungen

Wenn dies noch nicht erfolgt ist, [registrieren Sie eine Webanwendung in Azure Active Directory B2C](tutorial-register-applications.md).

::: zone pivot="b2c-user-flow"

## <a name="create-a-password-reset-user-flow"></a>Erstellen eines Benutzerflows zur Kennwortrücksetzung

Damit Benutzer Ihrer Anwendung die eigenen Kennwörter zurücksetzten können, verwenden Sie einen Benutzerflow für die Kennwortzurücksetzung.

1. Wählen Sie im Übersichtsmenü des Azure AD B2C-Mandanten die Option **Benutzerflows** und dann **Neuer Benutzerflow** aus.
1. Wählen Sie auf der Seite **Benutzerflow erstellen** den Benutzerflow **Kennwortzurücksetzung** aus. 
1. Wählen Sie unter **Version auswählen** die Option **Empfohlen** und dann **Erstellen** aus.
1. Geben Sie unter **Name** einen Namen für den Benutzerflow ein. Beispiel *passwordreset1*.
1. Aktivieren Sie unter **Identitätsanbieter** die Option **Kennwort mittels E-Mail-Adresse zurücksetzen**.
2. Klicken Sie unter „Anwendungsansprüche“ auf **Mehr anzeigen**, und wählen Sie die Ansprüche aus, die in dem an die Anwendung gesendeten Autorisierungstoken zurückgegeben werden sollen. Wählen Sie beispielsweise **Objekt-ID des Benutzers**.
3. Klicken Sie auf **OK**.
4. Klicken Sie auf **Erstellen**, um den Benutzerflow hinzuzufügen. Dem Namen wird automatisch das Präfix *B2C_1* vorangestellt.

### <a name="test-the-user-flow"></a>Testen des Benutzerflows

1. Wählen Sie den von Ihnen erstellten Benutzerflow aus, um die entsprechende Übersichtsseite zu öffnen, und wählen Sie dann **Benutzerflow ausführen** aus.
1. Wählen Sie für **Anwendung** die Webanwendung *webapp1* aus, die Sie zuvor registriert haben. Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Klicken Sie auf **Benutzerflow ausführen**, überprüfen Sie die E-Mail-Adresse des Kontos, das Sie zuvor erstellt haben, und klicken Sie auf **Weiter**.
1. Sie haben nun die Möglichkeit, das Kennwort des Benutzers zu ändern. Ändern Sie das Kennwort, und klicken Sie auf **Weiter**. Das Token wird an `https://jwt.ms` zurückgegeben und sollte Ihnen angezeigt werden.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-password-reset-policy"></a>Erstellen einer Richtlinie zur Kennwortrücksetzung

Benutzerdefinierte Richtlinien bestehen aus mehreren XML-Dateien, die Sie in den Azure AD B2C-Mandanten hochladen, um User Journeys zu definieren. Es sind Starter Packs mit mehreren integrierten Richtlinien verfügbar, z. B. für die Registrierung und Anmeldung, für die Kennwortzurücksetzung und für die Profilbearbeitung. Weitere Informationen finden Sie unter [Erste Schritte für benutzerdefinierte Richtlinien in Azure Active Directory B2C](custom-policy-get-started.md).

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

Richten Sie einen [Flow zur Profilbearbeitung](add-profile-editing-policy.md) ein.