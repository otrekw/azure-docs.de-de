---
title: 'Tutorial: Konfigurieren von Samanage für die automatische Benutzerbereitstellung mit Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Benutzerkonten aus Azure AD automatisch für Samanage bereitstellen und die Bereitstellung wieder aufheben.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 01/13/2020
ms.author: Zhchia
ms.openlocfilehash: 21a3c81d9a24cc63d3fc77c95c94df1e9113d292
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88543447"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Samanage für die automatische Benutzerbereitstellung
In diesem Tutorial werden die Schritte beschrieben, die Sie sowohl in Samanage als auch in Azure Active Directory (Azure AD) ausführen müssen, um die automatische Benutzerbereitstellung zu konfigurieren. Nach der Konfiguration stellt Azure AD mithilfe des Azure AD-Bereitstellungsdiensts automatisch Benutzer und Gruppen für [Samanage](https://www.samanage.com/pricing/) bereit bzw. hebt die Bereitstellung von Benutzern und Gruppen auf. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="migrate-to-the-new-samange-application"></a>Migrieren zur neuen Samange-Anwendung

Wenn Sie bereits über eine Integration mit Samanage verfügen, finden Sie in diesem Abschnitt Informationen zu bevorstehenden Änderungen. Wenn Sie Samanage zum ersten Mal einrichten, können Sie diesen Abschnitt überspringen und mit dem Abschnitt **Unterstützte Funktionen** fortfahren.

#### <a name="whats-changing"></a>Was hat sich geändert?
* Änderungen auf der Azure AD-Seite: Die Autorisierungsmethode für die Bereitstellung von Benutzern in Samanage war bisher die **Basisautorisierung**. Bald werden Sie sehen, dass diese Autorisierungsmethode durch ein **langlebiges geheimes Token** ersetzt wurde.


#### <a name="what-do-i-need-to-do-to-migrate-my-existing-custom-integration-to-the-new-application"></a>Wie muss ich vorgehen, um meine bestehende benutzerdefinierte Integration in die neue Anwendung zu migrieren?
Wenn Sie bereits über eine Samanage-Integration mit gültigen Administratoranmeldeinformationen verfügen, **müssen Sie nichts weiter tun**. Wir migrieren Kunden automatisch zur neuen Anwendung. Dieser Vorgang erfolgt vollständig im Hintergrund. Wenn die vorhandenen Anmeldeinformationen ablaufen oder Sie den Zugriff auf die Anwendung noch mal autorisieren müssen, müssen Sie ein langlebiges geheimes Token generieren. Informationen zum Generieren eines neuen Tokens finden Sie in Schritt 2 dieses Artikels.


#### <a name="how-can-i-tell-if-my-application-has-been-migrated"></a>Wie kann ich erkennen, ob meine Anwendung migriert wurde? 
Wenn Ihre Anwendung migriert wird, werden im Abschnitt **Administratoranmeldeinformationen** die Felder **Administratorbenutzername** und **Administratorkennwort** durch ein einziges Feld namens **Geheimes Token** ersetzt.

## <a name="capabilities-supported"></a>Unterstützte Funktionen
> [!div class="checklist"]
> * Erstellen von Benutzern in Samanage
> * Entfernen von Benutzern aus Samanage, wenn diese keinen Zugriff mehr benötigen
> * Synchronisieren von Benutzerattributen zwischen Azure AD und Samanage
> * Bereitstellen von Gruppen und Gruppenmitgliedschaften in Samanage
> * [Einmaliges Anmelden](https://docs.microsoft.com/azure/active-directory/saas-apps/samanage-tutorial) bei Samanage (empfohlen)

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* [Azure AD-Mandant](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Ein Benutzerkonto in Azure AD mit der [Berechtigung](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für die Konfiguration von Bereitstellungen (z.B. Anwendungsadministrator, Cloudanwendungsadministrator, Anwendungsbesitzer oder Globaler Administrator). 
* Einen [Samanage-Mandanten](https://www.samanage.com/pricing/) mit dem Professional-Tarif.
* Ein Benutzerkonto in Samanage mit Teamadministratorberechtigungen.

## <a name="step-1-plan-your-provisioning-deployment"></a>Schritt 1: Planen der Bereitstellung
1. Erfahren Sie, [wie der Bereitstellungsdienst funktioniert](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Bestimmen Sie, wer [in den Bereitstellungsbereich](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) einbezogen werden soll.
3. Legen Sie fest, welche Daten [zwischen Azure AD und Samanage zugeordnet werden sollen](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-samanage-to-support-provisioning-with-azure-ad"></a>Schritt 2: Konfigurieren von Samanage für die Unterstützung der Bereitstellung mit Azure AD

Informationen zum Generieren eines geheimen Tokens für die Authentifizierung finden Sie [hier](https://help.samanage.com/s/article/Tutorial-Tokens-Authentication-for-API-Integration-1536721557657).

## <a name="step-3-add-samanage-from-the-azure-ad-application-gallery"></a>Schritt 3: Hinzufügen von Samanage aus dem Azure AD-Anwendungskatalog

Fügen Sie Samanage aus dem Azure AD-Anwendungskatalog hinzu, um mit dem Verwalten der Bereitstellung in Samanage zu beginnen. Wenn Sie Samanage zuvor für das einmalige Anmelden (Single Sign-On, SSO) eingerichtet haben, können Sie dieselbe Anwendung verwenden. Es ist jedoch empfehlenswert, beim erstmaligen Testen der Integration eine separate App zu erstellen. [Hier](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) erfahren Sie mehr über das Hinzufügen einer Anwendung aus dem Katalog. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Schritt 4. Definieren der Benutzer für den Bereitstellungsbereich 

Mit dem Azure AD-Bereitstellungsdienst können Sie anhand der Zuweisung zur Anwendung oder aufgrund von Attributen für den Benutzer/die Gruppe festlegen, wer in die Bereitstellung einbezogen werden soll. Wenn Sie sich dafür entscheiden, anhand der Zuweisung festzulegen, wer für Ihre App bereitgestellt werden soll, können Sie der Anwendung mithilfe der folgenden [Schritte](../manage-apps/assign-user-or-group-access-portal.md) Benutzer und Gruppen zuweisen. Wenn Sie allein anhand der Attribute des Benutzers oder der Gruppe auswählen möchten, wer bereitgestellt wird, können Sie einen [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) beschriebenen Bereichsfilter verwenden. 

* Beim Zuweisen von Benutzern und Gruppen zu Samanage müssen Sie eine andere Rolle als **Standardzugriff** auswählen. Benutzer mit der Rolle „Standardzugriff“ werden von der Bereitstellung ausgeschlossen und in den Bereitstellungsprotokollen als „nicht effektiv berechtigt“ gekennzeichnet. Wenn für die Anwendung nur die Rolle „Standardzugriff“ verfügbar ist, können Sie das [Anwendungsmanifest aktualisieren](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) und weitere Rollen hinzufügen. 

* Fangen Sie klein an. Testen Sie die Bereitstellung mit einer kleinen Gruppe von Benutzern und Gruppen, bevor Sie sie für alle freigeben. Wenn der Bereitstellungsbereich auf zugewiesene Benutzer und Gruppen festgelegt ist, können Sie dies durch Zuweisen von einem oder zwei Benutzern oder Gruppen zur App kontrollieren. Ist der Bereich auf alle Benutzer und Gruppen festgelegt, können Sie einen [attributbasierten Bereichsfilter](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) angeben. 


## <a name="step-5-configure-automatic-user-provisioning-to-samanage"></a>Schritt 5: Konfigurieren der automatischen Benutzerbereitstellung für Samanage 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern bzw. Gruppen in ServiceNow auf der Grundlage von Benutzer- oder Gruppenzuweisungen in Azure AD erläutert.

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Gehen Sie wie folgt vor, um die automatische Benutzerbereitstellung für Samanage in Azure AD zu konfigurieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Samanage**aus.

    ![Samanage-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Registerkarte „Bereitstellung“](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Registerkarte „Bereitstellung“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** im Feld **Mandanten-URL** die Zeichenfolge `https://api.samanage.com` ein.  Geben Sie den Wert für das geheime Token ein, den Sie zuvor unter **Geheimes Token** abgerufen haben. Klicken Sie auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Samanage herstellen kann. Wenn beim Herstellen der Verbindung ein Fehler auftritt, stellen Sie sicher, dass Ihr Samanage-Konto über Administratorberechtigungen verfügt, und wiederholen Sie den Vorgang.

    ![Bereitstellung](./media/samanage-provisioning-tutorial/provisioning.png)

6. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

7. Wählen Sie **Speichern** aus.

8. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Samanage synchronisieren**.

9. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Samanage synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Samanage für Updatevorgänge verwendet werden. Wenn Sie sich dafür entscheiden, das [übereinstimmende Zielattribut](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) zu ändern, müssen Sie sicherstellen, dass die Samanage-API das Filtern von Benutzern anhand dieses Attributs unterstützt. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

      ![Benutzerzuordnungen in Samange](./media/samanage-provisioning-tutorial/user-attributes.png)

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Gruppen mit Samanage synchronisieren**.

11. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit Samanage synchronisiert werden. Die als **Übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Gruppen in Samanage für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

      ![Gruppenzuordnungen in Samange](./media/samanage-provisioning-tutorial/group-attributes.png)

12. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Um den Azure AD-Bereitstellungsdienst für Samanage zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

14. Legen Sie die Benutzer bzw. Gruppen fest, die in Samanage bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

15. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Durch diesen Vorgang wird der erstmalige Synchronisierungszyklus für alle Benutzer und Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert wurden. Der erste Zyklus dauert länger als nachfolgende Zyklen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. 

## <a name="step-6-monitor-your-deployment"></a>Schritt 6: Überwachen der Bereitstellung
Nachdem Sie die Bereitstellung konfiguriert haben, können Sie mit den folgenden Ressourcen die Bereitstellung überwachen:

1. Mithilfe der [Bereitstellungsprotokolle](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) können Sie ermitteln, welche Benutzer erfolgreich bzw. nicht erfolgreich bereitgestellt wurden.
2. Anhand der [Fortschrittsleiste](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) können Sie den Status des Bereitstellungszyklus überprüfen und den Fortschritt der Bereitstellung verfolgen.
3. Wenn sich die Bereitstellungskonfiguration in einem fehlerhaften Zustand zu befinden scheint, wird die Anwendung unter Quarantäne gestellt. Weitere Informationen zu den verschiedenen Quarantänestatus finden Sie [hier](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).

## <a name="connector-limitations"></a>Connector-Einschränkungen

Wenn Sie die Option **Alle Benutzer und Gruppen synchronisieren** auswählen und einen Wert für das Samanage-Attribut **Rollen** festlegen, muss der Wert unter dem Feld **Standardwert bei NULL (optional)** im folgenden Format ausgedrückt werden:

- {„displayName“:„role“}, wobei „role“ der gewünschte Standradwert ist.

## <a name="change-log"></a>Änderungsprotokoll

* 22.04.2020: Aktualisierung der Autorisierungsmethode von Basisautorisierung in langlebiges geheimes Token

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../manage-apps/check-status-user-account-provisioning.md)
