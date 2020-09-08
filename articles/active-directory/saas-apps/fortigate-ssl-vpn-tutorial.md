---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit FortiGate SSL VPN | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und FortiGate SSL VPN konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 18a3d9d5-d81c-478c-be7e-ef38b574cb88
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/11/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ecb53d661b1171f9c1b18d37d0bb35952645ba7e
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89299710"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-fortigate-ssl-vpn"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit FortiGate SSL VPN

In diesem Tutorial erfahren Sie, wie Sie FortiGate SSL VPN in Azure Active Directory (Azure AD) integrieren. Die Integration von FortiGate SSL VPN in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf FortiGate SSL VPN hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei FortiGate SSL VPN anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* FortiGate SSL VPN-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* FortiGate SSL VPN unterstützt **SP-initiiertes** einmaliges Anmelden.
* Nach dem Konfigurieren von FortiGate SSL VPN können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.

## <a name="adding-fortigate-ssl-vpn-from-the-gallery"></a>Hinzufügen von FortiGate SSL VPN aus dem Katalog

Zum Konfigurieren der Integration von FortiGate SSL VPN in Azure AD müssen Sie FortiGate SSL VPN aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **FortiGate SSL VPN** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **FortiGate SSL VPN** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-fortigate-ssl-vpn"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für FortiGate SSL VPN

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit FortiGate SSL VPN mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in FortiGate SSL VPN eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit FortiGate SSL VPN die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für FortiGate SSL VPN](#configure-fortigate-ssl-vpn-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **Erstellen eines FortiGate SSL VPN-Testbenutzers** , um eine Entsprechung von B. Simon in FortiGate SSL VPN zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **FortiGate SSL VPN** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** die Werte für die folgenden Felder ein:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<FQDN>/remote/login`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<FQDN>/remote/saml/metadata`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://><FQDN/remote/saml/login`

    d. Geben Sie im Textfeld **Abmelde-URL** eine URL im folgenden Format ein: `https://<FQDN>/remote/saml/logout`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit den tatsächlichen Werten für Anmelde-URL, Bezeichner, Antwort-URL und Abmelde-URL. Diese Werte erhalten Sie vom [Supportteam für den FortiGate SSL VPN-Client](mailto:tac_amer@fortinet.com). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Die FortiGate SSL VPN-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute.

    ![image](common/default-attributes.png)

1. Darüber hinaus erwartet die FortiGate SSL VPN-Anwendung, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden (siehe unten). Diese Attribute werden ebenfalls vorab aufgefüllt, Sie können sie jedoch nach Bedarf überprüfen.
    
    | Name |  Quellattribut|
    | ------------ | --------- |
    | username | user.userprincipalname |
    | Gruppe | user.groups |

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zum Eintrag **Zertifikat (Base64)** . Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **FortiGate SSL VPN einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf FortiGate SSL VPN gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **FortiGate SSL VPN** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-a-security-group-for-the-test-user"></a>Erstellen einer Sicherheitsgruppe für den Testbenutzer

In diesem Abschnitt erstellen Sie in Azure Active Directory eine Sicherheitsgruppe für den Testbenutzer. Die Sicherheitsgruppe wird von FortiGate verwendet, um dem Benutzer Netzwerkzugriff über das VPN zu gewähren.

1. Wählen Sie im linken Bereich des Azure-Portals **Azure Active Directory** und dann **Gruppen** aus.
1. Wählen Sie am oberen Bildschirmrand die Option **Neue Gruppe** aus.
1. Gehen Sie in den Eigenschaften für **Neue Gruppe** folgendermaßen vor:
   1. Wählen Sie im Feld **Gruppentyp** die Option **Sicherheit** aus.
   1. Geben Sie im Feld **Name** die Zeichenfolge `FortiGateAccess` ein.
   1. Geben Sie im Feld **Gruppenbeschreibung** den Text `Group for granting FortiGate VPN access` ein.
   1. Wählen Sie für die Einstellungen **Azure AD-Rollen können der Gruppe zugewiesen werden (Vorschau)** die Option **Nein** aus.
   1. Wählen Sie im Feld **Mitgliedschaftstyp** die Option **Zugewiesen** aus.
   1. Wählen Sie unter **Mitglieder** die Option **Keine Mitglieder ausgewählt** aus.
   1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
   1. Klicken Sie auf **Erstellen**.
1. Nachdem Sie zum Blatt **Gruppen** in Azure Active Directory zurückgekehrt sind, suchen Sie die Gruppe **FortiGate-Zugriff**, und notieren Sie sich die **Objekt-ID** zur späteren Verwendung.

## <a name="configure-fortigate-ssl-vpn-sso"></a>Konfigurieren des einmaligen Anmeldens für FortiGate SSL VPN

### <a name="upload-the-base64-saml-certificate-to-the-fortigate-appliance"></a>Hochladen des Base64-SAML-Zertifikats für die FortiGate-Appliance

Nach Abschluss der SAML-Konfiguration der FortiGate-App in Ihrem Mandanten haben Sie das Base64-codierte SAML-Zertifikat heruntergeladen. Dieses muss auf die FortiGate-Appliance hochgeladen werden:

1. Melden Sie sich beim Verwaltungsportal Ihrer FortiGate-Appliance an.
1. Klicken Sie im Menü auf der linken Seite auf **System**.
1. Klicken Sie unter **System** auf **Certificates** (Zertifikate).
1. Klicken Sie auf **Import** -> **Remote Certificate** (Importieren > Remotezertifikat).
1. Navigieren Sie zu dem Zertifikat, das bei der FortiGate-App-Bereitstellung im Azure-Mandanten heruntergeladen wurde, wählen Sie es aus, und klicken Sie auf **OK**.

Nachdem das Zertifikat hochgeladen wurde, notieren Sie sich den Namen, der unter **System** > **Certificates** > **Remote Certificate** (System > Zertifikate > Remotezertifikat) angezeigt wird. Der Standardname lautet „REMOTE_Cert_**N**“. **N** ist dabei eine ganze Zahl.

### <a name="perform-fortigate-command-line-configuration"></a>Ausführen der Befehlszeilenkonfiguration für FortiGate

Für die folgenden Schritte muss die Azure-Abmelde-URL konfiguriert sein. Diese URL enthält ein Fragezeichen (?). Damit dieses Zeichen erfolgreich übermittelt wird, sind einige zusätzliche Schritte erforderlich. Diese Schritte können nicht an der CLI-Konsole von FortiGate ausgeführt werden. Richten Sie stattdessen mithilfe eines Tools wie z. B. PuTTY eine SSH-Sitzung mit der FortiGate-Appliance ein. Wenn es sich bei Ihrer FortiGate-Appliance um eine Azure-VM handelt, können Sie die folgenden Schritte an der seriellen Konsole für Azure-VMs ausführen.

Hierfür benötigen Sie die zuvor erfassten Werte:

- Entitäts-ID
- Antwort-URL
- Abmelde-URL
- Azure-Anmelde-URL
- Azure AD-Bezeichner
- Azure-Abmelde-URL
- Name des Base64-SAML-Zertifikats (REMOTE_Cert_N)

1. Richten Sie eine SSH-Sitzung mit der FortiGate-Appliance ein, und melden Sie sich mit einem FortiGate-Administratorkonto an.
1. Führen Sie die folgenden Befehle aus:

   ```console
    config user saml
    edit azure
    set entity-id <Entity ID>
    set single-sign-on-url <Reply URL>
    set single-logout-url <Logout URL>
    set idp-single-sign-on-url <Azure Login URL>
    set idp-entity-id <Azure AD Identifier>
    set idp-single-logout-url <Azure Logout URL>
    set idp-cert <Base64 SAML Certificate Name>
    set user-name username
    set group-name group
    end

   ```

   > [!NOTE]
   > Die **Azure-Abmelde-URL** enthält das Zeichen `?`. Sie müssen eine spezielle Schlüsselsequenz eingeben, um die URL ordnungsgemäß an die serielle Konsole für FortiGate zu übermitteln. Die URL lautet in der Regel `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`.
   >
   > Geben Sie die Azure-Abmelde-URL folgendermaßen an der seriellen Konsole ein: `set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation`.
   > 
   > Fügen Sie dann die restliche URL per STRG+V ein, um die Zeile zu vervollständigen: `set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`.

### <a name="configure-fortigate-for-group-matching"></a>Konfigurieren von FortiGate für den Gruppenabgleich

In diesem Abschnitt konfigurieren Sie FortiGate so, dass die Objekt-ID der Sicherheitsgruppe erkannt wird, in der sich der Testbenutzer befindet. So kann FortiGate Entscheidungen basierend auf der Mitglied in dieser Gruppe treffen.

Für diese Schritte benötigen Sie die Objekt-ID der zuvor erstellten Sicherheitsgruppe **FortiGateAccess**.

1. Richten Sie eine SSH-Sitzung mit der FortiGate-Appliance ein, und melden Sie sich mit einem FortiGate-Administratorkonto an.
1. Führen Sie die folgenden Befehle aus:

   ```
    config user group
    edit FortiGateAccess
    set member azure
    config match
    edit 1
    set server-name azure
    set group-name <Object Id>
    next
    end
    next
    end
   ```

### <a name="create-fortigate-vpn-portals-and-firewall-policy"></a>Erstellen von VPN-Portalen und einer Firewallrichtlinie in FortiGate

In diesem Abschnitt konfigurieren Sie VPN-Portale und eine Firewallrichtlinie in FortiGate, um Zugriff auf die weiter oben erstellte Sicherheitsgruppe **FortiGateAccess** zu gewähren.

Arbeiten Sie mit dem  [FortiGate-Supportteam](mailto:tac_amer@fortinet.com) zusammen, um die VPN-Portale und Firewallrichtlinie zur FortiGate-VPN-Plattform hinzuzufügen. Diese Schritt müssen abgeschlossen sein, bevor das einmalige Anmelden genutzt werden kann.

## <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „FortiGate SSL VPN“ klicken, sollten Sie automatisch bei der FortiGate SSL VPN-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

Für ein optimales Benutzererlebnis empfehlen Microsoft und FortiGate die Verwendung des Fortinet-VPN-Clients: FortiClient.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [FortiGate SSL VPN mit Azure AD ausprobieren](https://aad.portal.azure.com/)

- [Was ist Sitzungssteuerung in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
