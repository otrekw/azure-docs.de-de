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
ms.topic: tutorial
ms.date: 01/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17d0c91d31f7746c53d62af87670c40e9902554c
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86026830"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-akamai"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Akamai

In diesem Tutorial erfahren Sie, wie Sie Akamai in Azure Active Directory (Azure AD) integrieren. Die Integration von Akamai in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Akamai hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Akamai anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

Die Integration von Azure Active Directory und Akamai Enterprise Application Access ermöglicht den nahtlosen Zugriff auf in der Cloud oder lokal gehostete Legacyanwendungen. Die integrierte Lösung nutzt alle modernen Azure Active Directory-Funktionen wie [bedingten Azure AD-Zugriff](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal), [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) und [Azure AD Identity Governance](https://docs.microsoft.com/azure/active-directory/governance/identity-governance-overview), um den Zugriff auf Legacyanwendungen ohne App-Änderungen oder Agent-Installation zu ermöglichen.

Die folgende Abbildung zeigt, wie sich Akamai EAA in das allgemeine Szenario für sicheren Hybridzugriff einfügt:

![Akamai EAA fügt sich in das allgemeine Szenario für sicheren Hybridzugriff ein.](./media/header-akamai-tutorial/introduction01.png)

### <a name="key-authentication-scenarios"></a>Wichtige Authentifizierungsszenarien

Neben der nativen Azure Active Directory-Integrationsunterstützung für moderne Authentifizierungsprotokolle wie OpenID Connect, SAML und WS-Fed erweitert Akamai EAA den sicheren Zugriff für legacybasierte Authentifizierungs-Apps sowohl für den internen als auch für den externen Zugriff mit Azure AD, um für diese Anwendungen moderne Szenarien wie etwa Zugriff ohne Kennwort zu ermöglichen. Dies schließt Folgendes ein:

* Apps mit headerbasierter Authentifizierung
* Remotedesktop
* SSH (Secure Shell)
* Apps mit Kerberos-Authentifizierung
* VNC (Virtual Network Computing)
* Apps mit anonymer Authentifizierung oder ohne integrierte Authentifizierung
* Apps mit NTLM-Authentifizierung (Schutz mit doppelten Eingabeaufforderungen für den Benutzer)
* Formularbasierte Anwendung (Schutz mit doppelten Eingabeaufforderungen für den Benutzer)

### <a name="integration-scenarios"></a>Integrationsszenarien

Die Partnerschaft zwischen Microsoft und Akamai EAA ermöglicht eine flexible Erfüllung Ihrer geschäftlichen Anforderungen. Hierzu werden basierend auf Ihrer geschäftlichen Anforderung verschiedene Integrationsszenarien unterstützt. Dies ermöglicht eine Zero-Day-Abdeckung für sämtliche Anwendungen mit anschließender schrittweiser Klassifizierung und Konfiguration geeigneter Richtlinienklassifizierungen.

#### <a name="integration-scenario-1"></a>Integrationsszenario 1

Akamai EAA wird als einzelne Anwendung in der Azure AD-Instanz konfiguriert. Der Administrator kann die Richtlinie für bedingten Zugriff für die Anwendung konfigurieren, und Benutzer können auf das Akamai EAA-Portal zugreifen, sobald die Bedingungen erfüllt sind.

**Vorteile:**

• Der IDP muss lediglich einmal konfiguriert werden.

**Nachteile:**

• Benutzer haben letztendlich zwei Anwendungsportale.

• Abdeckung aller Anwendungen durch eine einzelne allgemeine Richtlinie für bedingten Zugriff.

![Integrationsszenario 1](./media/header-akamai-tutorial/scenario1.png)

#### <a name="integration-scenario-2"></a>Integrationsszenario 2

Die Akamai EAA-Anwendung wird individuell im Azure AD-Portal eingerichtet. Der Administrator kann eine individuelle Richtlinie für bedingten Zugriff für die Anwendungen konfigurieren, und Benutzer können direkt zur jeweiligen Anwendung umgeleitet werden, sobald die Bedingungen erfüllt sind.

**Vorteile:**

• Sie können individuelle Richtlinien für bedingten Zugriff definieren.

• Alle Apps werden im O365-Waffel-Menü und im Bereich „myApps.microsoft.com“ dargestellt.

**Nachteile:**

• Es müssen mehrere IDPs konfiguriert werden.

![Integrationsszenario 2](./media/header-akamai-tutorial/scenario2.png)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Akamai-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

- Akamai unterstützt IDP-initiiertes einmaliges Anmelden.

#### <a name="important"></a>Wichtig

Alle unten aufgeführten Einrichtungsschritte gelten sowohl für das **Integrationsszenario 1** als auch für das **Szenario 2**. Für das **Integrationsszenario 2** müssen Sie einen individuellen IdP in der Akamai EAA-Instanz einrichten, und die URL-Eigenschaft muss so geändert werden, dass sie auf die Anwendungs-URL verweist.

![Wichtig](./media/header-akamai-tutorial/important.png)

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
    * **[Einrichten des IDP](#setting-up-idp)**
    * **[Headerbasierte Authentifizierung](#header-based-authentication)**
    * **[Remotedesktop](#remote-desktop)**
    * **[SSH](#ssh)**
    * **[Kerberos-Authentifizierung](#kerberos-authentication)**
    * **[Erstellen eines Akamai-Testbenutzers](#create-akamai-test-user)** , um in Akamai eine Entsprechung von B. Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Akamai** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<Yourapp>.login.go.akamai-access.com/saml/sp/response`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https:// <Yourapp>.login.go.akamai-access.com/saml/sp/response`

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

**Akamai EAA: IDP-Konfiguration**

1. Melden Sie sich bei der **Enterprise Application Access-Konsole von Akamai** an.
1. Wählen Sie in der **EAA-Konsole von Akamai** die Optionen **Identity** > **Identity Providers** (Identität > Identitätsanbieter) aus, und klicken Sie auf **Add Identity Provider** (Identitätsanbieter hinzufügen).

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure01.png)

1. Führen Sie im Dialogfeld **Create New Identity Provider** (Neuen Identitätsanbieter erstellen) die folgenden Schritte aus:

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

### <a name="session-settings"></a>Sitzungseinstellungen

Übernehmen Sie die Standardeinstellungen.

![Konfigurieren von Akamai](./media/header-akamai-tutorial/sessionsettings.png)

### <a name="directories"></a>Verzeichnisse

Überspringen Sie die Verzeichniskonfiguration.

![Konfigurieren von Akamai](./media/header-akamai-tutorial/directories.png)

### <a name="customization-ui"></a>Benutzeroberfläche für die Anpassung

Der IDP kann auf Wunsch angepasst werden.

![Konfigurieren von Akamai](./media/header-akamai-tutorial/customizationui.png)

### <a name="advanced-settings"></a>Erweiterte Einstellungen

Überspringen Sie die erweiterten Einstellungen. (Weitere Informationen finden Sie in der Akamai-Dokumentation.)

![Konfigurieren von Akamai](./media/header-akamai-tutorial/advancesettings.png)

### <a name="deployment"></a>Bereitstellung

1. Klicken Sie auf die Option zum Bereitstellen des Identitätsanbieters.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/deployment.png)

2. Vergewissern Sie sich, dass die Bereitstellung erfolgreich war.

### <a name="header-based-authentication"></a>Headerbasierte Authentifizierung

Headerbasierte Authentifizierung von Akamai

1. Wählen Sie im Assistenten zum Hinzufügen von Anwendungen die Option **Custom HTTP** (Benutzerdefiniertes HTTP) aus.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure05.png)

2. Geben Sie unter **Application Name** (Anwendungsname) einen Anwendungsnamen und unter **Description** (Beschreibung) eine Beschreibung ein.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure06.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure07.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure08.png)

#### <a name="authentication"></a>Authentifizierung

1. Wählen Sie die Registerkarte **Authentication** (Authentifizierung) aus.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure09.png)

2. Weisen Sie den **Identitätsanbieter** zu.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure10.png)

#### <a name="services"></a>Dienste

Klicken Sie auf „Save“ (Speichern), und gehen Sie zu „Authentication“ (Authentifizierung).

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

1. Endbenutzererfahrung:

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/enduser01.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/enduser02.png)

