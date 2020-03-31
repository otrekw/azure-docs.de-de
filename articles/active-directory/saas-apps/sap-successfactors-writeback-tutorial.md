---
title: 'Tutorial: Konfigurieren des SuccessFactors-Rückschreibens in Azure Active Directory | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie das Rückschreiben von Attributen für SuccessFactors aus Azure AD konfigurieren.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: jodadzie
ms.assetid: ad255bd4-9e50-43a1-a92b-359215868b6b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2019
ms.author: chmutali
ms.openlocfilehash: 2de0cdd32428884170f549afacdbd52c3a10c93f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77060047"
---
# <a name="tutorial-configure-attribute-writeback-from-azure-ad-to-sap-successfactors-preview"></a>Tutorial: Konfigurieren des Rückschreibens von Attributen für SAP SuccessFactors aus Azure AD (Vorschau)
In diesem Tutorial werden die Schritte beschrieben, die Sie ausführen müssen, um Attribute aus Azure AD für SuccessFactors Employee Central zurückzuschreiben. Das einzige Attribut, das derzeit für das Rückschreiben unterstützt wird, ist das E-Mail-Attribut. 

## <a name="overview"></a>Übersicht

Nachdem Sie die Bereitstellungsintegration in eingehender Richtung entweder mit der Bereitstellungs-App vom Typ [Von SuccessFactors zu lokaler AD-Instanz](sap-successfactors-inbound-provisioning-tutorial.md) oder [Von SuccessFactors zu Azure AD](sap-successfactors-inbound-provisioning-cloud-only-tutorial.md) eingerichtet haben, können Sie optional die SuccessFactors-Rückschreibe-App konfigurieren, um die E-Mail-Adresse für SuccessFactors zurückzuschreiben. 

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Für wen ist diese Benutzerbereitstellungslösung am besten geeignet?

Diese Lösung zur Benutzerbereitstellung für das SuccessFactors-Rückschreiben ist ideal geeignet für:

* Organisationen, die Office 365 nutzen und von der IT-Abteilung verwaltete autoritative Attribute (z. B. die E-Mail-Adresse) für SuccessFactors zurückschreiben möchten.

## <a name="configuring-successfactors-for-the-integration"></a>Konfigurieren von SuccessFactors für die Integration

Eine allgemeine Anforderung für alle SuccessFactors-Bereitstellungsconnectors besteht darin, dass sie Anmeldeinformationen eines SuccessFactors-Kontos mit den erforderlichen Berechtigungen zum Aufrufen der OData-APIs von SuccessFactors benötigen. In diesem Abschnitt werden die Schritte zum Erstellen des Dienstkontos in SuccessFactors und zum Erteilen der benötigten Berechtigungen beschrieben. 

