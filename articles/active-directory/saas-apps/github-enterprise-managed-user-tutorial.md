---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit GitHub Enterprise Managed User | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und GitHub Enterprise Managed User konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/15/2021
ms.author: jeedes
ms.openlocfilehash: ffdd6c30e279cc5df7f97e5ab5bb77a87c18dd8b
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110480497"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-github-enterprise-managed-user"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit GitHub Enterprise Managed User

In diesem Tutorial erfahren Sie, wie Sie GitHub Enterprise Managed User (EMU) in Azure Active Directory (Azure AD) integrieren. Die Integration von GitHub Enterprise Managed User mit Azure AD ermöglicht Folgendes:

* Sie können in Azure AD steuern, wer Zugriff auf GitHub Enterprise Managed User haben soll.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei GitHub Enterprise Managed User anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* SSO-fähiges GitHub Enterprise Managed User-Abonnement

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* GitHub Enterprise Managed User unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.
* Für GitHub Enterprise Managed User ist die [**automatische** Benutzerbereitstellung](./github-enterprise-managed-user-provisioning-tutorial.md) erforderlich.

## <a name="adding-github-enterprise-managed-user-from-the-gallery"></a>Hinzufügen von GitHub Enterprise Managed User aus dem Katalog

Zum Konfigurieren der Integration von GitHub Enterprise Managed User in Azure AD müssen Sie GitHub Enterprise Managed User aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **GitHub Enterprise Managed User** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **GitHub Enterprise Managed User** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-sso-for-github-enterprise-managed-user"></a>Konfigurieren und Testen des einmaligen Anmeldens für GitHub Enterprise Managed User

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit GitHub Enterprise Managed User die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um in Ihrem AAD-Mandanten das einmalige Anmelden für SAML (SAML-SSO) zu aktivieren.
1. **[Konfigurieren des einmaligen Anmeldens für GitHub Enterprise Managed User](#configure-github-enterprise-managed-user-sso)** , um die Einstellungen für einmaliges Anmelden in Ihrer GitHub Enterprise-Instanz zu konfigurieren.

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **GitHub Enterprise Managed User** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Stellen Sie sicher, dass Sie die URL Ihrer Enterprise-Instanz kennen, bevor Sie beginnen. Das unten genannte Feld „ENTITÄT“ enthält den Unternehmensnamen Ihrer EMU-fähigen Enterprise-URL, beispielsweise https://github.com/enterprises/contoso  - **contoso** als ENTITÄT. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://github.com/enterprises/<ENTITY>`
    
    > [!NOTE]
    > Beachten Sie, dass sich das Bezeichnerformat von dem von der Anwendung vorgeschlagenen Format unterscheidet. Sie müssen jedoch das oben genannte Format verwenden. Stellen Sie außerdem sicher, dass der **Bezeichner keinen nachstehenden Schrägstrich enthält.
    
    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://github.com/enterprises/<ENTITY>/saml/consume`
    

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://github.com/enterprises/<ENTITY>/sso`

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zum Eintrag **Zertifikat (Base64)** . Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/certificate-base64-download.png)

1. Kopieren Sie im Abschnitt **Einrichten von GitHub Enterprise Managed User** die unten angegebenen URLs, und speichern Sie sie für die spätere Konfiguration von GitHub.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt weisen Sie GitHub Enterprise Managed User Ihr Konto zu, um das Einrichten des einmaligen Anmeldens abzuschließen.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste die Option **GitHub Enterprise Managed User** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ Ihr Konto aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wählen Sie im Dialogfeld **Rolle auswählen** die Rolle **Unternehmensbesitzer** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**. Ihr Konto wird Ihrer GitHub-Instanz als Unternehmensbesitzer zugewiesen, wenn Sie im nächsten Tutorial Ihr Konto bereitstellen. 
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-github-enterprise-managed-user-sso"></a>Konfigurieren des einmaligen Anmeldens für GitHub Enterprise Managed User

Zum Konfigurieren des einmaligen Anmeldens für **GitHub Enterprise Managed User** benötigen Sie Folgendes:

1. Die oben genannten URLs aus Ihrer AAD Enterprise Managed User-Anwendung: Anmelde-URL; Azure AD-Bezeichner und Abmelde-URL
1. Kontoname und Kennwort für den ersten Administratorbenutzer Ihrer GitHub Enterprise-Instanz. Die Anmeldeinformationen werden über eine E-Mail für die Kennwortzurücksetzung von Ihrem GitHub Solutions Engineering-Kontakt bereitgestellt. 

### <a name="enable-github-enterprise-managed-user-saml-sso"></a>Konfigurieren von SAML-SSO für GitHub Enterprise Managed User

In diesem Abschnitt verwenden Sie die oben von AAD bereitgestellten Informationen und geben sie in Ihre Enterprise-Einstellungen ein, um die SSO-Unterstützung zu aktivieren.

1. Besuchen Sie https://github.com.
1. Klicken Sie in der oberen rechten Ecke auf „Anmelden“.
1. Geben Sie die Anmeldeinformationen für das erste Administratorbenutzerkonto ein. Das Anmeldehandle sollte folgendes Format haben: `<your enterprise short code>_admin`
1. Navigieren Sie zu https://github.com/enterprises/ `<your enterprise name>`. Diese Informationen sollten von Ihrem Solution Engineering-Kontakt bereitgestellt werden.
1. Wählen Sie im Navigationsmenü auf der linken Seite **Einstellungen** und dann **Sicherheit** aus.
1. Aktivieren Sie das Kontrollkästchen **SAML-Authentifizierung aktivieren**.
1. Geben Sie die Anmelde-URL ein. Diese URL ist die Anmelde-URL, die Sie oben aus AAD kopiert haben.
1. Geben Sie den Zertifikataussteller ein. Diese URL ist der Azure AD-Bezeichner, den Sie oben aus AAD kopiert haben.
1. Geben Sie das öffentliche Zertifikat ein. Öffnen Sie das zuvor heruntergeladene base64-Zertifikat, und fügen Sie den Textinhalt dieser Datei in dieses Dialogfeld ein.
1. Klicken Sie auf **SAML-Konfiguration testen**. Dadurch wird ein Dialogfeld geöffnet, in dem Sie sich mit Ihren Azure AD-Anmeldeinformationen anmelden können, um zu überprüfen, ob SAML-SSO ordnungsgemäß konfiguriert ist. Melden Sie sich mit Ihren AAD-Anmeldeinformationen an. Sie erhalten die Meldung **Erfolgreich: Ihre SAML-SSO-Identität nach erfolgreicher Überprüfung erfolgreich authentifiziert**.
1. Klicken Sie auf **Speichern**, um diese Einstellungen zu speichern.
1. Sie sollten die Wiederherstellungscodes an einem sicheren Ort speichern (herunterladen, drucken oder kopieren).
1. Klicken Sie auf **SAML-Authentifizierung aktivieren**.
1. An diesem Punkt können sich nur Konten mit SSO bei Ihrem Unternehmen anmelden. Befolgen Sie die Anweisungen zur Bereitstellung im folgenden Dokument, um Konten mit SSO-Unterstützung bereitzustellen.

## <a name="next-steps"></a>Nächste Schritte

GitHub Enterprise Managed User **setzt die Bereitstellung aller Konten über die automatisierte Benutzerbereitstellung voraus**. Weitere Informationen zum Konfigurieren der automatischen Benutzerbereitstellung finden Sie [hier](./github-enterprise-managed-user-provisioning-tutorial.md).
