---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit EasySSO for BitBucket | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und EasySSO for BitBucket konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3ed96e97-b590-4dca-8d00-36288444c641
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 05/18/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 946785f02461706d1d3675d089c19d68816fb8ca
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86077098"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-easysso-for-bitbucket"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit EasySSO for BitBucket

In diesem Tutorial erfahren Sie, wie Sie EasySSO for BitBucket in Azure Active Directory (Azure AD) integrieren. Die Integration von EasySSO for BitBucket in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf EasySSO for BitBucket hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei EasySSO for BitBucket anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps (Software as a Service) mit Azure AD finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes:

* Ein Azure AD-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein SSO-fähiges EasySSO for BitBucket-Abonnement.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* EasySSO for BitBucket unterstützt SP- und IdP-initiiertes einmaliges Anmelden.
* EasySSO for BitBucket unterstützt die Just-In-Time-Benutzerbereitstellung.
* Nach dem Konfigurieren von EasySSO for BitBucket können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.

## <a name="add-easysso-for-bitbucket-from-the-gallery"></a>Hinzufügen von EasySSO for BitBucket aus dem Katalog

Zum Konfigurieren der Integration von EasySSO for BitBucket in Azure AD müssen Sie EasySSO for BitBucket aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **EasySSO for BitBucket** in das Suchfeld ein.
1. Wählen Sie in den Ergebnissen die Option **EasySSO for BitBucket** aus, und fügen Sie die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-single-sign-on-for-easysso-for-bitbucket"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für EasySSO for BitBucket

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit EasySSO for BitBucket mithilfe eines Testbenutzers namens **B.Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in EasySSO for BitBucket eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit EasySSO for BitBucket die folgenden Schritte aus:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso), um Ihren Benutzern die Verwendung dieses Features zu ermöglichen
    1. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden von Azure AD mit B.Simon zu testen
    1. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um B.Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen
