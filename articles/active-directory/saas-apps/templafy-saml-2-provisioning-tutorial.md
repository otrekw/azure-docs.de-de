---
title: 'Tutorial: Konfigurieren von Templafy SAML2 für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Templafy SAML2 konfigurieren.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.assetid: 8a966ef5-e364-435b-9e29-3caf27ffb498
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/19/2021
ms.author: zhchia
ms.openlocfilehash: 4cc89c1ad94ae738b25e4ea18d5fe687880969c1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101648219"
---
# <a name="tutorial-configure-templafy-saml2-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Templafy SAML2 für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Templafy SAML2 und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Templafy SAML2 zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Dieser Connector befindet sich derzeit in der Public Preview-Phase. Weitere Informationen zu den allgemeinen Nutzungsbedingungen von Microsoft Azure für Previewfunktionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* Einen Azure AD-Mandanten.
* [Einen Templafy-Mandanten](https://www.templafy.com/pricing/)
* Ein Benutzerkonto in Templafy mit Administratorberechtigungen

## <a name="step-1-plan-your-provisioning-deployment"></a>Schritt 1: Planen der Bereitstellung
1. Erfahren Sie, [wie der Bereitstellungsdienst funktioniert](../app-provisioning/user-provisioning.md).
2. Bestimmen Sie, wer [in den Bereitstellungsbereich](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) einbezogen werden soll.
3. Legen Sie fest, welche Daten [zwischen Azure AD und Templafy SAML2 zugeordnet werden sollen](../app-provisioning/customize-application-attributes.md). 

## <a name="assigning-users-to-templafy-saml2"></a>Zuweisen von Benutzern zu Templafy SAML2

Azure Active Directory ermittelt anhand von *Zuweisungen*, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Templafy SAML2 benötigen. Anschließend können Sie diese Benutzer und/oder Gruppen Templafy SAML2 wie folgt zuweisen:
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-templafy-saml2"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Templafy SAML2

* Es wird empfohlen, Templafy SAML2 einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

* Beim Zuweisen eines Benutzers zu Templafy SAML2 müssen Sie eine gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="step-2-configure-templafy-saml2-to-support-provisioning-with-azure-ad"></a>Schritt 2: Konfigurieren von Templafy SAML2 für die Unterstützung der Bereitstellung mit Azure AD

Bevor Sie Templafy SAML2 für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie in Templafy SAML2 die SCIM-Bereitstellung aktivieren.

1. Melden Sie sich bei der Templafy-Verwaltungskonsole an. Klicken Sie auf **Administration** (Verwaltung).

    ![Templafy-Verwaltungskonsole](media/templafy-saml-2-provisioning-tutorial/templafy-admin.png)

2. Klicken Sie auf **Authentication Method** (Authentifizierungsmethode).

    ![Screenshot des Abschnitts der Templafy-Verwaltung mit hervorgehobener Option „Authentifizierungsmethode“.](media/templafy-saml-2-provisioning-tutorial/templafy-auth.png)

3. Kopieren Sie den Wert von **SCIM Api Key** (SCIM-API-Schlüssel). Dieser Wert wird im Azure-Portal auf der Registerkarte „Bereitstellung“ der Templafy SAML2-Anwendung in das Feld **Geheimes Token** eingegeben.

    ![Screenshot des SCIM-API-Schlüssels.](media/templafy-saml-2-provisioning-tutorial/templafy-token.png)

## <a name="step-3-add-templafy-saml2-from-the-gallery"></a>Schritt 3: Hinzufügen von Templafy SAML2 aus dem Katalog

Bevor Sie Templafy SAML2 für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Templafy SAML2 aus dem Azure AD-Anwendungskatalog der Liste mit den verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie die folgenden Schritte aus, um Templafy SAML2 aus dem Azure AD-Anwendungskatalog hinzuzufügen:**

1. Wählen Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Bereich auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Templafy SAML2** ein, wählen Sie im Ergebnisbereich **Templafy SAML2** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Templafy SAML2 in der Ergebnisliste](common/search-new-app.png)

