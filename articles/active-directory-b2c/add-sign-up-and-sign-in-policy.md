---
title: Einrichten eines Registrierungs- und Anmeldeflows
titleSuffix: Azure AD B2C
description: Hier erfahren Sie, wie Sie einen Registrierungs- und Anmeldeflow in Azure Active Directory B2C einrichten.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/16/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: b2c-support
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: fbcdef0002a227d5319fc01e625146480e4c99fc
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105043749"
---
# <a name="set-up-a-sign-up-and-sign-in-flow-in-azure-active-directory-b2c"></a>Einrichten eines Registrierungs- und Anmeldeflows in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

## <a name="sign-up-and-sign-in-flow"></a>Registrierungs- und Anmeldeflow

Eine Registrierungs- und Anmelderichtlinie ermöglicht Benutzern Folgendes: 

* Registrieren mit einem lokalen Konto
* Anmelden mit einem lokalen Konto
* Registrieren oder Anmelden mit einem Social Media-Konto
* Zurücksetzen von Kennwörtern

![Profilbearbeitungsflow](./media/add-sign-up-and-sign-in-policy/add-sign-up-and-sign-in-flow.png)

## <a name="prerequisites"></a>Voraussetzungen

Wenn dies noch nicht erfolgt ist, [registrieren Sie eine Webanwendung in Azure Active Directory B2C](tutorial-register-applications.md).

::: zone pivot="b2c-user-flow"

## <a name="create-a-sign-up-and-sign-in-user-flow"></a>Erstellen eines Benutzerflows für Registrierung und Anmeldung

Der Benutzerflow für Registrierung und Anmeldung verarbeitet die Benutzeroberflächen für die Registrierung und Anmeldung in ein und derselben Konfiguration. Die Benutzer Ihrer Anwendung werden je nach Kontext auf den entsprechenden Pfad geleitet.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie unter **Richtlinien** die Option **Benutzerflows** und dann **Neuer Benutzerflow** aus.

    ![Seite „Benutzerflows“ im Portal mit hervorgehobener Schaltfläche „Neuer Benutzerflow“](./media/add-sign-up-and-sign-in-policy/signup-signin-user-flow.png)

1. Wählen Sie auf der Seite **Benutzerflow erstellen** den Benutzerflow **Registrierung und Anmeldung** aus.

    ![Seite „Benutzerflow auswählen“ mit hervorgehobenem „Anmeldung“ und „Anmeldeflow“](./media/add-sign-up-and-sign-in-policy/select-user-flow-type.png)

1. Wählen Sie unter **Version auswählen** die Option **Empfohlen** und dann **Erstellen** aus. (Weitere Informationen zu [Benutzerflowversionen](user-flow-versions.md))

    ![Seite „Benutzerflow erstellen“ im Azure-Portal mit hervorgehobenen Eigenschaften](./media/add-sign-up-and-sign-in-policy/select-version.png)

1. Geben Sie unter **Name** einen Namen für den Benutzerflow ein. Beispiel: *signupsignin1*.
1. Klicken Sie unter **Identitätsanbieter** auf **E-Mail-Registrierung**.
1. Wählen Sie für **Benutzerattribute und Ansprüche** die Ansprüche und Attribute aus, die Sie bei der Registrierung vom Benutzer sammeln und senden möchten. Wählen Sie z.B. **Mehr anzeigen** und dann Attribute und Ansprüche für **Land/Region**, **Anzeigename** und **Postleitzahl** aus. Klicken Sie auf **OK**.

    ![Auswahlseite für Attribute und Ansprüche mit drei ausgewählten Ansprüchen](./media/add-sign-up-and-sign-in-policy/signup-signin-attributes.png)

1. Klicken Sie auf **Erstellen**, um den Benutzerflow hinzuzufügen. Dem Namen wird automatisch das Präfix *B2C_1* vorangestellt.
2. Führen Sie die Schritte für die [Verarbeitung des Ablaufs für "Kennwort vergessen?"](add-password-reset-policy.md?pivots=b2c-user-flow.md#self-service-password-reset-recommended) in der Registrierungs- oder Anmelderichtlinie aus.

### <a name="test-the-user-flow"></a>Testen des Benutzerflows

1. Wählen Sie den von Ihnen erstellten Benutzerflow aus, um die entsprechende Übersichtsseite zu öffnen, und wählen Sie dann **Benutzerflow ausführen** aus.
1. Wählen Sie für **Anwendung** die Webanwendung *webapp1* aus, die Sie zuvor registriert haben. Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Klicken Sie auf **Benutzerflow ausführen**, und wählen Sie dann **Jetzt registrieren** aus.

    ![Seite „Benutzerflow ausführen“ im Portal mit hervorgehobener Schaltfläche „Benutzerflow ausführen“](./media/add-sign-up-and-sign-in-policy/signup-signin-run-now.png)

1. Geben Sie eine gültige E-Mail-Adresse ein, klicken Sie auf **Überprüfungscode senden**, geben Sie den Überprüfungscode ein, und wählen Sie dann **Code überprüfen** aus.
1. Geben Sie ein neues Kennwort ein, und bestätigen Sie es.
1. Wählen Sie das Land und die Region aus, geben Sie den anzuzeigenden Namen und eine Postleitzahl ein, und klicken Sie dann auf **Erstellen**. Das Token wird an `https://jwt.ms` zurückgegeben und sollte Ihnen angezeigt werden.
1. Sie können den Benutzerflow jetzt erneut ausführen. Sie sollten in der Lage sein, sich mit dem Konto, das Sie erstellt haben, anzumelden. Das zurückgegebene Token enthält die Ansprüche, die Sie für Land/Region, Name und Postleitzahl ausgewählt haben.

> [!NOTE]
> Die Benutzeroberfläche für „Benutzerflow ausführen“ ist derzeit nicht mit dem SPA-URL-Antworttyp kompatibel, für den der Autorisierungscodeflow verwendet wird. Wenn Sie die Benutzeroberfläche für „Benutzerflow ausführen“ mit dieser Art von App nutzen möchten, müssen Sie eine Antwort-URL vom Typ „Web“ registrieren und den impliziten Flow wie [hier](tutorial-register-spa.md) beschrieben aktivieren.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-sign-up-and-sign-in-policy"></a>Erstellen einer Registrierungs- und Anmelderichtlinie

Benutzerdefinierte Richtlinien bestehen aus mehreren XML-Dateien, die Sie in den Azure AD B2C-Mandanten hochladen, um User Journeys zu definieren. Es sind Starter Packs mit mehreren integrierten Richtlinien verfügbar, z. B. für die Registrierung und Anmeldung, für die Kennwortzurücksetzung und für die Profilbearbeitung. Weitere Informationen finden Sie unter [Erste Schritte für benutzerdefinierte Richtlinien in Azure Active Directory B2C](custom-policy-get-started.md).

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

* [Hinzufügen eines Identitätsanbieters zu Ihrem Azure Active Directory B2C-Mandanten](add-identity-provider.md)
* [Einrichten eines Kennwortzurücksetzungsflows in Azure Active Directory B2C](add-password-reset-policy.md)
