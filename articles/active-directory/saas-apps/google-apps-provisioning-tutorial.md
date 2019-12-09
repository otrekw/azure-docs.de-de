---
title: 'Tutorial: Konfigurieren von G Suite für die automatische Benutzerbereitstellung mit Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Benutzerkonten aus Azure AD für G Suite automatisch bereitstellen und die Bereitstellung wieder aufheben.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: beaa8561028a9e21d0623c0eb8e19592f3cad055
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2019
ms.locfileid: "74167864"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von G Suite für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in G Suite und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD zum automatischen Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in G Suite zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).

> [!NOTE]
> Der G Suite-Connector wurde vor Kurzem im Oktober 2019 aktualisiert. An dem G Suite-Connector vorgenommene Änderungen umfassen Folgendes:
- Unterstützung für zusätzliche G Suite-Benutzer- und -Gruppenattribute hinzugefügt. 
- Die Namen der G Suite-Zielattribute wurden so aktualisiert, dass dem entsprechen, was [hier](https://developers.google.com/admin-sdk/directory)definiert ist.
- Aktualisierte Standardattributzuordnungen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit G Suite konfigurieren zu können, benötigen Sie Folgendes:

- Einen Azure AD-Mandanten
- [Einen G Suite-Mandanten](https://gsuite.google.com/pricing.html)
- Ein Benutzerkonto in G Suite mit Administratorberechtigungen.

## <a name="assign-users-to-g-suite"></a>Zuweisen von Benutzern zu G Suite

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf G Suite benötigen. Anschließend können Sie diese Benutzer und/oder Gruppen G Suite wie folgt zuweisen:

* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-g-suite"></a>Wichtige Tipps zum Zuweisen von Benutzern zu G Suite

* Es wird empfohlen, G Suite einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

* Beim Zuweisen eines Benutzers zu G Suite müssen Sie im Dialogfeld für die Zuweisung eine gültige anwendungsspezifische Rolle auswählen (sofern verfügbar). Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="setup-g-suite-for-provisioning"></a>Einrichten von G Suite für die Bereitstellung

Bevor Sie G Suite für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie in G Suite die SCIM-Bereitstellung aktivieren.

1. Melden Sie sich bei der [G Suite-Verwaltungskonsole](https://admin.google.com/) mit Ihrem Administratorkonto an, und wählen Sie **Sicherheit** aus. Sollte der Link nicht zu sehen sein, befindet er sich möglicherweise unter dem Menü **Weitere Steuerelemente** am unteren Bildschirmrand.

    ![Wählen Sie „Sicherheit“ aus.][10]

2. Wählen Sie auf der Seite **Sicherheit** die Option **API-Referenz** aus.

    ![Wählen Sie „API-Referenz“ aus.][15]

3. Wählen Sie **API-Zugriff aktivieren**aus.

    ![Wählen Sie „API-Referenz“ aus.][16]

   > [!IMPORTANT]
   > Für jeden Benutzer, den Sie für G Suite bereitstellen möchten, **muss** der Benutzername in Azure AD an eine benutzerdefinierte Domäne gebunden werden. Beispielsweise werden Benutzernamen wie bob@contoso.onmicrosoft.com von G Suite nicht akzeptiert. bob@contoso.com ist hingegen zulässig. Sie können die Domäne eines vorhandenen Benutzers ändern, indem Sie die [hier](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain) zu findenden Anleitungen befolgen.

4.  Nachdem Sie Ihre gewünschten benutzerdefinierten Domänen mit Azure AD hinzugefügt und überprüft haben, müssen Sie sie erneut mit G Suite überprüfen. Zum Überprüfen von Domänen in G Suite verwenden Sie die folgenden Schritte:

    a. Wählen Sie in der [G Suite-Verwaltungskonsole](https://admin.google.com/) die Option **Domänen** aus.

    ![Auswählen von „Domänen“][20]

    b. Wählen Sie **Domäne oder Domänenalias hinzufügen** aus.

    ![Hinzufügen einer neuen Domäne][21]

    c. Wählen Sie **Andere Domäne hinzufügen** aus, und geben Sie dann den Namen der Domäne ein, die Sie hinzufügen möchten.

    ![Geben Sie Ihren Domänennamen ein][22]

    d. Wählen Sie **Weiter und Domänenbesitzrecht überprüfen** aus. Führen Sie dann die Schritte aus, mit denen Sie prüfen, ob Sie den Domänennamen besitzen. Umfassende Anweisungen zum Überprüfen Ihrer Domäne mit Google finden Sie unter [Überprüfen des Websitebesitzrechts](https://support.google.com/webmasters/answer/35179).

    e. Wiederholen Sie die obigen Schritte für alle weiteren Domänen, die Sie G Suite hinzufügen möchten.

5. Bestimmen Sie als Nächstes, welches Administratorkonto Sie zum Verwalten der Benutzerbereitstellung in G Suite verwenden möchten. Navigieren Sie zur Registerkarte **Administratorrollen**.

    ![Auswählen von „Google Apps“][26]
    
6. Bearbeiten Sie für die **Administratorrolle** des Kontos die **Berechtigungen** für diese Rolle. Aktivieren Sie alle **Admin-API-Berechtigungen**, damit dieses Konto für die Bereitstellung verwendet werden kann.

    ![Auswählen von „Google Apps“][27]

## <a name="add-g-suite-from-the-gallery"></a>Hinzufügen von G Suite aus dem Katalog

Um G Suite für die automatische Benutzerbereitstellung in Azure AD konfigurieren zu können, müssen Sie Ihrer Liste der verwalteten SaaS-Anwendungen G Suite aus dem Azure AD-Anwendungskatalog hinzufügen. 

1. Wählen Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Bereich auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **G Suite** ein, wählen Sie im Ergebnisbereich **G Suite** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![G Suite in der Ergebnisliste](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-g-suite"></a>Konfigurieren der automatischen Benutzerbereitstellung in G Suite 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in G Suite auf der Grundlage von Benutzer- bzw. Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für G Suite aktivieren. Befolgen Sie hierzu die Anweisungen unter [Tutorial: Azure Active Directory-Integration mit G Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-tutorial). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

> [!NOTE]
> Weitere Informationen über den Directory-API-Endpunkt von G Suite finden Sie unter [Directory-API](https://developers.google.com/admin-sdk/directory).

### <a name="to-configure-automatic-user-provisioning-for-g-suite-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für G Suite in Azure AD:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste den Eintrag **G Suite** aus.

    ![G Suite-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Registerkarte „Bereitstellung“](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Registerkarte „Bereitstellung“](common/provisioning-automatic.png)

5. Wählen Sie im Abschnitt **Administratoranmeldeinformationen** die Option **Autorisieren** aus. In einem neuen Browserfenster wird ein Dialogfeld für die Google-Autorisierung geöffnet.

    ![G Suite Authorize](media/google-apps-provisioning-tutorial/authorize.png)

6. Bestätigen Sie, dass Sie Azure AD die Berechtigungen erteilen möchten, Änderungen an Ihrem G Suite-Mandanten vorzunehmen. Wählen Sie **Akzeptieren** aus.

    ![Überprüfen Sie die Berechtigungen.][28]

7. Wählen Sie im Azure-Portal die Option **Verbindung testen** aus, um zu überprüfen, ob Azure AD eine Verbindung mit Ihrer App herstellen kann. Falls die Verbindung nicht hergestellt werden kann, überprüfen Sie, ob Ihr G Suite-Konto über Teamadministratorberechtigungen verfügt. Führen Sie dann erneut den Schritt **Autorisieren** aus.

8. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

8. Klicken Sie auf **Speichern**.

9. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit G Suite synchronisieren** aus.

    ![G Suite: Benutzerzuordnungen](media/google-apps-provisioning-tutorial/usermappings.png)

10. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit G Suite synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in G Suite für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![G Suite: Benutzerattribute](media/google-apps-provisioning-tutorial/userattributes.png)

11. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Gruppen mit G Suite synchronisieren** aus.

    ![G Suite: Gruppenzuordnungen](media/google-apps-provisioning-tutorial/groupmappings.png)

12. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit G Suite synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden verwendet, um die Gruppen in G Suite für Updatevorgänge abzugleichen. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![G Suite: Gruppenattribute](media/google-apps-provisioning-tutorial/groupattributes.png)

13. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Um den Azure AD-Bereitstellungsdienst für G Suite zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

15. Legen Sie die Benutzer und/oder Gruppen fest, die in G Suite bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

16. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in G Suite ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../manage-apps/check-status-user-account-provisioning.md)


<!--Image references-->

[10]: ./media/google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/google-apps-provisioning-tutorial/gapps-auth.png
