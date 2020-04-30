---
title: 'Tutorial: Azure Active Directory-Integration in lokales SharePoint | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und die lokale SharePoint-Instanz konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/15/2020
ms.author: miguego
ms.openlocfilehash: 7e47891a74feb60b0f3e4594bd1621e8b62d64d1
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "81867020"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sharepoint-on-premises"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit SharePoint (lokal)

In diesem Tutorial erfahren Sie, wie Sie SharePoint (lokal) in Azure Active Directory (Azure AD) integrieren. Die Integration von SharePoint (lokal) in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf SharePoint (lokal) hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihrem Azure AD-Konto automatisch bei SharePoint (lokal) anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure Active Directory-Integration in die lokale SharePoint-Instanz konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure Active Directory-Abonnement. Sollten Sie über keine Azure AD-Umgebung verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* Eine SharePoint 2013-Farm (oder neuer).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure Active Directory in einer Testumgebung. Benutzer in Azure Active Directory können auf Ihre lokale SharePoint-Instanz zugreifen.

## <a name="create-the-enterprise-applications-in-azure-portal"></a>Erstellen der Unternehmensanwendungen im Azure-Portal

Zum Konfigurieren der Integration der lokalen SharePoint-Instanz in Azure AD müssen Sie die lokale Instanz aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

So fügen Sie die lokale SharePoint-Instanz aus dem Katalog hinzu:

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

 > [!NOTE]
 > Wenn das Element nicht verfügbar sein sollte, kann es auch über den festen Link **Alle Dienste** oben im linken Navigationsbereich geöffnet werden. In der folgenden Übersicht befindet sich der Link **Azure Active Directory** im Abschnitt **Identität**. Er kann auch über das Filtertextfeld gesucht werden.

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

4. Geben Sie im Suchfeld **SharePoint (lokal)** ein, und wählen Sie im Ergebnisbereich **SharePoint (lokal)** aus.

    <kbd>![SharePoint (lokal) in der Ergebnisliste](./media/sharepoint-on-premises-tutorial/search-new-app.png)</kbd>

1. Geben Sie einen Namen für Ihre lokale SharePoint-Instanz an, und klicken Sie auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

1. Klicken Sie in der neuen Unternehmensanwendung auf „Eigenschaften“, und überprüfen Sie den Wert für **Benutzerzuweisung erforderlich?** .

   <kbd>![SharePoint (lokal) in der Ergebnisliste](./media/sharepoint-on-premises-tutorial/user-assignment-required.png)</kbd>

In diesem Szenario ist der Wert auf **Nein** festgelegt.

