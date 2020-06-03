---
title: 'Tutorial: Azure Active Directory-Integration mit Mind Tools Toolkit | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und Mind Tools Toolkit konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 65b2979d-9e2f-4530-bc08-546975269ebc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/12/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c0c1cab94a6b83ca429fd640759bed8af0ae881
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124855"
---
# <a name="tutorial-azure-active-directory-integration-with-mind-tools-toolkit"></a>Tutorial: Azure Active Directory-Integration mit Mind Tools Toolkit

In diesem Tutorial erfahren Sie, wie Sie Mind Tools Toolkit in Azure Active Directory (Azure AD) integrieren.

Diese Integration ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Mind Tools Toolkit hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Mind Tools Toolkit anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps (Software as a Service) mit Azure AD finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Voraussetzungen

Für die Konfiguration der Azure AD-Integration mit Mind Tools Toolkit benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Mind Tools Toolkit-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Mind Tools Toolkit unterstützt SP-initiiertes einmaliges Anmelden.
* Mind Tools Toolkit unterstützt die Just-In-Time-Benutzerbereitstellung.
* Nach dem Konfigurieren von Mind Tools Toolkit können Sie die Sitzungssteuerung erzwingen. Diese Steuerung schützt in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.

## <a name="add-mind-tools-toolkit-from-the-gallery"></a>Hinzufügen von Mind Tools Toolkit aus dem Katalog

Zum Konfigurieren der Integration von Mind Tools Toolkit in Azure AD müssen Sie Mind Tools Toolkit aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im Navigationsbereich ganz links den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Mind Tools Toolkit** in das Suchfeld ein.
1. Wählen Sie in den Suchergebnissen **Mind Tools Toolkit** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Mind Tools Toolkit mithilfe eines Testbenutzers namens **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Mind Tools Toolkit eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Mind Tools Toolkit die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Mind Tools Toolkit](#configure-mind-tools-toolkit-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines Mind Tools Toolkit-Testbenutzers](#create-a-mind-tools-toolkit-test-user)** , um eine Entsprechung von B. Simon in Mind Tools Toolkit zu erhalten Diese Entsprechung ist mit der Benutzerdarstellung in Azure AD verknüpft.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

In diesem Abschnitt führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit Mind Tools Toolkit die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Mind Tools Toolkit** die Option **Einmaliges Anmelden** aus.

    ![Abschnitt „Verwalten“ mit hervorgehobener Option für einmaliges Anmelden](common/select-sso.png)

1. Wählen Sie im Dialogfeld **SSO-Methode auswählen** die Option **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „SSO-Methode auswählen“ mit Hervorhebung von „SAML“](common/select-saml-option.png)

1. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** das Stiftsymbol für **Grundlegende SAML-Konfiguration** aus, um die Einstellungen zu bearbeiten.

    ![Seite „Einmaliges Anmelden (SSO) mit SAML einrichten“ mit hervorgehobenem Stiftsymbol für „Grundlegende SAML-Konfiguration“](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** im Feld **Anmelde-URL** eine URL im folgenden Format ein: `https://app.goodpractice.net/#/<subscriptionUrl>/s/<locationId>`.

    > [!NOTE]
    > Der Wert von **Anmelde-URL** entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Den Wert erhalten Sie vom [Supportteam für den Mind Tools Toolkit-Client](mailto:support@goodpractice.com).

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** zum Abschnitt **SAML-Signaturzertifikat**. Wählen Sie rechts neben **Verbundmetadaten-XML** die Option **Herunterladen** aus, um den XML-Text herunterzuladen, und speichern Sie ihn auf Ihrem Computer. Der XML-Inhalt hängt von den ausgewählten Optionen ab.

    ![Abschnitt „SAML-Signaturzertifikat“ neben „Verbundmetadaten-XML“ mit hervorgehobener Option „Herunterladen“](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **Mind Tools Toolkit einrichten** nach Bedarf eine der folgenden URLs:

    * **Anmelde-URL**

    * **Azure AD-Bezeichner**

    * **Abmelde-URL**

    ![Abschnitt „Mind Tools Toolkit einrichten“ mit hervorgehobenen Konfigurations-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon:

1. Wählen Sie im Azure-Portal ganz links **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie **B.Simon** in das Feld **Name** ein.  
   1. Geben Sie im Feld **Benutzername** Folgendes ein: **B.Simon@** _Unternehmensdomäne_ **.** _Erweiterung_. Beispiel: B.Simon@contoso.com.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Mind Tools Toolkit gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Mind Tools Toolkit** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Abschnitt „Verwalten“ mit hervorgehobener Option „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Klicken Sie auf **Benutzer hinzufügen**. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

   ![Fenster „Benutzer und Gruppen“ mit hervorgehobener Option zum Hinzufügen eines Benutzers](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ die Option **B.Simon** aus. Wählen Sie dann unten auf dem Bildschirm die Schaltfläche **Auswählen** aus.
1. Falls Sie in der SAML-Assertion einen Rollenwert erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer aus der Liste aus. Wählen Sie dann unten auf dem Bildschirm die Schaltfläche **Auswählen** aus.
1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.

## <a name="configure-mind-tools-toolkit-sso"></a>Konfigurieren des einmaligen Anmeldens für Mind Tools Toolkit

Senden Sie zum Konfigurieren des einmaligen Anmeldens aufseiten von **Mind Tools Toolkit** den heruntergeladenen Text der **Verbundmetadaten-XML** und die zuvor kopierten URLs an das [Supportteam von Mind Tools Toolkit](mailto:support@goodpractice.com). Das Team konfiguriert diese Einstellung, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-a-mind-tools-toolkit-test-user"></a>Erstellen eines Mind Tools Toolkit-Testbenutzers

In diesem Abschnitt erstellen Sie in Mind Tools Toolkit einen Benutzer namens B. Simon.

Mind Tools Toolkit unterstützt die Just-In-Time-Bereitstellung (standardmäßig aktiviert). In diesem Abschnitt ist keine Aktion erforderlich. Wenn ein Benutzer nicht bereits in Mind Tools Toolkit vorhanden ist, wird ein neuer Benutzer erstellt, sobald Sie versuchen, auf Mind Tools Toolkit zuzugreifen.

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mithilfe des Portals „Meine Apps“.

Wenn Sie im Portal „Meine Apps“ die Kachel „Mind Tools Toolkit“ auswählen, werden Sie automatisch bei der Mind Tools Toolkit-Instanz angemeldet, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Portal „Meine Apps“ finden Sie unter [Anmelden beim Portal „Meine Apps“ und Starten von Apps über dieses](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Mind Tools Toolkit mit Azure AD ausprobieren](https://aad.portal.azure.com/)

- [Was ist Sitzungssteuerung in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Schützen von Apps mit der App-Steuerung für bedingten Zugriff von Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
