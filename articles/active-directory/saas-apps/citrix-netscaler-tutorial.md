---
title: 'Tutorial: Integration des einmaligen Anmeldens von Azure Active Directory mit Citrix NetScaler (Kerberos-basierte Authentifizierung) | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden (Single Sign-On, SSO) zwischen Azure Active Directory und Citrix NetScaler mit Kerberos-basierter Authentifizierung konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: af501bd0-8ff5-468f-9b06-21e607ae25de
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/27/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6771060f05a03c82879738dc5e8caccb67e55abc
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478005"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-citrix-netscaler-kerberos-based-authentication"></a>Tutorial: Integration des einmaligen Anmeldens von Azure Active Directory mit Citrix NetScaler (Kerberos-basierte Authentifizierung)

In diesem Tutorial erfahren Sie, wie Sie Citrix NetScaler in Azure Active Directory (Azure AD) integrieren. Die Integration von Citrix NetScaler in Azure AD ermöglicht Folgendes:

* Sie können in Azure AD steuern, wer Zugriff auf Citrix NetScaler haben soll.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Citrix NetScaler anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps (Software as a Service) mit Azure AD finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* SSO-fähiges Citrix NetScaler-Abonnement

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung. Das Tutorial umfasst folgende Szenarien:

* **SP-initiiertes** einmaliges Anmelden für Citrix NetScaler

* **Just-In-Time**-Benutzerbereitstellung für Citrix NetScaler

