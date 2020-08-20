---
title: 'Tutorial: Konfigurieren von Netskope Administrator Console für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Netskope Administrator Console konfigurieren.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 11/07/2019
ms.author: Zhchia
ms.openlocfilehash: cef356619d437d53d94c8f5d3be67cf6ac95501c
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88554550"
---
# <a name="tutorial-configure-netskope-administrator-console-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Netskope Administrator Console für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Netskope Administrator Console und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Netskope Administrator Console zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Dieser Connector befindet sich derzeit in der Public Preview-Phase. Weitere Informationen zu den allgemeinen Nutzungsbedingungen von Microsoft Azure für Previewfunktionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* Einen Azure AD-Mandanten
* [Einen Netskope Administrator Console-Mandanten](https://www.netskope.com/)
* Ein Benutzerkonto in Netskope Administrator Console mit Administratorberechtigungen.

## <a name="assigning-users-to-netskope-administrator-console"></a>Zuweisen von Benutzern zu Netskope Administrator Console

Azure Active Directory ermittelt anhand von *Zuweisungen*, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Netskope Administrator Console benötigen. Anschließend können Sie diese Benutzer und/oder Gruppen Netskope Administrator Console anhand der folgenden Anweisungen zuweisen:
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-netskope-administrator-console"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Netskope Administrator Console

* Es wird empfohlen, Netskope Administrator Console einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

* Beim Zuweisen eines Benutzers an Netskope Administrator Console müssen Sie im Dialogfeld für die Zuweisung eine gültige anwendungsspezifische Rolle auswählen (sofern verfügbar). Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="set-up-netskope-administrator-console-for-provisioning"></a>Einrichten von Netskope Administrator Console für die Bereitstellung

1. Melden Sie sich bei Ihrer [Netskope Administrator Console-Verwaltungskonsole](https://netskope.goskope.com/) an. Navigieren Sie zu **Startseite > Einstellungen**.

    ![Netskope Administrator Console-Verwaltungskonsole](media/netskope-administrator-console-provisioning-tutorial/admin.png)

2.  Navigieren Sie zu **Tools**. Navigieren Sie unter dem Menü **Tools** zu **Verzeichnistools > SCIM-INTEGRATION**.

    ![Netskope Administrator Console – Tools](media/netskope-administrator-console-provisioning-tutorial/tools.png)

    ![Netskope Administrator Console – SCIM hinzufügen](media/netskope-administrator-console-provisioning-tutorial/directory.png)

3. Scrollen Sie nach unten, und klicken Sie auf die Schaltfläche **Token hinzufügen**. Geben Sie im Dialogfeld **OAuth-Clientname hinzufügen** einen **Clientnamen** an, und klicken Sie auf die Schaltfläche **Speichern**.

    ![Netskope Administrator Console – Token hinzufügen](media/netskope-administrator-console-provisioning-tutorial/add.png)

    ![Netskope Administrator Console – Clientname](media/netskope-administrator-console-provisioning-tutorial/clientname.png)

3.  Kopieren Sie die Werte für **SCIM Server URL** und **TOKEN**. Diese Werte werden im Azure-Portal auf der Registerkarte „Bereitstellung“ der Netskope Administrator Console-Anwendung in das Feld „Mandanten-URL“ bzw. „Geheimes Token“ eingegeben.

    ![Netskope Administrator Console – Token erstellen](media/netskope-administrator-console-provisioning-tutorial/token.png)

## <a name="add-netskope-administrator-console-from-the-gallery"></a>Hinzufügen von Netskope Administrator Console aus dem Katalog

Bevor Sie Netskope Administrator Console für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Netskope Administrator Console aus dem Azure AD-Anwendungskatalog der Liste mit den verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie zum Hinzufügen von Netskope Administrator Console aus dem Azure AD-Anwendungskatalog die folgenden Schritte aus:**

1. Wählen Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Bereich auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Netskope Administrator Console** ein, wählen Sie **Netskope Administrator Console** im Ergebnisbereich aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Netskope Administrator Console in der Ergebnisliste](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-netskope-administrator-console"></a>Konfigurieren der automatischen Benutzerbereitstellung für Netskope Administrator Console 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in Netskope Administrator Console auf der Grundlage von Benutzer- bzw. Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Netskope Administrator Console aktivieren. Befolgen Sie hierzu die Anweisungen im [SSO-Tutorial zu Netskope Administrator Console](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-cloud-security-tutorial). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

> [!NOTE]
> Weitere Informationen über den SCIM-Endpunkt von Netskope Administrator Console finden Sie [hier](https://docs.google.com/document/d/1n9P_TL98_kd1sx5PAvZL2HS6MQAqkQqd-OSkWAAU6ck/edit#heading=h.prxq74iwdpon).

### <a name="to-configure-automatic-user-provisioning-for-netskope-administrator-console-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für Netskope Administrator Console in Azure AD

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste den Eintrag **Netskope Administrator Console** aus.

    ![Netskope Administrator Console-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Registerkarte „Bereitstellung“](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Registerkarte „Bereitstellung“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen**  den Wert für **SCIM Server URL** ein, den Sie zuvor in **Mandanten-URL** abgerufen haben. Geben Sie den Wert für **TOKEN** ein, den Sie zuvor unter **Geheimes Token** abgerufen haben. Klicken Sie auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Netskope Administrator Console herstellen kann. Vergewissern Sie sich im Falle eines Verbindungsfehlers, dass Ihr Netskope Administrator Console-Konto über Administratorberechtigungen verfügt, und wiederholen Sie den Vorgang.

    ![Mandanten-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

6. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

7. Klicken Sie auf **Speichern**.

8. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Netskope Administrator Console synchronisieren**.

    ![Netskope Administrator Console – Benutzerzuordnungen](media/netskope-administrator-console-provisioning-tutorial/usermappings.png)

9. Überprüfen Sie im Abschnitt **Attributzuordnung** die Benutzerattribute, die von Azure AD mit Netskope Administrator Console synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in Netskope Administrator Console für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Netskope Administrator Console – Benutzerattribute](media/netskope-administrator-console-provisioning-tutorial/userattributes.png)

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Gruppen mit Netskope Administrator Console synchronisieren**.

    ![Netskope Administrator Console – Gruppenzuordnungen](media/netskope-administrator-console-provisioning-tutorial/groupmappings.png)

11. Überprüfen Sie im Abschnitt **Attributzuordnung** die Gruppenattribute, die von Azure AD mit Netskope Administrator Console synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Gruppen in Netskope Administrator Console für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Netskope Administrator Console – Gruppenattribute](media/netskope-administrator-console-provisioning-tutorial/groupattributes.png)

12. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Um den Azure AD-Bereitstellungsdienst für Netskope Administrator Console zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

14. Legen Sie die Benutzer und/oder Gruppen fest, die in Netskope Administrator Console bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

15. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. In diesen Berichten sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Netskope Administrator Console ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../app-provisioning/check-status-user-account-provisioning.md)