1. Bedingter Zugriff:

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess01.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess02.png)

#### <a name="remote-desktop"></a>Remotedesktop

1. Wählen Sie im Assistenten zum Hinzufügen von Anwendungen die Option **RDP** aus.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure16.png)

1. Geben Sie unter **Application Name** (Anwendungsname) einen Anwendungsnamen und unter **Description** (Beschreibung) eine Beschreibung ein.

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

1. Klicken Sie auf **Save and go to Deployment** (Speichern und zur Bereitstellung wechseln).

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure22.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure23.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure24.png)

1. Endbenutzererfahrung

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/enduser03.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/enduser02.png)

1. Bedingter Zugriff

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess04.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess05.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess06.png)

1. Alternativ können Sie die RDP-Anwendungs-URL auch direkt eingeben.

#### <a name="ssh"></a>SSH

1. Navigieren Sie zu „Add Applications“ (Anwendungen hinzufügen), und wählen Sie **SSH** aus.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure25.png)

1. Geben Sie unter **Application Name** (Anwendungsname) einen Anwendungsnamen und unter **Description** (Beschreibung) eine Beschreibung ein.

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

1. Klicken Sie auf **Deploy Application** (Anwendung bereitstellen).

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure32.png)

1. Endbenutzererfahrung

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/enduser03.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/enduser04.png)

