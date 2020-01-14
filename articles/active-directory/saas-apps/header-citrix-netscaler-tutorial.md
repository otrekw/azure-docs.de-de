---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Citrix NetScaler (headerbasierte Authentifizierung) | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und Citrix NetScaler konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b0adc7bf-696d-44c9-a57a-f9e9471b8710
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/13/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9c442ca731ecb10f977c19b86cb32caded36659
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75470535"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-citrix-netscaler-header-based-authentication"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Citrix NetScaler (headerbasierte Authentifizierung)

In diesem Tutorial erfahren Sie, wie Sie Citrix NetScaler in Azure Active Directory (Azure AD) integrieren. Die Integration von Citrix NetScaler in Azure AD ermöglicht Folgendes:

* Sie können in Azure AD steuern, wer Zugriff auf Citrix NetScaler haben soll.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Citrix NetScaler anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* SSO-fähiges Citrix NetScaler-Abonnement

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Citrix NetScaler unterstützt **SP-initiiertes** einmaliges Anmelden.

* Citrix NetScaler unterstützt die **Just-In-Time**-Benutzerbereitstellung.

- [Konfigurieren des einmaligen Anmeldens von Citrix NetScaler für die headerbasierte Authentifizierung](#configure-citrix-netscaler-single-sign-on-for-header-based-authentication)

- [Konfigurieren des einmaligen Anmeldens von Citrix NetScaler für die Kerberos-basierte Authentifizierung](citrix-netscaler-tutorial.md)


## <a name="adding-citrix-netscaler-from-the-gallery"></a>Hinzufügen von Citrix NetScaler aus dem Katalog

Zum Konfigurieren der Integration von Citrix NetScaler in Azure AD müssen Sie Citrix NetScaler aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Citrix NetScaler** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Citrix NetScaler** aus, und fügen Sie die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-single-sign-on-for-citrix-netscaler"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Citrix NetScaler

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Citrix NetScaler mithilfe eines Testbenutzers namens **B.Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Citrix NetScaler eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Citrix NetScaler die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Citrix NetScaler](#configure-citrix-netscaler-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
    1. **[Erstellen eines Citrix NetScaler-Testbenutzers](#create-citrix-netscaler-test-user)** , um in Citrix NetScaler eine Entsprechung von B.Simon zu erhalten, die mit der Benutzerdarstellung in Azure AD verknüpft ist.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Citrix NetScaler** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<<Your FQDN>>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<<Your FQDN>>/CitrixAuthService/AuthService.asmx`

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<<Your FQDN>>/CitrixAuthService/AuthService.asmx`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL, den tatsächlichen Bezeichner und die tatsächliche Antwort-URL. Wenden Sie sich an das [Supportteam für den Citrix NetScaler-Client](https://www.citrix.com/contact/technical-support.html), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

    > [!NOTE]
    > Damit einmaliges Anmelden auch funktioniert, muss von öffentlichen Sites auf diese URLs zugegriffen werden können. Sie müssen die Firewall oder andere Sicherheitseinstellungen auf der Seite von Netscaler so aktivieren, dass Azure AD das Token in der konfigurierten ACS-URL bereitstellen kann.

1. Suchen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** nach **App-Verbundmetadaten-URL**, kopieren Sie die URL, und speichern Sie sie im Editor.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Citrix NetScaler erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. Klicken Sie auf das Symbol **Bearbeiten**, und ändern Sie die Attributzuordnung.

    ![image](common/edit-attribute.png)

1. Darüber hinaus werden von Citrix NetScaler noch einige weitere Attribute in der SAML-Antwort erwartet. Führen Sie im Dialogfeld „Benutzerattribute“ im Abschnitt „Benutzeransprüche“ die folgenden Schritte aus, um das SAML-Tokenattribut wie in der folgenden Tabelle gezeigt hinzuzufügen:

    | Name | Quellattribut|
    | ---------------| --------------- |
    | mySecretID  | user.userprincipalname |
    
    1. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    1. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    1. Lassen Sie den **Namespace** leer.

    1. Wählen Sie „Source“ als **Attribut** aus.

    1. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    1. Klicken Sie auf **OK**.

    1. Klicken Sie auf **Speichern**.


1. Kopieren Sie im Abschnitt **Citrix NetScaler einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B.Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Citrix NetScaler gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Liste der Anwendungen **Citrix NetScaler** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-citrix-netscaler-sso"></a>Konfigurieren des einmaligen Anmeldens für Citrix NetScaler

- [Konfigurieren des einmaligen Anmeldens von Citrix NetScaler für die headerbasierte Authentifizierung](#configure-citrix-netscaler-single-sign-on-for-header-based-authentication)

- [Konfigurieren des einmaligen Anmeldens von Citrix NetScaler für die Kerberos-basierte Authentifizierung](citrix-netscaler-tutorial.md)

### <a name="publishing-web-server"></a>Veröffentlichen des Webservers 

1. Erstellen Sie einen **virtuellen Server**.

    a. Navigieren Sie zu **Traffic Management > Load Balancing > Services** (Datenverkehrsverwaltung > Lastenausgleich > Dienste).
    
    b. Klicken Sie auf **Hinzufügen**.

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/web01.png)

    c. Geben Sie die Details des Webservers an, auf dem die Anwendungen ausgeführt werden:
    * **Dienstname**
    * **IP-Adresse des Servers/vorhandener Server**
    * **Protokoll**
    * **Port**

     ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/web01.png)

### <a name="configuring-load-balancer"></a>Konfigurieren des Lastenausgleichs

1. Führen Sie die folgenden Schritte aus, um den Lastenausgleich zu konfigurieren:

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/load01.png)

    a. Navigieren Sie zu **Traffic Management > Load Balancing > Virtual Servers** (Datenverkehrsverwaltung > Lastenausgleich > Virtuelle Server).

    b. Klicken Sie auf **Hinzufügen**.

    c. Geben Sie die folgenden Details an:

    * **Name**
    * **Protokoll**
    * **IP-Adresse**
    * **Port**
    * Klicken Sie auf **OK**.

### <a name="bind-virtual-server"></a>Binden des virtuellen Servers

Binden Sie den Lastenausgleich mit dem zuvor erstellten virtuellen Server.

![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/bind01.png)

![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/bind02.png)

### <a name="bind-certificate"></a>Binden des Zertifikats

Da dieser Dienst als SSL veröffentlicht wird, muss das Serverzertifikat gebunden und die Anwendung anschließend getestet werden.

![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/bind03.png)

![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/bind04.png)

## <a name="citrix-adc-saml-profile"></a>Citrix ADC-SAML-Profil

### <a name="create-authentication-policy"></a>Erstellen der Authentifizierungsrichtlinie

1. Navigieren Sie zu **Security > AAA – Application Traffic > Policies > Authentication > Authentication Policies** (Sicherheit > AAA – Anwendungsdatenverkehr > Richtlinien > Authentifizierung > Authentifizierungsrichtlinien).

2. Klicken Sie auf **Add** (Hinzufügen), und geben Sie die erforderlichen Details an.

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/policy01.png)

    a. Geben Sie unter **Name** einen Namen für die Authentifizierungsrichtlinie ein.

    b. Expression (Ausdruck): **true**

    c. Action type (Aktionstyp): **SAML**

    d. Action (Aktion): Klicken Sie auf **Add** (Hinzufügen), und führen Sie die Schritte des Assistenten zum Erstellen eines SAML-Authentifizierungsservers aus.
    
    e. Klicken Sie unter der Authentifizierungsrichtlinie auf **Create** (Erstellen).

### <a name="create-authentication-saml-server"></a>Erstellen des SAML-Authentifizierungsservers

1. Führen Sie die folgenden Schritte aus:

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/server01.png)

    a. Geben Sie unter **Name** den Namen an.

    b. Importieren Sie Metadaten. (Geben Sie die Verbundmetadaten-URL von der Azure-SAML-Benutzeroberfläche ein, die Sie weiter oben kopiert haben.)
    
    c. Geben Sie den **Ausstellernamen** an.

    d. Klicken Sie auf **Create** (Erstellen).

### <a name="create-authentication-virtual-server"></a>Erstellen des virtuellen Authentifizierungsservers

1.  Navigieren Sie zu **Security > AAA - Application Traffic > Authentication Virtual Servers** (Sicherheit > AAA – Anwendungsdatenverkehr > Virtuelle Authentifizierungsserver).

2.  Klicken Sie auf **Add** (Hinzufügen), und führen Sie die folgenden Schritte aus:

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/server02.png)

    a.  Geben Sie einen **Namen** an.

    b.  Wählen Sie **Non-Addressable** (Nicht adressierbar) aus.

    c.  Protokoll: **SSL**

    d.  Klicken Sie auf **OK**.

    e.  Klicken Sie auf **Weiter**.

### <a name="configure-the-authentication-virtual-server-to-use-azure-ad"></a>Konfigurieren des virtuellen Authentifizierungsservers für die Verwendung von Azure AD

Sie müssen die beiden Abschnitte des virtuellen Authentifizierungsservers ändern.

1.  **Advanced Authentication Policies** (Erweiterte Authentifizierungsrichtlinien)

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/virtual01.png)

    a. Wählen Sie die zuvor erstellte **Authentifizierungsrichtlinie** aus.

    b. Klicken Sie auf **Bind** (Binden).

      ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/virtual02.png)

2. **Form Based Virtual Servers** (Formularbasierte virtuelle Server)

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/virtual03.png)

    a.  Sie müssen einen **FQDN** angeben, da dies von der Benutzeroberfläche erzwungen wird.

    b.  Wählen Sie den **virtuellen Server für den Lastenausgleich** aus, den Sie mittels Azure AD-Authentifizierung schützen möchten.

    c.  Klicken Sie auf **Bind** (Binden).

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/virtual04.png)

    >[!NOTE]
    >Auf der Konfigurationsseite des virtuellen Authentifizierungsservers muss außerdem auf **Done** (Fertig) geklickt werden.

3. Überprüfen Sie die Änderungen. Navigieren Sie zur Anwendungs-URL. Daraufhin sollte anstelle des vorherigen nicht authentifizierten Zugriffs Ihre Mandantenanmeldeseite angezeigt werden.

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/virtual05.png)

## <a name="configure-citrix-netscaler-single-sign-on-for-header-based-authentication"></a>Konfigurieren des einmaligen Anmeldens von Citrix NetScaler für die headerbasierte Authentifizierung

### <a name="citrix-adc-configuration"></a>Citrix ADC-Konfiguration

### <a name="create-an-rewrite-action"></a>Erstellen einer Aktion zum erneuten Generieren

1. Navigieren Sie zu **AppExpert > Rewrite > Rewrite Actions** (AppExpert > Erneut generieren > Aktionen zum erneuten Generieren).
 
    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/header01.png)

2.  Klicken Sie auf **Hinzufügen**.

    a.  Geben Sie unter **Name** einen Namen an.

    b.  Type (Typ): **INSERT_HTTP_HEADER**

    c.  Geben Sie unter **HEADER NAME** (Headername) den Headernamen an (in diesem Beispiel: SecretID).

    d.  Geben Sie den Ausdruck **aaa.USER.ATTRIBUTE(`mySecretID`)** an. **mySecretID** ist hierbei der an Citrix ADC gesendete Azure AD-SAML-Anspruch.

    e.  Klicken Sie auf **Erstellen**.

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/header02.png)
 
### <a name="create-a-rewrite-policy"></a>Erstellen Sie eine Richtlinie zum erneuten Generieren.

1.  Navigieren Sie zu **AppExpert > Rewrite > Rewrite Actions** (AppExpert > Erneut generieren > Richtlinien zum erneuten Generieren).
 
    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/header03.png)

2.  Klicken Sie auf **Hinzufügen**.

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/header04.png)

    a.  Geben Sie unter **Name** den Namen an.

    b.  Wählen Sie unter **Action** (Aktion) die zuvor erstellte Aktion aus.

    c. Expression (Ausdruck): **true**

    d.  Klicken Sie auf **Create** (Erstellen).

### <a name="bind-rewrite-policy-to-virtual-servers"></a>Binden der Richtlinie zum erneuten Generieren an virtuelle Server

1. So binden Sie eine Richtlinie zum erneuten Generieren unter Verwendung der grafischen Benutzeroberfläche an einen bestimmten virtuellen Server:

2. Navigieren Sie zu **Traffic Management > Load Balancing > Virtual Servers** (Datenverkehrsverwaltung > Lastenausgleich > Virtuelle Server).

3. Wählen Sie im Detailbereich in der Liste mit den **virtuellen Servern** den virtuellen Server aus, an den Sie die Richtlinie zum erneuten Generieren binden möchten, und klicken Sie anschließend auf **Open** (Öffnen).

4. Wählen Sie im Dialogfeld zum Konfigurieren des virtuellen Servers für den Lastenausgleich die Registerkarte **Policies** (Richtlinien) aus. In der Liste werden alle für Ihre NetScaler-Instanz konfigurierten Richtlinien angezeigt.
 
    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/header05.png)

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/header06.png)

5.  Aktivieren Sie das **Kontrollkästchen** neben dem Namen der Richtlinie, die Sie an diesen virtuellen Server binden möchten.

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/header07.png)

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/header08.png)
 
6.  Klicken Sie auf **OK**. Auf der Statusleiste wird eine Meldung mit dem Hinweis angezeigt, dass die Richtlinie erfolgreich konfiguriert wurde.

### <a name="modify-saml-server-to-extract-attributes-from-claim"></a>Ändern des SAML-Servers, um Attribute aus dem Anspruch zu extrahieren

1.  Navigieren Sie zu **Security > AAA - Application Traffic >Policies > Authentication > Advanced Policies > Actions > Servers** (Sicherheit > AAA – Anwendungsdatenverkehr > Richtlinien > Authentifizierung > Erweiterte Richtlinien > Aktionen > Server).

2.  Wählen Sie den entsprechenden **SAML-Authentifizierungsserver** für die Anwendung aus.
 
    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/header09.png)

3. Geben im Attributabschnitt die zu extrahierenden SAML-Attribute ein, und trennen Sie sie jeweils durch ein Komma. In diesem Fall geben wir das Attribut **mySecretID** an.
 
    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/header10.png)

4. Überprüfen Sie meinen Zugriff auf die Anwendungen.

    ![Citrix NetScaler-Konfiguration](./media/header-citrix-netscaler-tutorial/header11.png)

### <a name="create-citrix-netscaler-test-user"></a>Erstellen eines Citrix NetScaler-Testbenutzers

In diesem Abschnitt wird in Citrix NetScaler ein Benutzer namens B.Simon erstellt. Citrix NetScaler unterstützt die Just-In-Time-Benutzerbereitstellung (standardmäßig aktiviert). Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in Citrix NetScaler vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

> [!NOTE]
> Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem [Supportteam für den Citrix NetScaler-Client](https://www.citrix.com/contact/technical-support.html) in Verbindung.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Citrix NetScaler“ klicken, sollten Sie automatisch bei der Citrix NetScaler-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Ausprobieren von Citrix NetScaler mit Azure AD](https://aad.portal.azure.com/)

- [Konfigurieren des einmaligen Anmeldens von Citrix NetScaler für die Kerberos-basierte Authentifizierung](citrix-netscaler-tutorial.md)
