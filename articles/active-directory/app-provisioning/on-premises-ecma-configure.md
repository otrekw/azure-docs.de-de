---
title: 'Azure AD-ECMA-Connectorhost: Konfiguration'
description: In diesem Artikel wird das Konfigurieren des Azure AD-ECMA-Connectorhosts beschrieben.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 06/04/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: cad38ff8c0dc18d363773783e95e544d67f55193
ms.sourcegitcommit: ff1aa951f5d81381811246ac2380bcddc7e0c2b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2021
ms.locfileid: "111570446"
---
# <a name="configure-the-azure-ad-ecma-connector-host-and-the-provisioning-agent"></a>Konfigurieren Sie den Azure AD-ECMA-Connectorhost und den Bereitstellungs-Agent.

>[!IMPORTANT]
> Die Vorschauversion der lokalen Bereitstellung ist derzeit nur auf Einladung verfügbar. Sie können [hier](https://aka.ms/onpremprovisioningpublicpreviewaccess) den Zugriff auf die Funktion beantragen. Wir öffnen die Vorschauversion im Rahmen der Vorbereitung auf die allgemeine Verfügbarkeit in den nächsten Monaten für weitere Kunden und Connectors.

In diesem Artikel finden Sie eine Anleitung zum Konfigurieren des Azure AD-ECMA-Connectorhosts und des Bereitstellungs-Agents nach der erfolgreichen Installation.

Bei der Installation und Konfiguration des Azure AD-ECMA-Connectorhosts handelt es sich um einen Prozess.  Befolgen Sie bei der Durchführung des Prozesses den nachstehenden Ablauf.

 ![Installationsablauf](./media/on-premises-ecma-configure/flow-1.png)  

Weitere Installations- und Konfigurationsinformationen finden Sie unter:
   - [Voraussetzungen für den Azure AD-ECMA-Connectorhost](on-premises-ecma-prerequisites.md)
   - [Installation des Azure AD-ECMA-Connectorhosts](on-premises-ecma-install.md)
    - [Konfiguration des generischen SQL-Connectors mit Azure AD-ECMA-Connectorhost](on-premises-sql-connector-configure.md)
## <a name="configure-the-azure-ad-ecma-connector-host"></a>Konfigurieren des Azure AD-ECMA-Connectorhosts
Die Konfiguration des Azure AD-ECMA-Connectorhosts erfolgt in zwei Phasen.
    
   - **Konfigurieren der Einstellungen:** Konfigurieren Sie den Port und das Zertifikat für den Azure AD-ECMA-Connectorhost.  Dies erfolgt nur beim ersten Start des ECMA-Connectorhosts.
   - **Erstellen eines Connectors:** Erstellen Sie einen Connector (z. B. SQL oder LDAP), damit der Azure AD-ECMA-Connectorhost Daten in eine Datenquelle exportieren oder importieren kann.

### <a name="configure-the-settings"></a>Konfigurieren der Einstellungen
Wenn Sie den Azure AD-ECMA-Connectorhost zum ersten Mal starten, wird eine Portnummer angezeigt, die bereits mit dem Standardwert 8585 ausgefüllt ist.  

 ![Konfigurieren Ihrer Einstellungen](.\media\on-premises-ecma-configure\configure-1.png)

Für die Vorschau müssen Sie ein neues selbstsigniertes Zertifikat generieren.

 >[!NOTE]
 >In dieser Vorschauversion wird ein zeitabhängiges Zertifikat verwendet. Bei dem automatisch generierten Zertifikat handelt es sich um ein selbstsigniertes und vertrauenswürdiges Stammzertifikat, dessen SAN mit dem Hostnamen übereinstimmt.


### <a name="create-a-connector"></a>Erstellen eines Connectors
Nun müssen Sie einen Connector für den Azure AD-ECMA-Connectorhost erstellen.  Durch diesen Connector wird dem ECMA-Connectorhost ermöglicht, Daten in die Datenquelle für den von Ihnen erstellten Connector zu exportieren (und auf Wunsch auch zu importieren).  

Die Konfigurationsschritte für die einzelnen Connectors sind umfangreicher und werden in separaten Dokumenten bereitgestellt.

Nutzen Sie einen der folgenden Links, um einen Connector zu erstellen und zu konfigurieren.

- [Generischer SQL-Connector:](on-premises-sql-connector-configure.md) Ein Connector, der mit SQL-Datenbanken wie Microsoft SQL oder MySQL verwendet werden kann.


## <a name="establish-connectivity-between-azure-ad-and-the-azure-ad-ecma-connector-host"></a>Herstellen einer Verbindung zwischen Azure AD und dem Azure AD-ECMA-Connectorhost
Die folgenden Abschnitte dienen als Leitfaden für das Herstellen einer Verbindung mit dem lokalen Azure AD-ECMA-Connectorhost und Azure AD.

#### <a name="ensure-ecma2host-service-is-running"></a>Sicherstellen, dass der ECMA2Host-Dienst ausgeführt wird
1.  Klicken Sie auf dem Server, auf dem der Azure AD-ECMA-Connectorhost ausgeführt wird, auf Starten.
2. Geben Sie „run“ (Ausführen) und „services.msc“ in das Feld ein.
3. Stellen Sie innerhalb der Dienste sicher, dass **Microsoft ECMA2Host** vorhanden ist und ausgeführt wird.  Ist dies nicht der Fall, klicken Sie auf **Starten**.
 ![Dienst wird ausgeführt](.\media\on-premises-ecma-configure\configure-2.png)

#### <a name="add-enterprise-application"></a>Hinzufügen einer Unternehmensanwendung
1.  Melden Sie sich als Anwendungsadministrator beim Azure-Portal an.
2. Navigieren Sie im Portal zu Azure Active Directory, **Unternehmensanwendungen**.
3. Klicken Sie auf **Neue Anwendung**.
 ![Neue Anwendung hinzufügen](.\media\on-premises-ecma-configure\configure-4.png)
4. Suchen Sie im Katalog nach der Anwendung „Lokale Bereitstellung“ und klicken Sie auf **Erstellen**.

### <a name="configure-the-application-and-test"></a>Anwendungskonfiguration und Tests
  1. Klicken Sie nach der Erstellung der Anwendung auf die Seite **Bereitstellung**.
  2. Klicken Sie auf **Erste Schritte**.
 ![Erste Schritte](.\media\on-premises-ecma-configure\configure-6.png)
  3. Ändern Sie auf der Seite **Bereitstellung** den Modus zu **Automatisch**
   ![Modus ändern](.\media\on-premises-ecma-configure\configure-7.png)
  4. Wählen Sie im Abschnitt Lokale Konnektivität den Agent aus, den Sie gerade bereitgestellt haben, und klicken Sie auf Agent(s) zuweisen.
     ![Zuweisen eines Agents](.\media\on-premises-ecma-configure\configure-8.png)</br>

  >[!NOTE]
  >Nach dem Hinzufügen eines Agents müssen Sie 10 bis 20 Minuten warten, bis die Registrierung abgeschlossen ist.  Der Konnektivitätstest funktioniert erst, wenn die Registrierung abgeschlossen ist.
  >
  >Alternativ können Sie erzwingen, dass die Agent-Registrierung abgeschlossen wird, indem Sie den Bereitstellungs-Agent auf Ihrem Server neu starten. Navigieren Sie zu Ihrem Server > suchen Sie in der Windows-Suchleiste nach Diensten > identifizieren Sie den Bereitstellungs-Agent-Dienst von Azure AD Connect > klicken Sie mit der rechten Maustaste auf den Dienst und starten Sie ihn neu.
  

  5.  Geben Sie nach 10 Minuten die folgende URL im Bereich **Administratoranmeldeinformationen** ein und ersetzen Sie dabei den Abschnitt „connectorName“ durch den Namen des Connectors auf dem ECMA-Host.

      |Eigenschaft|Wert|
      |-----|-----|
      |Mandanten-URL|https://localhost:8585/ecma2host_connectorName/scim|

  6. Geben Sie den Wert des geheimen Tokens ein, das Sie beim Erstellen des Connectors definiert haben.
  7. Klicken Sie auf Verbindung testen und warten Sie eine Minute.
     ![Testen der Verbindung](.\media\on-premises-ecma-configure\configure-5.png)

     >[!NOTE]
     >Warten Sie nach dem Zuweisen des Agents 10 bis 20 Minuten bis Sie die Verbindung testen.  Die Verbindung kann erst hergestellt werden, wenn die Registrierung abgeschlossen wurde.
  8. Wenn der Verbindungstest erfolgreich war, klicken Sie auf **Speichern**.</br>
   ![Erfolgreicher Test](.\media\on-premises-ecma-configure\configure-9.png)

## <a name="configure-who-is-in-scope-for-provisioning"></a>Definieren des Bereitstellungsbereichs
Da der Azure AD-ECMA-Connectorhost und Azure AD nun miteinander kommunizieren, können Sie mit der Konfiguration des Bereitstellungsbereichs fortfahren.  Die folgenden Abschnitte enthalten Informationen darüber, wie Sie Ihre Benutzer eingrenzen.

### <a name="assign-users-to-your-application"></a>Zuweisen von Benutzern zu Ihrer Anwendung
Mit Azure AD können Sie festlegen, welche Benutzer basierend auf der Zuweisung zu einer Anwendung und/oder durch Filtern nach einem bestimmten Attribut bereitgestellt werden sollen. Definieren Sie den Bereitstellungsbereich und Ihre Bereichsregeln nach Bedarf.  Weitere Informationen finden Sie unter [Verwalten von Benutzerzuweisungen für eine App in Azure Active Directory](../../active-directory/manage-apps/assign-user-or-group-access-portal.md).

### <a name="configure-your-attribute-mappings"></a>Konfigurieren Ihrer Attributzuordnungen
Sie müssen die Benutzerattribute in Azure AD den Attributen in der Zielanwendung zuordnen. Der Azure AD-Bereitstellungsdienst basiert auf dem SCIM-Bereitstellungsstandard. Deshalb verfügen die bereitgestellten Attribute über einen SCIM-Namespace. Das folgende Beispiel zeigt, wie Sie die Attribute mail und objectId in Azure AD den Attributen Email und InternalGUID in einer Anwendung zuordnen können. 

>[!NOTE]
>In der Standardzuordnung wird userPrincipalName dem Attributnamen PLACEHOLDER zugeordnet. Sie müssen das PLACEHOLDER-Attribut in ein Attribut ändern, das Bestandteil Ihrer Anwendung ist. Weitere Informationen finden Sie unter [Abgleichen von Benutzern im Quell- und Zielsystem](customize-application-attributes.md#matching-users-in-the-source-and-target--systems).

|Attributname in Azure AD|Attributname in SCIM|Attributname in der Zielanwendung|
|-----|-----|-----|
|mail|urn:ietf:params:scim:schemas:extension:ECMA2Host:2.0:User:Email|Email|
|objectId|urn:ietf:params:scim:schemas:extension:ECMA2Host:2.0:User:InternalGUID|InternalGUID|

#### <a name="configure-attribute-mapping"></a>Konfigurieren von Attributzuordnungen
 1. Klicken Sie im Azure AD-Portal unter **Unternehmensanwendungen** auf die Seite **Bereitstellung**.
 2. Klicken Sie auf **Erste Schritte**.
 3. Erweitern Sie die **Zuordnungen** und klicken Sie auf **Bereitstellung Azure Active Directory-Benutzer**
   ![Benutzer bereitstellen](.\media\on-premises-ecma-configure\configure-10.png)
 4. Klicken Sie auf **Neue Zuordnung hinzufügen**
   ![Zuordnung hinzufügen](.\media\on-premises-ecma-configure\configure-11.png)
 5. Geben Sie die Quell- und Zielattribute an und klicken Sie auf **OK**.</br>
   ![Bearbeiten von Attributen](.\media\on-premises-ecma-configure\configure-12.png)


Weitere Informationen über das Zuordnen von Benutzerattributen von Anwendungen zu Azure AD finden Sie im [Tutorial: Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory](customize-application-attributes.md).

### <a name="test-your-configuration-by-provisioning-users-on-demand"></a>Testen Ihrer Konfiguration durch die bedarfsorientierte Bereitstellung von Benutzern
Zum Testen Ihrer Konfiguration können Sie die bedarfsorientierte Bereitstellung von Benutzern verwenden.  Weitere Informationen über die bedarfsorientierte Bereitstellung von Benutzern finden Sie unter [Bedarfsorientierte Bereitstellung](provision-on-demand.md).

 1. Navigieren Sie zunächst zum Blatt „einmaliges Anmelden“ und dann zurück zum Blatt „Bereitstellung“. Klicken Sie auf dem neuen Blatt mit der Bereitstellungsübersicht auf „bedarfsorientiert“ (On-Demand).
 2. Testen Sie die bedarfsorientierte Bereitstellung einiger Benutzer, wie [hier](provision-on-demand.md) beschrieben.
   ![Testen der Bereitstellung](.\media\on-premises-ecma-configure\configure-13.png)

### <a name="start-provisioning-users"></a>Benutzerbereitstellung starten
 1. Nachdem die bedarfsorientierte Bereitstellung erfolgreich war, wechseln Sie zurück zur Bereitstellungs-Konfigurationsseite. Stellen Sie sicher, dass der Bereich nur auf zugewiesene Benutzer und Gruppen festgelegt ist, **aktivieren Sie die Bereitstellung (On)** und klicken Sie dann auf **Speichern**.
   ![Bereitstellung beginnen](.\media\on-premises-ecma-configure\configure-14.png)
  2.  Warten Sie einige Minuten, bis die Bereitstellung gestartet wurde (dies kann bis zu 40 Minuten dauern). Hier können Sie mehr über die Leistung des Bereitstellungsdiensts erfahren. Nachdem der Bereitstellungsauftrag, wie im nächsten Abschnitt beschrieben, abgeschlossen wurde, können Sie den Bereitstellungsstatus wieder deaktivieren (Off) und auf Speichern klicken. Dadurch wird der Bereitstellungsdienst in Zukunft nicht mehr ausgeführt.

### <a name="verify-users-have-been-successfully-provisioned"></a>Überprüfen, ob Benutzer erfolgreich bereitgestellt wurden
Kontrollieren Sie im Anschluss an die Wartezeit Ihre Datenquelle, um zu überprüfen, ob neue Benutzer bereitgestellt werden.
 ![Überprüfen, ob Benutzer bereitgestellt werden](.\media\on-premises-ecma-configure\configure-15.png)

## <a name="monitor-your-deployment"></a>Überwachen der Bereitstellung

1. Mithilfe der Bereitstellungsprotokolle können Sie ermitteln, welche Benutzer erfolgreich bzw. nicht erfolgreich bereitgestellt wurden.
2. Erstellen Sie benutzerdefinierte Warnungen, Dashboards und Abfragen durch eine Integration von Azure Monitor.
3. Wenn sich die Bereitstellungskonfiguration in einem fehlerhaften Zustand zu befinden scheint, wird die Anwendung unter Quarantäne gestellt. Weitere Informationen zu den verschiedenen Quarantänestatus finden Sie hier.

## <a name="next-steps"></a>Nächste Schritte

- [Azure AD-ECMA-Connectorhost: Voraussetzungen](on-premises-ecma-prerequisites.md)
- [Azure AD-ECMA-Connectorhost: Installation](on-premises-ecma-install.md)
- [Generischer SQL-Connector](on-premises-sql-connector-configure.md)