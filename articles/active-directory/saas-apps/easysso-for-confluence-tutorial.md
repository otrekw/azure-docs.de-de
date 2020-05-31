---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit EasySSO for Confluence | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und EasySSO for Confluence konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 959019be-c7f6-41cc-b0ff-c5927ce64a74
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 05/18/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a0ac6d86c1ef4fb8f2f5b75ac47f1a25e2cda0b
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83748172"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-easysso-for-confluence"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit EasySSO for Confluence

In diesem Tutorial erfahren Sie, wie Sie EasySSO for Confluence in Azure Active Directory (Azure AD) integrieren. Die Integration von EasySSO for Confluence in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf EasySSO for Confluence hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei EasySSO for Confluence anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* EasySSO for Confluence-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* EasySSO for Confluence unterstützt **SP- und IDP**-initiiertes einmaliges Anmelden.
* EasySSO for Confluence unterstützt die **Just-In-Time**-Benutzerbereitstellung.
* Nach dem Konfigurieren von EasySSO for Confluence können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.

## <a name="adding-easysso-for-confluence-from-the-gallery"></a>Hinzufügen von EasySSO for Confluence aus dem Katalog

Zum Konfigurieren der Integration von EasySSO for Confluence in Azure AD müssen Sie EasySSO for Confluence aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **EasySSO for Confluence** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **EasySSO for Confluence** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-easysso-for-confluence"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für EasySSO for Confluence

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit EasySSO for Confluence mithilfe einer Testbenutzerin mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in EasySSO for Confluence eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit EasySSO for Confluence die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für EasySSO for Confluence](#configure-easysso-for-confluence-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
    1. **[Erstellen eines EasySSO for Confluence-Testbenutzers](#create-easysso-for-confluence-test-user)** , um eine Entsprechung von B. Simon in EasySSO for Confluence zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **EasySSO for Confluence** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<server-base-url>/plugins/servlet/easysso/saml`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<server-base-url>/plugins/servlet/easysso/saml`

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<server-base-url>/`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem tatsächlichen Bezeichner, der Antwort-URL und der Anmelde-URL aktualisieren. Diese Werte erhalten Sie vom [Supportteam für den EasySSO for Confluence-Client](mailto:support@techtime.co.nz). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Die Anwendung EasySSO for Confluence erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute.

    ![image](common/default-attributes.png)

1. Darüber hinaus wird von der EasySSO for Confluence-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden (siehe unten). Diese Attribute werden ebenfalls vorab aufgefüllt, Sie können sie jedoch nach Bedarf überprüfen.

    | Name |  Quellattribut|
    | --------------- | --------- |
    | urn:oid:2.16.840.1.113730.3.1.241 | user.displayname |
    | urn:oid:2.5.4.42 | user.givenname |
    | urn:oid:2.5.4.4 | user.surname |
    | urn:oid:0.9.2342.19200300.100.1.1 | user.userprincipalname|
    | urn:oid:0.9.2342.19200300.100.1.3 | user.userprincipalname |
    | | |

1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/copy-metadataurl.png)

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf EasySSO for Confluence gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **EasySSO for Confluence** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-easysso-for-confluence-sso"></a>Konfigurieren des einmaligen Anmeldens für EasySSO for Confluence

1. Melden Sie sich bei Ihrer EasySSO for Confluence-Instanz mit Administratorrechten an, und navigieren Sie zum Abschnitt **Apps verwalten**.

    ![Apps verwalten](./media/easysso-for-confluence-tutorial/jira-admin-1.png)

1. Klicken Sie auf **EasySSO**.

    ![Einfaches einmaliges Anmelden (SSO)](./media/easysso-for-confluence-tutorial/jira-admin-2.png)

1. Wählen Sie die Option **SAML** aus. Hierdurch gelangen Sie zum Abschnitt „SAML-Konfiguration“.

    ![SAML](./media/easysso-for-confluence-tutorial/jira-admin-3.png)

1. Wählen Sie oben die Registerkarte **Zertifikate** aus, und Ihnen wird der folgende Bildschirm angezeigt, wo Sie **Zertifikat (Base64)** oder **Metadatendatei** suchen, die Sie in den vorherigen Schritten der Konfiguration von **einmaliges Anmelden (SSO) für Azure AD** gespeichert haben. Folgende Möglichkeiten, um fortzufahren, stehen zur Verfügung:

    ![Metadaten-URL](./media/easysso-for-confluence-tutorial/jira-admin-4.png)

    a. Verwenden Sie die **Metadatendatei** des App-Verbunds, die Sie als lokale Datei auf Ihren Computer heruntergeladen haben. Aktivieren Sie das Optionsfeld **Hochladen**, und befolgen Sie das Dialogfeld „Datei hochladen“, das für Ihr Betriebssystem zutrifft.

    **OR**

    b. Öffnen Sie die **Metadatendatei** des App-Verbunds, um den Inhalt der Datei (in einem beliebigen Text-Editor) anzuzeigen und in die Zwischenablage zu kopieren. Wählen Sie die Option **Eingabe** aus, und fügen Sie den Inhalt der Zwischenablage in das Textfeld ein.

    **OR**

    c. Vollständig manuelle Konfiguration. Öffnen Sie das **Zertifikat (Base64)** des App-Verbunds, um den Inhalt der Datei (in einem beliebigen Text-Editor) anzuzeigen und in die Zwischenablage zu kopieren. Fügen Sie ihn in das Textfeld **IdP-Tokensignaturzertifikate** ein. Navigieren Sie dann zur Registerkarte **Allgemein**, und füllen Sie die Felder **POST-Bindungs-URL** und **Entitäts-ID** mit den entsprechenden Werten für **Anmelde-URL** und **Azure AD-Bezeichner**, die Sie zuvor gespeichert haben, aus.

1. Klicken Sie unten auf der Seite auf die Schaltfläche **Speichern**. Sie sehen dann, wie der Inhalt der Metadaten- oder Zertifikatsdatei in die Konfigurationsfelder analysiert wird. Die -Konfiguration von EasySSO for Confluence ist abgeschlossen.

1. Für eine optimale Testerfahrung navigieren Sie zur Registerkarte **Aussehen und Verhalten**, und aktivieren Sie die Option **SAML-Anmeldeschaltfläche**. Dadurch wird eine separate Schaltfläche auf dem Jira-Anmeldebildschirm aktiviert,die speziell dem End-to-End-Testen Ihrer Azure AD SAML-Integration dient. Sie können diese Schaltfläche aktiviert lassen und außerdem ihre Platzierung, Farbe und Übersetzung für den Produktionsmodus konfigurieren.

    ![Aussehen und Verhalten](./media/easysso-for-confluence-tutorial/jira-admin-5.png)

    > [!NOTE]
    > Sollten Probleme auftreten, wenden Sie sich an das [EasySSO-Supportteam](mailto:support@techtime.co.nz).

### <a name="create-easysso-for-confluence-test-user"></a>Erstellen eines EasySSO for Confluence-Testbenutzers

In diesem Abschnitt wird in EasySSO for Confluence eine Benutzerin mit dem Namen B.Simon erstellt. EasySSO für Confluence unterstützt die Just-In-Time-Benutzerbereitstellung (standardmäßig **deaktiviert**). Um die Benutzerbereitstellung zu aktivieren, müssen Sie die Option **Benutzer bei erfolgreicher Anmeldung erstellen** im Abschnitt „Allgemein“ der EasySSO-Plug-In-Konfiguration explizit aktivieren. Ist ein Benutzer noch nicht in EasySSO for Confluence vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

Wenn Sie die automatische Benutzerbereitstellung bei der ersten Benutzeranmeldung jedoch nicht aktivieren möchten, müssen Benutzer in Back-End-Benutzerverzeichnissen vorhanden sein, die von der EasySSO for Confluence-Instanz verwendet werden, wie z. B. „LDAP“ oder „Atlassian Crowd“.

![Benutzerbereitstellung](./media/easysso-for-confluence-tutorial/jira-admin-6.png)

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „EasySSO for Confluence“ klicken, sollten Sie automatisch bei Ihrer EasySSO for Confluence-Anwendung angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Testen von EasySSO for Confluence mit Azure AD](https://aad.portal.azure.com/)

- [Was ist Sitzungssteuerung in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Schützen von EasySSO for Confluence mit erweiterter Sichtbarkeit und komplexeren Kontrollen](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

