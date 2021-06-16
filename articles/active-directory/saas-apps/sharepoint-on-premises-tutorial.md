---
title: 'Tutorial: Azure Active Directory-Integration in lokales SharePoint | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Verbundauthentifizierung zwischen Azure Active Directory und SharePoint lokal implementieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/31/2021
ms.author: jeedes
ms.openlocfilehash: 142b16d58fae508614324746345bc8d33f41cab4
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111890049"
---
# <a name="tutorial-implement-federated-authentication-between-azure-active-directory-and-sharepoint-on-premises"></a>Tutorial: Implementieren der Verbundauthentifizierung zwischen Azure Active Directory und SharePoint lokal

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren Sie eine Verbundauthentifizierung zwischen Azure Active Directory und SharePoint lokal. Ziel ist, Benutzern die Anmeldung bei Azure Active Directory zu ermöglichen und ihre Identität für den Zugriff auf die lokalen SharePoint-Websites zu verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Um die Konfiguration vornehmen zu können, benötigen Sie die folgenden Ressourcen:
* Ein Azure Active Directory-Mandant. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.
* Eine SharePoint 2013-Farm (oder neuer).

In diesem Artikel werden folgende Werte verwendet:
- Name der Unternehmensanwendung (in Azure AD): `SharePoint corporate farm`
- Vertrauensbezeichner (in Azure AD) bzw. Bereich (in SharePoint): `urn:sharepoint:federation`
- loginUrl (an Azure AD): `https://login.microsoftonline.com/dc38a67a-f981-4e24-ba16-4443ada44484/wsfed`
- SharePoint Website-URL: `https://spsites.contoso.local/`
- Antwort-URL der SharePoint-Website: `https://spsites.contoso.local/_trust/`
- Konfigurationsname der SharePoint-Vertrauensstellung: `AzureADTrust`
- UserPrincipalName des Azure AD-Testbenutzers: `AzureUser1@demo1984.onmicrosoft.com`

## <a name="configure-an-enterprise-application-in-azure-active-directory"></a>Konfigurieren einer Unternehmensanwendung in Azure Active Directory

Zum Konfigurieren des Verbunds in Azure AD müssen Sie eine dedizierte Unternehmensanwendung erstellen. Die Konfiguration wird mithilfe der vorkonfigurierten Vorlage `SharePoint on-premises` vereinfacht, die sich im Anwendungskatalog befindet.

### <a name="create-the-enterprise-application"></a>Erstellen einer Unternehmensanwendung