1. Bedingter Zugriff

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess04.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess07.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess08.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess09.png)

### <a name="kerberos-authentication"></a>Kerberos-Authentifizierung

Im folgenden Beispiel wird ein interner Webserver ([http://frp-app1.superdemo.live](http://frp-app1.superdemo.live/)) veröffentlicht und einmaliges Anmelden mit KCD aktiviert:

#### <a name="general-tab"></a>Registerkarte Allgemein

![Konfigurieren von Akamai](./media/header-akamai-tutorial/generaltab.png)

#### <a name="authentication-tab"></a>Registerkarte „Authentication“ (Authentifizierung)

Weisen Sie den Identitätsanbieter zu.

![Konfigurieren von Akamai](./media/header-akamai-tutorial/authenticationtab.png)

#### <a name="services-tab"></a>Registerkarte „Services“ (Dienste)

![Konfigurieren von Akamai](./media/header-akamai-tutorial/servicestab.png)

#### <a name="advanced-settings"></a>Erweiterte Einstellungen

![Konfigurieren von Akamai](./media/header-akamai-tutorial/advancesettings02.png)

> [!NOTE]
> Der SPN für den Webserver muss im Format SPN@Domain angegeben werden. (In dieser Demo wird beispielsweise `HTTP/frp-app1.superdemo.live@SUPERDEMO.LIVE` verwendet.) Lassen Sie die restlichen Einstellungen unverändert.

#### <a name="deployment-tab"></a>Registerkarte „Deployment“ (Bereitstellung)

![Konfigurieren von Akamai](./media/header-akamai-tutorial/deploymenttab.png)

#### <a name="adding-directory"></a>Hinzufügen eines Verzeichnisses

1. Wählen Sie in der Dropdownliste die Option **AD** aus.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure33.png)

1. Geben Sie die erforderlichen Daten an.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/configure34.png)

1. Überprüfen Sie die Verzeichniserstellung.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/directorydomain.png)

1. Fügen Sie die Gruppen/Organisationseinheiten hinzu, die Zugriff benötigen.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/addgroup.png)

1. Hier heißt die Gruppe „EAAGroup“ und hat ein einzelnes Mitglied.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/eaagroup.png)

1. Fügen Sie das Verzeichnis dem Identitätsanbieter hinzu, indem Sie unter **Identity** > **Identity Providers** (Identität > Identitätsanbieter) auf der Registerkarte **Directories** (Verzeichnisse) auf **Assign directory** (Verzeichnis zuweisen) klicken.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/assigndirectory.png)

### <a name="configure-kcd-delegation-for-eaa-walkthrough"></a>Konfigurieren der KCD-Delegierung für EAA: Exemplarische Vorgehensweise

#### <a name="step-1-create-an-account"></a>Schritt 1: Erstellen eines Kontos 

