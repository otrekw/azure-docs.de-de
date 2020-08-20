---
title: 'Tutorial: Integrieren von Lenses.io in das einmalige Anmelden (Single Sign-On, SSO) von Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Lenses.io konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/02/2020
ms.author: jeedes
ms.openlocfilehash: 0b8d736ab169ad07bd23a21d3a420bb6a044bf01
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88528637"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-the-lensesio-dataops-portal"></a>Tutorial: Integration des einmaligen Anmeldens (SSO) von Azure Active Directory in das Lenses.io-DataOps-Portal.

In diesem Tutorial erfahren Sie, wie Sie das [Lenses.io](https://lenses.io/)-DataOps-Portal in Azure Active Directory (Azure AD) integrieren. Die Integration von Lenses.io in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf das Lenses.io-Portal hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Lenses anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Eine Instanz eines Lenses-Portals. Es gibt [verschiedene Möglichkeiten](https://lenses.io/product/deployment/), um ein Lenses-Portal bereitzustellen.
* Eine Lenses.io-[Lizenz](https://lenses.io/product/pricing/), die einmaliges Anmelden (Single Sign-On, SSO) unterstützt.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Lenses.io unterstützt **SP-initiiertes** einmaliges Anmelden.

* Nach dem Konfigurieren von Lenses.io können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.

## <a name="adding-lensesio-from-the-gallery"></a>Hinzufügen von Lenses.io aus dem Katalog

Zum Konfigurieren der Integration von Lenses.io in Azure AD müssen Sie Ihrer Liste der verwalteten SaaS-Apps Lenses.io aus dem Katalog hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** im Suchfeld den Begriff **Lenses.io** ein.
1. Wählen Sie im Ergebnisbereich den Eintrag **Lenses.io** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-sso-for-lensesio"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Lenses.io

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Ihrem Lenses.io-Portal mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Lenses.io eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Lenses.io müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers und einer -Gruppe](#create-an-azure-ad-test-user-and-group)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Lenses.io](#configure-lensesio-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
    1. **[Erstellen von Berechtigungen für Lenses.io-Testgruppen](#create-lensesio-test-group-permissions)** , um zu steuern, worauf B. Simon in Lenses.io zugreifen soll (Autorisierung).
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Lenses.io** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<CUSTOMER_LENSES_BASE_URL>`, z. B. `https://lenses.my.company.com`

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein: `https://<CUSTOMER_LENSES_BASE_URL>`, z. B. `https://lenses.my.company.com`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<CUSTOMER_LENSES_BASE_URL>/api/v2/auth/saml/callback?client_name=SAML2Client`
    Beispiel: `https://lenses.my.company.com/api/v2/auth/saml/callback?client_name=SAML2Client`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte basierend auf der Basis-URL Ihrer Instanz des Lenses-Portals durch die tatsächlichen Werte für die Anmelde-URL, die Antwort-URL und den Bezeichner. Weitere Informationen finden Sie in der [Lenses.io-Dokumentation zu SSO](https://docs.lenses.io/install_setup/configuration/security.html#single-sign-on-sso-saml-2-0).

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Verwenden Sie im Abschnitt **Lenses.io einrichten** die obige XML-Datei, um Lenses für Azure SSO zu konfigurieren.

### <a name="create-an-azure-ad-test-user-and-group"></a>Erstellen eines Azure AD-Testbenutzers und einer zugehörigen Gruppe

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon. Sie erstellen auch eine Testgruppe für B. Simon, mit der gesteuert wird, worauf B. Simon in Lenses Zugriff hat.
Die [Lenses-Dokumentation zu SSO](https://docs.lenses.io/install_setup/configuration/security.html#id3) enthält Informationen dazu, wie in Lenses die Zuordnung der Gruppenmitgliedschaft für die Autorisierung genutzt wird.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

Erstellen Sie die Gruppe wie folgt:
1. Navigieren Sie zurück zu **Azure Active Directory**, und wählen Sie die Option **Gruppen** aus.
1. Wählen Sie am oberen Bildschirmrand die Option **Neue Gruppe** aus.
1. Führen Sie unter **Gruppeneigenschaften** die folgenden Schritte aus:
   1. Wählen Sie im Feld **Gruppentyp** die Option `Security` aus.
   1. Geben Sie im Feld **Gruppenname** den Namen `LensesUsers` ein.
   1. Klicken Sie auf **Erstellen**.
1. Wählen Sie die Gruppe `LensesUsers` aus, und notieren Sie sich die **Objekt-ID** (z. B. `f8b5c1ec-45de-4abd-af5c-e874091fb5f7`). Diese ID wird in Lenses verwendet, um Benutzer dieser Gruppe den [richtigen Berechtigungen](https://docs.lenses.io/install_setup/configuration/security.html#id3) zuzuordnen.  
   
Weisen Sie die Gruppe wie folgt dem Testbenutzer zu: 
1. Navigieren Sie zurück zu **Azure Active Directory**, und wählen Sie die Option **Benutzer** aus.
1. Wählen Sie den Testbenutzer `B.Simon` aus.
1. Wählen Sie **Gruppen** aus.
1. Wählen Sie am oberen Bildschirmrand die Option **Mitgliedschaften hinzufügen** aus.
1. Suchen Sie nach `LensesUsers`, und wählen Sie diese Option aus.
1. Klicken Sie auf **Auswählen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Lenses.io gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste die Anwendung **Lenses.io** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-lensesio-sso"></a>Konfigurieren des einmaligen Anmeldens für Lenses.io

Installieren Sie zum Konfigurieren des einmaligen Anmeldens im **Lenses.io**-Portal die heruntergeladene **Verbundmetadaten-XML**-Datei auf Ihrer Lenses-Instanz, und [konfigurieren Sie Lenses für die Aktivierung von SSO](https://docs.lenses.io/install_setup/configuration/security.html#configure-lenses). 

### <a name="create-lensesio-test-group-permissions"></a>Erstellen von Berechtigungen für die Lenses.io-Testgruppe

In diesem Abschnitt erstellen Sie eine Gruppe in Lenses, indem Sie die **Objekt-ID** der Gruppe `LensesUsers` verwenden, die wir im [Abschnitt für die Benutzererstellung](#create-an-azure-ad-test-user-and-group) notiert haben.
Sie weisen die gewünschten Berechtigungen zu, über die `B.Simon` in Lenses verfügen soll.
Weitere Informationen finden Sie im Abschnitt zum Thema [Azure: Lenses-Gruppenzuordnung](https://docs.lenses.io/install_setup/configuration/security.html#azure-groups).

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Lenses.io“ klicken, sollten Sie automatisch bei Ihrem Lenses.io-Portal angemeldet werden, für das Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einrichten von SSO auf Ihrer Lenses.io-Instanz](https://docs.lenses.io/install_setup/configuration/security.html#single-sign-on-sso-saml-2-0)

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Lenses.io mit Azure AD ausprobieren](https://aad.portal.azure.com/)

- [Was ist Sitzungssteuerung in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Schützen von Apps mit der App-Steuerung für bedingten Zugriff von Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
