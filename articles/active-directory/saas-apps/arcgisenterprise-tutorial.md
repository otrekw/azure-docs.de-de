---
title: 'Tutorial: Azure Active Directory-Integration mit ARcGIS Enterprise | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ArcGIS Enterprise konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/28/2018
ms.author: jeedes
ms.openlocfilehash: fc0303cc4ba00f873ecfea7b02047ebd1bff7da1
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91713877"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-enterprise"></a>Tutorial: Azure Active Directory-Integration in ArcGIS Enterprise

In diesem Tutorial erfahren Sie, wie Sie ArcGIS Enterprise in Azure Active Directory (Azure AD) integrieren.
Die Integration von ArcGIS Enterprise in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer auf ArcGIS Enterprise Zugriff hat.
* Sie können es Ihren Benutzern ermöglichen, dass sie mit ihren Azure AD-Konten automatisch bei ArcGIS Enterprise angemeldet werden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit ArcGIS Enterprise konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* ArcGIS Enterprise-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Diese Integration kann auch über die Azure AD-Umgebung für die US Government-Cloud verwendet werden. Sie finden diese Anwendung im Azure AD-Katalog für US Government-Cloudanwendungen und konfigurieren sie auf die gleiche Weise wie in der öffentlichen Cloud.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* ArcGIS Enterprise unterstützt **SP- und IDP**-initiiertes einmaliges Anmelden.
* ArcGIS Enterprise unterstützt die **Just-in-Time**-Benutzerbereitstellung.


## <a name="adding-arcgis-enterprise-from-the-gallery"></a>Hinzufügen von ArcGIS Enterprise aus dem Katalog

Zum Konfigurieren der Integration von ArcGIS Enterprise in Azure AD müssen Sie ArcGIS Enterprise aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um ArcGIS Enterprise aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **ArcGIS Enterprise** ein, wählen Sie im Ergebnisbereich **ArcGIS Enterprise** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![ArcGIS Enterprise in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei [Anwendungsname] mithilfe einer Testbenutzerin namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in [Anwendungsname] eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei [Anwendungsname] müssen Sie die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für ArcGIS Enterprise](#configure-arcgis-enterprise-single-sign-on)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines ArcGIS Enterprise-Testbenutzers](#create-arcgis-enterprise-test-user)**, um eine Entsprechung von Britta Simon in ArcGIS Enterprise zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit [Anwendungsname] zu konfigurieren:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **ArcGIS Enterprise** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![Screenshot: Seite „Grundlegende SAML-Konfiguration“ zum Eingeben des Bezeichners und einer Antwort-URL sowie zum Klicken auf „Speichern“](common/idp-intiated.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `<EXTERNAL_DNS_NAME>.portal`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin2`

    c. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![Screenshot: Option „Zusätzliche URLs festlegen“ zum Eingeben einer Anmelde-URL](common/metadata-upload-additional-signon.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem tatsächlichen Bezeichner, der Antwort-URL und der Anmelde-URL aktualisieren. Wenden Sie sich an das [Supportteam von ArcGIS Enterprise Client](mailto:support@esri.com), um diese Werte zu erhalten. Sie erhalten den Bezeichnerwert im Abschnitt **Festlegen des Identitätsanbieters** weiter unten in diesem Tutorial.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/copy-metadataurl.png)

### <a name="configure-arcgis-enterprise-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für ArcGIS Enterprise

1. Wenn Sie die Konfiguration in ArcGIS Enterprise automatisieren möchten, müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![Erweiterung „Meine Apps“](common/install-myappssecure-extension.png)

1. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **ArcGIS Enterprise einrichten**, um zur Anwendung ArcGIS Enterprise weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei ArcGIS Enterprise anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert die Schritte 3 bis 7.

    ![Einrichtungskonfiguration](common/setup-sso.png)

1. Wenn Sie ArcGIS Enterprise manuell einrichten möchten, melden Sie sich bei der ArcGIS Enterprise-Unternehmenswebsite als Administrator an.


1. Wählen Sie **Organisation > EINSTELLUNGEN BEARBEITEN**.

    ![Screenshot: Registerkarte „Organisation“ für ArcGIS Enterprise mit hervorgehobener Option „Einstellungen bearbeiten“](./media/arcgisenterprise-tutorial/configure1.png)

1. Wählen Sie die Registerkarte **Security** (Sicherheit) aus.

    ![Screenshot: Ausgewählte Registerkarte „Sicherheit“](./media/arcgisenterprise-tutorial/configure2.png)

1. Führen Sie einen Bildlauf nach unten zum Abschnitt **Enterprise-Anmeldungen über SAML** aus, und wählen Sie **ENTERPRISE-ANMELDUNG FESTLEGEN** aus.

    ![Screenshot: Abschnitt „Enterprise Logins via SAML“ (Unternehmensanmeldungen über SAML) mit der Option „Set Enterprise Login“ (Unternehmensanmeldung festlegen)](./media/arcgisenterprise-tutorial/configure3.png)

1. Führen Sie im Abschnitt **Festlegen des Identitätsanbieters** die folgenden Schritte aus:

    ![Screenshot: Abschnitt „Set Identity Provider“ (Identitätsanbieter festlegen) zum Ausführen der hier beschriebenen Schritte](./media/arcgisenterprise-tutorial/configure4.png)

    a. Geben Sie einen Namen wie **Azure Active Directory-Test** in das Textfeld **Name** ein.

    b. Fügen Sie im Textfeld **URL** den Wert der **Verbundmetadaten-URL der App** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Klicken Sie auf **Erweiterte Einstellungen anzeigen**, und kopieren Sie den Wert **Entitäts-ID** und fügen Sie ihn in das Textfeld **Bezeichner** im Abschnitt **ArcGIS-Enterprise-Domäne und URLs** im Azure-Portal ein.
    
    ![Screenshot: Abrufen der Entitäts-ID und Aktualisieren des Identitätsanbieters](./media/arcgisenterprise-tutorial/configure5.png)

    d. Klicken Sie auf **IDENTITÄTSANBIETER AKTUALISIEREN**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers 

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** Folgendes ein: **brittasimon\@ihreunternehmensdomäne.erweiterung**.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ArcGIS Enterprise gewähren.

1. Wählen Sie im Azure-Portal nacheinander die Optionen **Unternehmensanwendungen**, **Alle Anwendungen** und **ArcGIS Enterprise**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Geben Sie in der Anwendungsliste **ArcGIS Enterprise** ein, und wählen Sie den Eintrag aus.

    ![ArcGIS Enterprise-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-arcgis-enterprise-test-user"></a>Erstellen eines ArcGIS Enterprise-Testbenutzers

In diesem Abschnitt wird in ArcGIS Enterprise ein Benutzer namens Britta Simon erstellt. ArcGIS Enterprise unterstützt die Just-in-Time-Benutzerbereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn ein Benutzer noch nicht in ArcGIS Enterprise vorhanden ist, wird nach der Authentifizierung ein neuer Benutzer erstellt.

> [!Note]
> Wenn Sie einen Benutzer manuell erstellen müssen, können Sie sich an das [ArcGIS Enterprise-Supportteam](mailto:support@esri.com) wenden.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ArcGIS Enterprise“ klicken, sollten Sie automatisch bei Ihrer ArcGIS Enterprise-Anwendung angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist bedingter Zugriff?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

