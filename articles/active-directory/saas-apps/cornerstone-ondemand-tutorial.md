---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Cornerstone Single Sign-On | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und Cornerstone Single Sign-On konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/09/2021
ms.author: jeedes
ms.openlocfilehash: f7167df523ca6f84eacd92fc7af1011e8b3b00b6
ms.sourcegitcommit: ac035293291c3d2962cee270b33fca3628432fac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2021
ms.locfileid: "104950334"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cornerstone-single-sign-on"></a>Tutorial: Integration des einmaligen Anmeldens von Azure Active Directory (Single Sign-On, SSO) mit Cornerstone Single Sign-On

In diesem Tutorial erfahren Sie, wie Sie Cornerstone Single Sign-On in Azure Active Directory (Azure AD) integrieren. Die Integration von Cornerstone Single Sign-On in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Cornerstone Single Sign-On hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Cornerstone Single Sign-On anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Cornerstone Single Sign-On-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

> [!NOTE]
> Diese Integration kann auch über die Azure AD-Umgebung für die US Government-Cloud verwendet werden. Sie finden diese Anwendung im Azure AD-Katalog für US Government-Cloudanwendungen und konfigurieren sie auf die gleiche Weise wie in der öffentlichen Cloud.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Cornerstone Single Sign-On unterstützt **SP**-initiiertes einmaliges Anmelden.
* Cornerstone Single Sign-On unterstützt die [automatisierte Benutzerbereitstellung](cornerstone-ondemand-provisioning-tutorial.md).


## <a name="adding-cornerstone-single-sign-on-from-the-gallery"></a>Hinzufügen von Cornerstone Single Sign-On aus dem Katalog

Zum Konfigurieren der Integration von Cornerstone Single Sign-On in Azure AD müssen Sie Cornerstone Single Sign-On aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** im Suchfeld den Suchbegriff **Cornerstone Single Sign-On** ein.
1. Wählen Sie im Ergebnisbereich **Cornerstone Single Sign-On** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-cornerstone-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Cornerstone Single Sign-On

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD für Cornerstone Single Sign-On mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Cornerstone Single Sign-On eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Cornerstone Single Sign-On die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Cornerstone Single Sign-On](#configure-cornerstone-single-sign-on-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines Cornerstone Single Sign-On-Testbenutzers](#create-cornerstone-single-sign-on-test-user)** , um in Cornerstone Single Sign-On eine Entsprechung von B. Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
3. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Cornerstone Single Sign-On** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<PORTAL_NAME>.csod.com`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<PORTAL_NAME>.csod.com/samldefault.aspx?ouid=<OUID>`

    c. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<PORTAL_NAME>.csod.com/samldefault.aspx?ouid=<OUID>`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Antwort-URL, den tatsächlichen Bezeichner und die tatsächliche Anmelde-URL. Diese Werte erhalten Sie vom [Supportteam für den Cornerstone Single Sign-On-Client](mailto:moreinfo@csod.com). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

4. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zum Eintrag **Zertifikat (Base64)** . Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

6. Kopieren Sie im Abschnitt **Cornerstone Single Sign-On einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Cornerstone Single Sign-On gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Cornerstone Single Sign-On** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-cornerstone-single-sign-on-sso"></a>Konfigurieren des einmaligen Anmeldens für Cornerstone Single Sign-On SSO

1. Melden Sie sich bei Cornerstone Single Sign-On SSO als Administrator an.

1. Navigieren Sie zur **Admin > Tools** (Verwaltung > Tools).

    ![Screenshot: Seite „Admin“ (Verwaltung)](./media/cornerstone-ondemand-tutorial/admin.png)

1. Wählen Sie unter **Configuration Tools** (Konfigurationstools) den Bereich **EDGE** aus.

    ![Screenshot: Bereich „EDGE“](./media/cornerstone-ondemand-tutorial/edge-panel.png)

1. Wählen Sie im Abschnitt **Integrate** (Integrieren) die Option „Single Sign-On“ (Einmaliges Anmelden) aus.

    ![Screenshot: Option „Single Sign-On“ (Einmaliges Anmelden)](./media/cornerstone-ondemand-tutorial/single-sign-on.png)

1. Klicken Sie auf die Schaltfläche **Add SSO** (SSO hinzufügen). Wählen Sie in dem unten gezeigten Popupfenster **Inbound SAML** (SAML (eingehend)) aus, und klicken Sie dann auf **Add** (Hinzufügen).

    ![Screenshot: Inbound SAML (SAML (eingehend))](./media/cornerstone-ondemand-tutorial/inbound.png)

1. Führen Sie die folgenden Schritte auf der im Anschluss angegebenen Seite aus:

    ![Screenshot: Konfigurationsabschnitt für Cornerstone](./media/cornerstone-ondemand-tutorial/configuration.png)

    a. Klicken Sie unter **General Properties** (Allgemeine Eigenschaften) auf **Upload File** (Datei hochladen), um die Datei **Zertifikat (Base64)** hochzuladen, die Sie aus dem Azure-Portal heruntergeladen haben.

    b. Aktivieren Sie das Kontrollkästchen **Enable** (Aktivieren), und fügen Sie im Textfeld **IDP URL** (IDP-URL) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Klicken Sie auf **Speichern**.

### <a name="create-cornerstone-single-sign-on-test-user"></a>Erstellen eines Cornerstone Single Sign-On-Testbenutzers

In diesem Abschnitt wird in Cornerstone Single Sign-On ein Benutzer namens B. Simon erstellt. Cornerstone Single Sign-On unterstützt die automatische Benutzerbereitstellung (standardmäßig aktiviert). Weitere Details zur Konfiguration der automatischen Benutzerbereitstellung finden Sie [hier](./cornerstone-ondemand-provisioning-tutorial.md).

**Wenn Sie einen Benutzer manuell erstellen möchten, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei Cornerstone Single Sign-On SSO als Administrator an.

1. Navigieren Sie zu **Admin > Users** (Verwaltung > Benutzer), und klicken Sie unten auf der Seite auf **Add User** (Benutzer hinzufügen).

    ![Screenshot: Erstellung eines Cornerstone-Testbenutzers](./media/cornerstone-ondemand-tutorial/user-1.png)

1. Füllen Sie auf der Seite **Add new user** (Neuen Benutzer hinzufügen) die erforderlichen Felder aus, und klicken Sie auf **Save** (Speichern).

    ![Screenshot: Erstellung eines Testbenutzers mit den erforderlichen Feldern](./media/cornerstone-ondemand-tutorial/user-2.png)

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen: 

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch werden Sie zur Anmelde-URL für Cornerstone Single Sign-On weitergeleitet, wo Sie den Anmeldeflow initiieren können. 

* Rufen Sie die Cornerstone Single Sign-On-Anmelde-URL direkt auf, und initiieren Sie den Anmeldeflow.

* Sie können „Meine Apps“ von Microsoft verwenden. Wenn Sie in „Meine Apps“ auf die Kachel „Cornerstone Single Sign-On“ klicken, werden Sie zur Anmelde-URL von Cornerstone Single Sign-On weitergeleitet. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von Cornerstone Single Sign-On können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](/cloud-app-security/proxy-deployment-aad) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.