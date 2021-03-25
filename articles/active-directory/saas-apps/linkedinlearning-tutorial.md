---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory-Integration mit LinkedIn Learning | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LinkedIn Learning konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/28/2020
ms.author: jeedes
ms.openlocfilehash: e5c6bf41e1a3bf92c9141c0d3b54dd58ead2bf3c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98727299"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-linkedin-learning"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory-Integration mit LinkedIn Learning

In diesem Tutorial erfahren Sie, wie Sie LinkedIn Learning in Azure Active Directory (Azure AD) integrieren. Die Integration von LinkedIn Learning in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf LinkedIn Learning hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei LinkedIn Learning anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein LinkedIn Learning-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* LinkedIn Learning unterstützt durch **SP und IDP** initiiertes einmaliges Anmelden.
* LinkedIn Learning unterstützt die **Just-in-Time**-Benutzerbereitstellung.


## <a name="adding-linkedin-learning-from-the-gallery"></a>Hinzufügen von LinkedIn Learning aus dem Katalog

Zum Konfigurieren der Integration von LinkedIn Learning in Azure AD müssen Sie LinkedIn Learning aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **LinkedIn Learning** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **LinkedIn Learning** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-sso-for-linkedin-learning"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für LinkedIn Learning

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit LinkedIn Learning mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LinkedIn Learning eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit LinkedIn Learning die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für LinkedIn Learning](#configure-linkedin-learning-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines LinkedIn Learning-Testbenutzers](#create-linkedin-learning-test-user)** , um in LinkedIn Learning eine Entsprechung von B. Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **LinkedIn Learning** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

     a. Geben Sie im Textfeld **Bezeichner** die **Entitäts-ID** ein, die Sie aus dem LinkedIn-Portal kopiert haben. 

    b. Geben Sie in das Textfeld **Antwort-URL** den Wert für **Assertion Consumer Service (ACS)-URL** ein, den Sie aus dem LinkedIn-Portal kopiert haben.

    c. Wenn Sie die Anwendung im **SP-initiierten** Modus konfigurieren möchten, klicken Sie im Abschnitt **Grundlegende SAML-Konfiguration**, in dem Sie Ihre Anmelde-URL angeben, auf **Zusätzliche URLs festlegen**. Um Ihre Anmelde-URL zu erstellen, kopieren Sie die **Assertion Consumer Service (ACS)-URL**, und ersetzen dabei „/saml/“ durch „/login/“. Danach sollte die Anmelde-URL folgendes Muster aufweisen:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![SSO-Informationen zur Domäne und zu den URLs für LinkedIn Learning](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Diese Werte müssen durch die tatsächlichen Werte für Bezeichner und Antwort-URL ersetzt werden. Entsprechende Informationen finden Sie im Abschnitt **Konfigurieren des einmaligen Anmeldens für LinkedIn Learning** dieses Tutorials.

1. Die LinkedIn Learning-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. **nameidentifier** ist hier **user.userprincipalname** zugeordnet. Die LinkedIn Learning-Anwendung erwartet, dass **nameidentifier** der Wert **user.mail** zugeordnet ist. Sie müssen die Attributzuordnung daher entsprechend ändern, indem Sie auf das Symbol **Bearbeiten** klicken.

    ![image](common/edit-attribute.png)

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **LinkedIn Learning einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf LinkedIn Learning gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **LinkedIn Learning** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-linkedin-learning-sso"></a>Konfigurieren des einmaligen Anmeldens für LinkedIn Learning

1. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrem LinkedIn Learning-Mandanten als Administrator an.

2. Klicken Sie im **Kontocenter** unter **Einstellungen** auf **Globale Einstellungen**. Wählen Sie außerdem aus der Dropdownliste die Option **Learning - Default** aus.

    ![Screenshot der globalen Einstellungen, in denen Sie die Option „Default“ (Standard) auswählen können](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

3. Klicken Sie auf **ODER klicken Sie hier, um einzelne Felder aus dem Formular zu laden und zu kopieren**, kopieren Sie die **Entitäts-ID** und die **Assertion Consumer Service (ACS)-URL**, und fügen Sie sie im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** ein.

    ![Screenshot: „Single Sign-On“ (Einmaliges Anmelden), wo Sie die beschriebenen Werte eingeben können](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

4. Wechseln Sie zum Abschnitt **LinkedIn-Administratoreinstellungen**. Laden Sie die XML-Datei hoch, die Sie gerade aus dem Azure-Portal heruntergeladen haben, indem Sie auf die Option **XML-Datei hochladen** klicken.

    ![Screenshot: Abschnitt „Configure the LinkedIn service provider SSO settings“ (SSO-Einstellungen des LinkedIn-Dienstanbieters konfigurieren), in dem Sie eine XML-Datei hochladen können](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

5. Klicken Sie auf **Ein**, um das einmalige Anmelden zu aktivieren. Der Status des einmaligen Anmeldens wechselt von **Nicht verbunden** zu **Verbunden**.

    ![Screenshot: „Single Sign-On“ (Einmaliges Anmelden), wo Sie die Option „Authenticate users with SSO“ (Benutzer mit SSO authentifizieren) auswählen können](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-linkedin-learning-test-user"></a>Erstellen eines LinkedIn Learning-Testbenutzers

Die LinkedIn Learning-Anwendung unterstützt die Just-In-Time-Benutzerbereitstellung. Nach der Authentifizierung werden Benutzer in der Anwendung automatisch erstellt. Aktivieren Sie im LinkedIn Learning-Portal auf der Seite mit den Administratoreinstellungen die Option **Automatically Assign licenses** (Lizenzen automatisch zuweisen), um die Just-In-Time-Bereitstellung zu aktivieren. Dadurch wird dem Benutzer auch eine Lizenz zugewiesen.

   ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen: 

#### <a name="sp-initiated"></a>SP-initiiert:

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch werden Sie zur Anmelde-URL für LinkedIn Learning weitergeleitet, wo Sie den Anmeldeflow initiieren können.  

* Rufen Sie direkt die LinkedIn Learning-Anmelde-URL auf, und initiieren Sie den Anmeldeflow.

#### <a name="idp-initiated"></a>IDP-initiiert:

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch sollten Sie automatisch bei der LinkedIn Learning-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. 

Sie können auch den Microsoft-Bereich „Meine Apps“ verwenden, um die Anwendung in einem beliebigen Modus zu testen. Beim Klicken auf die Kachel „LinkedIn Learning“ in „Meine Apps“ geschieht Folgendes: Wenn Sie die Anwendung im SP-Modus konfiguriert haben, werden Sie zum Initiieren des Anmeldeflows zur Anmeldeseite der Anwendung weitergeleitet. Wenn Sie die Anwendung im IDP-Modus konfiguriert haben, sollten Sie automatisch bei der LinkedIn Learning-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](../user-help/my-apps-portal-end-user-access.md).


## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von LinkedIn Learning können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](/cloud-app-security/proxy-deployment-aad) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.