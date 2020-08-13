---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory in Freshservice | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Freshservice konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 07/29/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06b52ec552434f74c56333d1afe00cde2a285418
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87905396"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-freshservice"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory in Freshservice

In diesem Tutorial erfahren Sie, wie Sie Freshservice in Azure Active Directory (Azure AD) integrieren. Bei der Integration von Freshservice in Azure AD haben Sie folgende Möglichkeiten:

* Steuern Sie in Azure AD, wer Zugriff auf Freshservice hat.
* Ermöglichen Sie Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Freshservice anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Freshservice-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist.

> [!NOTE]
> Diese Integration kann auch über die Azure AD-Umgebung für die US Government-Cloud verwendet werden. Sie finden diese Anwendung im Azure AD-Katalog für US Government-Cloudanwendungen und konfigurieren sie auf die gleiche Weise wie in der öffentlichen Cloud.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Freshservice unterstützt **SP**-initiiertes einmaliges Anmelden.
* Nach dem Konfigurieren von Freshservice können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.

## <a name="adding-freshservice-from-the-gallery"></a>Hinzufügen von Freshservice aus dem Katalog

Zum Konfigurieren der Integration von Freshservice in Azure AD müssen Sie Freshservice aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Freshservice** in das Suchfeld ein.
1. Wählen Sie **Freshservice** im Ergebnisbereich aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-freshservice"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Freshservice

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Freshservice mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Freshservice eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Freshservice müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Freshservice](#configure-freshservice-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
    1. **[Erstellen eines Freshservice-Testbenutzers](#create-freshservice-test-user)** , um eine Entsprechung von B. Simon in Freshservice zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Freshservice** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<democompany>.freshservice.com`

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein: `https://<democompany>.freshservice.com`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Clientsupportteam von Freshservice](https://support.freshservice.com/), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Suchen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** den Eintrag **Zertifikat (Base64)** . Klicken Sie auf **Herunterladen**, um das Zertifikat herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im **Azure-Portal** im Abschnitt **Freshservice einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Freshservice gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste den Eintrag **Freshservice** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-freshservice-sso"></a>Konfigurieren des einmaligen Anmeldens für Freshservice

1. Melden Sie sich in einem neuen Webbrowserfenster bei der Freshservice-Unternehmenswebsite als Administrator an.

1. Klicken Sie im Menü auf der linken Seite auf **Admin** (Verwaltung), und wählen Sie unter **General Settings** (Allgemeine Einstellungen) die Option **Helpdesk Security** (Helpdesksicherheit) aus.

    ![Administrator](./media/freshservice-tutorial/configure-1.png "Admin")

1. Klicken Sie unter **Security** (Sicherheit) auf **Go to Freshworks 360 Security** (Zu Freshworks 360-Sicherheit wechseln).

    ![Security](./media/freshservice-tutorial/configure-2.png "Sicherheit")

1. Führen Sie im Abschnitt **Sicherheit** die folgenden Schritte aus:

    ![Einmaliges Anmelden](./media/freshservice-tutorial/configure-3.png "Einmaliges Anmelden")
  
    a. Wählen Sie unter **Single Sign On** (Einmaliges Anmelden) die Einstellung **On** (Ein) aus.

    b. Wählen Sie unter **Login Method** (Anmeldemethode) die Option **SAML SSO** (SAML-SSO) aus.

    c. Fügen Sie im Textfeld **Entity ID provided by the IdP** (Vom IdP bereitgestellte Entitäts-ID) den Wert der **Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Fügen Sie im Textfeld **SAML SSO URL** (SAML-SSO-URL) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. Wählen Sie unter **Signing Options** (Signaturoptionen) im Dropdownmenü die Option **Only Signed Assertions** (Nur signierte Assertionen) aus.

    f. Fügen Sie im Textfeld **Abmelde-URL** den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    g. Fügen Sie im Textfeld **Security Certificate** (Sicherheitszertifikat) den Wert für **Zertifikat (Base64)** ein, den Sie zuvor erhalten haben.
  
    h. Klicken Sie auf **Speichern**.


## <a name="create-freshservice-test-user"></a>Erstellen eines Freshservice-Testbenutzers

Damit sich Azure AD-Benutzer bei Freshservice anmelden können, müssen sie in Freshservice bereitgestellt werden. Im Fall von FreshService ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der **Freshservice** -Unternehmenswebsite als Administrator an.

2. Klicken Sie im Menü auf der linken Seite auf **Admin** (Verwaltung).

3. Klicken Sie im Abschnitt **Benutzerverwaltung** auf **Anfordernde Personen**.

    ![Anfordernde Personen](./media/freshservice-tutorial/create-user-1.png "Anfordernde Personen")

4. Klicken Sie auf **Neue anfordernde Person**.

    ![Neue anfordernde Personen](./media/freshservice-tutorial/create-user-2.png "Neue anfordernde Personen")

5. Füllen Sie im Abschnitt **New Requester** (Neuer Anforderer) die erforderlichen Felder aus, und klicken Sie auf **Save** (Speichern).
    ![Neue anfordernde Person](./media/freshservice-tutorial/create-user-3.png "Neue anfordernde Person")  

    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
    >  

    > [!NOTE]
    > Sie können Azure AD-Benutzerkonten auch mit anderen Tools zum Erstellen von FreshService-Benutzerkonten oder mit den APIs von FreshService bereitstellen.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Freshservice“ klicken, sollten Sie automatisch bei Ihrer Freshservice-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Freshservice mit Azure AD ausprobieren](https://aad.portal.azure.com/)