## <a name="step-4-configure-automatic-user-provisioning-to-templafy-saml2"></a>Schritt 4: Konfigurieren der automatischen Benutzerbereitstellung in Templafy SAML2 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in Templafy SAML2 auf Grundlage von Benutzer- und/oder Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Templafy aktivieren. Befolgen Sie dazu die Anweisungen im [SSO-Tutorial zu Templafy](templafy-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-templafy-saml2-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für Templafy SAML2 in Azure AD

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Templafy SAML2** aus.

    ![Templafy SAML2-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Screenshot der Optionen zum Verwalten mit aufgerufener Bereitstellungsoption](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Screenshot der Dropdownliste „Bereitstellungsmodus“ mit aufgerufener Option „Automatisch“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** im Feld **Mandanten-URL** die Zeichenfolge `https://scim.templafy.com/scim` ein. Geben Sie den Wert des **SCIM-API-Schlüssels** ein, den Sie zuvor unter **Geheimes Token** abgerufen haben. Klicken Sie auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Templafy herstellen kann. Vergewissern Sie sich im Fall eines Verbindungsfehlers, dass Ihr Templafy-Konto über Administratorberechtigungen verfügt, und wiederholen Sie den Vorgang.

    ![Mandanten-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

6. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

7. Klicken Sie auf **Speichern**.

8. Wählen Sie im Abschnitt **Zuordnungen** die Option **Synchronize Azure Active Directory Users to Templafy SAML2** (Azure Active Directory-Benutzer mit Templafy SAML2 synchronisieren) aus.

    ![Benutzerzuordnungen in Templafy SAML2](media/templafy-saml-2-provisioning-tutorial/user-mapping.png)

9. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Templafy SAML2 synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in Templafy SAML2 für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

   |attribute|type|Unterstützung für das Filtern|
   |---|---|---|
   |userName|String|&check;|
   |aktiv|Boolean|
   |displayName|String|
   |title|String|
   |preferredLanguage|String|
   |name.givenName|String|
   |name.familyName|String|
   |phoneNumbers[type eq "work"].value|String|
   |phoneNumbers[type eq "mobile"].value|String|
   |phoneNumbers[type eq "fax"].value|String|
   |externalId|String|
   |addresses[type eq "work"].locality|String|
   |addresses[type eq "work"].postalCode|String|
   |addresses[type eq "work"].region|String|
   |addresses[type eq "work"].streetAddress|String|
   |addresses[type eq "work"].country|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|String|

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Synchronize Azure Active Directory Groups to Templafy** (Azure Active Directory-Gruppen mit Templafy synchronisieren) aus.

    ![Gruppenzuordnungen in Templafy SAML2](media/templafy-saml-2-provisioning-tutorial/group-mapping.png)

11. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit Templafy SAML2 synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden verwendet, um die Gruppen in Templafy SAML2 für Updatevorgänge abzugleichen. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

      |attribute|type|Unterstützung für das Filtern|
      |---|---|---|
      |displayName|String|&check;|
      |members|Verweis|
      |externalId|String|      


12. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Um den Azure AD-Bereitstellungsdienst für Templafy SAML2 zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

14. Legen Sie die Benutzer und/oder Gruppen fest, die in Templafy SAML2 bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

15. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

    Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Templafy SAML2 ausgeführt werden.

## <a name="step-5-monitor-your-deployment"></a>Schritt 5: Überwachen der Bereitstellung
Nachdem Sie die Bereitstellung konfiguriert haben, können Sie mit den folgenden Ressourcen die Bereitstellung überwachen:

* Mithilfe der [Bereitstellungsprotokolle](../reports-monitoring/concept-provisioning-logs.md) können Sie ermitteln, welche Benutzer erfolgreich bzw. nicht erfolgreich bereitgestellt wurden.
* Anhand der [Fortschrittsleiste](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) können Sie den Status des Bereitstellungszyklus überprüfen und den Fortschritt der Bereitstellung verfolgen.
* Wenn sich die Bereitstellungskonfiguration in einem fehlerhaften Zustand zu befinden scheint, wird die Anwendung unter Quarantäne gestellt. Weitere Informationen zu den verschiedenen Quarantänestatus finden Sie [hier](../app-provisioning/application-provisioning-quarantine-status.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../app-provisioning/check-status-user-account-provisioning.md)