1. [Konfigurieren des einmaligen Anmeldens für EasySSO for BitBucket](#configure-easysso-for-bitbucket-sso), um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. [Erstellen eines EasySSO for BitBucket-Testbenutzers](#create-an-easysso-for-bitbucket-test-user), um in EasySSO for BitBucket eine Entsprechung von B.Simon zu erhalten, die mit der Benutzerdarstellung in Azure AD verknüpft ist.
1. [Testen des einmaligen Anmeldens](#test-sso), um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **EasySSO for BitBucket** zum Abschnitt **Verwalten**. Wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** das Stiftsymbol für **Grundlegende SAML-Konfiguration** aus, um die Einstellungen zu bearbeiten.

   ![Screenshot: Seite „Einmaliges Anmelden (SSO) mit SAML einrichten“ mit hervorgehobenem Stiftsymbol](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein, wenn Sie die Anwendung im **IdP-initiierten** Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<server-base-url>/plugins/servlet/easysso/saml`.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<server-base-url>/plugins/servlet/easysso/saml`.

1. Wählen Sie **Zusätzliche URLs festlegen** aus, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten** Modus konfigurieren möchten:

    - Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<server-base-url>/login.jsp`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem tatsächlichen Bezeichner, der Antwort-URL und der Anmelde-URL aktualisieren. Diese Werte erhalten Sie im Zweifelsfall vom [Supportteam für EasySSO](mailto:support@techtime.co.nz). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Die SAML-Assertionen werden von der EasySSO for BitBucket-Anwendung in einem bestimmten Format erwartet. Daher müssen der SAML-Tokenattributkonfiguration benutzerdefinierte Attributzuordnungen hinzugefügt werden. Der folgende Screenshot zeigt die Liste der Standardattribute.

    ![Screenshot der Standardattribute](common/default-attributes.png)

1. Von der EasySSO for BitBucket-Anwendung wird außerdem die Rückgabe einiger weiterer Attribute in der SAML-Antwort erwartet. Diese finden Sie in der folgenden Tabelle. Diese Attribute werden ebenfalls vorab aufgefüllt, Sie können sie jedoch nach Bedarf überarbeiten.
    
    | Name | Quellattribut|
    | ---------------| --------- |
    | urn:oid:0.9.2342.19200300.100.1.1 | user.userprincipalname |
    | urn:oid:0.9.2342.19200300.100.1.3 | user.mail |
    | urn:oid:2.16.840.1.113730.3.1.241 | user.displayname |
    | urn:oid:2.5.4.4 | user.surname |
    | urn:oid:2.5.4.42 | user.givenname |
    
    Falls für Ihre Azure AD-Benutzer **sAMAccountName** konfiguriert ist, muss **urn:oid:0.9.2342.19200300.100.1.1** dem Attribut **sAMAccountName** zugeordnet werden.
    
1. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** den Downloadlink für die Option **Zertifikat (Base64)** oder **Verbundmetadaten-XML** aus. Speichern Sie die heruntergeladenen Elemente auf Ihrem Computer. Sie benötigen sie später für die Konfiguration von BitBucket EasySSO.

    ![Screenshot: Abschnitt „SAML-Signaturzertifikat“ mit hervorgehobenen Downloadlinks](./media/easysso-for-bitbucket-tutorial/certificate.png)
    
    Wenn Sie EasySSO for BitBucket manuell mit einem Zertifikat konfigurieren möchten, müssen Sie außerdem die Werte für **Anmelde-URL** und **Azure AD-Bezeichner** kopieren und auf Ihrem Computer speichern.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt wird über das Azure-Portal ein Testbenutzer namens „B.Simon“ erstellt.

1. Wählen Sie im Azure-Portal im linken Bereich **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie unter **Name**`B.Simon` ein.  
   1. Geben Sie unter **Benutzername** einen Benutzernamen im Format username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich das Kennwort.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf EasySSO for BitBucket gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **EasySSO for BitBucket** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Screenshot: Abschnitt „Verwalten“ mit hervorgehobener Option „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Klicken Sie auf **Benutzer hinzufügen**. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Screenshot: Seite „Benutzer und Gruppen“ mit hervorgehobener Option zum Hinzufügen eines Benutzers](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste **Benutzer** den Eintrag **B. Simon** aus, und wählen Sie dann im unteren Bereich des Bildschirms die Option **Auswählen** aus.
1. Falls Sie in der SAML-Assertion einen Rollenwert erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer aus der Liste aus. Wählen Sie anschließend am unteren Bildschirmrand **Auswählen** aus.
1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.

## <a name="configure-easysso-for-bitbucket-sso"></a>Konfigurieren des einmaligen Anmeldens für EasySSO for BitBucket

1. Melden Sie sich mit Administratorrechten bei Ihrer Atlassian BitBucket-Instanz an, und navigieren Sie zum Abschnitt **Administration** (Verwaltung). 

    ![Screenshot: BitBucket-Instanz mit hervorgehobenem Zahnradsymbol](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-1.png)
1. Suchen Sie nach **EasySSO**, und wählen Sie diese Option aus.

    ![Screenshot: Option „EasySSO“](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-2.png)

1. Wählen Sie **SAML** aus. Dadurch gelangen Sie zum Abschnitt für die SAML-Konfiguration.

    ![Screenshot: EasySSO-Verwaltungsseite mit hervorgehobener Option „SAML“](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-3.png)

1. Wählen Sie die Registerkarte **Certificates** (Zertifikate) aus. Daraufhin wird der folgende Bildschirm angezeigt:

    ![Screenshot: Registerkarte „Certificates“ (Zertifikate) mit verschiedenen hervorgehobenen Optionen](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-4.png)

1. Suchen Sie nach dem **Zertifikat (Base64)** oder nach der **Metadatendatei**, das bzw. die Sie im vorherigen Abschnitt dieses Tutorials gespeichert haben. Fahren Sie mit einem der folgenden Schritte fort:

    - Verwenden Sie die **Metadatendatei** des App-Verbunds, die Sie als lokale Datei auf Ihren Computer heruntergeladen haben. Wählen Sie das Optionsfeld **Hochladen** aus, und folgen Sie dem spezifischen Pfad für Ihr Betriebssystem.

    - Öffnen Sie die **Metadatendatei** des App-Verbunds in einem beliebigen Text-Editor, um den Inhalt der Datei anzuzeigen. Kopieren Sie ihn in die Zwischenablage. Wählen Sie **Input** (Eingabe) aus, und fügen Sie den Inhalt aus der Zwischenablage in das Textfeld ein.
 
    - Führen Sie eine vollständig manuelle Konfiguration durch. Öffnen Sie das **Zertifikat (Base64)** des App-Verbunds in einem beliebigen Text-Editor, um den Inhalt der Datei anzuzeigen. Kopieren Sie ihn Datei in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **IdP Token Signing Certificates** (IdP-Tokensignaturzertifikate) ein. Navigieren Sie dann zur Registerkarte **General** (Allgemein), und füllen Sie die Felder **POST Binding URL** (POST-Bindungs-URL) und **Entity ID** (Entitäts-ID) mit den entsprechenden Werten für **Anmelde-URL** und **Azure AD-Bezeichner** aus, die Sie zuvor gespeichert haben.
 
1. Wählen Sie unten auf der Seite **Save** (Speichern) aus. Daraufhin wird der Inhalt der Metadaten- oder Zertifikatsdatei analysiert und in die Konfigurationsfelder eingefügt. Die EasySSO for BitBucket-Konfiguration ist abgeschlossen.

1. Navigieren Sie zum Testen der Konfiguration zur Registerkarte **Look & Feel** (Erscheinungsbild), und aktivieren Sie das Kontrollkästchen **SAML Login Button** (SAML-Anmeldeschaltfläche). Dadurch wird im BitBucket-Anmeldebildschirm eine separate Schaltfläche aktiviert, mit der Sie Ihre gesamte Azure AD-SAML-Integration testen können. Sie können diese Schaltfläche aktiviert lassen und außerdem ihre Position, Farbe und Übersetzung für den Produktionsmodus konfigurieren.

    ![Screenshot: SAML-Seite mit hervorgehobener Option „SAML Login Button“ (SAML-Anmeldeschaltfläche) auf der Registerkarte „Look & Feel“ (Erscheinungsbild)](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-5.png)
    > [!NOTE]
    >Sollten Probleme auftreten, wenden Sie sich an das [EasySSO-Supportteam](mailto:support@techtime.co.nz).

### <a name="create-an-easysso-for-bitbucket-test-user"></a>Erstellen eines EasySSO for BitBucket-Testbenutzers

In diesem Abschnitt wird in BitBucket ein Benutzer namens Britta Simon erstellt. Von EasySSO for BitBucket wird die Just-In-Time-Benutzerbereitstellung unterstützt. Diese ist jedoch standardmäßig deaktiviert. Um sie zu aktivieren, muss **Create user on successful login** (Benutzer bei erfolgreicher Anmeldung erstellen) im Abschnitt **General** (Allgemein) der EasySSO-Plug-In-Konfiguration explizit aktiviert werden. Ist noch kein Benutzer in BitBucket vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

Falls Sie die automatische Benutzerbereitstellung bei erstmaliger Anmeldung eines Benutzers nicht aktivieren möchten, müssen Benutzer in Benutzerverzeichnissen vorhanden sein, die von der BitBucket-Instanz genutzt werden. Bei einem solchen Verzeichnis kann es sich beispielsweise um LDAP oder um Atlassian Crowd handeln.

![Screenshot: Abschnitt „General“ (Allgemein) der EasySSO-Plug-In-Konfiguration mit hervorgehobener Option „Create user on successful login“ (Benutzer bei erfolgreicher Anmeldung erstellen)](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-6.png)

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

### <a name="idp-initiated-workflow"></a>IdP-initiierter Workflow

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie die Kachel „EasySSO for BitBucket“ auswählen, sollten Sie automatisch bei der BitBucket-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen finden Sie unter [Anmelden beim Portal „Meine Apps“ und Starten von Apps über dieses](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

### <a name="sp-initiated-workflow"></a>SP-initiierter Workflow

In diesem Abschnitt wird die Azure AD-Konfiguration für einmaliges Anmelden über die BitBucket-Schaltfläche **SAML Login** (SAML-Anmeldung) getestet.

![Screenshot: Anmeldebildschirm mit hervorgehobener Schaltfläche für die SAML-Anmeldung](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-7.png)

In diesem Szenario wird davon ausgegangen, dass Sie auf der BitBucket EasySSO-Konfigurationsseite auf der Registerkarte **Look & Feel** (Erscheinungsbild) die Option **SAML Login Button** (SAML-Anmeldeschaltfläche) aktiviert haben. Öffnen Sie Ihre BitBucket-Anmelde-URL im Inkognitomodus des Browsers, um Beeinträchtigungen der vorhandenen Sitzungen zu vermeiden. Wählen Sie die Schaltfläche **SAML Login** (SAML-Anmeldung) aus. Sie werden zum Azure AD-Benutzerauthentifizierungsflow umgeleitet. Nach erfolgreichem Abschluss des Vorgangs werden Sie als authentifizierter Benutzer über SAML wieder zu Ihrer BitBucket-Instanz umgeleitet.

Nach der Rückkehr von Azure AD wird gegebenenfalls der folgende Bildschirm angezeigt:

![Screenshot: EasySSO-Fehlerbildschirm mit hervorgehobener Referenznummer](./media/easysso-for-bitbucket-tutorial/bitbucket-admin-8.png)

Folgen Sie in diesem Fall den Anweisungen auf [dieser Seite](https://techtime.co.nz/display/TECHTIME/EasySSO+How+to+get+the+logs#EasySSOHowtogetthelogs-RETRIEVINGTHELOGS), um Zugriff auf die Datei **atlassian-bitbucket.log** zu erhalten. Details zum Fehler können anhand der Referenz-ID auf der EasySSO-Fehlerseite ermittelt werden.

Sollten Probleme auftreten, wenden Sie sich an das [EasySSO-Supportteam](mailto:support@techtime.co.nz).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Tutorials zur Integration von SaaS-Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Testen von EasySSO for BitBucket mit Azure AD](https://aad.portal.azure.com/)

- [Was ist Sitzungssteuerung in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Schützen von Apps mit der App-Steuerung für bedingten Zugriff von Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
