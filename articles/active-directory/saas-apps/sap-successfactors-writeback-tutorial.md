---
title: 'Tutorial: Konfigurieren des SAP SuccessFactors-Rückschreibens in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das Rückschreiben von Attributen aus Azure AD in SAP SuccessFactors konfigurieren.
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
ms.date: 08/05/2020
ms.author: chmutali
ms.openlocfilehash: f150d6abf2ac6a423a99d3347df9bf0adc9b294b
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2020
ms.locfileid: "87809930"
---
# <a name="tutorial-configure-attribute-write-back-from-azure-ad-to-sap-successfactors"></a>Tutorial: Konfigurieren des Rückschreibens von Attributen aus Azure AD in SAP SuccessFactors
In diesem Tutorial werden die Schritte zum Rückschreiben von Attributen aus Azure AD in SuccessFactors Employee Central beschrieben. 

## <a name="overview"></a>Übersicht

Sie können die SAP Success Factors Writeback-App so konfigurieren, dass bestimmte Attribute aus Azure Active Directory in SAP SuccessFactors Employee Central geschrieben werden. Die Bereitstellungs-App SuccessFactors Writeback unterstützt das Zuweisen von Werten zu den folgenden Employee Central-Attributen:

* Geschäftliche E-Mail-Adresse
* Username
* Geschäftliche Telefonnummer (einschließlich Landeskennzahl, Ortsvorwahl, Rufnummer und Durchwahl)
* Primäres Flag für die geschäftliche Telefonnummer
* Mobiltelefonnummer (einschließlich Landeskennzahl, Ortsvorwahl, Rufnummer)
* Primäres Flag für die Mobiltelefonnummer 
* Benutzerattribute „custom01“ bis „custom15“
* Attribut „loginmethod“

> [!NOTE]
> Diese App weist keine Abhängigkeit von den Integrations-Apps für die eingehende Benutzerbereitstellung von SuccessFactors auf. Sie können sie unabhängig von der Bereitstellungs-App [SuccessFactors to on-premises AD](sap-successfactors-inbound-provisioning-tutorial.md) bzw. der Bereitstellungs-App [SuccessFactors to Azure AD](sap-successfactors-inbound-provisioning-cloud-only-tutorial.md) konfigurieren.

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Für wen ist diese Benutzerbereitstellungslösung am besten geeignet?

Diese Lösung zur Benutzerbereitstellung für das SuccessFactors-Rückschreiben ist ideal geeignet für:

* Organisationen, die Office 365 nutzen und von der IT-Abteilung verwaltete autoritative Attribute (z. B. E-Mail-Adresse, Telefonnummer, Benutzername) in SuccessFactors Employee Central rückschreiben möchten.

## <a name="configuring-successfactors-for-the-integration"></a>Konfigurieren von SuccessFactors für die Integration

Die SuccessFactors-Bereitstellungsconnectors setzen zum Aufrufen der OData-APIs von Employee Central Anmeldeinformationen eines SuccessFactors-Kontos mit den passenden Berechtigungen voraus. In diesem Abschnitt werden die Schritte zum Erstellen des Dienstkontos in SuccessFactors und zum Erteilen der benötigten Berechtigungen beschrieben. 

