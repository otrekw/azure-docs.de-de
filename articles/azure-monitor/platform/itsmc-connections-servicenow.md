---
title: Verbinden von ServiceNow mit dem ITSM-Connector
description: Dieser Artikel enthält Informationen dazu, wie Sie ServiceNow mit dem ITSM-Connector (ITSMC) in Azure Monitor verbinden, um die ITSM-Arbeitselemente zentral zu überwachen und zu verwalten.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/21/2020
ms.openlocfilehash: 662c36e4f0082c376a6e250e9a0885f0cd225964
ms.sourcegitcommit: a4533b9d3d4cd6bb6faf92dd91c2c3e1f98ab86a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2020
ms.locfileid: "97729544"
---
# <a name="connect-servicenow-with-it-service-management-connector"></a>Verbinden von ServiceNow mit dem ITSM-Connector

Dieser Artikel bietet Informationen dazu, wie Sie die Verbindung zwischen Ihrer ServiceNow-Instanz und dem ITSM-Connector (ITSMC) in Log Analytics konfigurieren, um Arbeitselemente zentral zu verwalten.

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres ServiceNow-Produkts mit dem ITSMC in Azure.

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:
- ITSMC wurde installiert. Weitere Informationen: [Hinzufügen der IT Service Management Connector-Lösung](./itsmc-definition.md#add-it-service-management-connector).
- Von ServiceNow unterstützte Versionen: Orlando, New York, Madrid, London, Kingston, Jakarta, Istanbul, Helsinki, Genf.
- Aktuell kann durch die Warnungen, die von Azure Monitor gesendet werden, in ServiceNow eines der folgenden Elemente erstellt werden: Ereignisse, Incidents oder Warnungen.
> [!NOTE]
> ITSMC unterstützt nur das offizielle SaaS-Angebot von ServiceNow. Private Bereitstellungen von ServiceNow werden nicht unterstützt. 

**ServiceNow-Administratoren müssen in ihrer ServiceNow-Instanz die folgenden Schritte ausführen**:
- Generieren der Client-ID und des geheimen Clientschlüssels für das ServiceNow-Produkt. Informationen zum Generieren der Client-ID und des geheimen Clientschlüssels finden Sie in dem für Sie zutreffenden Abschnitt:

    - [OAuth-Setup für Orlando](https://docs.servicenow.com/bundle/orlando-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für New York](https://docs.servicenow.com/bundle/newyork-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für Madrid](https://docs.servicenow.com/bundle/madrid-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für London](https://docs.servicenow.com/bundle/london-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für Kingston](https://docs.servicenow.com/bundle/kingston-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für Jakarta](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für Istanbul](https://docs.servicenow.com/bundle/istanbul-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für Helsinki](https://docs.servicenow.com/bundle/helsinki-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth-Setup für Geneva](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/security/task/t_SettingUpOAuth.html)
> [!NOTE]
> Im Rahmen der Definition von „OAuth-Setup“ wird Folgendes empfohlen:
>
> 1) **Aktualisieren Sie die Lebensdauer des Aktualisierungstokens auf 90 Tage (7.776.000 Sekunden):** Im Rahmen von [OAuth-Setup](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.servicenow.com%2Fbundle%2Fnewyork-platform-administration%2Fpage%2Fadminister%2Fsecurity%2Ftask%2Ft_SettingUpOAuth.html&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C2c6812e429a549e71cdd08d7d1b148d8%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637208431696739125&sdata=Q7mF6Ej8MCupKaEJpabTM56EDZ1T8vFVyihhoM594aA%3D&reserved=0) in Phase 2: [Erstellen eines Endpunkts für Clients zum Zugreifen auf die Instanz](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.servicenow.com%2Fbundle%2Fnewyork-platform-administration%2Fpage%2Fadminister%2Fsecurity%2Ftask%2Ft_CreateEndpointforExternalClients.html&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C2c6812e429a549e71cdd08d7d1b148d8%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637208431696749123&sdata=hoAJHJAFgUeszYCX1Q%2FXr4N%2FAKiFcm5WV7mwR2UqeWA%3D&reserved=0) Suchen Sie nach der Definition des Endpunkts auf dem ServiceNow-Blatt nach „System-OAuth“, und wählen Sie dann „Anwendungsregistrierung“ aus. Wählen Sie den Namen des definierten OAuth aus, und aktualisieren Sie das Feld für die Lebensdauer des Aktualisierungstokens auf 7.776.000 (90 Tage in Sekunden).
> Klicken Sie zum Schluss auf „Aktualisieren“.
> 2) **Es wird empfohlen, eine interne Prozedur einzurichten, um sicherzustellen, dass die Verbindung aktiv bleibt:** Gemäß der Lebensdauer des Aktualisierungstokens zum Aktualisieren des Tokens. Stellen Sie sicher, dass die folgenden Vorgänge vor der erwarteten Ablaufzeit des Aktualisierungstokens ausgeführt werden (einige Tage vor Ablauf der Lebensdauer des Aktualisierungstokens wird empfohlen):
>
>     1. [Ausführen eines manuellen Synchronisierungsprozesses für die Konfiguration des ITSM-Connectors](./itsmc-resync-servicenow.md)
>     2. Widerrufen Sie das alte Aktualisierungstoken, da aus Sicherheitsgründen davon abgeraten wird, alte Schlüssel aufzubewahren. Suchen Sie auf dem ServiceNow-Blatt nach „System-OAuth“, und wählen Sie dann „Token verwalten“ aus. Wählen Sie das alte Token nach OAuth-Name und Ablaufdatum aus der Liste aus.
> ![ServiceNow: Definition von System-OAuth](media/itsmc-connections/snow-system-oauth.png)
>     3. Klicken Sie auf „Zugriff widerrufen“ und dann auf „Widerrufen“.