* [Erstellen/Ermitteln des API-Benutzerkontos in SuccessFactors](#createidentify-api-user-account-in-successfactors)
* [Erstellen einer API-Berechtigungsrolle](#create-an-api-permissions-role)
* [Erstellen einer Berechtigungsgruppe für den API-Benutzer](#create-a-permission-group-for-the-api-user)
* [Erteilen der Berechtigungsrolle für die Berechtigungsgruppe](#grant-permission-role-to-the-permission-group)

### <a name="createidentify-api-user-account-in-successfactors"></a>Erstellen/Ermitteln des API-Benutzerkontos in SuccessFactors
Wenden Sie sich an das Administratorteam von SuccessFactors oder Ihren Implementierungspartner, um ein Benutzerkonto in SuccessFactors zu erstellen oder zu bestimmen, das zum Aufrufen der OData-APIs verwendet wird. Die Anmeldeinformationen (Benutzername und Kennwort) dieses Kontos sind für das Konfigurieren der Bereitstellungs-Apps in Azure AD erforderlich. 

### <a name="create-an-api-permissions-role"></a>Erstellen einer API-Berechtigungsrolle

* Melden Sie sich bei SAP SuccessFactors mit einem Benutzerkonto an, das Zugriff auf das Admin Center hat.
* Suchen Sie nach *Manage Permission Roles* (Berechtigungsrollen verwalten), und wählen Sie dann in den Suchergebnissen **Manage Permission Roles** (Berechtigungsrollen verwalten) aus.
  ![Verwalten von Berechtigungsrollen](./media/sap-successfactors-inbound-provisioning/manage-permission-roles.png)
* Klicken Sie in der Liste „Permission Role“ (Berechtigungsrolle) auf **Create New** (Neue erstellen).
  > [!div class="mx-imgBorder"]
  > ![Erstellen einer neuen Berechtigungsrolle](./media/sap-successfactors-inbound-provisioning/create-new-permission-role-1.png)
* Fügen Sie einen **Rollennamen** und eine **Beschreibung** für die neue Berechtigungsrolle hinzu. Der Name und die Beschreibung sollten darauf hindeuten, dass die Rolle für Berechtigungen zur API-Verwendung vorgesehen ist.
  > [!div class="mx-imgBorder"]
  > ![Details zur Berechtigungsrolle](./media/sap-successfactors-inbound-provisioning/permission-role-detail.png)
* Klicken Sie unter den Berechtigungseinstellungen auf **Permission...** (Berechtigung), scrollen Sie in der Berechtigungsliste nach unten, und klicken Sie auf **Manage Integration Tools** (Integrationstools verwalten). Aktivieren Sie das Kontrollkästchen für **Allow Admin to Access to OData API through Basic Authentication** (Administratoren den Zugriff auf OData-API über die Standardauthentifizierung erlauben).
  > [!div class="mx-imgBorder"]
  > ![Verwalten von Integrationstools](./media/sap-successfactors-inbound-provisioning/manage-integration-tools.png)
* Scrollen Sie in demselben Feld nach unten, und wählen Sie **Employee Central API** aus. Fügen Sie wie unten gezeigt Berechtigungen für das Lesen und Bearbeiten über die OData-API hinzu. Wählen Sie die Bearbeitungsoption aus, wenn Sie dasselbe Konto für das Rückschreiben in ein SuccessFactors-Szenario verwenden möchten. 
  > [!div class="mx-imgBorder"]
  > ![Lese-/Schreibberechtigungen](./media/sap-successfactors-inbound-provisioning/odata-read-write-perm.png)
* Klicken Sie auf **Done** (Fertig). Klicken Sie auf **Änderungen speichern**.

### <a name="create-a-permission-group-for-the-api-user"></a>Erstellen einer Berechtigungsgruppe für den API-Benutzer

* Suchen Sie im SuccessFactors Admin Center nach *Manage Permission Groups* (Berechtigungsgruppen verwalten), und wählen Sie in den Suchergebnissen dann **Manage Permission Groups** (Berechtigungsgruppen verwalten) aus.
  > [!div class="mx-imgBorder"]
  > ![Verwalten von Berechtigungsgruppen](./media/sap-successfactors-inbound-provisioning/manage-permission-groups.png)
* Klicken Sie im Fenster „Manage Permission Groups“ (Berechtigungsgruppen verwalten) auf **Create New** (Neue erstellen).
  > [!div class="mx-imgBorder"]
  > ![Hinzufügen einer neuen Gruppe](./media/sap-successfactors-inbound-provisioning/create-new-group.png)
* Fügen Sie einen Gruppennamen für die neue Gruppe hinzu. Im Gruppennamen sollte angegeben werden, dass die Gruppe für API-Benutzer vorgesehen ist.
  > [!div class="mx-imgBorder"]
  > ![Name der Berechtigungsgruppe](./media/sap-successfactors-inbound-provisioning/permission-group-name.png)
* Fügen Sie der Gruppe Mitglieder hinzu. Beispielsweise können Sie im Dropdownmenü „People Pool“ (Personenpool) die Option **Username** (Benutzername) auswählen und dann den Benutzernamen des API-Kontos eingeben, das für die Integration verwendet werden soll. 
  > [!div class="mx-imgBorder"]
  > ![Add group members](./media/sap-successfactors-inbound-provisioning/add-group-members.png) (Hinzufügen von Gruppenmitgliedern)
* Klicken Sie auf **Done** (Fertig), um die Erstellung der Berechtigungsgruppe abzuschließen.

### <a name="grant-permission-role-to-the-permission-group"></a>Erteilen der Berechtigungsrolle für die Berechtigungsgruppe

* Suchen Sie im SuccessFactors Admin Center nach *Manage Permission Roles* (Berechtigungsrollen verwalten), und wählen Sie in den Suchergebnissen dann **Manage Permission Roles** (Berechtigungsrollen verwalten) aus.
* Wählen Sie in der Liste **Permission Role** (Berechtigungsrolle) die Rolle aus, die Sie für die Berechtigungen zur API-Verwendung erstellt haben.
* Klicken Sie unter **Grant this role to...** (Rolle erteilen für) auf die Schaltfläche **Add...** (Hinzufügen).
* Wählen Sie im Dropdownmenü die Option **Permission Group...** (Berechtigungsgruppe) aus, und klicken Sie dann auf **Select...** (Auswählen), um das Fenster „Groups“ (Gruppen) zu öffnen. Darin können Sie die oben erstellte Gruppe suchen und auswählen. 
  > [!div class="mx-imgBorder"]
  > ![Hinzufügen der Berechtigungsgruppe](./media/sap-successfactors-inbound-provisioning/add-permission-group.png)
* Überprüfen Sie die erteilte Berechtigungsrolle für die Berechtigungsgruppe. 
  > [!div class="mx-imgBorder"]
  > ![Details zu Berechtigungsrolle und -gruppe](./media/sap-successfactors-inbound-provisioning/permission-role-group.png)
* Klicken Sie auf **Änderungen speichern**.

## <a name="configuring-successfactors-writeback"></a>Konfigurieren des SuccessFactors-Rückschreibens

Dieser Abschnitt enthält die folgenden Schritte: 

* [Hinzufügen der Bereitstellungsconnector-App und Konfigurieren der Konnektivität mit SuccessFactors](#part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors)
* [Konfigurieren von Attributzuordnungen](#part-2-configure-attribute-mappings)
* [Aktivieren und Starten der Benutzerbereitstellung](#enable-and-launch-user-provisioning)

### <a name="part-1-add-the-provisioning-connector-app-and-configure-connectivity-to-successfactors"></a>Teil 1: Hinzufügen der Bereitstellungsconnector-App und Konfigurieren der Konnektivität mit SuccessFactors

**Konfigurieren Sie das SuccessFactors-Rückschreiben wie folgt:**

1. Besuchen Sie <https://portal.azure.com>.

2. Wählen Sie auf der linken Navigationsleiste **Azure Active Directory** aus.

3. Klicken Sie auf **Unternehmensanwendungen** und dann auf **Alle Anwendungen**.

4. Klicken Sie auf **Anwendung hinzufügen**, und wählen Sie die Kategorie **Alle** aus.

5. Suchen Sie nach **SuccessFactors Writeback** (SuccessFactors-Rückschreiben), und fügen Sie die App aus dem Katalog hinzu.

6. Sobald die App hinzugefügt wurde und der Bildschirm mit den App-Details angezeigt wird, wählen Sie **Bereitstellung** aus.

7. Ändern Sie den **Modus** **Bereitstellung** in **Automatisch**.

8. Vervollständigen Sie den Abschnitt **Administratoranmeldeinformationen** wie folgt:

   * **Administratorbenutzername**: Geben Sie den Benutzernamen des Benutzerkontos für die SuccessFactors-API ein, und fügen Sie die Unternehmens-ID an. Verwenden Sie das Format **Benutzername\@Unternehmens-ID**.

   * **Administratorkennwort**: Geben Sie das Kennwort des Benutzerkontos für die SuccessFactors-API ein. 

   * **Mandanten-URL**: Geben Sie den Namen des Dienstendpunkts für die SuccessFactors-OData-API ein. Geben Sie nur den Hostnamen des Servers ohne „http“ oder „https“ ein. Dieser Wert sollte das folgende Format aufweisen: **API-Servername.successfactors.com**.

   * **Benachrichtigungs-E-Mail**: Geben Sie Ihre E-Mail-Adresse ein, und aktivieren Sie das Kontrollkästchen „E-Mail senden, wenn Fehler auftritt“.
    > [!NOTE]
    > Der Azure AD-Bereitstellungsdienst sendet eine E-Mail-Benachrichtigung, wenn der Bereitstellungsauftrag in den Zustand [Quarantäne](/azure/active-directory/manage-apps/application-provisioning-quarantine-status) wechselt.

   * Klicken Sie auf die Schaltfläche **Verbindung testen**. Wenn der Verbindungstest erfolgreich ist, klicken Sie oben auf die Schaltfläche **Speichern**. Falls nicht, sollten Sie überprüfen, ob die SuccessFactors-Anmeldeinformationen und die URL gültig sind.
    >[!div class="mx-imgBorder"]
    >![Azure portal](./media/sap-successfactors-inbound-provisioning/sfwb-provisioning-creds.png)

   * Nachdem die Anmeldeinformationen erfolgreich gespeichert wurden, wird im Abschnitt **Zuordnungen** die Standardzuordnung **Synchronize Azure Active Directory Users to SuccessFactors** (Azure Active Directory-Benutzer mit SuccessFactors synchronisieren) angezeigt.

### <a name="part-2-configure-attribute-mappings"></a>Teil 2: Konfigurieren von Attributzuordnungen

In diesem Abschnitt konfigurieren Sie den Fluss von Benutzerdaten aus SuccessFactors in Active Directory.

1. Klicken Sie auf der Registerkarte „Bereitstellung“ unter **Zuordnungen** auf **Synchronize Azure Active Directory Users to SuccessFactors** (Azure Active Directory-Benutzer mit SuccessFactors synchronisieren).

1. Im Feld **Quellobjektbereich** können Sie auswählen, für welche Benutzergruppen in Azure AD das Rückschreiben durchgeführt werden soll, indem Sie verschiedene attributbasierte Filter definieren. Die Standardoption ist „Alle Benutzer in Azure AD“. 
   > [!TIP]
   > Wenn Sie die Bereitstellungs-App zum ersten Mal konfigurieren, müssen Sie Ihre Attributzuordnungen und Ausdrücke testen und überprüfen, um sicherzustellen, dass sie damit das gewünschte Ergebnis erzielen. Microsoft empfiehlt, die Bereichsfilter unter **Quellobjektbereich** zu verwenden, um Ihre Zuordnungen mit einigen Testbenutzern von Azure AD zu testen. Sobald Sie sich vergewissert haben, dass die Zuordnungen funktionieren, können Sie den Filter entweder entfernen oder schrittweise erweitern, um mehr Benutzer einzubinden.

1. Für das Feld **Zielobjektaktionen** wird nur der Vorgang **Aktualisieren** unterstützt.

1. Im Abschnitt **Attributzuordnungen** können Sie nur die übereinstimmende ID ändern, die zum Verknüpfen eines SuccessFactors-Benutzerprofils mit einem Azure AD-Benutzer verwendet wird. Außerdem können Sie angeben, welches Attribut in Azure AD als Quelle für die E-Mail dient. 
    >[!div class="mx-imgBorder"]
    >![Azure portal](./media/sap-successfactors-inbound-provisioning/sfwb-attribute-mapping.png)

   >[!NOTE]
   >Für das SuccessFactors-Rückschreiben wird nur das E-Mail-Attribut unterstützt. Verwenden Sie nicht **Neue Zuordnung hinzufügen**, um neue Attribute hinzuzufügen. 

1. Klicken Sie oben im Abschnitt „Attributzuordnung“ auf **Speichern**, um Ihre Zuordnungen zu speichern.

Sobald Ihre Attributzuordnungskonfiguration abgeschlossen ist, können Sie jetzt [den Benutzerbereitstellungsdienst aktivieren und starten](#enable-and-launch-user-provisioning).

## <a name="enable-and-launch-user-provisioning"></a>Aktivieren und Starten der Benutzerbereitstellung

Nachdem die Konfiguration der SuccessFactors-Bereitstellungs-App abgeschlossen ist, können Sie den Bereitstellungsdienst im Azure-Portal aktivieren.

> [!TIP]
> Beim Aktivieren des Bereitstellungsdiensts werden Bereitstellungsvorgänge für alle Benutzer im Bereich initiiert. Wenn Fehler in der Zuordnung oder Workday-Datenprobleme auftreten, kann der Bereitstellungsauftrag fehlschlagen und in den Quarantänezustand wechseln. Um dies zu vermeiden, empfehlen wir, den Filter **Quellobjektbereich** zu konfigurieren und Ihre Attributzuordnungen mit einigen Testbenutzern zu testen, bevor Sie die vollständige Synchronisierung für alle Benutzer starten. Sobald Sie sich vergewissert haben, dass die Zuordnungen funktionieren und Sie die gewünschten Ergebnisse erhalten, können Sie den Filter entweder entfernen oder schrittweise erweitern, um mehr Benutzer einzubinden.

1. Legen Sie auf der Registerkarte **Bereitstellung** die Einstellung **Bereitstellungsstatus** auf **Ein** fest.

2. Klicken Sie auf **Speichern**.

3. Dieser Vorgang startet die erste Synchronisierung, die abhängig von der Anzahl von Benutzern im SuccessFactors-Mandanten mehrere Stunden dauern kann. Sie können die Statusanzeige überprüfen, um den Fortschritt des Synchronisierungszyklus zu verfolgen. 

4. Im Azure-Portal können Sie sich auf der Registerkarte **Überwachungsprotokolle** jederzeit ansehen, welche Aktionen der Bereitstellungsdienst ausgeführt hat. Die Überwachungsprotokolle enthalten alle einzelnen Synchronisierungsereignisse des Bereitstellungsdiensts – beispielsweise, welche Benutzer in Workday gelesen und anschließend Active Directory hinzugefügt oder dort aktualisiert wurden. 

5. Nach Abschluss der ersten Synchronisierung wird auf der Registerkarte **Bereitstellung** ein Überwachungszusammenfassungsbericht ausgegeben:

   > [!div class="mx-imgBorder"]
   > ![Statusanzeige für die Bereitstellung](./media/sap-successfactors-inbound-provisioning/prov-progress-bar-stats.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../app-provisioning/check-status-user-account-provisioning.md)
* [Weitere Informationen zum Konfigurieren des einmaligen Anmeldens zwischen SuccessFactors und Azure Active Directory](successfactors-tutorial.md)
* [Erfahren Sie, wie Sie andere SaaS-Anwendungen in Azure Active Directory integrieren.](tutorial-list.md)
* [Weitere Informationen zum Exportieren und Importieren von Bereitstellungskonfigurationen](../app-provisioning/export-import-provisioning-configuration.md)