* [Erstellen/Ermitteln des API-Benutzerkontos in SuccessFactors](#createidentify-api-user-account-in-successfactors)
* [Erstellen einer API-Berechtigungsrolle](#create-an-api-permissions-role)
* [Erstellen einer Berechtigungsgruppe für den API-Benutzer](#create-a-permission-group-for-the-api-user)
* [Erteilen der Berechtigungsrolle für die Berechtigungsgruppe](#grant-permission-role-to-the-permission-group)

### <a name="createidentify-api-user-account-in-successfactors"></a>Erstellen/Ermitteln des API-Benutzerkontos in SuccessFactors
Wenden Sie sich an das Administratorteam von SuccessFactors oder Ihren Implementierungspartner, um ein Benutzerkonto in SuccessFactors zu erstellen oder zu bestimmen, das zum Aufrufen der OData-APIs verwendet wird. Die Anmeldeinformationen (Benutzername und Kennwort) dieses Kontos sind für das Konfigurieren der Bereitstellungs-Apps in Azure AD erforderlich. 

### <a name="create-an-api-permissions-role"></a>Erstellen einer API-Berechtigungsrolle

1. Melden Sie sich bei SAP SuccessFactors mit einem Benutzerkonto an, das Zugriff auf das Admin Center hat.
1. Suchen Sie nach *Manage Permission Roles* (Berechtigungsrollen verwalten), und wählen Sie dann in den Suchergebnissen **Manage Permission Roles** (Berechtigungsrollen verwalten) aus.

   ![Verwalten von Berechtigungsrollen](./media/sap-successfactors-inbound-provisioning/manage-permission-roles.png)

1. Klicken Sie in der Liste „Permission Role“ (Berechtigungsrolle) auf **Create New** (Neue erstellen).

   > [!div class="mx-imgBorder"]
   > ![Erstellen einer neuen Berechtigungsrolle](./media/sap-successfactors-inbound-provisioning/create-new-permission-role-1.png)

1. Fügen Sie einen **Rollennamen** und eine **Beschreibung** für die neue Berechtigungsrolle hinzu. Der Name und die Beschreibung sollten darauf hindeuten, dass die Rolle für Berechtigungen zur API-Verwendung vorgesehen ist.

   > [!div class="mx-imgBorder"]
   > ![Details zur Berechtigungsrolle](./media/sap-successfactors-inbound-provisioning/permission-role-detail.png)

1. Klicken Sie unter den Berechtigungseinstellungen auf **Permission...** (Berechtigung), scrollen Sie in der Berechtigungsliste nach unten, und klicken Sie auf **Manage Integration Tools** (Integrationstools verwalten). Aktivieren Sie das Kontrollkästchen für **Allow Admin to Access to OData API through Basic Authentication** (Administratoren den Zugriff auf OData-API über die Standardauthentifizierung erlauben).

   > [!div class="mx-imgBorder"]
   > ![Verwalten von Integrationstools](./media/sap-successfactors-inbound-provisioning/manage-integration-tools.png)

1. Scrollen Sie in demselben Feld nach unten, und wählen Sie **Employee Central API** aus. Fügen Sie wie unten gezeigt Berechtigungen für das Lesen und Bearbeiten über die OData-API hinzu. Wählen Sie die Bearbeitungsoption aus, wenn Sie dasselbe Konto für das Rückschreiben in ein SuccessFactors-Szenario verwenden möchten. 

   > [!div class="mx-imgBorder"]
   > ![Lese-/Schreibberechtigungen](./media/sap-successfactors-inbound-provisioning/odata-read-write-perm.png)

1. Klicken Sie auf **Done** (Fertig). Klicken Sie auf **Änderungen speichern**.

### <a name="create-a-permission-group-for-the-api-user"></a>Erstellen einer Berechtigungsgruppe für den API-Benutzer

1. Suchen Sie im SuccessFactors Admin Center nach *Manage Permission Groups* (Berechtigungsgruppen verwalten), und wählen Sie in den Suchergebnissen dann **Manage Permission Groups** (Berechtigungsgruppen verwalten) aus.

   > [!div class="mx-imgBorder"]
   > ![Verwalten von Berechtigungsgruppen](./media/sap-successfactors-inbound-provisioning/manage-permission-groups.png)

1. Klicken Sie im Fenster „Manage Permission Groups“ (Berechtigungsgruppen verwalten) auf **Create New** (Neue erstellen).

   > [!div class="mx-imgBorder"]
   > ![Hinzufügen einer neuen Gruppe](./media/sap-successfactors-inbound-provisioning/create-new-group.png)

1. Fügen Sie einen Gruppennamen für die neue Gruppe hinzu. Im Gruppennamen sollte angegeben werden, dass die Gruppe für API-Benutzer vorgesehen ist.

   > [!div class="mx-imgBorder"]
   > ![Name der Berechtigungsgruppe](./media/sap-successfactors-inbound-provisioning/permission-group-name.png)

1. Fügen Sie der Gruppe Mitglieder hinzu. Beispielsweise können Sie im Dropdownmenü „People Pool“ (Personenpool) die Option **Username** (Benutzername) auswählen und dann den Benutzernamen des API-Kontos eingeben, das für die Integration verwendet werden soll. 

   > [!div class="mx-imgBorder"]
   > ![Add group members](./media/sap-successfactors-inbound-provisioning/add-group-members.png) (Hinzufügen von Gruppenmitgliedern)

1. Klicken Sie auf **Done** (Fertig), um die Erstellung der Berechtigungsgruppe abzuschließen.

### <a name="grant-permission-role-to-the-permission-group"></a>Erteilen der Berechtigungsrolle für die Berechtigungsgruppe

1. Suchen Sie im SuccessFactors Admin Center nach *Manage Permission Roles* (Berechtigungsrollen verwalten), und wählen Sie in den Suchergebnissen dann **Manage Permission Roles** (Berechtigungsrollen verwalten) aus.
1. Wählen Sie in der Liste **Permission Role** (Berechtigungsrolle) die Rolle aus, die Sie für die Berechtigungen zur API-Verwendung erstellt haben.
1. Klicken Sie unter **Grant this role to...** (Rolle erteilen für) auf die Schaltfläche **Add...** (Hinzufügen).
1. Wählen Sie im Dropdownmenü die Option **Permission Group...** (Berechtigungsgruppe) aus, und klicken Sie dann auf **Select...** (Auswählen), um das Fenster „Groups“ (Gruppen) zu öffnen. Darin können Sie die oben erstellte Gruppe suchen und auswählen. 

   > [!div class="mx-imgBorder"]
   > ![Hinzufügen der Berechtigungsgruppe](./media/sap-successfactors-inbound-provisioning/add-permission-group.png)

1. Überprüfen Sie die erteilte Berechtigungsrolle für die Berechtigungsgruppe. 
   > [!div class="mx-imgBorder"]
   > ![Details zu Berechtigungsrolle und -gruppe](./media/sap-successfactors-inbound-provisioning/permission-role-group.png)

1. Klicken Sie auf **Änderungen speichern**.

## <a name="preparing-for-successfactors-writeback"></a>Vorbereitungen für SuccessFactors Writeback

Die Bereitstellungs-App SuccessFactors Writeback verwendet bestimmte *Codewerte* für das Festlegen von E-Mail-Adressen und Telefonnummern in Employee Central. Diese *Codewerte* werden in der Attributzuordnungstabelle als konstante Werte festgelegt und sind für jede SuccessFactors-Instanz anders. In diesem Abschnitt wird [Postman](https://www.postman.com/downloads/) zum Abrufen der Codewerte verwendet. Zum Senden von HTTP-Anforderungen können Sie [cURL](https://curl.haxx.se/), [Fiddler](https://www.telerik.com/fiddler) oder jedes vergleichbare Tool verwenden. 

### <a name="download-and-configure-postman-with-your-successfactors-tenant"></a>Herunterladen und Konfigurieren von Postman mit Ihrem SuccessFactors-Mandanten

1. Herunterladen von [Postman](https://www.postman.com/downloads/)
1. Erstellen Sie in der Postman-App eine neue Sammlung. Geben Sie ihr den Namen „SuccessFactors“. 

   > [!div class="mx-imgBorder"]
   > ![Neue Postman-Sammlung](./media/sap-successfactors-inbound-provisioning/new-postman-collection.png)

1. Geben Sie auf der Registerkarte „Autorisierung“ die Anmeldeinformationen des API-Benutzers an, den Sie im vorherigen Abschnitt konfiguriert haben. Konfigurieren Sie „Basisautorisierung“ als Typ. 

   > [!div class="mx-imgBorder"]
   > ![Postman-Autorisierung](./media/sap-successfactors-inbound-provisioning/postman-authorization.png)

1. Speichern Sie die Konfiguration. 

### <a name="retrieve-constant-value-for-emailtype"></a>Abrufen des konstanten Werts für „emailType“

1. Klicken Sie in Postman auf die Auslassungspunkte (...), die der SuccessFactors-Sammlung zugeordnet sind, und fügen Sie eine neue Anforderung namens „Get Email Types“ hinzu, wie nachstehend gezeigt. 

   > [!div class="mx-imgBorder"]
   > ![E-Mail-Anforderung in Postman](./media/sap-successfactors-inbound-provisioning/postman-email-request.png)

1. Öffnen Sie den Anforderungsbereich „Get Email Type“. 
1. Fügen Sie in der GET-URL die folgende URL hinzu, und ersetzen Sie dabei `successFactorsAPITenantName` durch den API-Mandanten für Ihre SuccessFactors-Instanz. 
   `https://<successfactorsAPITenantName>/odata/v2/Picklist('ecEmailType')?$expand=picklistOptions&$select=picklistOptions/id,picklistOptions/externalCode&$format=json`

   > [!div class="mx-imgBorder"]
   > ![„Get Email Type“ in Postman](./media/sap-successfactors-inbound-provisioning/postman-get-email-type.png)

1. Auf der Registerkarte „Autorisierung“ sehen Sie, dass die für die Sammlung konfigurierte Autorisierung geerbt wird. 
1. Klicken Sie auf „Senden“, um den API-Aufruf zu starten. 
1. Zeigen Sie im Antworttext das JSON-Resultset an, und suchen Sie nach der ID, die `externalCode = B` entspricht. 

   > [!div class="mx-imgBorder"]
   > ![Antwort auf „Get Email Type“ in Postman](./media/sap-successfactors-inbound-provisioning/postman-email-type-response.png)

1. Notieren Sie sich diesen Wert, der in der Attributzuordnungstabelle für *emailType* als konstanter Wert verwendet werden soll.

### <a name="retrieve-constant-value-for-phonetype"></a>Abrufen des konstanten Werts für „phoneType“

1. Klicken Sie in Postman auf die Auslassungspunkte (...), die der SuccessFactors-Sammlung zugeordnet sind, und fügen Sie eine neue Anforderung namens „Get Phone Types“ hinzu, wie nachstehend gezeigt. 

   > [!div class="mx-imgBorder"]
   > ![Telefonnummernanforderung in Postman](./media/sap-successfactors-inbound-provisioning/postman-phone-request.png)

1. Öffnen Sie den Anforderungsbereich „Get Phone Type“. 
1. Fügen Sie in der GET-URL die folgende URL hinzu, und ersetzen Sie dabei `successFactorsAPITenantName` durch den API-Mandanten für Ihre SuccessFactors-Instanz. 
   `https://<successfactorsAPITenantName>/odata/v2/Picklist('ecPhoneType')?$expand=picklistOptions&$select=picklistOptions/id,picklistOptions/externalCode&$format=json`

   > [!div class="mx-imgBorder"]
   > ![„Get Phone Type“ in Postman](./media/sap-successfactors-inbound-provisioning/postman-get-phone-type.png)

1. Auf der Registerkarte „Autorisierung“ sehen Sie, dass die für die Sammlung konfigurierte Autorisierung geerbt wird. 
1. Klicken Sie auf „Senden“, um den API-Aufruf zu starten. 
1. Zeigen Sie im Antworttext das JSON-Resultset an, und suchen Sie nach der *ID*, die `externalCode = B` und `externalCode = C` entspricht. 

   > [!div class="mx-imgBorder"]
   > ![Telefon in Postman](./media/sap-successfactors-inbound-provisioning/postman-phone-type-response.png)

1. Notieren Sie sich diese Werte, die in der Attributzuordnungstabelle für *businessPhoneType* und *cellPhoneType* als konstante Werte verwendet werden sollen.

## <a name="configuring-successfactors-writeback-app"></a>Konfigurieren der SuccessFactors Writeback-App

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

   * **Mandanten-URL**: Geben Sie den Namen des Dienstendpunkts für die SuccessFactors-OData-API ein. Geben Sie nur den Hostnamen des Servers ohne „http“ oder „https“ ein. Dieser Wert sollte folgendermaßen aussehen: `api4.successfactors.com`.

   * **Benachrichtigungs-E-Mail**: Geben Sie Ihre E-Mail-Adresse ein, und aktivieren Sie das Kontrollkästchen „E-Mail senden, wenn Fehler auftritt“.
    > [!NOTE]
    > Der Azure AD-Bereitstellungsdienst sendet eine E-Mail-Benachrichtigung, wenn der Bereitstellungsauftrag in den Zustand [Quarantäne](/azure/active-directory/manage-apps/application-provisioning-quarantine-status) wechselt.

   * Klicken Sie auf die Schaltfläche **Verbindung testen**. Wenn der Verbindungstest erfolgreich ist, klicken Sie oben auf die Schaltfläche **Speichern**. Falls nicht, sollten Sie überprüfen, ob die SuccessFactors-Anmeldeinformationen und die URL gültig sind.
    >[!div class="mx-imgBorder"]
    >![Azure portal](./media/sap-successfactors-inbound-provisioning/sfwb-provisioning-creds.png)

   * Nachdem die Anmeldeinformationen erfolgreich gespeichert wurden, wird im Abschnitt **Zuordnungen** die Standardzuordnung angezeigt. Aktualisieren Sie die Seite, wenn die Attributzuordnungen nicht angezeigt werden.  

### <a name="part-2-configure-attribute-mappings"></a>Teil 2: Konfigurieren von Attributzuordnungen

In diesem Abschnitt konfigurieren Sie den Fluss von Benutzerdaten aus SuccessFactors in Active Directory.

1. Klicken Sie auf der Registerkarte „Bereitstellung“ unter **Zuordnungen** auf **Azure Active Directory-Benutzer bereitstellen**.

1. Im Feld **Quellobjektbereich** können Sie auswählen, für welche Benutzergruppen in Azure AD das Rückschreiben durchgeführt werden soll, indem Sie verschiedene attributbasierte Filter definieren. Die Standardoption ist „Alle Benutzer in Azure AD“. 
   > [!TIP]
   > Wenn Sie die Bereitstellungs-App zum ersten Mal konfigurieren, müssen Sie Ihre Attributzuordnungen und Ausdrücke testen und überprüfen, um sicherzustellen, dass sie damit das gewünschte Ergebnis erzielen. Microsoft empfiehlt, die Bereichsfilter unter **Quellobjektbereich** zu verwenden, um Ihre Zuordnungen mit einigen Testbenutzern von Azure AD zu testen. Sobald Sie sich vergewissert haben, dass die Zuordnungen funktionieren, können Sie den Filter entweder entfernen oder schrittweise erweitern, um mehr Benutzer einzubinden.

1. Für das Feld **Zielobjektaktionen** wird nur der Vorgang **Aktualisieren** unterstützt.

1. In der Zuordnungstabelle können Sie im Abschnitt **Attributzuordnungen** die folgenden Azure Active Directory-Attribute zu SuccessFactors zuordnen. In der folgenden Tabelle finden Sie Hinweise, wie Attribute zum Rückschreiben zugeordnet werden können. 

   | \# | Azure AD-Attribut | SuccessFactors-Attribut | Bemerkungen |
   |--|--|--|--|
   | 1 | employeeId | personIdExternal | Standardmäßig ist dieses Attribut der übereinstimmende Kennzeichner. Anstelle von „employeeId“ können Sie jedes andere Azure AD-Attribut verwenden, das in SuccessFactors dem Wert „personIdExternal“ entspricht und entsprechend gespeichert wird.    |
   | 2 | mail | email | Ordnet die Attributquelle für „email“ zu. Zu Testzwecken können Sie „userPrincipalName“ zu „email“ zuordnen. |
   | 3 | 8448 | emailType | Dieser konstante Wert ist der SuccessFactors-ID-Wert, der mit der geschäftlichen E-Mail-Adresse verknüpft ist. Aktualisieren Sie diesen Wert, und passen Sie ihn an Ihre SuccessFactors-Umgebung an. Die Schritte zum Festlegen dieses Werts sind im Abschnitt [Abrufen des konstanten Werts für „emailType“](#retrieve-constant-value-for-emailtype) beschrieben. |
   | 4 | true | emailIsPrimary | Verwenden Sie dieses Attribut, um die geschäftliche E-Mail-Adresse in SuccessFactors als primäre Adresse festzulegen. Wenn die geschäftliche E-Mail-Adresse nicht als primäre Adresse verwendet werden soll, setzen Sie dieses Flag auf „ false“. |
   | 5 | userPrincipalName | [custom01 – custom15] | Mit der Option **Neue Zuordnung hinzufügen** können Sie optional „userPrincipalName“ oder jedes andere Azure AD-Attribut in ein benutzerdefiniertes Attribut schreiben, das im SuccessFactors-Benutzerobjekt verfügbar ist.  |
   | 6 | on-prem-samAccountName | username | Mit der Option **Neue Zuordnung hinzufügen** können Sie optional das lokale Attribut „samAccountName“ zum SuccessFactors-Attribut „username“ hinzufügen. |
   | 7 | SSO | loginMethod | Wenn der SuccessFactors-Mandant für das [partielle einmalige Anmelden](https://apps.support.sap.com/sap/support/knowledge/en/2320766) eingerichtet ist, können Sie über „Neue Zuordnung hinzufügen“ das Attribut „loginMethod“ optional auf den konstanten Wert „SSO“ oder „KENNWORT“ festlegen. |
   | 8 | telephoneNumber | businessPhoneNumber | Verwenden Sie diese Zuordnung, um *telephoneNumber* von Azure AD an die geschäftliche Telefonnummer von SuccessFactors zu übergeben. |
   | 9 | 10605 | businessPhoneType | Dieser konstante Wert ist der SuccessFactors-ID-Wert, der mit der geschäftlichen Telefonnummer verknüpft ist. Aktualisieren Sie diesen Wert, und passen Sie ihn an Ihre SuccessFactors-Umgebung an. Die Schritte zum Festlegen dieses Werts sind im Abschnitt [Abrufen des konstanten Werts für „phoneType“](#retrieve-constant-value-for-phonetype) beschrieben. |
   | 10 | true | businessPhoneIsPrimary | Verwenden Sie dieses Attribut, um das primäre Flag für die geschäftliche Telefonnummer festzulegen. Gültige Werte sind „true“ oder „false“. |
   | 11 | mobile | cellPhoneNumber | Verwenden Sie diese Zuordnung, um *telephoneNumber* von Azure AD an die geschäftliche Telefonnummer von SuccessFactors zu übergeben. |
   | 12 | 10606 | cellPhoneType | Dieser konstante Wert ist der SuccessFactors-ID-Wert, der mit dem Mobiltelefon verknüpft ist. Aktualisieren Sie diesen Wert, und passen Sie ihn an Ihre SuccessFactors-Umgebung an. Die Schritte zum Festlegen dieses Werts sind im Abschnitt [Abrufen des konstanten Werts für „phoneType“](#retrieve-constant-value-for-phonetype) beschrieben. |
   | 13 | false | cellPhoneIsPrimary | Verwenden Sie dieses Attribut, um das primäre Flag für die Mobiltelefonnummer festzulegen. Gültige Werte sind „true“ oder „false“. |
 
1. Validieren und überprüfen Sie Ihre Attributzuordnungen. 
 
    >[!div class="mx-imgBorder"]
    >![Writeback-Attributzuordnung](./media/sap-successfactors-inbound-provisioning/writeback-attribute-mapping.png)

1. Klicken Sie auf **Speichern**, um die Zuordnungen zu speichern. Als Nächstes aktualisieren Sie die JSON Path-API-Ausdrücke, um die phoneType-Codes in Ihrer SuccessFactors-Instanz zu verwenden. 
1. Wählen Sie **Erweiterte Optionen anzeigen** aus. 

    >[!div class="mx-imgBorder"]
    >![Erweiterte Optionen anzeigen](./media/sap-successfactors-inbound-provisioning/show-advanced-options.png)

1. Klicken Sie auf **Attributliste für SuccessFactors bearbeiten**. 

   > [!NOTE] 
   > Wenn die Option **Attributliste für SuccessFactors bearbeiten** im Azure-Portal nicht angezeigt wird, können Sie über die URL *https://portal.azure.com/?Microsoft_AAD_IAM_forceSchemaEditorEnabled=true* auf die Seite zugreifen. 

1. In der Ansicht werden in der Spalte **API-Ausdruck** die JSON Path-Ausdrücke angezeigt, die vom Connector verwendet werden. 
1. Aktualisieren Sie die JSON Path-Ausdrücke für geschäftliche Telefonnummer und Mobiltelefonnummer so, dass der ID-Wert (*businessPhoneType* und *cellPhoneType*) angezeigt wird, der Ihrer Umgebung entspricht. 

    >[!div class="mx-imgBorder"]
    >![JSON Path-Änderung für Telefon](./media/sap-successfactors-inbound-provisioning/phone-json-path-change.png)

1. Klicken Sie auf **Speichern**, um die Zuordnungen zu speichern.

## <a name="enable-and-launch-user-provisioning"></a>Aktivieren und Starten der Benutzerbereitstellung

Nachdem die Konfiguration der SuccessFactors-Bereitstellungs-App abgeschlossen ist, können Sie den Bereitstellungsdienst im Azure-Portal aktivieren.

> [!TIP]
> Beim Aktivieren des Bereitstellungsdiensts werden Bereitstellungsvorgänge für alle Benutzer im Bereich initiiert. Wenn Fehler in der Zuordnung oder Datenprobleme auftreten, kann der Bereitstellungsauftrag fehlschlagen und in den Quarantänezustand wechseln. Um dies zu vermeiden, empfehlen wir, den Filter **Quellobjektbereich** zu konfigurieren und Ihre Attributzuordnungen mit einigen Testbenutzern zu testen, bevor Sie die vollständige Synchronisierung für alle Benutzer starten. Sobald Sie sich vergewissert haben, dass die Zuordnungen funktionieren und Sie die gewünschten Ergebnisse erhalten, können Sie den Filter entweder entfernen oder schrittweise erweitern, um mehr Benutzer einzubinden.

1. Legen Sie auf der Registerkarte **Bereitstellung** die Einstellung **Bereitstellungsstatus** auf **Ein** fest.

2. Klicken Sie auf **Speichern**.

3. Dieser Vorgang startet die erste Synchronisierung, die abhängig von der Anzahl von Benutzern im SuccessFactors-Mandanten mehrere Stunden dauern kann. Sie können die Statusanzeige überprüfen, um den Fortschritt des Synchronisierungszyklus zu verfolgen. 

4. Im Azure-Portal können Sie sich auf der Registerkarte **Überwachungsprotokolle** jederzeit ansehen, welche Aktionen der Bereitstellungsdienst ausgeführt hat. Die Überwachungsprotokolle enthalten alle einzelnen Synchronisierungsereignisse des Bereitstellungsdiensts (z. B. die Benutzer, die in Employee Central gelesen und anschließend zu Active Directory hinzugefügt oder darin aktualisiert wurden). 

5. Nach Abschluss der ersten Synchronisierung wird auf der Registerkarte **Bereitstellung** ein Überwachungszusammenfassungsbericht ausgegeben:

   > [!div class="mx-imgBorder"]
   > ![Statusanzeige für die Bereitstellung](./media/sap-successfactors-inbound-provisioning/prov-progress-bar-stats.png)

## <a name="supported-scenarios-known-issues-and-limitations"></a>Unterstützte Szenarien, bekannte Probleme und Begrenzungen

Lesen Sie hierzu den Abschnitt [Writeback-Szenarien](../app-provisioning/sap-successfactors-integration-reference.md#writeback-scenarios) im Referenzhandbuch zur Integration von SAP SuccessFactors. 

## <a name="next-steps"></a>Nächste Schritte

* [Studieren Sie aufmerksam das Referenzhandbuch zur Integration von Azure AD und SAP SuccessFactors](../app-provisioning/sap-successfactors-integration-reference.md)
* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../app-provisioning/check-status-user-account-provisioning.md)
* [Weitere Informationen zum Konfigurieren des einmaligen Anmeldens zwischen SuccessFactors und Azure Active Directory](successfactors-tutorial.md)
* [Erfahren Sie, wie Sie andere SaaS-Anwendungen in Azure Active Directory integrieren.](tutorial-list.md)
* [Weitere Informationen zum Exportieren und Importieren von Bereitstellungskonfigurationen](../app-provisioning/export-import-provisioning-configuration.md)

