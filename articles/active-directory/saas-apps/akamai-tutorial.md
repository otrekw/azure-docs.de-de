---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Akamai | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Akamai konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1b7e0d7a-e78f-43a5-af93-b626186e2376
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 042dd242285081001ca48c9f17e4d42c2294c0ff
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "74979136"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-akamai"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Akamai

In diesem Tutorial erfahren Sie, wie Sie Akamai in Azure Active Directory (Azure AD) integrieren. Die Integration von Akamai in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Akamai hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Akamai anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Akamai-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

- Akamai unterstützt IDP-initiiertes einmaliges Anmelden.

## <a name="adding-akamai-from-the-gallery"></a>Hinzufügen von Akamai über den Katalog

Zum Konfigurieren der Integration von Akamai in Azure AD müssen Sie Akamai aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Akamai** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Akamai** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-akamai"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Akamai

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Akamai mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Akamai eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Akamai die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    * **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    * **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Akamai](#configure-akamai-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    * **[Erstellen eines Akamai-Testbenutzers](#create-akamai-test-user)** , um in Akamai eine Entsprechung von B. Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Akamai** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<Yourapp>.login.go.akamai-access.com/sp/response`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https:// <Yourapp>.login.go.akamai-access.com/sp/response`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Diese Werte erhalten Sie vom [Supportteam für den Akamai-Client](https://www.akamai.com/us/en/contact-us/). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **Akamai einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Akamai gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Akamai** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-akamai-sso"></a>Konfigurieren des einmaligen Anmeldens für Akamai

### <a name="setting-up-idp"></a>Einrichten des IDP

1. Melden Sie sich bei der **Enterprise Application Access-Konsole von Akamai** an.
1. Wählen Sie in der **EAA-Konsole von Akamai** die Optionen **Identity** > **Identity Providers** (Identität > Identitätsanbieter) aus.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure01.png)

1. Klicken Sie auf **Add Identity Provider** (Identitätsanbieter hinzufügen).

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure02.png)

    a. Geben Sie einen **eindeutigen Namen** ein.
    
    b. Wählen Sie **Third Party SAML** (Drittanbieter-SAML) aus, und klicken Sie auf **Create Identity Provider and Configure** (Identitätsanbieter erstellen und konfigurieren).

### <a name="general-settings"></a>Allgemeine Einstellungen

1. **Identity Intercept** (Identität abfangen): Geben Sie die SP-basierte URL ein. Diese wird für die Azure AD-Konfiguration verwendet.

    > [!NOTE]
    > Sie können eine eigene benutzerdefinierte Domäne verwenden. (Dafür sind ein DNS-Eintrag und ein Zertifikat erforderlich.) In diesem Beispiel verwenden wir die Akamai-Domäne.

1. **Akamai Cloud Zone** (Akamai-Cloudzone): Wählen Sie die entsprechende Cloudzone aus.
1. **Certificate Validation** (Zertifikatüberprüfung): Lesen Sie die Akamai-Dokumentation. (optional)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure03.png)

### <a name="authentication-configuration"></a>Authentifizierungskonfiguration

1. URL: Geben Sie die gleiche URL wie unter „Identity Intercept“ (Identität abfangen) ein. (An diese URL werden Benutzer nach der Authentifizierung weitergeleitet.)
2. Logout URL (Abmelde-URL): Aktualisieren Sie die Abmelde-URL.
3. Sign SAML Request (SAML-Anforderung signieren): Diese Option ist standardmäßig deaktiviert.
4. Fügen Sie für die IDP-Metadatendatei die Anwendung in der Azure AD-Konsole hinzu.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure04.png)

### <a name="header-based-authentication"></a>Headerbasierte Authentifizierung

Headerbasierte Authentifizierung von Akamai

1. Wählen Sie im Assistenten zum Hinzufügen von Anwendungen die Option **Custom HTTP** (Benutzerdefiniertes HTTP) aus.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure05.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure06.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure07.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure08.png)

#### <a name="authentication"></a>Authentifizierung

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure09.png)

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure10.png)

#### <a name="services"></a>Dienste