## <a name="configure-and-test-azure-ad"></a>Konfigurieren und Testen von Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit SharePoint (lokal).
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SharePoint (lokal) eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure Active Directory mit einer lokalen SharePoint-Instanz zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Konfigurieren von SharePoint (lokal)](#configure-sharepoint-on-premises)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
1. **[Erstellen eines Azure AD-Testbenutzers im Azure-Portal](#create-an-azure-ad-test-user-in-the-azure-portal)** , um in Azure AD einen neuen Benutzer für einmaliges Anmelden zu erstellen
1. **[Erstellen einer Azure AD-Sicherheitsgruppe im Azure-Portal](#create-an-azure-ad-security-group-in-the-azure-portal)** , um in Azure AD eine neue Sicherheitsgruppe für einmaliges Anmelden zu aktivieren
1. **[Erteilen von Berechtigungen für das Azure Active Directory-Konto in SharePoint (lokal)](#grant-permissions-to-azure-active-directory-account-in-sharepoint-on-premises)** , um einem Azure AD-Benutzer Berechtigungen zu erteilen
1. **[Erteilen von Berechtigungen für die Azure AD-Gruppe in SharePoint (lokal)](#grant-permissions-to-azure-ad-group-in-sharepoint-on-premises)** , um der Azure AD-Gruppe Berechtigungen zu erteilen
1. **[Gewähren des Zugriffs auf ein Gastkonto für SharePoint (lokal) im Azure-Portal](#grant-access-to-a-guest-account-to-sharepoint-on-premises-in-the-azure-portal)** , um SharePoint (lokal) Berechtigungen für das Gastkonto in Azure AD zu erteilen
1. **[Konfigurieren des vertrauenswürdigen Identitätsanbieters für mehrere Webanwendungen](#configuring-the-trusted-identity-provider-for-multiple-web-applications)** , um den gleichen vertrauenswürdigen Identitätsanbieter für mehrere Webanwendungen zu verwenden

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit SharePoint (lokal) die folgenden Schritte aus:

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) das Azure AD-Verzeichnis, und klicken Sie auf **Unternehmensanwendungen**, auf den **Namen der zuvor erstellten Unternehmensanwendung** und dann auf **Einmaliges Anmelden**.

2. Klicken Sie im Dialogfeld **SSO-Methode auswählen** auf den Modus **SAML**, um einmaliges Anmelden zu aktivieren.
 
3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für die lokale SharePoint-Instanz](./media/sharepoint-on-premises-tutorial/sp-identifier-reply.png)

    1. Geben Sie im Feld **Bezeichner** eine URL im folgenden Format ein: `urn:<sharepointFarmName>:<federationName>`.

    1. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<YourSharePointSiteURL>/_trust/`

    1. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<YourSharePointSiteURL>/`
    1. Klicken Sie auf „Speichern“.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL, den tatsächlichen Bezeichner und die tatsächliche Antwort-URL.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](./media/sharepoint-on-premises-tutorial/certificatebase64.png)

6. Kopieren Sie im Abschnitt **SharePoint (lokal) einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.
    
    1. **Anmelde-URL**
    
        Kopieren Sie die Anmelde-URL, und ersetzen Sie **/saml2** am Ende durch **/wsfed**. Das Ergebnis sieht ungefähr wie folgt aus: **https://login.microsoftonline.com/2c4f1a9f-be5f-10ee-327d-a95dac567e4f/wsfed** (Diese URL ist nur ein Beispiel.)

    1. **Azure AD-Bezeichner**
    1. **Abmelde-URL**
    > [!NOTE]
    > Diese URL kann nicht wie vorliegend in SharePoint verwendet werden: Sie müssen **/saml2** durch **/wsfed** ersetzen. Die lokale SharePoint-Anwendung verwendet SAML 1.1-Token, sodass Azure AD eine WS-Fed-Anforderung vom SharePoint-Server erwartet und nach der Authentifizierung SAML 1.1-Token ausstellt.

### <a name="configure-sharepoint-on-premises"></a>Konfigurieren von SharePoint (lokal)

1. **Erstellen eines neuen vertrauenswürdigen Identitätsanbieters in SharePoint Server 2016**

    Melden Sie sich beim SharePoint-Server an, und öffnen Sie die SharePoint-Verwaltungsshell. Geben Sie die folgenden Werte ein:
    1. **$realm**: Bezeichnerwert aus dem Abschnitt mit der Domäne und den URLs für SharePoint (lokal) im Azure-Portal
    1. **$wsfedurl**: URL des Diensts für einmaliges Anmelden
   1. **$filepath**: Dateipfad, unter den Sie die Zertifikatsdatei aus dem Azure-Portal heruntergeladen haben

    Führen Sie die folgenden Befehle aus, um einen neuen vertrauenswürdigen Identitätsanbieter zu konfigurieren.

    > [!TIP]
    > Unter [SharePoint PowerShell](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps) finden Sie eine Übersicht über PowerShell und die Funktionen.


    ```
    $realm = "urn:sharepoint:sps201x"
    $wsfedurl="https://login.microsoftonline.com/2c4f1a9f-be5f-10ee-327d-a95dac567e4f/wsfed"
    $filepath="C:\temp\SharePoint 2019 OnPrem.cer"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map1 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.microsoft.com/ws/2008/06/identity/claims/role" -IncomingClaimTypeDisplayName "Role" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "Azure AD SharePoint server 201x" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map1,$map2 -SignInUrl $wsfedurl -IdentifierClaim $map1.InputClaimType
    ```
1. **Aktivieren des vertrauenswürdigen Identitätsanbieters für Ihre Anwendung**

    a. Navigieren Sie in der Zentraladministration zu **Webanwendung verwalten**, und wählen Sie die Webanwendung aus, die Sie mit Azure AD schützen möchten.

    b. Klicken Sie im Menüband auf **Authentifizierungsanbieter**, und wählen Sie die Zone aus, die Sie verwenden möchten.

    c. Wählen Sie **Vertrauenswürdiger Identitätsanbieter** und den Identitätsanbieter aus, den Sie mit dem Namen *AzureAD* registriert haben.

    d. Klicken Sie auf **OK**.

    ![Konfigurieren des Authentifizierungsanbieters](./media/sharepoint-on-premises-tutorial/config-auth-provider.png)

### <a name="create-an-azure-ad-test-user-in-the-azure-portal"></a>Erstellen eines Azure AD-Testbenutzers im Azure-Portal

In diesem Abschnitt wird ein Testbenutzer im Azure-Portal erstellt.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory** und im Bereich **Verwalten** die Option **Benutzer** aus.

2. Wählen Sie dann **Alle Benutzer** und danach oben auf dem Bildschirm **Neuer Benutzer** aus.

3. Wählen Sie die Option **Benutzer erstellen** aus, und führen Sie in den Benutzereigenschaften die folgenden Schritte aus.  
   Unter Umständen können Sie Benutzer in Ihrer Azure AD-Instanz unter Verwendung des Mandantensuffixes oder einer überprüften Domäne erstellen. 

    a. Geben Sie in das Feld **Name** den Benutzernamen ein. Hier wurde **TestUser** verwendet.
  
    b. Geben Sie im Feld **Benutzername** den Namen `TestUser@yourcompanydomain.extension` ein.  
    Zum Beispiel, TestUser@contoso.com

    ![Dialogfeld „Benutzer“](./media/sharepoint-on-premises-tutorial/user-properties.png)

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

    e. Sie können die Website jetzt für TestUser@contoso.com freigeben und diesem Benutzer den Zugriff darauf erlauben.

### <a name="create-an-azure-ad-security-group-in-the-azure-portal"></a>Erstellen einer Azure AD-Sicherheitsgruppe im Azure-Portal

1. Klicken Sie auf **Azure Active Directory > Gruppen**.

2. Klicken Sie auf **Neue Gruppe**:

3. Geben Sie **Gruppentyp**, **Gruppenname**, **Gruppenbeschreibung** und **Mitgliedschaftstyp** an. Klicken Sie auf den Pfeil, um Mitglieder auszuwählen, und suchen Sie nach dem Mitglied, das Sie der Gruppe hinzufügen möchten, oder klicken Sie auf das gewünschte Mitglied. Klicken Sie auf **Auswählen**, um die ausgewählten Mitglieder hinzuzufügen, und klicken Sie anschließend auf **Erstellen**.

![Erstellen einer Azure AD-Sicherheitsgruppe](./media/sharepoint-on-premises-tutorial/new-group.png)

### <a name="grant-permissions-to-azure-active-directory-account-in-sharepoint-on-premises"></a>Erteilen von Berechtigungen für das Azure Active Directory-Konto in SharePoint (lokal)

Wenn Sie dem Azure Active Directory-Benutzer in der lokalen SharePoint-Instanz Zugriff gewähren möchten, müssen Sie die Websitesammlung freigeben oder den Azure Active Directory-Benutzer zu einer der Websitesammlungsgruppen hinzufügen. Benutzer können sich nun unter Verwendung von Identitäten aus Azure AD bei SharePoint 201x anmelden, die Benutzerumgebung kann jedoch noch weiter optimiert werden. Beispielsweise liefert die Suche nach einem Benutzer mehrere Suchergebnisse in der Personenauswahl. Es gibt ein Suchergebnis für jeden der Anspruchstypen, die in der Anspruchszuordnung erstellt wurden. Um einen Benutzer mit der Personenauswahl auszuwählen, geben Sie seinen genauen Benutzernamen ein, und wählen Sie das Anspruchsergebnis **Name** aus.

![Suchergebnisse für Ansprüche](./media/sharepoint-on-premises-tutorial/claims-search-results.png)

Die gesuchten Werte werden nicht überprüft, sodass möglicherweise Rechtschreibfehler auftreten oder Benutzer versehentlich den falschen Anspruchstyp zum Zuweisen auswählen. Folglich können Benutzer nicht auf Ressourcen zugreifen.

Das Problem mit der **Personenauswahl** lässt sich mit einer Open-Source-Lösung namens [AzureCP](https://yvand.github.io/AzureCP/) beheben, die einen benutzerdefinierten Anspruchsanbieter für SharePoint 2013, 2016 und 2019 bereitstellt. Die Microsoft Graph-API klärt und überprüft die Eingabe der Benutzer. Weitere Informationen finden Sie unter [ AzureCP](https://yvand.github.io/AzureCP/).

  > [!NOTE]
  > Ohne AzureCP können Sie Gruppen hinzufügen, indem Sie die ID der Azure AD-Gruppe hinzufügen. Diese Vorgehensweise ist jedoch weder benutzerfreundlich noch zuverlässig. Dies sieht wie folgt aus:  
  >   
  >![Hinzufügen einer Azure AD-Gruppe zur SharePoint-Gruppe](./media/sharepoint-on-premises-tutorial/adding-group-by-id.png)
  
### <a name="grant-permissions-to-azure-ad-group-in-sharepoint-on-premises"></a>Erteilen von Berechtigungen für die Azure AD-Gruppe in SharePoint (lokal)

Wenn Sie SharePoint (lokal) Azure Active Directory-Sicherheitsgruppen hinzufügen möchten, müssen Sie einen benutzerdefinierten Anspruchsanbieter für SharePoint Server verwenden. In diesem Beispiel wurde AzureCP verwendet.

 > [!NOTE]
 > Hinweis: AzureCP ist kein Produkt von Microsoft, und Microsoft bietet keinen technischen Support für das Produkt. Laden Sie AzureCP von https://yvand.github.io/AzureCP/ herunter, installieren Sie die Lösung in Ihrer lokalen SharePoint-Farm, und konfigurieren Sie sie entsprechend. 

1. Konfigurieren Sie AzureCP in der lokalen SharePoint-Farm, oder konfigurieren Sie eine andere benutzerdefinierte Anspruchsanbieterlösung. Die Schritte für die Konfiguration von AzureCP sind unter https://yvand.github.io/AzureCP/Register-App-In-AAD.html verfügbar.

1. Öffnen Sie im Azure-Portal das Azure AD-Verzeichnis. Klicken Sie auf **Unternehmensanwendungen**, auf den **Namen der zuvor erstellten Unternehmensanwendung** und dann auf **Einmaliges Anmelden**.

1. Bearbeiten Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** den Abschnitt **Benutzerattribute und Ansprüche**.

1. Klicken Sie auf **Gruppenanspruch hinzufügen**.

1. Wählen Sie aus, welche dem Benutzer zugeordneten Gruppen im Anspruch zurückgegeben werden sollen. Wählen Sie in diesem Fall die Option **Alle Gruppen** und dann im Abschnitt „Quellattribut“ die Option **Gruppen-ID** aus. Klicken Sie anschließend auf **Speichern**.

Wenn Sie den Zugriff auf die Azure Active Directory-Sicherheitsgruppe in der lokalen SharePoint-Instanz gewähren möchten, müssen Sie die Websitesammlung freigeben oder die Azure Active Directory-Sicherheitsgruppe zu einer der Websitesammlungsgruppen hinzufügen.

1. Navigieren Sie zur SharePoint-Websitesammlung, und klicken Sie in den Websiteeinstellungen für die Websitesammlung auf „Benutzer und Gruppen“. Wählen Sie die SharePoint-Gruppe aus, und klicken Sie auf „Neu“ > „Dieser Gruppe Benutzer hinzufügen“, und beginnen Sie mit der Eingabe des Gruppennamens. In der Personenauswahl wird die Azure Active Directory-Sicherheitsgruppe angezeigt.

    ![Hinzufügen einer Azure AD-Gruppe zur SharePoint-Gruppe](./media/sharepoint-on-premises-tutorial/permission-azure-ad-group.png)

### <a name="grant-access-to-a-guest-account-to-sharepoint-on-premises-in-the-azure-portal"></a>Gewähren des Zugriffs auf ein Gastkonto für SharePoint (lokal) im Azure-Portal

Es ist jetzt möglich, einer SharePoint-Website auf konsistente Weise Zugriff auf ein Gastkonto zu gewähren. Es kann vorkommen, dass sich der UPN ändert. Benutzer mit jdoe@outlook.com werden als `jdoe_outlook.com#ext#@TENANT.onmicrosoft.com` dargestellt. Bei der Freigabe Ihrer Website für externe Benutzer kann eine benutzerfreundliche Umgebung bereitgestellt werden. Dazu müssen im Azure-Portal im Abschnitt **Benutzerattribute und Ansprüche** einige Änderungen vorgenommen werden.

1. Öffnen Sie im Azure-Portal das Azure AD-Verzeichnis. Klicken Sie auf **Unternehmensanwendungen**, auf den **Namen der zuvor erstellten Unternehmensanwendung** und dann auf **Einmaliges Anmelden**.

1. Bearbeiten Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** den Abschnitt **Benutzerattribute und Ansprüche**.

1. Klicken Sie in der Zone **Erforderlicher Anspruch** auf **Eindeutiger Benutzerbezeichner (Namens-ID)** .

1. Ändern Sie die Eigenschaft **Quellattribut** in den Wert **user.localuserprincipalname**, und klicken Sie auf **Speichern**.

    ![Anzeige von „Benutzerattribute und Ansprüche“ vor der Bearbeitung](./media/sharepoint-on-premises-tutorial/manage-claim.png)

1. Navigieren Sie mithilfe des Menübands zurück zu **SAML-basierte Anmeldung**. Der Abschnitt **Benutzerattribute und Ansprüche** sieht nun wie folgt aus: 

    ![Anzeige von „Benutzerattribute und Ansprüche“ nach der Bearbeitung](./media/sharepoint-on-premises-tutorial/user-attributes-claims-final.png)  

    > [!NOTE]
    > Nachname und Vorname sind in diesem Setup nicht erforderlich.

1. Wählen Sie im linken Bereich des Azure-Portals **Azure Active Directory** und dann **Benutzer** aus.

1. Klicken Sie auf **Neuer Gastbenutzer**.

1. Wählen Sie die Option **Invite User** (Benutzer einladen) aus, füllen Sie die Benutzereigenschaften aus, und klicken Sie auf **Einladen**.

1. Sie können die Website jetzt für MyGuestAccount@outlook.com freigeben und diesem Benutzer den Zugriff darauf erlauben.

    ![Freigeben der Website für das Gastkonto](./media/sharepoint-on-premises-tutorial/sharing-guest-account.png)

### <a name="configuring-the-trusted-identity-provider-for-multiple-web-applications"></a>Konfigurieren des vertrauenswürdigen Identitätsanbieters für mehrere Webanwendungen

Die Konfiguration funktioniert für eine einzelne Webanwendung, benötigt jedoch weitere Konfiguration, wenn Sie beabsichtigen, denselben vertrauenswürdigen Identitätsanbieter für mehrere Webanwendungen zu verwenden. Angenommen, eine Webanwendung wäre erweitert worden, um die URL `https://sales.contoso.com` zu verwenden. Nun möchten Sie die Benutzer für `https://marketing.contoso.com` ebenfalls authentifizieren. Dazu müssen Sie den Identitätsanbieter aktualisieren, sodass dieser den WReply-Parameter berücksichtigt. Außerdem müssen Sie die Anwendungsregistrierung in Azure AD aktualisieren, um eine Antwort-URL hinzuzufügen.

1. Öffnen Sie im Azure-Portal das Azure AD-Verzeichnis. Klicken Sie auf **Unternehmensanwendungen**, auf den **Namen der zuvor erstellten Unternehmensanwendung** und dann auf **Einmaliges Anmelden**.

2. Bearbeiten Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** den Abschnitt **Grundlegende SAML-Konfiguration**.

    ![Bearbeiten der grundlegenden SAML-Konfiguration](./media/sharepoint-on-premises-tutorial/add-reply-url.png)

3. Fügen Sie unter **Antwort-URL (Assertionsverbraucherdienst-URL)** die URL für die zusätzlichen Webanwendungen hinzu, und klicken Sie auf **Speichern**.

    ![Bearbeiten der grundlegenden SAML-Konfiguration](./media/sharepoint-on-premises-tutorial/reply-url-for-web-application.png)

4. Öffnen Sie auf dem SharePoint-Server die **SharePoint 201x-Verwaltungsshell**, und führen Sie die folgenden Befehle mithilfe des Namens des Tokenausstellers des vertrauenswürdigen Identitätsanbieters aus, den Sie zuvor verwendet haben.
    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```
5. Navigieren Sie in der Zentraladministration zu der Web-Anwendung, und aktivieren Sie den vorhandenen vertrauenswürdigen Identitätsanbieter.

Unter Umständen möchten Sie in einem anderen Szenario Ihren internen Benutzern Zugriff auf die lokale Sharepoint-Instanz gewähren. In diesem Fall müssten Sie Microsoft Azure Active Directory Connect bereitstellen, um die Synchronisierung Ihrer lokalen Benutzer mit Azure Active Directory zu ermöglichen. Dieses Setup ist Gegenstand einer anderen Artikels. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Was ist bedingter Zugriff?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Was bedeutet Hybrididentität in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
