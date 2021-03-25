---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Resource Central | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmeldung zwischen Azure Active Directory und Resource Central konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/13/2021
ms.author: jeedes
ms.openlocfilehash: dbc148fcbcd9c3be86a29df1e08755611a347b07
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100586591"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-resource-central"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Resource Central

In diesem Tutorial erfahren Sie, wie Sie Resource Central in Azure Active Directory (Azure AD) integrieren. Die Integration von Resource Central in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Resource Central hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Resource Central anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Resource Central-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Resource Central unterstützt **SP-initiiertes** einmaliges Anmelden.

* Resource Central unterstützt die **Just-In-Time**-Benutzerbereitstellung.

## <a name="add-resource-central-from-the-gallery"></a>Hinzufügen von Resource Central aus dem Katalog

Zum Konfigurieren der Integration von Resource Central in Azure AD müssen Sie Resource Central aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Resource Central** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Resource Central** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-resource-central"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Resource Central

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Resource Central mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Resource Central eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Resource Central die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
    1. **[Erstellen eines Resource Central-Testbenutzers](#create-resource-central-test-user)** , um eine Entsprechung von B. Simon in Resource Central zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Konfigurieren des einmaligen Anmeldens für Resource Central](#configure-resource-central-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Resource Central** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie unter **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

   1. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<DOMAIN_NAME>/ResourceCentral`.

   1. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein: `https://<DOMAIN_NAME>/ResourceCentral`.

   1. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<DOMAIN_NAME>/ResourceCentral/ExAuth/Saml2Authentication/Acs`

    > [!NOTE]
    > Hierbei handelt es sich nicht um Literalwerte. Ersetzen Sie diese Werte durch die tatsächlichen Werte für Anmelde-URL, Bezeichner und Antwort-URL. Diese Werte erhalten Sie vom [Supportteam für den Resource Central-Client](mailto:st@aod.vn).  Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zum Eintrag **Zertifikat (Base64)** . Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie unter **Resource Central einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge `username@companydomain.extension` ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Resource Central gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Resource Central** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Bereich **Benutzer und Gruppen** in der Liste **Benutzer** den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie unter **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle **Standardzugriff** ausgewählt.
1. Klicken Sie im Bereich **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-resource-central-test-user"></a>Erstellen eines Resource Central-Testbenutzers

In diesem Abschnitt wird in **Resource Central** ein Benutzer namens **B. Simon** erstellt.

1. Wählen Sie in Resource Central **Security** > **Persons** > **New** (Sicherheit > Personen > Neu) aus.
  
    :::image type="content" source="./media/resource-central/new-person.png" alt-text="Screenshot: Bereich „Persons“ (Personen) in Resource Central, in dem die Schaltfläche „New“ (Neu) hervorgehoben ist":::

1. Geben Sie unter **Person Details** (Angaben zur Person) für **Display name** (Anzeigename) den Benutzer **B.Simon** ein. Geben Sie unter **SMTP Address** (SMTP-Adresse) den Azure AD-Benutzernamen des Benutzers ein. z. B. `B.Simon@contoso.com`.

    :::image type="content" source="./media/resource-central/person.png" alt-text="Screenshot: Bereich „Person Details“ (Angaben zur Person) in Resource Central":::

## <a name="configure-resource-central-sso"></a>Konfigurieren des einmaligen Anmeldens für Resource Central

In diesem Abschnitt Konfigurieren Sie einmaliges Anmelden unter **Resource Central System Administrator** (Resource Central-Systemadministrator).

1. Wählen Sie unter „Resource Central System Administrator“ (Resource Central-Systemadministrator) die Option **External Authentication** (Externe Authentifizierung) aus.
1.  Wählen Sie unter **Enable Configuration** (Konfiguration aktivieren) die Option **Yes** (Ja) aus.

    ![Screenshot: In Resource Central im Bereich „External Authentication“ (Externe Authentifizierung) hervorgehobene Option „Enable Configuration“ (Konfiguration aktivieren)](./media/resource-central/enable.png)

1. Wählen Sie unter **Authentication Protocol** (Authentifizierungsprotokoll) die Option **SAML2** aus. 

   :::image type="content" source="./media/resource-central/protocol.png" alt-text="Screenshot: Option „SAML2“ unter „Authentication Protocol“ (Authentifizierungsprotokoll) in Resource Central ausgewählt":::

1. Geben Sie unter **SAML2 Configuration** (SAML2-Konfiguration) die Werte für die folgenden Felder ein:

    1. Geben Sie für **Identifier (Entity ID)** (Bezeichner (Entitäts-ID)), **Login URL** (Anmelde-URL), **Logout URL** (Abmelde-URL) und **Azure AD Identifier** (Azure AD-Bezeichner) die entsprechenden URLs ein:

       :::image type="content" source="./media/resource-central/auth.png" alt-text="Screenshot: Bereich „SAML2 Configuration“ (SAML2-Konfiguration) in Resource Central":::

        Kopieren Sie die URLs aus dem Bereich **Set up Resource Central** (Resource Central einrichten):

        :::image type="content" source="./media/resource-central/setup.png" alt-text="Screenshot: Bereich „Set up Resource Central“ (Resource Central einrichten) in Resource Central":::

   1. Geben Sie unter **Return URL** (Rückgabe-URL) Folgendes ein: `https://<DOMAIN_NAME>/ResourceCentral/ExAuth/Saml2Authentication/CallbackHandler`.
  
1. Laden Sie unter **Certificate** (Zertifikat) Ihr Zertifikat hoch, und geben Sie Ihr Kennwort ein.

   ![Screenshot: Abschnitt „Certificate“ (Zertifikat) in Resource Central](./media/resource-central/cert.png)
   
1. Wählen Sie **Speichern** aus.

1. Kehren Sie zum **Azure-Portal** zurück. Laden Sie unter **SAML-Signaturzertifikat** Ihr Zertifikat hoch, und geben Sie Ihr Kennwort ein.

   ![Screenshot: Bereich „Zertifikat importieren“ im Azure-Portal](./media/resource-central/cert2.png).

1. Wählen Sie **Hinzufügen**.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für das einmalige Anmelden. Es gibt drei Optionen zum Testen des einmaligen Anmeldens:

* Wählen Sie im Azure-Portal die Option **Diese Anwendung testen** aus. Über den Link werden Sie zur Anmelde-URL für Resource Central umgeleitet, wo Sie die Anmeldung initiieren können.

* Rufen Sie direkt die Resource Central-Anmelde-URL auf, und initiieren Sie die Anmeldung.

   :::image type="content" source="./media/resource-central/test.png" alt-text="Screenshot: Resource Central-Webseite zum Testen des einmaligen Anmeldens":::

* Verwenden Sie das Portal „Meine Apps“ von Microsoft. Wählen Sie im Portal „Meine Apps“ die Kachel **Resource Central** aus, um zur Anmelde-URL für Resource Central umgeleitet zu werden. Weitere Informationen finden Sie unter [Anmelden beim Portal „Meine Apps“ und Starten von Apps über dieses](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Einrichten von Resource Central für einmaliges Anmelden mit Azure AD können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](/cloud-app-security/proxy-deployment-any-app) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.