- Installieren der Benutzer-App für die Microsoft Log Analytics-Integration (ServiceNow-App). [Weitere Informationen](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1 )
> [!NOTE]
> ITSMC unterstützt nur die offizielle Benutzer-App für die Microsoft Log Analytics-Integration, die aus dem ServiceNow Store heruntergeladen wird. ITSMC unterstützt keine Codeerfassung seitens ServiceNow oder der Anwendung, die nicht Teil der offiziellen ServiceNow-Lösung ist. 
- Erstellen der Benutzerrolle „Integration“ für die installierte Benutzer-App. Informationen zum Erstellen der Benutzerrolle „Integration“ finden Sie [hier](#create-integration-user-role-in-servicenow-app).

## <a name="connection-procedure"></a>**Verbindungsverfahren**
Verwenden Sie das folgende Verfahren, um eine ServiceNow-Verbindung zu erstellen:


1. Navigieren Sie im Azure-Portal zu **Alle Ressourcen**, und suchen Sie nach **ServiceDesk(YourWorkspaceName)** .

2.  Klicken Sie unter **ARBEITSBEREICHSDATENQUELLEN** auf **ITSM-Verbindungen**.
    ![Neue Verbindung](media/itsmc-connections/add-new-itsm-connection.png)

3. Klicken Sie oben im rechten Bereich auf **Hinzufügen**.

4. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **OK**, um die Verbindung zu erstellen.


> [!NOTE]
> Alle diese Parameter sind erforderlich.

| **Feld** | **Beschreibung** |
| --- | --- |
| **Verbindungsname**   | Geben Sie einen Namen für die ServiceNow-Instanz ein, die Sie mit ITSMC verbinden möchten.  Diesen Namen verwenden Sie später in Log Analytics beim Konfigurieren von Arbeitselementen in dieser ITSM-Instanz bzw. beim Anzeigen ausführlicher Protokollanalysen. |
| **Partner type** (Partnertyp)   | Wählen Sie **ServiceNow** aus. |
| **Benutzername**   | Geben Sie den Integrationsbenutzernamen ein, den Sie in der ServiceNow-App zur Unterstützung der Verbindung mit ITSMC erstellt haben. Weitere Informationen: [Erstellen der ServiceNow-App-Benutzerrolle](#create-integration-user-role-in-servicenow-app).|
| **Kennwort**   | Geben Sie das diesem Benutzernamen zugeordnete Kennwort ein. **Hinweis**: Benutzername und Kennwort werden nur zum Generieren von Authentifizierungstoken verwendet und werden nicht im ITSMC-Dienst gespeichert.  |
| **Server-URL**   | Geben Sie die URL der ServiceNow-Instanz ein, die Sie mit ITSMC verbinden möchten. Die URL sollte auf eine unterstützte SaaS-Version mit dem Suffix „.servicenow.com“ verweisen.|
| **Client-ID**   | Geben Sie die Client-ID ein, die Sie für die OAuth2-Authentifizierung verwenden möchten, die Sie zuvor generiert haben.  Weitere Informationen zum Generieren der Client-ID und des geheimen Clientschlüssels:   [OAuth-Setup](https://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Geheimer Clientschlüssel**   | Geben Sie den für diese ID generierten geheimen Clientschlüssel ein.   |
| **Datensynchronisierungsbereich**   | Wählen Sie die ServiceNow-Arbeitselemente aus, die Sie über den ITSMC mit Azure Log Analytics synchronisieren möchten.  Die ausgewählten Werte werden in Log Analytics importiert.   **Optionen:**  Incidents und Änderungsanforderungen.|
| **Daten synchronisieren** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximales Limit**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt der ITSMC die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert. |

![ServiceNow-Verbindung](media/itsmc-connections/itsm-connection-servicenow-connection-latest.png)

**Bei erfolgreicher Verbindung und Synchronisierung**:

- Ausgewählte Arbeitselemente aus der ServiceNow-Instanz werden in Azure **Log Analytics** importiert. Die Zusammenfassung dieser Arbeitselemente können Sie auf der IT Service Management Connector-Kachel anzeigen.

- Sie können Incidents über Log Analytics-Warnungen oder Protokolldatensätze oder über Azure-Warnungen in dieser ServiceNow-Instanz erstellen.

> [!NOTE]
> In ServiceNow gibt es eine Begrenzung der Anforderungen pro Stunde. Um das Limit zu konfigurieren, definieren Sie die „Beschränkung für eingehende REST-API-Raten“ in der ServiceNow-Instanz.

## <a name="create-integration-user-role-in-servicenow-app"></a>Erstellen der Benutzerrolle „Integration“ in der ServiceNow-App

Gehen Sie dazu wie folgt vor:

1. Rufen Sie den [ServiceNow Store](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1) auf, und installieren Sie die **Benutzer-App für die ServiceNow- und Microsoft OMS-Integration** in Ihrer ServiceNow-Instanz.
   
   >[!NOTE]
   >Im Rahmen der laufenden Umstellung von der Microsoft Operations Management Suite (OMS) auf Azure Monitor wird OMS jetzt als Log Analytics bezeichnet.     
2. Nach der Installation suchen Sie in der linken Navigationsleiste der ServiceNow-Instanz nach dem Microsoft OMS-Integrator und wählen ihn aus.  
3. Klicken Sie auf **Installation Checklist** (Installationsprüfliste).

   Der Status **Nicht abgeschlossen** wird angezeigt, wenn die Benutzerrolle noch nicht erstellt wurde.

4. Geben Sie in den Textfeldern neben **Create integration user** (Integrationsbenutzer erstellen) den Benutzernamen für den Benutzer ein, der in Azure eine Verbindung mit dem ITSMC herstellen kann.
5. Geben Sie das Kennwort für diesen Benutzer ein, und klicken Sie auf **OK**.  

> [!NOTE]
> Diese Anmeldeinformationen verwenden Sie zum Herstellen der ServiceNow-Verbindung in Azure.

Der neu erstellte Benutzer wird mit den zugewiesenen Standardrollen angezeigt.

**Standardrollen**:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.user
-   itil
-   template_editor
-   view_changer

Sobald der Benutzer erfolgreich erstellt wurde, wechselt der Status von **Check Installation Checklist** (Installationsprüfliste überprüfen) zu „Abgeschlossen“, und die Details der für die App erstellten Benutzerrolle werden aufgeführt.

> [!NOTE]
> Der ITSM-Connector kann Vorfälle an ServiceNow senden, ohne dass andere Module auf Ihrer ServiceNow-Instanz installiert sind. Wenn Sie das EventManagement-Modul in Ihrer ServiceNow-Instanz verwenden und Ereignisse oder Warnungen mit dem Connector in ServiceNow erstellen möchten, fügen Sie dem Integrationsbenutzer die folgenden Rollen hinzu:
> 
>    - evt_mgmt_integration
>    - evt_mgmt_operator  


## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über den ITSM-Connector](itsmc-overview.md)
* [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts)
* [Problembehandlung in ITSM-Connector](./itsmc-resync-servicenow.md)