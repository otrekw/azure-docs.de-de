---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Nitro Productivity Suite | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und Nitro Productivity Suite konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/02/2020
ms.author: jeedes
ms.openlocfilehash: b67e280824c5e2336aa79e14d3e09d3a670118a5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88554329"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-nitro-productivity-suite"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Nitro Productivity Suite

In diesem Tutorial erfahren Sie, wie Sie Nitro Productivity Suite in Azure Active Directory (Azure AD) integrieren. Die Integration von Nitro Productivity Suite in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Nitro Productivity Suite hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Nitro Productivity Suite anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps (Software as a Service) mit Azure AD finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Voraussetzungen

Zunächst benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein [Enterprise-Abonnement](https://www.gonitro.com/pricing) für Nitro Productivity Suite

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Nitro Productivity Suite unterstützt **SP**- und **IDP**-initiiertes einmaliges Anmelden.
* Nitro Productivity Suite unterstützt die **Just-In-Time**-Benutzerbereitstellung.
* Nach dem Konfigurieren von Nitro Productivity Suite können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. Weitere Informationen finden Sie im Artikel zum [Erzwingen der Sitzungssteuerung mit Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-nitro-productivity-suite-from-the-gallery"></a>Hinzufügen von Nitro Productivity Suite aus dem Katalog

Zum Konfigurieren der Integration von Nitro Productivity Suite in Azure AD müssen Sie Nitro Productivity Suite aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Bereich **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Nitro Productivity Suite** in das Suchfeld ein.
1. Wählen Sie in den Ergebnissen **Nitro Productivity Suite** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-single-sign-on-for-nitro-productivity-suite"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Nitro Productivity Suite

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Nitro Productivity Suite mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Nitro Productivity Suite eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Nitro Productivity Suite die folgenden Schritte aus:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso), um Ihren Benutzern die Verwendung dieses Features zu ermöglichen
    1. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden von Azure AD mit B.Simon zu testen
    1. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um B.Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen
1. [Konfigurieren des einmaligen Anmeldens für Nitro Productivity Suite](#configure-nitro-productivity-suite-sso), um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. [Erstellen eines Nitro Productivity Suite-Testbenutzers](#create-a-nitro-productivity-suite-test-user), um ein Pendant von B. Simon in Nitro Productivity Suite zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
1. [Testen des einmaligen Anmeldens](#test-sso), um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Nitro Productivity Suite** zum Abschnitt **Verwalten**. Wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Navigieren Sie im Abschnitt **SAML-Signaturzertifikat** zur Option **Zertifikat (Base64)** . Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Screenshot: Abschnitt „SAML-Signaturzertifikat“ mit hervorgehobenem Downloadlink](common/certificatebase64.png)
    
1. Wählen Sie im Abschnitt **Nitro Productivity Suite einrichten** das Kopiersymbol neben **Anmelde-URL** aus.
    
    ![Screenshot des Abschnitts „Nitro Productivity Suite einrichten“: URLs und Kopiersymbol mit roter Umrandung](common/copy-configuration-urls.png)
    
1. Navigieren Sie im [Nitro-Verwaltungsportal](https://admin.gonitro.com/) auf der Seite **Enterprise Settings** (Unternehmenseinstellungen) zum Abschnitt **Single Sign-On** (Einmaliges Anmelden). Wählen Sie **Setup SAML SSO** (SAML-SSO einrichten) aus.

    a. Fügen Sie die **Anmelde-URL** aus dem vorherigen Schritt in das Feld **Sign In URL** (Anmelde-URL) ein.
    
    b. Laden Sie das **Zertifikat (Base64)** aus dem obigen Schritt in das Feld **X509 Signing Certificate** (X509-Signaturzertifikat) hoch.
    
    c. Klicken Sie auf **Submit** (Senden).
    
    d. Wählen Sie **Einmaliges Anmelden aktivieren**aus.


1. Kehren Sie zum [Azure-Portal](https://portal.azure.com/) zurück. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** das Stiftsymbol für **Grundlegende SAML-Konfiguration** aus, um die Einstellungen zu bearbeiten.

   ![Screenshot: Seite „Einmaliges Anmelden (SSO) mit SAML einrichten“ mit hervorgehobenem Stiftsymbol](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    a. Kopieren Sie im [Nitro-Verwaltungsportal](https://admin.gonitro.com/) den Wert im Feld **SAML Entity ID** (SAML-Entitäts-ID), und fügen Sie ihn in das Textfeld **Bezeichner** ein. Der Wert sollte das folgende Format haben: `urn:auth0:gonitro-prod:<ENVIRONMENT>`.

    b. Kopieren Sie im [Nitro-Verwaltungsportal](https://admin.gonitro.com/) den Wert im Feld **ACS URL** (ACS-URL), und fügen Sie ihn in das Textfeld **Antwort-URL** ein. Der Wert sollte das folgende Format haben: `https://gonitro-prod.eu.auth0.com/login/callback?connection=<ENVIRONMENT>`.

1. Wählen Sie **Zusätzliche URLs festlegen** aus, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten** Modus konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** die URL ein: `https://sso.gonitro.com/login`.

1. Wählen Sie **Speichern** aus.

1. Die Nitro Productivity Suite-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute.

    ![Screenshot der Standardattribute](common/default-attributes.png)

1. Von der Nitro Productivity Suite-Anwendung wird erwartet, dass in der SAML-Antwort zusätzlich zu den oben genannten Attributen noch einige weitere Attribute zurückgegeben werden. Diese Attribute werden vorab aufgefüllt, Sie können sie jedoch nach Bedarf überarbeiten.
    
    | Name  |  Quellattribut|
    | ---------------| --------------- |
    | employeeNumber |  user.objectid |


### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer namens B.Simon.

1. Wählen Sie im Azure-Portal im linken Bereich **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich das Kennwort.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Nitro Productivity Suite gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Nitro Productivity Suite** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Screenshot: Abschnitt „Verwalten“ mit hervorgehobener Option „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Klicken Sie auf **Benutzer hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Screenshot: Seite „Benutzer und Gruppen“ mit hervorgehobener Option zum Hinzufügen eines Benutzers](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste der Benutzer die Option **B.Simon** aus. Wählen Sie anschließend am unteren Bildschirmrand **Auswählen** aus.
1. Falls Sie in der SAML-Assertion einen Rollenwert erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer aus der Liste aus. Wählen Sie anschließend am unteren Bildschirmrand **Auswählen** aus.
1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.

## <a name="configure-nitro-productivity-suite-sso"></a>Konfigurieren des einmaligen Anmeldens für Nitro Productivity Suite

Senden Sie zum Konfigurieren des einmaligen Anmeldens aufseiten von Nitro Productivity Suite das heruntergeladene **Zertifikat (Base64)** und die kopierten URLs aus dem Azure-Portal an das [Supportteam von Nitro Productivity Suite](https://www.gonitro.com/support). Das Supportteam sorgt dafür, dass die SAML-Verbindung für einmaliges Anmelden auf beiden Seiten ordnungsgemäß eingerichtet wird.

### <a name="create-a-nitro-productivity-suite-test-user"></a>Erstellen eines Nitro Productivity Suite-Testbenutzers

Nitro Productivity Suite unterstützt die Just-In-Time-Benutzerbereitstellung (standardmäßig aktiviert). Es ist keine weitere Aktion Ihrerseits erforderlich. Ist ein Benutzer noch nicht in Nitro Productivity Suite vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „Nitro Productivity Suite“ auswählen, werden Sie automatisch bei der Nitro Productivity Suite-Instanz angemeldet, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen finden Sie unter [Anmelden beim Portal „Meine Apps“ und Starten von Apps über dieses](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Tutorials zur Integration von SaaS-Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Nitro Productivity Suite mit Azure AD ausprobieren](https://aad.portal.azure.com/)

- [Was ist Sitzungssteuerung in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Schützen von Apps mit der App-Steuerung für bedingten Zugriff von Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

