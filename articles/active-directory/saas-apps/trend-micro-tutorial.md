---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Trend Micro Web Security (TMWS) | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und Trend Micro Web Security (TMWS) konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 827285d3-8e65-43cd-8453-baeda32ef174
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a4c2cddbc9086c80922fcf9c5d96cd197ab4778
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81425279"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-trend-micro-web-securitytmws"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Trend Micro Web Security (TMWS)

In diesem Tutorial erfahren Sie, wie Sie Trend Micro Web Security (TMWS) in Azure Active Directory (Azure AD) integrieren. Die Integration von Trend Micro Web Security (TMWS) in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Trend Micro Web Security (TMWS) hat.
* Ermöglichen Sie es ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Trend Micro Web Security (TMWS) anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Abonnement für Trend Micro Web Security (TMWS), für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Trend Micro Web Security (TMWS) unterstützt **SP**-initiiertes einmaliges Anmelden.
* Nach dem Konfigurieren von Trend Micro Web Security (TMWS) können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.

## <a name="adding-trend-micro-web-securitytmws-from-the-gallery"></a>Hinzufügen von Trend Micro Web Security (TMWS) aus dem Katalog

Zum Konfigurieren der Integration von Trend Micro Web Security (TMWS) in Azure AD müssen Sie Trend Micro Web Security (TMWS) aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Trend Micro Web Security (TMWS)** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Trend Micro Web Security (TMWS)** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-trend-micro-web-securitytmws"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Trend Micro Web Security (TMWS)

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Trend Micro Web Security (TMWS) mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Trend Micro Web Security (TMWS) eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Trend Micro Web Security (TMWS) die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
    1. **[Konfigurieren der Einstellungen für die Benutzer- und Gruppensynchronisierung in Azure AD](#configure-user-and-group-synchronization-settings-in-azure-ad)**
1. **[Konfigurieren des einmaligen Anmeldens für Trend Micro Web Security (TMWS)](#configure-trend-micro-web-security-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Trend Micro Web Security (TMWS)** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

    a. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein: `https://auth.iws-hybrid.trendmicro.com/([0-9a-f]{16})`.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL ein: `https://auth.iws-hybrid.trendmicro.com/simplesaml/module.php/saml/sp/saml2-acs.php/ics-sp`.

    > [!NOTE]
    > Der ID-Wert ist nicht der tatsächliche Wert. Aktualisieren Sie den Wert mit dem tatsächlichen Bezeichner. Den Bezeichnerwert erhalten Sie vom [Supportteam für den Trend Micro Web Security-Client (TMWS)](https://success.trendmicro.com/contact-support-north-america). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Die TMWS-Anwendung (Trend Micro Web Security) erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute.

    ![image](common/default-attributes.png)

1. Darüber hinaus wird von der TMWS-Anwendung (Trend Micro Web Security) erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden (siehe unten). Diese Attribute werden ebenfalls vorab aufgefüllt, Sie können sie jedoch nach Bedarf überprüfen.
    
    | Name | Quellattribut|
    | --------------- | --------- |
    | sAMAccountName | user.onpremisessamaccountname |
    | uPN | user.userprincipalname |

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zum Eintrag **Zertifikat (Base64)** . Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **Trend Micro Web Security (TMWS) einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Trend Micro Web Security (TMWS) gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Trend Micro Web Security (TMWS)** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="configure-user-and-group-synchronization-settings-in-azure-ad"></a>Konfigurieren der Einstellungen für die Benutzer- und Gruppensynchronisierung in Azure AD

1. Klicken Sie im linken Navigationsmenü auf **Azure Active Directory**.

1. Klicken Sie unter **Verwalten** auf **App-Registrierungen** und dann im Bereich **Alle Anwendungen** auf Ihre neue Unternehmensanwendung.

1. Klicken Sie unter **Verwalten** auf **Zertifikate und Geheimnisse**.

1. Klicken Sie im daraufhin angezeigten Bereich „Geheime Clientschlüssel“ auf **Neuer geheimer Clientschlüssel**.

1. Fügen Sie auf dem angezeigten Bildschirm „Geheimen Clientschlüssel hinzufügen“ optional eine Beschreibung hinzu, und wählen Sie einen Ablaufzeitraum für den geheimen Clientschlüssel aus. Klicken Sie anschließend auf **Hinzufügen**. Der neu hinzugefügte geheime Clientschlüssel wird im Bereich „Geheime Clientschlüssel“ angezeigt.

1. Notieren Sie sich den Wert. Sie geben die Informationen später in TMWS ein.

1. Klicken Sie unter **Verwalten** auf **API-Berechtigungen**. 

1. Klicken Sie auf dem daraufhin angezeigten Bildschirm „API-Berechtigungen“ auf **Berechtigung hinzufügen**.

1. Klicken Sie auf dem angezeigten Bildschirm „API-Berechtigungen anfordern“ auf der Registerkarte „Microsoft-APIs“ auf **Microsoft Graph** und dann auf **Anwendungsberechtigungen**.

1. Suchen Sie die folgenden Berechtigungen, und fügen Sie sie hinzu: 

    * Group.Read.All
    * User.Read.All

1. Klicken Sie a **Berechtigungen hinzufügen**. Eine Meldung mit der Bestätigung wird angezeigt, dass die Einstellungen erfolgreich gespeichert wurden. Die neu hinzugefügten Berechtigungen werden auf dem Bildschirm „API-Berechtigungen“ angezeigt.

1. Klicken Sie im Bereich „Einwilligung erteilen“ auf **Administratorzustimmung für < Ihr Administratorkonto > erteilen (Standardverzeichnis)** und dann auf **Ja**. Eine Meldung mit der Bestätigung wird angezeigt, dass die Administratoreinwilligung für die angeforderten Berechtigungen erteilt wurde.

1. Klicken Sie auf **Overview**. 

1. Notieren Sie die Anwendungs-ID (Client) und die Verzeichnis-ID (Mandant) aus dem Bereich auf der rechten Seite. Sie geben die Informationen später in TMWS ein. Sie können auch unter **Azure Active Directory > Verwalten** auf **Namen der benutzerdefinierten Domänen** klicken und den Domänennamen im rechten Bereich notieren.

## <a name="configure-trend-micro-web-security-sso"></a>Konfigurieren des einmaligen Anmeldens für Trend Micro Web Security

Zum Konfigurieren des einmaligen Anmeldens aufseiten von **Trend Micro Web Security (TMWS)** müssen Sie das heruntergeladene **Zertifikat (Base64)** und die kopierten URLs aus dem Azure-Portal an das [Supportteam von Trend Micro Web Security (TMWS)](https://success.trendmicro.com/contact-support-north-america) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

Wenn Sie den Azure AD-Dienst konfiguriert und Azure AD als Methode für die Benutzerauthentifizierung angegeben haben, können Sie sich zum Überprüfen des Setups beim TMWS-Proxyserver anmelden. Nachdem bei der Azure AD-Anmeldung Ihr Konto überprüft wurde, können Sie im Internet surfen.

> [!NOTE]
> Das Testen des einmaligen Anmeldens über das Azure AD-Portal unter „Übersicht“ > „Einmaliges Anmelden“ > „Einmaliges Anmelden (SSO) mit SAML einrichten“ > „Test of your new enterprise application“ (Neue Unternehmensanwendung testen) wird von TMWS nicht unterstützt.

1. Löschen Sie alle Cookies im Browser, und starten Sie den Browser neu. 

1. Navigieren Sie im Browser zum TMWS-Proxyserver. Weitere Informationen finden Sie unter [Weiterleiten von Datenverkehr mit PAC-Dateien](https://docs.trendmicro.com/en-us/enterprise/trend-micro-web-security-online-help/administration_001/pac-files/traffic-forwarding-u.aspx#GUID-A4A83827-7A29-4596-B866-01ACCEDCC36B).

1. Rufen Sie eine beliebige Website im Internet auf. TMWS leitet Sie an das Captive Portal von TMWS weiter.

1. Geben Sie ein Active Directory-Konto (Format: „domain\sAMAccountName“ oder sAMAccountName@domain), eine E-Mail-Adresse oder einen UPN an, und klicken Sie dann auf **Log On** (Anmelden). TMWS leitet Sie zur Azure AD-Anmeldung weiter.

1. Geben Sie bei der Azure AD-Anmeldung Ihre AD-Kontoanmeldeinformationen ein. Sie sollten sich erfolgreich bei TMWS anmelden können.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Trend Micro Web Security (TMWS) mit Azure AD ausprobieren](https://aad.portal.azure.com/)

- [Was ist Sitzungssteuerung in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Schützen von Apps mit der App-Steuerung für bedingten Zugriff von Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