1. Klicken Sie auf „Save“ (Speichern), und gehen Sie zu „Authentication“ (Authentifizierung).

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure11.png)

#### <a name="advanced-settings"></a>Erweiterte Einstellungen

1. Geben Sie unter **Custom HTTP Headers** (Benutzerdefinierte HTTP-Header) Werte für **Custom Header** (Benutzerdefinierter Header) und **SAML Attribute** (SAML-Attribut) ein.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure12.png)

1. Klicken Sie auf die Schaltfläche **Save and go to Deployment** (Speichern und zur Bereitstellung wechseln).

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure13.png)

#### <a name="deploy-the-application"></a>Bereitstellen der Anwendung

1. Klicken Sie auf die Schaltfläche **Deploy Application** (Anwendung bereitstellen).

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure14.png)

1. Vergewissern Sie sich, dass die Anwendung erfolgreich bereitgestellt wurde.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure15.png)

### <a name="kerberos-authentication"></a>Kerberos-Authentifizierung

#### <a name="remote-desktop"></a>Remotedesktop

1. Wählen Sie im Assistenten zum Hinzufügen von Anwendungen die Option **RDP** aus.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure16.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure17.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure18.png)

1. Geben Sie den Connector dafür an.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure19.png)

#### <a name="authentication"></a>Authentifizierung

Klicken Sie auf **Save and go to Services** (Speichern und zu Diensten wechseln).

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure20.png)

#### <a name="services"></a>Dienste

Klicken Sie auf **Save and go to Advanced Settings** (Speichern und zu erweiterten Einstellungen wechseln).

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure21.png)

#### <a name="advanced-settings"></a>Erweiterte Einstellungen

Klicken Sie auf **Save and go to Deployment** (Speichern und zur Bereitstellung wechseln).

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure22.png)

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure23.png)

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure24.png)

### <a name="deployment"></a>Bereitstellung

#### <a name="ssh"></a>SSH

1. Navigieren Sie zu „Add Applications“ (Anwendungen hinzufügen), und wählen Sie **SSH** aus.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure25.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure26.png)

1. Konfigurieren Sie die Anwendungsidentität.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure27.png)

    a. Geben Sie einen Namen und eine Beschreibung ein.

    b. Geben Sie die Anwendungsserver-IP bzw. den FQDN und Port für SSH an.

    c. Geben Sie den SSH-Benutzernamen und die SSH-Passphrase an. *Diese finden Sie in Akamai EAA.

    d. Geben Sie den Namen des externen Hosts an.

    e. Geben Sie den Speicherort für den Connector an, und wählen Sie den Connector aus.

#### <a name="authentication"></a>Authentifizierung

Klicken Sie auf **Save and go to Services** (Speichern und zu Diensten wechseln).

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure28.png)

#### <a name="services"></a>Dienste

Klicken Sie auf **Save and go to Advanced Settings** (Speichern und zu erweiterten Einstellungen wechseln).

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure29.png)

#### <a name="advanced-settings"></a>Erweiterte Einstellungen

Klicken Sie auf „Save and go to Deployment“ (Speichern und zur Bereitstellung wechseln).

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure30.png)

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure31.png)

#### <a name="deployment"></a>Bereitstellung

Klicken Sie auf **Deploy Application** (Anwendung bereitstellen).

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure32.png)

### <a name="kerberos-applications"></a>Kerberos-Anwendungen

#### <a name="adding-directory"></a>Hinzufügen eines Verzeichnisses

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure33.png)

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure34.png)

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure35.png)

![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure36.png)

### <a name="create-akamai-test-user"></a>Erstellen eines Akamai-Testbenutzers

In diesem Abschnitt erstellen Sie in Akamai einen Benutzer namens B. Simon. Arbeiten Sie mit dem  [Supportteam für den Akamai-Client](https://www.akamai.com/us/en/contact-us/) zusammen, um die Benutzer zur Akamai-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Akamai“ klicken, sollten Sie automatisch bei der Akamai-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Akamai mit Azure AD ausprobieren](https://aad.portal.azure.com/)