1. Melden Sie sich beim [Azure Active Directory-Portal](https://aad.portal.azure.com/) an.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.
1. Wählen Sie oben im Dialogfeld **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.
1. Geben Sie in das Suchfeld **SharePoint lokal** ein. Wählen Sie im Ergebnisbereich **SharePoint (lokal)** aus.
1. Geben Sie einen Namen für Ihre Anwendung an (in diesem Tutorial lautet er `SharePoint corporate farm`) und klicken Sie auf **Erstellen**, um die Anwendung hinzuzufügen.
1. Wählen Sie in der neuen Unternehmensanwendung **Eigenschaften** aus, und überprüfen Sie den Wert für **Benutzerzuweisung erforderlich?** . Legen Sie für dieses Szenario den Wert auf **Nein** fest und klicken Sie auf **Speichern**.

### <a name="configure-the-enterprise-application"></a>Konfigurieren der Unternehmensanwendung

In diesem Abschnitt konfigurieren Sie die SAML-Authentifizierung und definieren die Ansprüche, die bei erfolgreicher Authentifizierung an SharePoint gesendet werden.

1. Wählen Sie in der Übersicht der Unternehmensanwendung `SharePoint corporate farm` die Option **2. Single Sign-On einrichten** und im nächsten Dialogfeld **SAML** aus.
 
1. Wählen Sie auf der Seite **Single Sign-On mit SAML einrichten** im Bereich **Grundlegende SAML-Konfiguration** das Symbol **Bearbeiten** aus.

1. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    1. Stellen Sie im Feld **Bezeichner** sicher, dass der folgende Wert angegeben wird: `urn:sharepoint:federation`.

    1. Geben Sie im Feld **Antwort-URL** eine URL im folgenden Format ein: `https://spsites.contoso.local/_trust/`.

    1. Geben Sie im Feld **Anmelde-URL** eine URL im folgenden Format ein: `https://spsites.contoso.local/`.
    
    1. Wählen Sie **Speichern** aus.

1. Löschen Sie im Abschnitt **Benutzerattribute und -ansprüche** die folgenden nutzlosen Anspruchstypen (werden von SharePoint nicht zum Gewähren von Berechtigungen verwendet):
    - `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`
    - `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`
    - `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`

1. Die Einstellungen sollten nun wie folgt aussehen:

    ![Grundlegende SAML-Einstellungen](./media/sharepoint-on-premises-tutorial/azure-active-directory-app-saml-ids.png)

1. Kopieren Sie die Informationen, die Sie später in SharePoint benötigen:

    - Laden Sie im Abschnitt **SAML-Signaturzertifikat** das **Zertifikat (Base64)** **herunter**. Dies ist der öffentliche Schlüssel des Signaturzertifikats, welcher von Azure AD zum Signieren des SAML-Tokens verwendet wird. SharePoint benötigt ihn, um die Integrität der eingehenden SAML-Token zu überprüfen.

    - Kopieren Sie im Abschnitt **SharePoint-Unternehmensfarm einrichten** die **Anmelde-URL** in einen Editor und ersetzen Sie die folgende Zeichenfolge **/saml2** durch **/wsfed**.
     
    > [!IMPORTANT]
    > Stellen Sie sicher, dass Sie **/saml2** durch **/wsfed** ersetzen, damit Azure AD ein für SharePoint erforderliches SAML 1.1-Token ausgibt.

    - Kopieren Sie im Abschnitt **SharePoint-Unternehmensfarm einrichten** die **Abmelde-URL**.

## <a name="configure-sharepoint-to-trust-azure-active-directory"></a>Konfigurieren von SharePoint für vertrauenswürdige Azure Active Directory

### <a name="create-the-trust-in-sharepoint"></a>Erstellen des Vertrauens in SharePoint

In diesem Schritt erstellen Sie einen SPTrustedLoginProvider, um die Konfiguration zu speichern, die SharePoint zum Vertrauen von Azure AD benötigt. Dazu benötigen Sie die Informationen aus Azure AD, die Sie oben kopiert haben. Starten Sie die SharePoint Management Shell und führen Sie zum Erstellen das folgende Skript aus:

```powershell
# Path to the public key of the Azure AD SAML signing certificate (self-signed), downloaded from the Enterprise application in the Azure AD portal
$signingCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2("C:\AAD app\SharePoint corporate farm.cer")
# Unique realm (corresponds to the "Identifier (Entity ID)" in the Azure AD Enterprise application)
$realm = "urn:sharepoint:federation"
# Login URL copied from the Azure AD enterprise application. Make sure to replace "saml2" with "wsfed" at the end of the URL:
$loginUrl = "https://login.microsoftonline.com/dc38a67a-f981-4e24-ba16-4443ada44484/wsfed"

# Define the claim types used for the authorization
$userIdentifier = New-SPClaimTypeMapping -IncomingClaimType `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
$role = New-SPClaimTypeMapping "http://schemas.microsoft.com/ws/2008/06/identity/claims/role" -IncomingClaimTypeDisplayName "Role" -SameAsIncoming

# Let SharePoint trust the Azure AD signing certificate
New-SPTrustedRootAuthority -Name "Azure AD signing certificate" -Certificate $signingCert

# Create a new SPTrustedIdentityTokenIssuer in SharePoint
$trust = New-SPTrustedIdentityTokenIssuer -Name "AzureADTrust" -Description "Azure AD" -Realm $realm -ImportTrustCertificate $signingCert -ClaimsMappings $userIdentifier, $role -SignInUrl $loginUrl -IdentifierClaim $userIdentifier.InputClaimType
```

### <a name="configure-the-sharepoint-web-application"></a>Konfigurieren der SharePoint-Webanwendung

In diesem Schritt konfigurieren Sie eine Webanwendung in SharePoint so, dass sie der oben erstellten Azure AD Enterprise-Anwendung vertraut. Es gibt wichtige Regeln, die zu beachten sind:
- Die Standardzone der SharePoint-Webanwendung muss über eine aktivierte Windows-Authentifizierung verfügen. Dies ist für den Search-Crawler erforderlich.
- Die SharePoint-URL, welche die Azure AD-Authentifizierung verwendet, muss mit HTTPS festgelegt werden.

1. Erstellen oder erweitern Sie die Webanwendung. In diesem Artikel werden zwei mögliche Konfigurationen beschrieben:

    - Wenn Sie eine neue Webanwendung erstellen, die sowohl Windows- als auch Azure AD-Authentifizierung in der Standardzone verwendet:

        1. Starten Sie die **SharePoint-Verwaltungsshell**, und führen Sie das folgende Skript aus:
            ```powershell
            # This script creates a new web application and sets Windows and Azure AD authentication on the Default zone
            # URL of the SharePoint site federated with Azure AD
            $trustedSharePointSiteUrl = "https://spsites.contoso.local/"
            $applicationPoolManagedAccount = "Contoso\spapppool"

            $winAp = New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication -DisableKerberos:$true
            $sptrust = Get-SPTrustedIdentityTokenIssuer "AzureADTrust"
            $trustedAp = New-SPAuthenticationProvider -TrustedIdentityTokenIssuer $sptrust    
            
            New-SPWebApplication -Name "SharePoint - Azure AD" -Port 443 -SecureSocketsLayer -URL $trustedSharePointSiteUrl -ApplicationPool "SharePoint - Azure AD" -ApplicationPoolAccount (Get-SPManagedAccount $applicationPoolManagedAccount) -AuthenticationProvider $winAp, $trustedAp
            ```
        1. Öffnen Sie die Website der **SharePoint-Zentraladministration**.
        1. Wählen Sie unter **Systemeinstellungen** die Option **Alternative Zugriffszuordnungen konfigurieren**. Das Feld **Alternative Zugriffszuordnungssammlung** wird geöffnet.
        1. Filtern Sie die Anzeige mit der neuen Webanwendung, und vergewissern Sie sich, dass die Anzeige in etwa wie folgt aussieht:
    
           ![Alternative Zugriffszuordnungen der Webanwendung](./media/sharepoint-on-premises-tutorial/sp-alternate-access-mappings-new-web-app.png)

    - Wenn Sie eine vorhandene Webanwendung erweitern, um Azure AD-Authentifizierung in einer neuen Zone zu verwenden:

        1. Starten Sie die SharePoint-Verwaltungsshell, und führen Sie das folgende Skript aus:

            ```powershell
            # This script extends an existing web application to set Azure AD authentication on a new zone
            # URL of the default zone of the web application
            $webAppDefaultZoneUrl = "http://spsites/"
            # URL of the SharePoint site federated with ADFS
            $trustedSharePointSiteUrl = "https://spsites.contoso.local/"
            $sptrust = Get-SPTrustedIdentityTokenIssuer "AzureADTrust"
            $ap = New-SPAuthenticationProvider -TrustedIdentityTokenIssuer $sptrust
            $wa = Get-SPWebApplication $webAppDefaultZoneUrl
            
            New-SPWebApplicationExtension -Name "SharePoint - Azure AD" -Identity $wa -SecureSocketsLayer -Zone Internet -Url $trustedSharePointSiteUrl -AuthenticationProvider $ap
            ```
        
        1. Öffnen Sie die Website der **SharePoint-Zentraladministration**.
        1. Wählen Sie unter **Systemeinstellungen** die Option **Alternative Zugriffszuordnungen konfigurieren**. Das Feld **Alternative Zugriffszuordnungssammlung** wird geöffnet.
        1. Filtern Sie die Anzeige mit der erweiterten Webanwendung, und vergewissern Sie sich, dass die Anzeige in etwa wie folgt aussieht:
    
            ![Alternative Zugriffszuordnungen der erweiterten Webanwendung](./media/sharepoint-on-premises-tutorial/sp-alternate-access-mappings-extended-zone.png)

Nachdem die Webanwendung erstellt wurde, können Sie eine Stammwebsitesammlung erstellen und Ihr Windows-Konto als Administrator für die primäre Websitesammlung hinzufügen.

1. Erstellen eines Zertifikats für die SharePoint-Website

    Die SharePoint-URL verwendet das HTTPS-Protokoll (`https://spsites.contoso.local/`). Daher muss auf der IIS-Website (Internet Information Services, Internetinformationsdienste) ein Zertifikat festgelegt werden. Führen Sie die folgenden Schritte aus, um ein selbstsigniertes Zertifikat zu generieren:
    
    > [!IMPORTANT]
    > Selbstsignierte Zertifikate sind nur für Testzwecke geeignet. In Produktionsumgebungen wird dringend empfohlen, stattdessen Zertifikate einer Zertifizierungsstelle zu verwenden.
    
    1. Öffnen Sie die Windows PowerShell-Konsole.
    1. Führen Sie das folgende Skript aus, um ein selbstsigniertes Zertifikat zu generieren und es dem Computerzertifikatspeicher „MY“ hinzuzufügen:
    
        ```powershell
        New-SelfSignedCertificate -DnsName "spsites.contoso.local" -CertStoreLocation "cert:\LocalMachine\My"
        ```
    
1. Festlegen des Zertifikats am IIS-Standort
    1. Öffnen Sie die Konsole „Internetinformationsdienste-Manager“.
    1. Erweitern Sie den Server in der Strukturansicht, erweitern Sie **Websites**, wählen Sie die Website **SharePoint: Azure AD** aus und anschließend **Bindungen**.
    1. Wählen Sie **HTTPS-Bindung** und anschließend **Bearbeiten** aus.
    1. Wählen Sie im Feld „TLS/SSL-Zertifikat“ das zu verwendende Zertifikat aus (z. B. das oben erstellte **spsites.contoso.local**) und anschließend **OK** aus.
    
    > [!NOTE]
    > Wenn Sie über mehrere Web-Frontend-Server verfügen, müssen Sie diesen Vorgang bei jedem Vorgang wiederholen.

Die grundlegende Konfiguration der Vertrauensstellung zwischen SharePoint und Azure AD ist jetzt abgeschlossen. Im Folgenden erfahren Sie, wie Sie sich als Azure Active Directory-Benutzer bei der SharePoint-Website anmelden.

## <a name="sign-in-as-a-member-user"></a>Melden Sie sich als Mitgliedsbenutzer an.

Azure Active Directory verfügt über [zwei Benutzertypen](../external-identities/user-properties.md): Gastbenutzer und Mitgliedsbenutzer. Beginnen wir mit einem Mitgliedsbenutzer, bei dem es sich lediglich um einen Benutzer handelt, der sich in Ihrer Organisation befindet.

### <a name="create-a-member-user-in-azure-active-directory"></a>Erstellen Sie einen Mitgliedsbenutzer in Azure Active Directory.

1. Wählen Sie im ganz linken Bereich des Azure-Portals die Option **Azure Active Directory** aus. Wählen Sie im Bereich **Verwalten** die Option **Benutzer** aus.

1. Wählen Sie oben im Bildschirm **Alle Benutzer** > **Neuer Benutzer** aus.

1. Wählen Sie **Benutzer erstellen** aus, und führen Sie in den Eigenschaften für Benutzer die folgenden Schritte aus.

    1. Geben Sie im Feld **Name** den Benutzernamen ein. Wir haben **TestUser** verwendet.
  
    1. Geben Sie im Feld **Benutzername** Folgendes ein: `AzureUser1@<yourcompanytenant>.onmicrosoft.com`. In diesem Beispiel wird `AzureUser1@demo1984.onmicrosoft.com` angezeigt:

       ![Dialogfeld „Benutzer“](./media/sharepoint-on-premises-tutorial/azure-active-directory-new-user.png)

    1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    1. Klicken Sie auf **Erstellen**.

    1. Sie können die Website jetzt für `AzureUser1@demo1984.onmicrosoft.com` freigeben und diesem Benutzer den Zugriff darauf erlauben.

### <a name="grant-permissions-to-the-azure-active-directory-user-in-sharepoint"></a>Erteilen Sie Berechtigungen für den Azure Active Directory-Benutzer in SharePoint.

Melden Sie sich bei der SharePoint-Stammwebsitesammlung mit Ihrem Windows-Konto (Websitesammlungsadministrator) an und klicken Sie auf **Freigeben**.  
Im Dialogfeld müssen Sie den genauen Wert für „userprincipalname“ eingeben, z. B. `AzureUser1@demo1984.onmicrosoft.com`. Achten Sie darauf, das Ergebnis des **Namensanspruchs** auszuwählen (bewegen Sie den Mauszeiger auf ein Ergebnis, um dessen Anspruchstyp anzuzeigen).

> [!IMPORTANT]
> Achten Sie darauf, den genauen Wert des Benutzers einzugeben, den Sie einladen möchten, und wählen Sie den entsprechenden Anspruchstyp in der Liste aus. Andernfalls funktioniert die Freigabe nicht.

![Ergebnisse der Personenauswahl ohne AzureCP](./media/sharepoint-on-premises-tutorial/sp-people-picker-search-no-azurecp.png)

Diese Einschränkung ist darauf zurückzuführen, dass SharePoint die Eingaben aus der Personenauswahl nicht überprüft. Dies kann verwirrend sein und zu Falschschreibung führen, oder Benutzer wählen versehentlich den falschen Anspruchstyp aus.  
Um dieses Szenario zu beheben, kann eine Open-Source-Lösung namens [AzureCP](https://yvand.github.io/AzureCP/) verwendet werden, um SharePoint 2019/2016/2013 mit Azure Active Directory zu verbinden und die Eingabe für Ihren Azure Active Directory-Mandanten aufzulösen. Weitere Informationen finden Sie unter [AzureCP](https://yvand.github.io/AzureCP/).

Im Folgenden finden Sie die gleiche Suche mit konfiguriertem AzureCP: SharePoint gibt die tatsächlichen Benutzer basierend auf der Eingabe zurück:

![Ergebnisse der Personenauswahl mit AzureCP](./media/sharepoint-on-premises-tutorial/sp-people-picker-search-with-azurecp.png)

> [!IMPORTANT]
> AzureCP ist kein Produkt von Microsoft, und Microsoft bietet keinen Support für das Produkt. Informationen, um AzureCP herunterzuladen und in der SharePoint-Farm (lokal) zu installieren und konfigurieren, finden Sie auf der [AzureCP](https://yvand.github.io/AzureCP/)-Website. 

Azure Active Directory-Benutzer `AzureUser1@demo1984.onmicrosoft.com` kann sich jetzt mit seiner Identität bei der SharePoint-Website `https://spsites.contoso.local/` anmelden.

## <a name="grant-permissions-to-a-security-group"></a>Erteilen von Berechtigungen für eine Sicherheitsgruppe

### <a name="add-the-group-claim-type-to-the-enterprise-application"></a>Hinzufügen des Gruppenanspruchstyps zur Unternehmensanwendung

1. Wählen Sie in der Übersicht der Unternehmensanwendung `SharePoint corporate farm` die Option **2 aus. Richten Sie einmaliges Anmelden** ein. 

1. Führen Sie im Abschnitt **Benutzerattribute und -ansprüche** die folgenden Schritte aus, wenn kein Gruppenanspruch vorhanden ist:

    1. Wählen Sie **Gruppenanspruch hinzufügen** und **Sicherheitsgruppen** aus und stellen Sie sicher, dass **Quellattribut** auf **Gruppen-ID** festgelegt ist.
    1. Aktivieren Sie **Namen des Gruppenanspruchs anpassen,** und anschließend **Gruppen als Rollenansprüche** aus und klicken Sie auf **Speichern**.
    1. Die **Benutzerattribute und -ansprüche** sollten wie folgt aussehen:

    ![Ansprüche für Benutzer und Gruppen](./media/sharepoint-on-premises-tutorial/azure-active-directory-claims-with-group.png)

### <a name="create-a-security-group-in-azure-active-directory"></a>Erstellen einer Sicherheitsgruppe in Azure Active Directory

Erstellen einer Sicherheitsgruppe in Azure Active Directory:

1. Klicken Sie auf **Azure Active Directory** > **Gruppen**.

1. Wählen Sie **Neue Gruppe** aus.

1. Geben Sie den **Gruppentyp** (Sicherheit), den **Gruppennamen** (z. B. `AzureGroup1` ) und den **Mitgliedschaftstyp** ein. Fügen Sie den oben erstellten Benutzer als Mitglied hinzu und klicken Sie auf **Erstellen**:

    ![Erstellen einer Azure AD-Sicherheitsgruppe](./media/sharepoint-on-premises-tutorial/azure-active-directory-new-group.png)
  
### <a name="grant-permissions-to-the-security-group-in-sharepoint"></a>Erteilen von Berechtigungen für die Sicherheitsgruppe in SharePoint

Azure AD-Sicherheitsgruppen werden mit ihrem Attribut `Id` identifiziert, bei dem es sich um eine GUID handelt (z. B. `E89EF0A3-46CC-45BF-93A4-E078FCEBFC45`).  
Ohne einen benutzerdefinierten Anspruchsanbieter müssen Benutzer den genauen Wert (`Id`) der Gruppe in der Personenauswahl eingeben und den entsprechenden Anspruchstyp auswählen. Dies ist weder benutzerfreundlich noch zuverlässig.  
Um dies zu vermeiden, verwendet dieser Artikel den Anspruchsanbieter [AzureCP](https://yvand.github.io/AzureCP/) eines Drittanbieters, um die Gruppe in SharePoint auf benutzerfreundliche Weise zu finden:

![Personenauswahl für Azure AD Gruppe](./media/sharepoint-on-premises-tutorial/sp-people-picker-search-azure-active-directory-group.png)

## <a name="manage-guest-users-access"></a>Verwalten des Zugriffs von Gastbenutzern

Es gibt zwei Typen von Gastkonten:

- B2B-Gastkonten: Diese Benutzer befinden sich in einem externen Azure Active Directory-Mandanten.
- MSA-Gastkonten: Diese Benutzer befinden sich in einem Microsoft-Identifizierungsanbieter (Hotmail, Outlook) oder einem Anbieter für Konten bei Sozialen Netzwerken (Google oder ähnliches).

Standardmäßig legt Azure Active Directory sowohl den „Eindeutigen Benutzerbezeichner“ als auch den Anspruch „name“ des Attributs `user.userprincipalname` fest.  
Leider ist dieses Attribut für Gastkonten mehrdeutig, wie die folgende Tabelle zeigt:

| Quellattribut in Azure AD | Tatsächliche Eigenschaft, die von Azure AD B2B-Gästen verwendet wird | Tatsächliche Eigenschaft, die von Azure AD MSA-Gästen verwendet wird | Eigenschaft, auf die sich SharePoint verlassen kann, um die Identität zu überprüfen |
|--|--|--|--|
| `user.userprincipalname` | `mail`, z. B.: `guest@PARTNERTENANT` | `userprincipalname`, z. B.: `guest_outlook.com#EXT#@TENANT.onmicrosoft.com` | Nicht eindeutig |
| `user.localuserprincipalname` | `userprincipalname`, z. B.: `guest_PARTNERTENANT#EXT#@TENANT.onmicrosoft.com` | `userprincipalname`, z. B.: `guest_outlook.com#EXT#@TENANT.onmicrosoft.com` | `userprincipalname` |

Um daher sicherzustellen, dass alle Gastkonten mit demselben Attribut identifiziert werden, sollten die Bezeichneransprüche der Unternehmensanwendung aktualisiert werden, um das Attribut `user.localuserprincipalname` anstelle von `user.userprincipalname` zu verwenden.

### <a name="update-the-application-to-use-a-consistent-attribute-for-all-guest-users"></a>Aktualisieren der Anwendung, um ein konsistentes Attribut für alle Gastbenutzer zu verwenden

1. Wählen Sie in der Übersicht der Unternehmensanwendung `SharePoint corporate farm` die Option **2 aus. Richten Sie einmaliges Anmelden** ein.
 
1. Wählen Sie auf der Seite **Single Sign-On mit SAML einrichten** im Bereich **Benutzerattribute und -ansprüche** das Symbol **Bearbeiten** aus.

1. Der Abschnitt **Benutzerattribute und -ansprüche** sieht nun wie folgt aus:

    1. Wählen Sie **Eindeutiger Benutzerbezeichner (Namens-ID)** aus, ändern Sie die Eigenschaft **Quellattribut** in **user.localuserprincipalname** und klicken Sie auf **Speichern**.
    
    1. Wählen Sie `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` aus, ändern Sie die Eigenschaft **Quellattribut** in **user.localuserprincipalname** und klicken Sie auf **Speichern**.
    
    1. Die **Benutzerattribute und -ansprüche** sollten wie folgt aussehen:
    
    ![Benutzerattribute und -ansprüche für Gäste](./media/sharepoint-on-premises-tutorial/azure-active-directory-claims-guests.png)

### <a name="invite-guest-users-in-sharepoint"></a>Gastbenutzer in SharePoint einladen

> [!NOTE]
> In diesem Abschnitt wird davon ausgegangen, dass der Anspruchsanbieter AzureCP verwendet wird.

Im obigen Abschnitt haben Sie die Unternehmensanwendung aktualisiert, um ein konsistentes Attribut für alle Gastkonten zu verwenden.  
Nun muss die Konfiguration von AzureCP aktualisiert werden, um diese Änderung widerzuspiegeln und das Attribut `userprincipalname` für Gastkonten zu verwenden:

1. Öffnen Sie die Website der **SharePoint-Zentraladministration**.
1. Wählen Sie unter **Sicherheit** die Option **Globale AzureCP-Konfiguration** aus.
1. Im Abschnitt **User identifier property**: Legen Sie den **Benutzerbezeichner für „Gastbenutzer“** auf **UserPrincipalName** fest.
1. Klicken Sie auf OK.

![Konfiguration von AzureCP-Gastkonten](./media/sharepoint-on-premises-tutorial/sp-azurecp-attribute-for-guests.png)

Sie können jetzt jeden Gastbenutzer auf den SharePoint-Websites einladen.

## <a name="configure-the-federation-for-multiple-web-applications"></a>Konfigurieren des Verbunds für mehrere Webanwendungen

Die Konfiguration funktioniert für eine einzelne Webanwendung, benötigt jedoch weitere Konfiguration, wenn Sie beabsichtigen, denselben vertrauenswürdigen Identitätsanbieter für mehrere Webanwendungen zu verwenden. Angenommen, Sie verfügen über eine separate Webanwendung `https://otherwebapp.contoso.local/` und möchten für diese jetzt Azure Active Directory-Authentifizierung aktivieren. Konfigurieren Sie dazu SharePoint so, dass der SAML WReply-Parameter übergeben wird, und fügen Sie die URLs in der Unternehmensanwendung hinzu.

### <a name="configure-sharepoint-to-pass-the-saml-wreply-parameter"></a>Konfigurieren von SharePoint zum Übergeben des SAML WReply-Parameters

1. Öffnen Sie auf dem SharePoint-Server die SharePoint 201x-Verwaltungsshell, und führen Sie die folgenden Befehle aus. Verwenden Sie den Namen für den Aussteller vertrauenswürdiger Identitätstoken, den Sie zuvor verwendet haben.

```powershell
$t = Get-SPTrustedIdentityTokenIssuer "AzureADTrust"
$t.UseWReplyParameter = $true
$t.Update()
```

### <a name="add-the-urls-in-the-enterprise-application"></a>Hinzufügen der URLs in der Unternehmensanwendung

1. Wählen Sie im Azure-Portal **Azure Active Directory** > **Unternehmens-Apps** aus. Wählen Sie den Namen der zuvor erstellten Unternehmensanwendung aus und dann **Einmaliges Anmelden**.

1. Bearbeiten Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** den Abschnitt **Grundlegende SAML-Konfiguration**.

1. Fügen Sie im Abschnitt **Antwort-URL (Assertionsverbraucherdienst-URL)** die URL (z. B. `https://otherwebapp.contoso.local/`) aller zusätzlichen Webanwendungen hinzu, die Benutzer mit Azure Active Directory anmelden müssen, und klicken Sie auf **Speichern**.

![Angeben zusätzlicher Webanwendungen](./media/sharepoint-on-premises-tutorial/azure-active-directory-app-reply-urls.png)