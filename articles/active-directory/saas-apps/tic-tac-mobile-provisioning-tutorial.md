---
title: 'Tutorial: Konfigurieren von Tic-Tac Mobile für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Benutzerkonten aus Azure AD für Tic-Tac Mobile automatisch bereitstellen und die Bereitstellung wieder aufheben.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: d0f24e81-fecf-4e71-bd8a-ab911366fdf5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2020
ms.author: Zhchia
ms.openlocfilehash: aae73d446b6feaf886f626818c7b63a1a3bd00cd
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92794990"
---
# <a name="tutorial-configure-tic-tac-mobile-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Tic-Tac Mobile für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte beschrieben, die Sie in Tic-Tac Mobile und in Azure Active Directory (Azure AD) ausführen müssen, um die automatische Benutzerbereitstellung zu konfigurieren. Wenn die automatische Benutzerbereitstellung konfiguriert ist, stellt Azure AD mithilfe des Azure AD-Bereitstellungsdiensts automatisch Benutzer und Gruppen in [Tic-Tac Mobile](https://www.tictacmobile.com/) bereit bzw. hebt die Bereitstellung von Benutzern und Gruppen auf. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Unterstützte Funktionen
> [!div class="checklist"]
> * Erstellen von Benutzern in Tic-Tac Mobile
> * Entfernen von Benutzern aus Tic-Tac Mobile, wenn diese keinen Zugriff mehr benötigen
> * Synchronisieren von Benutzerattributen zwischen Azure AD und Tic-Tac Mobile

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* [Azure AD-Mandant](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Ein Benutzerkonto in Azure AD mit der [Berechtigung](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für die Konfiguration von Bereitstellungen (z. B. Anwendungsadministrator, Cloudanwendungsadministrator, Anwendungsbesitzer oder Globaler Administrator). 
* Ein [Tic-Tac Mobile](https://www.tictacmobile.com/)-Konto mit Superadministratorrolle.


## <a name="step-1-plan-your-provisioning-deployment"></a>Schritt 1: Planen der Bereitstellung
1. Erfahren Sie, [wie der Bereitstellungsdienst funktioniert](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Bestimmen Sie, wer [in den Bereitstellungsbereich](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) einbezogen werden soll.
3. Legen Sie fest, welche Daten [zwischen Azure AD und Tic-Tac Mobile zugeordnet werden sollen](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-tic-tac-mobile-to-support-provisioning-with-azure-ad"></a>Schritt 2: Konfigurieren von Tic-Tac Mobile für die Unterstützung der Bereitstellung mit Azure AD

Schreiben Sie eine E-Mail an support@tictacmobile.com, um die **Mandanten-URL** und das **geheime Token** zu erhalten. Um ein Token zu erhalten, müssen Sie über die Superadministratorrolle in Tic-Tac Mobile verfügen. Das Token wird im Azure-Portal auf der Registerkarte „Bereitstellung“ im Feld „Geheimes Token“ für Ihre Tic-Tac Mobile-Anwendung eingegeben.

## <a name="step-3-add-tic-tac-mobile-from-the-azure-ad-application-gallery"></a>Schritt 3: Hinzufügen von Tic-Tac Mobile aus dem Azure AD-Anwendungskatalog

Fügen Sie Tic-Tac Mobile aus dem Azure AD-Anwendungskatalog hinzu, um mit dem Verwalten der Bereitstellung in Tic-Tac Mobile zu beginnen. Wenn Sie Tic-Tac Mobile zuvor für das einmalige Anmelden (SSO) eingerichtet haben, können Sie dieselbe Anwendung verwenden. Es ist jedoch empfehlenswert, beim erstmaligen Testen der Integration eine separate App zu erstellen. [Hier](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) erfahren Sie mehr über das Hinzufügen einer Anwendung aus dem Katalog. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Schritt 4. Definieren der Benutzer für den Bereitstellungsbereich 

Mit dem Azure AD-Bereitstellungsdienst können Sie anhand der Zuweisung zur Anwendung oder aufgrund von Attributen für den Benutzer/die Gruppe festlegen, wer in die Bereitstellung einbezogen werden soll. Wenn Sie sich dafür entscheiden, anhand der Zuweisung festzulegen, wer für Ihre App bereitgestellt werden soll, können Sie der Anwendung mithilfe der folgenden [Schritte](../manage-apps/assign-user-or-group-access-portal.md) Benutzer und Gruppen zuweisen. Wenn Sie allein anhand der Attribute des Benutzers oder der Gruppe auswählen möchten, wer bereitgestellt wird, können Sie einen [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) beschriebenen Bereichsfilter verwenden. 

* Beim Zuweisen von Benutzern und Gruppen zu Tic-Tac Mobile müssen Sie eine andere Rolle als **Standardzugriff** auswählen. Benutzer mit der Rolle „Standardzugriff“ werden von der Bereitstellung ausgeschlossen und in den Bereitstellungsprotokollen als „nicht effektiv berechtigt“ gekennzeichnet. Wenn für die Anwendung nur die Rolle „Standardzugriff“ verfügbar ist, können Sie das [Anwendungsmanifest aktualisieren](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) und weitere Rollen hinzufügen. 

* Fangen Sie klein an. Testen Sie die Bereitstellung mit einer kleinen Gruppe von Benutzern und Gruppen, bevor Sie sie für alle freigeben. Wenn der Bereitstellungsbereich auf zugewiesene Benutzer und Gruppen festgelegt ist, können Sie dies durch Zuweisen von einem oder zwei Benutzern oder Gruppen zur App kontrollieren. Ist der Bereich auf alle Benutzer und Gruppen festgelegt, können Sie einen [attributbasierten Bereichsfilter](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) angeben. 


## <a name="step-5-configure-automatic-user-provisioning-to-tic-tac-mobile"></a>Schritt 5: Konfigurieren der automatischen Benutzerbereitstellung in Tic-Tac Mobile 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern bzw. Gruppen in ServiceNow auf der Grundlage von Benutzer- oder Gruppenzuweisungen in Azure AD erläutert.

### <a name="to-configure-automatic-user-provisioning-for-tic-tac-mobile-in-azure-ad"></a>Gehen Sie wie folgt vor, um die automatische Benutzerbereitstellung für Tic-Tac Mobile in Azure AD zu konfigurieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen** .

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Tic-Tac Mobile** aus.

    ![Tic-Tac Mobile-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung** .

    ![Registerkarte „Bereitstellung“](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Registerkarte „Bereitstellung“, Bereitstellungsmodus „Automatisch“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** die Mandanten-URL und das geheime Token für Tic-Tac Mobile ein. Klicken Sie auf **Verbindung testen** , um sicherzustellen, dass Azure AD eine Verbindung mit Tic-Tac Mobile herstellen kann. Vergewissern Sie sich im Falle eines Verbindungsfehlers, dass Ihr Tic-Tac Mobile-Konto über Administratorberechtigungen verfügt, und wiederholen Sie den Vorgang.

    ![Token](common/provisioning-testconnection-tenanturltoken.png)

6. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden** .

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

7. Wählen Sie **Speichern** aus.

8. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Tic-Tac Mobile synchronisieren** aus.

9. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Tic-Tac Mobile synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Tic-Tac Mobile für Updatevorgänge verwendet werden. Wenn Sie das [übereinstimmende Zielattribut](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) ändern möchten, müssen Sie sicherstellen, dass die Tic-Tac Mobile-API das Filtern von Benutzern nach diesem Attribut unterstützt. Wählen Sie die Schaltfläche **Speichern** , um alle Änderungen zu übernehmen.

   |attribute|type|
   |---|---|
   |userName|String|
   |name.givenName|String|
   |name.familyName|String|
   |externalId|String|
   |title|String|
   |emails[type eq"work"].value|String|
   |preferredLanguage|String|
   |externalId|String|
   |userType|String|
   |locale|String|
   |timezone|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:division|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|String|

10. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

11. Um den Azure AD-Bereitstellungsdienst für Tic-Tac Mobile zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein** .

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

12. Legen Sie die Benutzer und/oder Gruppen fest, die in Tic-Tac Mobile bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

13. Wenn Sie fertig sind, klicken Sie auf **Speichern** .

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Durch diesen Vorgang wird der erstmalige Synchronisierungszyklus für alle Benutzer und Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert wurden. Der erste Zyklus dauert länger als nachfolgende Zyklen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. 

## <a name="step-6-monitor-your-deployment"></a>Schritt 6: Überwachen der Bereitstellung
Nachdem Sie die Bereitstellung konfiguriert haben, können Sie mit den folgenden Ressourcen die Bereitstellung überwachen:

1. Mithilfe der [Bereitstellungsprotokolle](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) können Sie ermitteln, welche Benutzer erfolgreich bzw. nicht erfolgreich bereitgestellt wurden.
2. Anhand der [Fortschrittsleiste](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) können Sie den Status des Bereitstellungszyklus überprüfen und den Fortschritt der Bereitstellung verfolgen.
3. Wenn sich die Bereitstellungskonfiguration in einem fehlerhaften Zustand zu befinden scheint, wird die Anwendung unter Quarantäne gestellt. Weitere Informationen zu den verschiedenen Quarantänestatus finden Sie [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).  

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../manage-apps/check-status-user-account-provisioning.md)
