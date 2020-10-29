---
title: 'Tutorial: Azure Active Directory-Integration mit Keeper Password Manager & Digital Vault | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Keeper Password Manager & Digital Vault konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/07/2020
ms.author: jeedes
ms.openlocfilehash: 05b81457af8e74f95bdd3af940fa2b39ba93504a
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92459140"
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a>Tutorial: Azure Active Directory-Integration mit Keeper Password Manager & Digital Vault

In diesem Tutorial erfahren Sie, wie Sie Keeper Password Manager & Digital Vault in Azure Active Directory (Azure AD) integrieren.
Die Integration von Keeper Password Manager & Digital Vault in Azure AD bietet Ihnen folgende Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Keeper Password Manager & Digital Vault hat.
* Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Keeper Password Manager & Digital Vault anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Keeper Password Manager & Digital Vault konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Ein Keeper Password Manager & Digital Vault-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Keeper Password Manager & Digital Vault unterstützt **SP** -initiiertes einmaliges Anmelden.

* Keeper Password Manager & Digital Vault unterstützt die **Just-in-Time** -Benutzerbereitstellung.

* Nach dem Konfigurieren von Keeper Password Manager & Digital Vault können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](/cloud-app-security/proxy-deployment-aad) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.

## <a name="adding-keeper-password-manager--digital-vault-from-the-gallery"></a>Hinzufügen von Keeper Password Manager & Digital Vault aus dem Katalog

Zum Konfigurieren der Integration von Keeper Password Manager & Digital Vault in Azure AD müssen Sie Keeper Password Manager & Digital Vault aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen** , und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Keeper Password Manager & Digital Vault** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Keeper Password Manager & Digital Vault** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-keeper-password-manager--digital-vault"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Keeper Password Manager & Digital Vault

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Keeper Password Manager & Digital Vault mithilfe eines Testbenutzers mit dem Namen **B. Simon** . Damit einmaliges Anmelden funktioniert, muss zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Keeper Password Manager & Digital Vault eine Linkbeziehung eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Keeper Password Manager & Digital Vault die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.

    * **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
    * **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.

1. **[Konfigurieren des einmaligen Anmeldens für Keeper Password Manager & Digital Vault](#configure-keeper-password-manager--digital-vault-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    * **[Erstellen eines Keeper Password Manager & Digital Vault-Testbenutzers](#create-keeper-password-manager--digital-vault-test-user)** , um eine Entsprechung von Britta Simon in Keeper Password Manager & Digital Vault zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Keeper Password Manager & Digital Vault** zum Abschnitt **Verwalten** , und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration** , um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: .
    * Für **Cloud-SSO** : `https://keepersecurity.com/api/rest/sso/saml/sso/<CLOUD_INSTANCE_ID>`
    * Für **lokales SSO** : `https://<KEEPER_FQDN>/sso-connect/saml/login`

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein:
    * Für **Cloud-SSO** : `https://keepersecurity.com/api/rest/sso/saml/<CLOUD_INSTANCE_ID>`
    * Für **lokales SSO** : `https://<KEEPER_FQDN>/sso-connect`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: 
    * Für **Cloud-SSO** : `https://keepersecurity.com/api/rest/sso/saml/sso/<CLOUD_INSTANCE_ID>`
    * Für **lokales SSO** : `https://<KEEPER_FQDN>/sso-connect/saml/sso`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächlichen Werte für die Anmelde-URL, den Bezeichner und die Antwort-URL. Wenden Sie sich an das [Clientsupportteam von Keeper Password Manager & Digital Vault](https://keepersecurity.com/contact.html), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen** , um den Ihren Anforderungen entsprechenden **Verbundmetadaten-XML** -Code aus den verfügbaren Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

6. Kopieren Sie im Abschnitt **Keeper Password Manager & Digital Vault einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers 

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen** , und notieren Sie sich den Wert aus dem Feld **Kennwort** .
   1. Klicken Sie auf **Erstellen** .

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Keeper Password Manager & Digital Vault gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen**  > **Alle Anwendungen** aus.
1. Wählen Sie in der Liste der Anwendungen **Keeper Password Manager & Digital Vault** .
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten** , und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen** .
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen** .
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen** .


## <a name="configure-keeper-password-manager--digital-vault-sso"></a>Konfigurieren des einmaligen Anmeldens für Keeper Password Manager & Digital Vault

Um einmaliges Anmelden auf der Seite **Keeper Password Manager & Digital Vault-Konfiguration** zu konfigurieren, befolgen Sie die Richtlinien im [Keeper Support Guide](https://docs.keeper.io/sso-connect-guide/).

### <a name="create-keeper-password-manager--digital-vault-test-user"></a>Erstellen eines Keeper Password Manager & Digital Vault-Testbenutzers

Damit sich Azure AD-Benutzer bei Keeper Password Manager & Digital Vault anmelden können, müssen sie in Keeper Password Manager & Digital Vault bereitgestellt werden. Die Anwendung unterstützt die Just-in-Time-Benutzerbereitstellung. Nach der Authentifizierung werden in der Anwendung automatisch Benutzer erstellt. Sie können sich an den [Keeper Support](https://keepersecurity.com/contact.html) wenden, wenn Sie Benutzer manuell einrichten möchten.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Keeper Password Manager & Digital Vault“ klicken, sollten Sie automatisch bei der Keeper Password Manager & Digital Vault-Anwendung angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](./tutorial-list.md)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Was ist bedingter Zugriff?](../conditional-access/overview.md)

- [Keeper Password Manager & Digital Vault mit Azure AD ausprobieren](https://aad.portal.azure.com/)

- [Was ist Sitzungssteuerung in Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)