* [Kerberos-basierte Authentifizierung für Citrix NetScaler](#publish-the-web-server)

* [Headerbasierte Authentifizierung für Citrix NetScaler](header-citrix-netscaler-tutorial.md#publish-the-web-server)

* Nach dem Konfigurieren von Citrix NetScaler können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.

## <a name="add-citrix-netscaler-from-the-gallery"></a>Hinzufügen von Citrix NetScaler über den Katalog

Um Citrix NetScaler in Azure AD zu integrieren, müssen Sie Citrix NetScaler zunächst aus dem Katalog Ihrer Liste mit verwalteten SaaS-Apps hinzufügen:

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Menü **Azure Active Directory** aus.

1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.

1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Citrix NetScaler** in das Suchfeld ein.

1. Wählen Sie in den Ergebnissen die Option **Citrix NetScaler** aus, und fügen Sie die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-citrix-netscaler"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Citrix NetScaler

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Citrix NetScaler mithilfe eines Testbenutzers namens **B.Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Citrix NetScaler eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Citrix NetScaler die folgenden Schritte aus:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso), um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.

    1. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden von Azure AD mit B.Simon zu testen.

    1. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um B.Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.

1. [Konfigurieren des einmaligen Anmeldens für Citrix NetScaler](#configure-citrix-netscaler-sso), um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.

    * [Erstellen eines Citrix NetScaler-Testbenutzers](#create-a-citrix-netscaler-test-user), um in Citrix NetScaler eine Entsprechung von B.Simon zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.

1. [Testen des einmaligen Anmeldens](#test-sso), um zu überprüfen, ob die Konfiguration funktioniert.

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Führen Sie die folgenden Schritte aus, um einmaliges Anmelden von Azure AD über das Azure-Portal zu aktivieren:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) im Anwendungsintegrationsbereich für **Citrix NetScaler** unter **Verwalten** die Option **Einmaliges Anmelden** aus.

1. Wählen Sie im Bereich **SSO-Methode auswählen** die Methode **SAML** aus.

1. Wählen Sie im Bereich **Einmaliges Anmelden (SSO) mit SAML einrichten** für **Grundlegende SAML-Konfiguration** die Option **Bearbeiten** (Stiftsymbol) aus, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Gehen Sie im Abschnitt **Grundlegende SAML-Konfiguration** wie folgt vor, um die Anwendung im **IDP-initiierten** Modus zu konfigurieren:

    1. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<Your FQDN>`

    1. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `http(s)://<Your FQDN>.of.vserver/cgi/samlauth`

1. Wenn Sie die Anwendung im **SP-initiierten** Modus konfigurieren möchten, wählen Sie **Zusätzliche URLs festlegen** aus, und führen Sie den folgenden Schritt aus:

    * Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<Your FQDN>/CitrixAuthService/AuthService.asmx`

    > [!NOTE]
    > * Die in diesem Abschnitt angegebenen URLs sind lediglich Beispielwerte. Ersetzen Sie diese Werte durch die tatsächlichen Werte für Bezeichner, Antwort-URL und Anmelde-URL. Die Werte erhalten Sie vom [Supportteam für den Citrix NetScaler-Client](https://www.citrix.com/contact/technical-support.html). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.
    > * Für die SSO-Einrichtung müssen die URLs über öffentliche Websites zugänglich sein. Sie müssen die Firewall- oder andere Sicherheitseinstellungen aufseiten von Citrix NetScaler aktivieren, um Azure AD die Veröffentlichung des Tokens unter der konfigurierten URL zu ermöglichen.

1. Kopieren Sie im Bereich **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** die URL unter **App-Verbundmetadaten-URL**, und speichern Sie sie in Editor.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **Citrix NetScaler einrichten** die relevanten URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer namens B.Simon.

1. Wählen Sie im linken Menü des Azure-Portals Folgendes aus: **Azure Active Directory** > **Benutzer** > **Alle Benutzer**.

1. Wählen Sie im oberen Bereich die Option **Neuer Benutzer** aus.

1. Führen Sie in den Eigenschaften für **Benutzer** die folgenden Schritte aus:

   1. Geben Sie unter **Name**`B.Simon` ein.  

   1. Geben Sie unter **Benutzername** einen Benutzernamen im Format _username@companydomain.extension_ ein. Beispiel: `B.Simon@contoso.com`.

   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den im Feld **Kennwort** angezeigten Wert, oder kopieren Sie ihn.

   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie dem Benutzer B.Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihm Zugriff auf Citrix NetScaler gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.

1. Wählen Sie in der Liste der Anwendungen **Citrix NetScaler** aus.

1. Wählen Sie in der App-Übersicht unter **Verwalten** die Option **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Klicken Sie auf **Benutzer hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste **Benutzer** die Option **B.Simon** aus. Wählen Sie **Auswählen**.

1. Falls Sie in der SAML-Assertion einen Rollenwert erwarten, wählen Sie in der Liste des Dialogfelds **Rolle auswählen** die relevante Rolle für den Benutzer und anschließend **Auswählen** aus.

1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.

## <a name="configure-citrix-netscaler-sso"></a>Konfigurieren des einmaligen Anmeldens für Citrix NetScaler

Wählen Sie einen der Links aus, um die Schritte für die Art der Authentifizierung anzuzeigen, die Sie konfigurieren möchten:

- [Konfigurieren des einmaligen Anmeldens von Citrix NetScaler für die Kerberos-basierte Authentifizierung](#publish-the-web-server)

- [Konfigurieren des einmaligen Anmeldens von Citrix NetScaler für die headerbasierte Authentifizierung](header-citrix-netscaler-tutorial.md#publish-the-web-server)

### <a name="publish-the-web-server"></a>Veröffentlichen des Webservers 

So erstellen Sie einen virtuellen Server:

1. Wählen Sie **Traffic Management** > **Load Balancing** > **Services** (Datenverkehrsverwaltung > Lastenausgleich > Dienste) aus.
    
1. Wählen Sie **Hinzufügen**.

    ![Citrix NetScaler-Konfiguration: Bereich „Services“ (Dienste)](./media/citrix-netscaler-tutorial/web01.png)

1. Legen Sie die folgenden Werte für den Webserver fest, auf dem die Anwendungen ausgeführt werden:

   * **Dienstname**
   * **IP-Adresse des Servers/vorhandener Server**
   * **Protokoll**
   * **Port**

### <a name="configure-the-load-balancer"></a>Konfigurieren Sie den Load Balancer.

So konfigurieren Sie den Lastenausgleich:

1. Navigieren Sie zu **Traffic Management** > **Load Balancing** > **Virtual Servers** (Datenverkehrsverwaltung > Lastenausgleich > Virtuelle Server).

1. Wählen Sie **Hinzufügen**.

1. Legen Sie die folgenden Werte wie im bereitgestellten Screenshot fest:

    * **Name**
    * **Protokoll**
    * **IP-Adresse**
    * **Port**

1. Klicken Sie auf **OK**.

    ![Citrix NetScaler-Konfiguration: Bereich „Basic Settings“ (Grundeinstellungen)](./media/citrix-netscaler-tutorial/load01.png)

### <a name="bind-the-virtual-server"></a>Binden des virtuellen Servers

So binden Sie den Lastenausgleich mit dem virtuellen Server:

1. Wählen Sie im Bereich **Services and Service Groups** (Dienste und Dienstgruppen) die Option **No Load Balancing Virtual Server Service Binding** (Keine Dienstbindung für virtuellen Lastenausgleichsserver) aus.

   ![Citrix NetScaler-Konfiguration: Bereich für die Dienstbindung für den virtuellen Lastenausgleichsserver](./media/citrix-netscaler-tutorial/bind01.png)

1. Vergewissern Sie sich, dass die Einstellungen wie im folgenden Screenshot festgelegt sind, und wählen Sie anschließend **Close** (Schließen) aus.

   ![Citrix NetScaler-Konfiguration: Überprüfen der Dienstbindung für den virtuellen Server](./media/citrix-netscaler-tutorial/bind02.png)

### <a name="bind-the-certificate"></a>Binden des Zertifikats

Wenn Sie diesen Dienst als TLS veröffentlichen möchten, muss das Serverzertifikat gebunden und die Anwendung anschließend getestet werden:

1. Wählen Sie unter **Certificate** (Zertifikat) die Option **No Server Certificate** (Kein Serverzertifikat) aus.

   ![Citrix NetScaler-Konfiguration: Bereich „Server Certificate“ (Serverzertifikat)](./media/citrix-netscaler-tutorial/bind03.png)

1. Vergewissern Sie sich, dass die Einstellungen wie im folgenden Screenshot festgelegt sind, und wählen Sie anschließend **Close** (Schließen) aus.

   ![Citrix NetScaler-Konfiguration: Überprüfen des Zertifikats](./media/citrix-netscaler-tutorial/bind04.png)

## <a name="citrix-adc-saml-profile"></a>Citrix ADC-SAML-Profil

Durchlaufen Sie zum Konfigurieren des Citrix ADC-SAML-Profils die folgenden Abschnitte:

### <a name="create-an-authentication-policy"></a>Erstellen einer Authentifizierungsrichtlinie

So erstellen Sie eine Authentifizierungsrichtlinie:

1. Navigieren Sie zu **Security** > **AAA – Application Traffic** > **Policies** > **Authentication** > **Authentication Policies** (Sicherheit > AAA – Anwendungsdatenverkehr > Richtlinien > Authentifizierung > Authentifizierungsrichtlinien).

1. Wählen Sie **Hinzufügen**.

1. Geben Sie im Bereich **Create Authentication Policy** (Authentifizierungsrichtlinie erstellen) die folgenden Werte ein, oder wählen Sie sie aus:

    * **Name**: Geben Sie einen Namen für Ihre Authentifizierungsrichtlinie ein.
    * **Aktion:** Geben Sie **SAML** ein, und wählen Sie **Add** (Hinzufügen) aus.
    * **Expression** (Ausdruck):  Geben Sie **true** ein.     
    
    ![Citrix NetScaler-Konfiguration: Bereich „Create Authentication Policy“ (Authentifizierungsrichtlinie erstellen)](./media/citrix-netscaler-tutorial/policy01.png)

1. Klicken Sie auf **Erstellen**.

### <a name="create-an-authentication-saml-server"></a>Erstellen eines SAML-Authentifizierungsservers

Navigieren Sie zum Erstellen eines SAML-Authentifizierungsservers zum Bereich **Create Authentication SAML Server** (SAML-Authentifizierungsserver erstellen), und führen Sie die folgenden Schritte aus:

1. Geben Sie unter **Name** einen Namen für den SAML-Authentifizierungsserver ein.

1. Gehen Sie unter **Export SAML Metadata** (SAML-Metadaten exportieren) wie folgt vor:

   1. Aktivieren Sie das Kontrollkästchen **Import Metadata** (Metadaten importieren).

   1. Geben Sie die Verbundmetadaten-URL ein, die Sie zuvor auf der Azure-SAML-Benutzeroberfläche kopiert haben.
    
1. Geben Sie unter **Issuer Name** (Ausstellername) die relevante URL ein.

1. Klicken Sie auf **Erstellen**.

![Citrix NetScaler-Konfiguration: Bereich „Create Authentication SAML Server“ (SAML-Authentifizierungsserver erstellen)](./media/citrix-netscaler-tutorial/server01.png)

### <a name="create-an-authentication-virtual-server"></a>Erstellen eines virtuellen Authentifizierungsservers

So erstellen Sie einen virtuellen Authentifizierungsserver:

1.  Navigieren Sie zu **Security** > **AAA - Application Traffic** > **Policies** > **Authentication** > **Authentication Virtual Servers** (Sicherheit > AAA – Anwendungsdatenverkehr > Richtlinien > Authentifizierung > Virtuelle Authentifizierungsserver).

1.  Wählen Sie **Add** (Hinzufügen) aus, und gehen Sie anschließend wie folgt vor:

    1. Geben Sie unter **Name** einen Namen für den virtuellen Authentifizierungsserver ein.

    1. Aktivieren Sie das Kontrollkästchen **Non-Addressable** (Nicht adressierbar).

    1. Wählen Sie unter **Protocol** (Protokoll) die Option **SSL** aus.

    1. Klicken Sie auf **OK**.
    
1. Wählen Sie **Weiter**.

### <a name="configure-the-authentication-virtual-server-to-use-azure-ad"></a>Konfigurieren des virtuellen Authentifizierungsservers für die Verwendung von Azure AD

Ändern Sie zwei Abschnitte für den virtuellen Authentifizierungsserver:

1.  Wählen Sie im Bereich **Advanced Authentication Policies** (Erweiterte Authentifizierungsrichtlinien) die Option **No Authentication Policy** (Keine Authentifizierungsrichtlinie) aus.

    ![Citrix NetScaler-Konfiguration: Bereich „Advanced Authentication Policies“ (Erweiterte Authentifizierungsrichtlinien)](./media/citrix-netscaler-tutorial/virtual01.png)

1. Wählen Sie im Bereich **Policy Binding** (Richtlinienbindung) die Authentifizierungsrichtlinie und anschließend **Bind** (Binden) aus.

    ![Citrix NetScaler-Konfiguration: Bereich „Policy Binding“ (Richtlinienbindung)](./media/citrix-netscaler-tutorial/virtual02.png)

1. Wählen Sie im Bereich **Form Based Virtual Servers** (Formularbasierte virtuelle Server) die Option **No Load Balancing Virtual Server** (Kein virtueller Lastenausgleichsserver) aus.

    ![Citrix NetScaler-Konfiguration: Bereich „Form Based Virtual Servers“ (Formularbasierte virtuelle Server)](./media/citrix-netscaler-tutorial/virtual03.png)

1. Geben Sie unter **Authentication FQDN** (FQDN für die Authentifizierung) einen vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) ein. (Diese Angabe ist erforderlich.)

1. Wählen Sie den virtuellen Lastenausgleichsserver aus, den Sie mittels Azure AD-Authentifizierung schützen möchten.

1. Wählen Sie **Bind** (Binden) aus.

    ![Citrix NetScaler-Konfiguration: Bereich zum Binden des virtuellen Lastenausgleichsservers](./media/citrix-netscaler-tutorial/virtual04.png)

    > [!NOTE]
    > Wählen Sie im Bereich **Authentication Virtual Server Configuration** (Konfiguration des virtuellen Authentifizierungsservers) die Option **Done** (Fertig) aus.

1. Navigieren Sie zum Überprüfen Ihrer Änderungen in einem Browser zur Anwendungs-URL. Daraufhin sollte anstelle des vorherigen nicht authentifizierten Zugriffs Ihre Mandantenanmeldeseite angezeigt werden.

    ![Citrix NetScaler-Konfiguration: Anmeldeseite in einem Webbrowser](./media/citrix-netscaler-tutorial/virtual05.png)

## <a name="configure-citrix-netscaler-sso-for-kerberos-based-authentication"></a>Konfigurieren des einmaligen Anmeldens von Citrix NetScaler für die Kerberos-basierte Authentifizierung

### <a name="create-a-kerberos-delegation-account-for-citrix-adc"></a>Erstellen eines Kerberos-Delegierungskontos für Citrix ADC

1. Erstellen Sie ein Benutzerkonto. (In diesem Beispiel wird _AppDelegation_ verwendet.)

    ![Citrix NetScaler-Konfiguration: Bereich „Properties“ (Eigenschaften)](./media/citrix-netscaler-tutorial/kerberos01.png)

1. Richten Sie einen Host-SPN für dieses Konto ein. 

    Beispiel: `setspn -S HOST/AppDelegation.IDENTT.WORK identt\appdelegation`
    
    In diesem Beispiel:

    * `IDENTT.WORK` ist der vollqualifizierte Domänenname.
    * `identt` ist der NetBIOS-Name der Domäne.
    * `appdelegation` ist der Name des Delegierungsbenutzerkontos.

1. Konfigurieren Sie die Delegierung für den Webserver, wie im folgenden Screenshot zu sehen:
 
    ![Citrix NetScaler-Konfiguration: Delegierung im Bereich „Properties“ (Eigenschaften)](./media/citrix-netscaler-tutorial/kerberos02.png)

    > [!NOTE]
    > Im Beispiel auf dem Screenshot hat der interne Webserver, auf dem die WIA-Website (Windows Integrated Authentication) ausgeführt wird, den Namen _CWEB2_.

### <a name="citrix-netscaler-aaa-kcd-kerberos-delegation-accounts"></a>Citrix NetScaler AAA KCD (Kerberos-Delegierungskonten)

So konfigurieren Sie das Citrix NetScaler AAA KCD-Konto:

1.  Navigieren Sie zu **Citrix Gateway** > **AAA KCD (Kerberos Constrained Delegation) Accounts** (Citrix-Gateway > AAA KCD-Konten (Kerberos Constrained Delegation)).

1.  Wählen Sie **Add** (Hinzufügen) aus, und geben Sie die folgenden Werte ein, oder wählen Sie sie aus:

    * **Name**: Geben Sie einen Namen für das KCD-Konto ein.

    * **Realm** (Bereich): Geben Sie die Domäne und die Erweiterung in Großbuchstaben ein.

    * **Service SPN** (Dienst-SPN): `http/<host/fqdn>@<DOMAIN.COM>`
    
        > [!NOTE]
        > `@DOMAIN.COM` ist erforderlich und muss in Großbuchstaben angegeben werden. Beispiel: `http/cweb2@IDENTT.WORK`.

    * **Delegated User** (Delegierter Benutzer): Geben Sie den Namen des delegierten Benutzers ein.

    * Aktivieren Sie das Kontrollkästchen **Password for Delegated User** (Kennwort für delegierten Benutzer), geben Sie ein Kennwort ein, und bestätigen Sie es.

1. Klicken Sie auf **OK**.
 
    ![Citrix NetScaler-Konfiguration: Bereich zum Konfigurieren des KCD-Kontos](./media/citrix-netscaler-tutorial/kerberos03.png)

### <a name="citrix-traffic-policy-and-traffic-profile"></a>Citrix-Datenverkehrsrichtlinie und -Datenverkehrsprofil

So konfigurieren Sie die Citrix-Datenverkehrsrichtlinie und das Datenverkehrsprofil

1.  Navigieren Sie zu **Security** > **AAA - Application Traffic** > **Policies** > **Traffic Policies, Profiles and Form SSO ProfilesTraffic Policies** (Sicherheit > AAA – Anwendungsdatenverkehr > Richtlinien > Datenverkehrsrichtlinien, Profile und SSO-Formularprofile).

1.  Wählen Sie **Traffic Profiles** (Datenverkehrsprofile) aus.

1.  Wählen Sie **Hinzufügen**.

1.  Geben Sie zum Konfigurieren eines Datenverkehrsprofils die folgenden Werte ein, oder wählen Sie sie aus:

    * **Name**: Geben Sie einen Namen für das Datenverkehrsprofil ein.

    * **Single Sign-on** (Einmaliges Anmelden): Wählen Sie **ON** (EIN) aus.

    * **KCD Account** (KCD-Konto): Wählen Sie das im vorherigen Abschnitt erstellte KCD-Konto aus.

1. Klicken Sie auf **OK**.

    ![Citrix NetScaler-Konfiguration: Bereich zum Konfigurieren des Datenverkehrsprofils](./media/citrix-netscaler-tutorial/kerberos04.png)
 
1.  Wählen Sie **Traffic Policy** (Datenverkehrsrichtlinie) aus.

1.  Wählen Sie **Hinzufügen**.

1.  Geben Sie zum Konfigurieren einer Datenverkehrsrichtlinie die folgenden Werte ein, oder wählen Sie sie aus:

    * **Name**: Geben Sie einen Namen für die Datenverkehrsrichtlinie ein.

    * **Profil:** Wählen Sie die im vorherigen Abschnitt erstellte Datenverkehrsrichtlinie aus.

    * **Expression** (Ausdruck): Geben Sie **true** ein.

1. Klicken Sie auf **OK**.

    ![Citrix NetScaler-Konfiguration: Bereich zum Konfigurieren der Datenverkehrsrichtlinie](./media/citrix-netscaler-tutorial/kerberos05.png)

### <a name="bind-a-traffic-policy-to-a-virtual-server-in-citrix"></a>Binden einer Datenverkehrsrichtlinie an einen virtuellen Server in Citrix

So binden Sie eine Datenverkehrsrichtlinie unter Verwendung der grafischen Benutzeroberfläche an einen virtuellen Server:

1. Navigieren Sie zu **Traffic Management** > **Load Balancing** > **Virtual Servers** (Datenverkehrsverwaltung > Lastenausgleich > Virtuelle Server).

1. Wählen Sie in der Liste mit den virtuellen Servern den virtuellen Server aus, an den Sie die Richtlinie zum erneuten Generieren binden möchten, und wählen Sie anschließend **Open** (Öffnen) aus.

1. Wählen Sie im Bereich **Load Balancing Virtual Server** (Virtueller Lastenausgleichsserver) unter **Advanced Settings** (Erweiterte Einstellungen) die Option **Policies** (Richtlinien) aus. Die Liste enthält alle für Ihre NetScaler-Instanz konfigurierten Richtlinien.
 
    ![Citrix NetScaler-Konfiguration: Bereich „Load Balancing Virtual Server“ (Virtueller Lastenausgleichsserver)](./media/citrix-netscaler-tutorial/kerberos06.png)

    ![Citrix NetScaler-Konfiguration: Dialogfeld „Policies“ (Richtlinien)](./media/citrix-netscaler-tutorial/kerberos07.png)

1.  Aktivieren Sie das Kontrollkästchen neben dem Namen der Richtlinie, die Sie an diesen virtuellen Server binden möchten.
 
    ![Citrix NetScaler-Konfiguration: Bereich zum Binden der Datenverkehrsrichtlinie für den virtuellen Lastenausgleichsserver](./media/citrix-netscaler-tutorial/kerberos09.png)

1. Gehen Sie im Dialogfeld **Choose Type** (Typ auswählen) wie folgt vor:

    1. Wählen Sie unter **Choose Policy** (Richtlinie auswählen) die Option **Traffic** (Datenverkehr) aus.

    1. Wählen Sie unter **Choose Type** (Typ auswählen) die Option **Request** (Anforderung) aus.

    ![Citrix NetScaler-Konfiguration: Bereich „Choose Type“ (Typ auswählen)](./media/citrix-netscaler-tutorial/kerberos08.png)

1. Wählen Sie nach dem Binden der Richtlinie die Option **Done** (Fertig) aus.
 
    ![Citrix NetScaler-Konfiguration: Bereich „Policies“ (Richtlinien)](./media/citrix-netscaler-tutorial/kerberos10.png)

1. Testen Sie die Bindung mithilfe der WIA-Website.

    ![Citrix NetScaler-Konfiguration: Testseite in einem Webbrowser](./media/citrix-netscaler-tutorial/kerberos11.png)    

### <a name="create-a-citrix-netscaler-test-user"></a>Erstellen eines Citrix NetScaler-Testbenutzers

In diesem Abschnitt wird in Citrix NetScaler ein Benutzer namens B.Simon erstellt. Citrix NetScaler unterstützt die Just-In-Time-Benutzerbereitstellung (standardmäßig aktiviert). In diesem Abschnitt ist keine Aktion erforderlich. Ist ein Benutzer noch nicht in Citrix NetScaler vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

> [!NOTE]
> Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem [Supportteam für den Citrix NetScaler-Client](https://www.citrix.com/contact/technical-support.html) in Verbindung.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „Citrix NetScaler“ auswählen, sollten Sie automatisch bei der Citrix NetScaler-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Ausprobieren von Citrix NetScaler mit Azure AD](https://aad.portal.azure.com/)

- [Konfigurieren des einmaligen Anmeldens von Citrix NetScaler für die headerbasierte Authentifizierung](header-citrix-netscaler-tutorial.md)

- [Was ist Sitzungssteuerung in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Schützen von Apps mit der App-Steuerung für bedingten Zugriff von Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)