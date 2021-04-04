---
title: 'Tutorial: Azure Active Directory-Integration mit Kintone | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Kintone konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 96ea3423d3c2dff2c8ba8c82b4c26d318c47211f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92459049"
---
# <a name="tutorial-azure-active-directory-integration-with-kintone"></a>Tutorial: Azure Active Directory-Integration mit Kintone

In diesem Tutorial erfahren Sie, wie Sie Kintone in Azure Active Directory (Azure AD) integrieren.
Die Integration von Kintone in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Kintone hat.
* Sie können es Ihren Benutzern ermöglichen, dass sie mit ihren Azure AD-Konten automatisch bei Kintone angemeldet werden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Kintone konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Sollten Sie über keine Azure AD-Umgebung verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* Kintone-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Kintone unterstützt **SP**-initiiertes einmaliges Anmelden.

## <a name="adding-kintone-from-the-gallery"></a>Hinzufügen von Kintone über den Katalog

Zum Konfigurieren der Integration von Kintone in Azure AD müssen Sie Kintone aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Kintone aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Kintone** ein, wählen Sie im Ergebnisbereich **Kintone** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Kintone in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Kintone mithilfe einer Testbenutzerin namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Kintone eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Kintone müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Kintone](#configure-kintone-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Kintone-Testbenutzers](#create-kintone-test-user)** , um eine Entsprechung von Britta Simon in Kintone zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Kintone die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Kintone** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Kintone](common/sp-identifier.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.kintone.com`.

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein:
    
    ```http
    https://<companyname>.cybozu.com
    https://<companyname>.kintone.com
    ```

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Clientsupportteam von Kintone](https://www.kintone.com/contact/), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

6. Kopieren Sie im Abschnitt **Kintone einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-kintone-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Kintone

1. Melden Sie sich in einem anderen Webbrowserfenster an der **Kintone**-Unternehmenswebsite als Administrator an.

1. Klicken Sie auf das Symbol **Einstellungen**.

    ![Einstellungen](./media/kintone-tutorial/ic785879.png "Einstellungen")

1. Klicken Sie auf **Benutzer und Systemadministration**.

    ![Benutzer- und Systemverwaltung](./media/kintone-tutorial/ic785880.png "Benutzer und Systemadministration")

1. Klicken Sie unter **Systemadministration \> Sicherheit** auf **Anmelden**.

    ![Anmeldung](./media/kintone-tutorial/ic785881.png "Anmeldename")

1. Klicken Sie auf **SAML-Authentifizierung aktivieren**.

    ![Screenshot, auf dem „Users & System Administration“ (Benutzer und Systemverwaltung) ausgewählt ist](./media/kintone-tutorial/ic785882.png "SAML-Authentifizierung")

1. Führen Sie im Abschnitt für die SAML-Authentifizierung die folgenden Schritte aus:

    ![SAML Authentication (SAML-Authentifizierung)](./media/kintone-tutorial/ic785883.png "SAML-Authentifizierung")

    a. Fügen Sie in das Textfeld **Login URL** (Anmelde-URL) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie in das Textfeld **Logout URL** (Abmelde-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Klicken Sie auf **Durchsuchen**, um die aus dem Azure-Portal heruntergeladene Zertifikatdatei hochzuladen.

    d. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen `brittasimon@yourcompanydomain.extension` ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Kintone gewähren.

1. Wählen Sie im Azure-Portal nacheinander die Optionen **Unternehmensanwendungen**, **Alle Anwendungen** und **Kintone**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Kintone** aus.

    ![Kintone-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-kintone-test-user"></a>Erstellen eines Kintone-Testbenutzers

Damit sich Azure AD-Benutzer bei Kintone anmelden können, müssen sie in Kintone bereitgestellt werden. Im Fall von Kintone ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:

1. Melden Sie sich an der **Kintone**-Unternehmenswebsite als Administrator an.

1. Klicken Sie auf das Symbol **Einstellungen**.

    ![Einstellungen](./media/kintone-tutorial/ic785879.png "Einstellungen")

1. Klicken Sie auf **Benutzer und Systemadministration**.

    ![Benutzer- und Systemverwaltung](./media/kintone-tutorial/ic785880.png "Benutzer und Systemadministration")

1. Klicken Sie unter **Benutzerverwaltung** auf **Abteilungen und Benutzer**.

    ![Abteilung und Benutzer](./media/kintone-tutorial/ic785888.png "Abteilung und Benutzer")

1. Klicken Sie auf **Neuer Benutzer**.

    ![Screenshot: Abschnitt „Users“ (Benutzer), in dem die Aktion „New User“ (Neuer Benutzer) ausgewählt ist](./media/kintone-tutorial/ic785889.png "Neue Benutzer")

1. Führen Sie im Abschnitt **Neuer Benutzer** die folgenden Schritte aus:

    ![Neue Benutzer](./media/kintone-tutorial/ic785890.png "Neue Benutzer")

    a. Geben Sie **Anzeigename**, **Anmeldename**, **Neues Kennwort**, **Kennwort bestätigen**, **E-Mail-Adresse** und weitere Details eines gültigen Azure AD-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.

    b. Klicken Sie auf **Speichern**.

> [!NOTE]
> Sie können Azure AD-Benutzerkonten auch mit anderen Tools zum Erstellen von Kintone-Benutzerkonten oder mit den APIs von Kintone bereitstellen.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Kintone“ klicken, sollten Sie automatisch bei der Kintone-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](./tutorial-list.md)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Was ist bedingter Zugriff?](../conditional-access/overview.md)