---
title: 'Tutorial: Konfigurieren der Oracle Cloud Infrastructure-Konsole für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Benutzerkonten aus Azure AD für die Oracle Cloud Infrastructure-Konsole automatisch bereitstellen und die Bereitstellung aufheben.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 01/16/2020
ms.author: Zhchia
ms.openlocfilehash: 0c3d68698621fe963074c7216e4636208d93deca
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88543868"
---
# <a name="tutorial-configure-oracle-cloud-infrastructure-console-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren der Oracle Cloud Infrastructure-Konsole für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte beschrieben, die Sie sowohl in der Oracle Cloud Infrastructure-Konsole als auch in Azure Active Directory (Azure AD) ausführen müssen, um die automatische Benutzerbereitstellung zu konfigurieren. Bei der Konfiguration stellt Azure AD mithilfe des Azure AD-Bereitstellungsdiensts automatisch Benutzer und Gruppen für die [Oracle Cloud Infrastructure-Konsole](https://www.oracle.com/cloud/free/?source=:ow:o:p:nav:0916BCButton&intcmp=:ow:o:p:nav:0916BCButton) bereit bzw. hebt die Bereitstellung von Benutzern und Gruppen auf. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Unterstützte Funktionen
> [!div class="checklist"]
> * Erstellen von Benutzern in der Oracle Cloud Infrastructure-Konsole
> * Entfernen von Benutzern aus der Oracle Cloud Infrastructure-Konsole, wenn sie keinen Zugriff mehr benötigen
> * Synchronisieren von Benutzerattributen zwischen Azure AD und der Oracle Cloud Infrastructure-Konsole
> * Bereitstellen von Gruppen und Gruppenmitgliedschaften in der Oracle Cloud Infrastructure-Konsole
> * [Einmaliges Anmelden](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-tutorial) bei der Oracle Cloud Infrastructure-Konsole (empfohlen)

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* [Azure AD-Mandant](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Ein Benutzerkonto in Azure AD mit der [Berechtigung](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für die Konfiguration von Bereitstellungen (z.B. Anwendungsadministrator, Cloudanwendungsadministrator, Anwendungsbesitzer oder Globaler Administrator). 
* Einen Oracle Cloud Infrastructure-[Mandanten](https://www.oracle.com/cloud/sign-in.html?intcmp=OcomFreeTier&source=:ow:o:p:nav:0916BCButton).
* Ein Benutzerkonto in der Oracle Cloud Infrastructure-Konsole mit Administratorberechtigungen.

## <a name="step-1-plan-your-provisioning-deployment"></a>Schritt 1: Planen der Bereitstellung
1. Erfahren Sie, [wie der Bereitstellungsdienst funktioniert](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Bestimmen Sie, wer [in den Bereitstellungsbereich](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) einbezogen werden soll.
3. Legen Sie fest, welche Daten zwischen [Azure AD und der Oracle Cloud Infrastructure-Konsole zugeordnet werden sollen](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-oracle-cloud-infrastructure-console-to-support-provisioning-with-azure-ad"></a>Schritt 2: Konfigurieren der Oracle Cloud Infrastructure-Konsole für die Unterstützung der Bereitstellung mit Azure AD

1. Melden Sie sich beim Verwaltungsportal der Oracle Cloud Infrastructure-Konsole an. Navigieren Sie in der oberen linken Ecke des Bildschirms zu **Identität > Verbund**.

    ![Oracle-Verwaltung](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/identity.png)

2. Klicken Sie auf die auf der Seite neben „Oracle Identity Cloud Service-Konsole“ angezeigte URL.

    ![Oracle-URL](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/url.png)

3. Klicken Sie auf **Identitätsanbieter hinzufügen**, um einen neuen Identitätsanbieter zu erstellen. Speichern Sie die IdP-ID, die als Teil der Mandanten-URL verwendet werden soll. Klicken Sie auf das Pluszeichen neben der Registerkarte **Anwendungen**, um einen OAuth-Client zu erstellen und die IDCS-App-Rolle „Identitätsdomänenadministrator“ zu erteilen.

    ![Oracle-Cloudsymbol](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/add.png)

4. Nutzen Sie die folgenden Screenshots, um Ihre Anwendung zu konfigurieren. Klicken Sie nach Abschluss der Konfiguration auf **Speichern**.

    ![Oracle-Konfiguration](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/configuration.png)

    ![Oracle-Tokenausstellungsrichtlinie](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/token-issuance.png)

5. Erweitern Sie auf der Registerkarte „Konfiguration“ Ihrer Anwendung die Option **Allgemeine Informationen**, um die Client-ID und den geheimen Clientschlüssel abzurufen.

    ![Oracle-Tokengenerierung](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/general-information.png)

6. Codieren Sie mit Base64 die Client-ID und den geheimen Clientschlüssel im Format **Client-ID:geheimer Clientschlüssel**, um ein geheimes Token zu generieren. Speichern Sie das geheime Token. Dieser Wert wird im Azure-Portal auf der Registerkarte „Bereitstellung“ Ihrer Oracle Cloud Infrastructure-Konsolenanwendung in das Feld **Geheimes Token** eingegeben.

## <a name="step-3-add-oracle-cloud-infrastructure-console-from-the-azure-ad-application-gallery"></a>Schritt 3: Hinzufügen der Oracle Cloud Infrastructure-Konsole aus dem Azure AD-Anwendungskatalog

Fügen Sie die Oracle Cloud Infrastructure-Konsole aus dem Azure AD-Anwendungskatalog hinzu, um mit dem Verwalten der Bereitstellung für die Oracle Cloud Infrastructure-Konsole zu beginnen. Wenn Sie die Oracle Cloud Infrastructure-Konsole zuvor für das einmalige Anmelden (Single Sign-On, SSO) eingerichtet haben, können Sie dieselbe Anwendung verwenden. Es ist jedoch empfehlenswert, beim erstmaligen Testen der Integration eine separate App zu erstellen. [Hier](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) erfahren Sie mehr über das Hinzufügen einer Anwendung aus dem Katalog. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Schritt 4. Definieren der Benutzer für den Bereitstellungsbereich 

Mit dem Azure AD-Bereitstellungsdienst können Sie anhand der Zuweisung zur Anwendung oder aufgrund von Attributen für den Benutzer/die Gruppe festlegen, wer in die Bereitstellung einbezogen werden soll. Wenn Sie sich dafür entscheiden, anhand der Zuweisung festzulegen, wer für Ihre App bereitgestellt werden soll, können Sie der Anwendung mithilfe der folgenden [Schritte](../manage-apps/assign-user-or-group-access-portal.md) Benutzer und Gruppen zuweisen. Wenn Sie allein anhand der Attribute des Benutzers oder der Gruppe auswählen möchten, wer bereitgestellt wird, können Sie einen [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) beschriebenen Bereichsfilter verwenden. 

* Beim Zuweisen von Benutzern und Gruppen für die Oracle Cloud Infrastructure-Konsole müssen Sie eine andere Rolle als **Standardzugriff** auswählen. Benutzer mit der Rolle „Standardzugriff“ werden von der Bereitstellung ausgeschlossen und in den Bereitstellungsprotokollen als „nicht effektiv berechtigt“ gekennzeichnet. Wenn für die Anwendung nur die Rolle „Standardzugriff“ verfügbar ist, können Sie das [Anwendungsmanifest aktualisieren](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) und weitere Rollen hinzufügen. 

* Fangen Sie klein an. Testen Sie die Bereitstellung mit einer kleinen Gruppe von Benutzern und Gruppen, bevor Sie sie für alle freigeben. Wenn der Bereitstellungsbereich auf zugewiesene Benutzer und Gruppen festgelegt ist, können Sie dies durch Zuweisen von einem oder zwei Benutzern oder Gruppen zur App kontrollieren. Ist der Bereich auf alle Benutzer und Gruppen festgelegt, können Sie einen [attributbasierten Bereichsfilter](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) angeben. 


## <a name="step-5-configure-automatic-user-provisioning-to-oracle-cloud-infrastructure-console"></a>Schritt 5: Konfigurieren der automatischen Benutzerbereitstellung für die Oracle Cloud Infrastructure-Konsole 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern bzw. Gruppen in ServiceNow auf der Grundlage von Benutzer- oder Gruppenzuweisungen in Azure AD erläutert.

### <a name="to-configure-automatic-user-provisioning-for-oracle-cloud-infrastructure-console-in-azure-ad"></a>Gehen Sie wie folgt vor, um die automatische Benutzerbereitstellung für die Oracle Cloud Infrastructure-Konsole in Azure AD zu konfigurieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste die Option **Oracle Cloud Infrastructure Console** aus.

    ![Der Link zur Oracle Cloud Infrastructure-Konsole in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Registerkarte „Bereitstellung“](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Registerkarte „Bereitstellung“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** die **Mandanten-URL** im Format `https://<IdP ID>.identity.oraclecloud.com/admin/v1` ein. Beispiel: `https://idcs-0bfd023ff2xx4a98a760fa2c31k92b1d.identity.oraclecloud.com/admin/v1`. Geben Sie den Wert für das geheime Token ein, den Sie zuvor unter **Geheimes Token** abgerufen haben. Klicken Sie auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit der Oracle Cloud Infrastructure-Konsole herstellen kann. Wenn die Verbindung nicht hergestellt werden kann, stellen Sie sicher, dass Ihr Konto für die Oracle Cloud Infrastructure-Konsole über Administratorberechtigungen verfügt, und versuchen Sie es noch einmal.

    ![Bereitstellung](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/provisioning.png)

6. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

7. Wählen Sie **Speichern** aus.

8. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Oracle Cloud Infrastructure-Konsole synchronisieren** aus.

9. Überprüfen Sie im Abschnitt **Attributzuordnung** die Benutzerattribute, die von Azure AD mit der Oracle Cloud Infrastructure-Konsole synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in der Oracle Cloud Infrastructure-Konsole für Aktualisierungsvorgänge verwendet. Wenn Sie sich dafür entscheiden, das [übereinstimmende Zielattribut](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) zu ändern, müssen Sie sicherstellen, dass die Oracle Cloud Infrastructure-Konsolen-API das Filtern von Benutzern anhand dieses Attributs unterstützt. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

      |attribute|type|
      |---|---|
      |displayName|String|
      |userName|String|
      |aktiv|Boolean|
      |title|String|
      |emails[type eq "work"].value|String|
      |preferredLanguage|String|
      |name.givenName|String|
      |name.familyName|String|
      |addresses[type eq "work"].formatted|String|
      |addresses[type eq "work"].locality|String|
      |addresses[type eq "work"].region|String|
      |addresses[type eq "work"].postalCode|String|
      |addresses[type eq "work"].country|String|
      |addresses[type eq "work"].streetAddress|String|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber|String|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|String|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter|String|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:division|String|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|Verweis|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|String|
      |urn:ietf:params:scim:schemas:oracle:idcs:extension:user:User:bypassNotification|Boolean|
      |urn:ietf:params:scim:schemas:oracle:idcs:extension:user:User:isFederatedUser|Boolean|

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Gruppen mit Oracle Cloud Infrastructure-Konsole synchronisieren** aus.

11. Überprüfen Sie im Abschnitt **Attributzuordnung** die Gruppenattribute, die von Azure AD mit der Oracle Cloud Infrastructure-Konsole synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Gruppen in der Oracle Cloud Infrastructure-Konsole für Aktualisierungsvorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

      |attribute|type|
      |---|---|
      |displayName|String|
      |externalId|String|
      |members|Verweis|

12. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Um den Azure AD-Bereitstellungsdienst für die Oracle Cloud Infrastructure-Konsole zu aktivieren, ändern Sie im Abschnitt **Einstellungen** den **Bereitstellungsstatus** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

14. Legen Sie die Benutzer und/oder Gruppen fest, die in der Oracle Cloud Infrastructure-Konsole bereitgestellt werden sollen, indem Sie im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte auswählen.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

15. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Durch diesen Vorgang wird der erstmalige Synchronisierungszyklus für alle Benutzer und Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert wurden. Der erste Zyklus dauert länger als nachfolgende Zyklen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. 

## <a name="step-6-monitor-your-deployment"></a>Schritt 6: Überwachen der Bereitstellung
Nachdem Sie die Bereitstellung konfiguriert haben, können Sie mit den folgenden Ressourcen die Bereitstellung überwachen:

* Mithilfe der [Bereitstellungsprotokolle](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) können Sie ermitteln, welche Benutzer erfolgreich bzw. nicht erfolgreich bereitgestellt wurden.
* Anhand der [Fortschrittsleiste](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) können Sie den Status des Bereitstellungszyklus überprüfen und den Fortschritt der Bereitstellung verfolgen.
* Wenn sich die Bereitstellungskonfiguration in einem fehlerhaften Zustand zu befinden scheint, wird die Anwendung unter Quarantäne gestellt. Weitere Informationen zu den verschiedenen Quarantänestatus finden Sie [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../manage-apps/check-status-user-account-provisioning.md)