1. In dem Beispiel wird ein Konto namens **EAADelegation** verwendet. Sie können hierzu das Snap-In **Active Directory-Benutzer und -Computer** verwenden.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/assigndirectory.png)

    > [!NOTE]
    > Der Benutzername muss in einem bestimmten Format vorliegen, das auf dem für **Identity Intercept** (Identität abfangen) angegebenen Namen basiert. In Abbildung 1 ist zu sehen, dass dieser **corpapps.login.go.akamai-access.com** lautet.

1. Der Benutzeranmeldename lautet: `HTTP/corpapps.login.go.akamai-access.com`

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/eaadelegation.png)

#### <a name="step-2-configure-the-spn-for-this-account"></a>Schritt 2: Konfigurieren des SPN für dieses Konto

1. In diesem Beispiel lautet der SPN wie folgt:

2. setspn -s **Http/corpapps.login.go.akamai-access.com eaadelegation**

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/spn.png)

#### <a name="step-3-configure-delegation"></a>Schritt 3: Konfigurieren der Delegierung

1. Klicken Sie für das Konto „EAADelegation“ auf die Registerkarte für die Delegierung.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/spn.png)

    * Verwenden Sie ein beliebiges Authentifizierungsprotokoll.
    * Klicken Sie auf „Add“ (Hinzufügen), und fügen Sie das App-Pool-Konto für die Kerberos-Website hinzu. Bei korrekter Konfiguration sollte es automatisch zum korrekten SPN aufgelöst werden.

#### <a name="step-4-create-a-keytab-file-for-akamai-eaa"></a>Schritt 4: Erstellen einer Schlüsseltabellendatei für Akamai EAA

1. Die generische Syntax lautet wie folgt:

1. ktpass /out ActiveDirectorydomain.keytab  /princ `HTTP/yourloginportalurl@ADDomain.com`  /mapuser serviceaccount@ADdomain.com /pass +rdnPass  /crypto All /ptype KRB5_NT_PRINCIPAL

1. Erläuterung des Beispiels

    | Codeausschnitt | Erklärung |
    | - | - |
    | Ktpass /out EAADemo.keytab | // Der Name der ausgegebenen Schlüsseltabellendatei. |
    | /princ HTTP/corpapps.login.go.akamai-access.com@superdemo.live | // HTTP/yourIDPName@YourdomainName |
    | /mapuser eaadelegation@superdemo.live | // EAA-Delegierungskonto |
    | /pass RANDOMPASS | // Kennwort des EAA-Delegierungskontos |
    | /crypto All ptype KRB5_NT_PRINCIPAL | // Siehe Akamai EAA-Dokumentation |
    | | |

1. Ktpass /out EAADemo.keytab  /princ HTTP/corpapps.login.go.akamai-access.com@superdemo.live /mapuser eaadelegation@superdemo.live /pass RANDOMPASS /crypto All ptype KRB5_NT_PRINCIPAL

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/administrator.png)

#### <a name="step-5-import-keytab-in-the-akamai-eaa-console"></a>Schritt 5: Importieren der Schlüsseltabelle in der Akamai EAA-Konsole

1. Klicken Sie auf **System** > **Keytabs** (System > Schlüsseltabellen).

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/keytabs.png)

1. Wählen Sie als Schlüsseltabellentyp die Option **Kerberos Delegation** (Kerberos-Delegierung) aus.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/keytabdelegation.png)

1. Vergewissern Sie sich, dass die Schlüsseltabelle als bereitgestellt und überprüft angezeigt wird.

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/keytabs02.png)

1. Benutzererfahrung

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/enduser03.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/enduser04.png)

1. Bedingter Zugriff

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess04.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess10.png)

    ![Konfigurieren von Akamai](./media/header-akamai-tutorial/conditionalaccess11.png)

### <a name="create-akamai-test-user"></a>Erstellen eines Akamai-Testbenutzers

In diesem Abschnitt erstellen Sie in Akamai einen Benutzer namens B. Simon. Arbeiten Sie mit dem [Supportteam für den Akamai-Client](https://www.akamai.com/us/en/contact-us/) zusammen, um die Benutzer zur Akamai-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Akamai“ klicken, sollten Sie automatisch bei der Akamai-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Akamai mit Azure AD ausprobieren](https://aad.portal.azure.com/)
