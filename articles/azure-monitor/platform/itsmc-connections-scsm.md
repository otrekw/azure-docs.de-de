---
title: Verbinden von SCSM mit dem ITSM-Connector
description: Dieser Artikel enthält Informationen dazu, wie Sie SCSM mit dem ITSM-Connector (ITSMC) in Azure Monitor verbinden, um die ITSM-Arbeitselemente zentral zu überwachen und zu verwalten.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/21/2020
ms.openlocfilehash: 79706b66dba46253843b1f53a26481170d6ff723
ms.sourcegitcommit: a4533b9d3d4cd6bb6faf92dd91c2c3e1f98ab86a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2020
ms.locfileid: "97729545"
---
# <a name="connect-system-center-service-manager-with-it-service-management-connector"></a>Verbinden von System Center Service Manager mit dem ITSM-Connector

Dieser Artikel bietet Informationen dazu, wie Sie die Verbindung zwischen Ihrer System Center Service Manager-Instanz und dem ITSM-Connector (ITSMC) in Log Analytics konfigurieren, um Arbeitselemente zentral zu verwalten.

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres System Center Service Manager-Produkts mit dem ITSMC in Azure.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:

- ITSMC wurde installiert. Weitere Informationen: [Hinzufügen der IT Service Management Connector-Lösung](./itsmc-definition.md).
- Die Service Manager-Webanwendung (Web-App) ist bereitgestellt und konfiguriert. Informationen zur Web-App finden Sie [hier](#create-and-deploy-service-manager-web-app-service).
- Hybridverbindung wurde erstellt und konfiguriert. Weitere Informationen: [Konfigurieren der Hybridverbindung](#configure-the-hybrid-connection).
- Unterstützte Versionen von Service Manager:  2012 R2 oder 2016.
- Benutzerrolle:  [Erweiterter Operator](/previous-versions/system-center/service-manager-2010-sp1/ff461054(v=technet.10)).
- Aktuell können durch die Warnungen, die von Azure Monitor gesendet werden, in System Center Service Manager Incidents erstellt werden.

> [!NOTE]
> - Der ITSM-Connector kann nur eine Verbindung mit cloudbasierten ServiceNow-Instanzen. Lokale ServiceNow-Instanzen werden derzeit nicht unterstützt.
> - Um benutzerdefinierte [Vorlagen](./itsmc-definition.md#template-definitions) als Teil der Aktionen zu verwenden, muss der Parameter „ProjectionType“ in der SCSM-Vorlage „IncidentManagement!System.WorkItem.Incident.ProjectionType“ zugeordnet werden.

## <a name="connection-procedure"></a>Verbindungsverfahren

Verwenden Sie das folgende Verfahren zum Verbinden Ihrer System Center Service Manager-Instanz mit ITSMC:

1. Navigieren Sie im Azure-Portal zu **Alle Ressourcen**, und suchen Sie nach **ServiceDesk(YourWorkspaceName)** .

2. Klicken Sie unter **ARBEITSBEREICHSDATENQUELLEN** auf **ITSM-Verbindungen**.

    ![Neue Verbindung](media/itsmc-connections/add-new-itsm-connection.png)

3. Klicken Sie oben im rechten Bereich auf **Hinzufügen**.

4. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **OK**, um die Verbindung zu erstellen.

> [!NOTE]
> Alle diese Parameter sind erforderlich.

| **Feld** | **Beschreibung** |
| --- | --- |
| **Verbindungsname**   | Geben Sie einen Namen für die System Center Service Manager-Instanz ein, die Sie mit ITSMC verbinden möchten.  Diesen Namen verwenden Sie später beim Konfigurieren von Arbeitselementen in dieser Instanz/beim Anzeigen ausführlicher Protokollanalysen. |
| **Partner type** (Partnertyp)   | Wählen Sie **System Center Service Manager** aus. |
| **Server-URL**   | Geben Sie die URL der Service Manager-Web-App ein. Weitere Informationen zur Service Manager-Web-App finden Sie [hier](#create-and-deploy-service-manager-web-app-service).
| **Client-ID**   | Geben Sie die Client-ID ein, die Sie (mithilfe des automatischen Skripts) zum Authentifizieren der Web-App generiert haben. Weitere Informationen zum automatisierten Skript finden Sie [hier](./itsmc-service-manager-script.md).|
| **Geheimer Clientschlüssel**   | Geben Sie den für diese ID generierten geheimen Clientschlüssel ein.   |
| **Daten synchronisieren**   | Wählen Sie die Service Manager-Arbeitselemente aus, die Sie über ITSMC synchronisieren möchten.  Diese Arbeitselemente werden in Log Analytics importiert. **Optionen:**  Incidents, Änderungsanforderungen.|
| **Datensynchronisierungsbereich** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximales Limit**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt Log Analytics die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert. |

![Service Manager-Verbindung](media/itsmc-connections/service-manager-connection.png)

**Bei erfolgreicher Verbindung und Synchronisierung**:

- Ausgewählte Arbeitselemente aus Service Manager werden in Azure **Log Analytics** importiert. Die Zusammenfassung dieser Arbeitselemente können Sie auf der IT Service Management Connector-Kachel anzeigen.

- Sie können Incidents über Log Analytics-Warnungen oder Protokolldatensätze oder über Azure-Warnungen in dieser Service Manager-Instanz erstellen.

Weitere Informationen: [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts).

## <a name="create-and-deploy-service-manager-web-app-service"></a>Erstellen und Bereitstellen des Service Manager-Web-App-Dienstes

Zur Verbindung der lokalen Service Manager-Instanz mit dem ITSMC in Azure hat Microsoft in GitHub eine Service Manager-Web-App erstellt.

Führen Sie die folgenden Schritte aus, um die ITSM-Web-App für Ihre Service Manager-Instanz einzurichten:

- **Bereitstellen der Web-App**: Stellen Sie die Web-App bereit, legen Sie die Eigenschaften fest, und führen Sie die Authentifizierung über Azure AD durch. Sie können die Web-App mithilfe des [automatisierten Skripts](./itsmc-service-manager-script.md) von Microsoft bereitstellen.
- **Konfigurieren der Hybridverbindung** - [Konfigurieren Sie diese Verbindung](#configure-the-hybrid-connection)manuell.

### <a name="deploy-the-web-app"></a>Bereitstellen der Web-App
Verwenden Sie das automatisierte [Skript](./itsmc-service-manager-script.md) zum Bereitstellen der Web-App, Festlegen der Eigenschaften und Authentifizierung über Azure AD.

Führen Sie das Skript nach Bereitstellung der folgenden erforderlichen Details aus:

- Details zum Azure-Abonnement
- Ressourcengruppenname
- Standort
- Details zum Service Manager-Server (Servername, Domäne, Benutzername und Kennwort)
- Präfix des Namens der Website für Ihre Web-App
- Service Bus-Namespace.

Das Skript erstellt die Web-App mit dem Namen, den Sie (zusammen mit einigen zusätzlichen Zeichenfolgen zum Sicherstellen der Eindeutigkeit) angegeben haben. Es generiert die **Web-App-URL**, **Client-ID** und das **Clientgeheimnis**.

Speichern Sie diese Werte, da Sie sie beim Erstellen einer Verbindung mit ITSMC verwenden.

**Überprüfen der Web-App-Installation**

1. Navigieren Sie zum **Azure-Portal** > **Ressourcen**.
2. Wählen Sie die Web-App aus, und klicken Sie auf **Einstellungen** > **Anwendungseinstellungen**.
3. Bestätigen Sie die Informationen zur Service Manager-Instanz, die Sie zum Zeitpunkt der App-Bereitstellung über das Skript angegeben haben.

## <a name="configure-the-hybrid-connection"></a>Konfigurieren der Hybridverbindung

Verwenden Sie das folgende Verfahren, um die Hybridverbindung zu konfigurieren, die die Service Manager-Instanz mit dem ITSMC in Azure verbindet.

1. Suchen Sie die Service Manager-Web-App unter **Azure-Ressourcen**.
2. Klicken Sie auf **Einstellungen** > **Netzwerk**.
3. Klicken Sie unter **Hybrid Connections** auf **Endpunkte der Hybridverbindung konfigurieren**.

    ![Hybridverbindung – Netzwerk](media/itsmc-connections/itsmc-hybrid-connection-networking-and-end-points.png)
4. Klicken Sie auf dem Blatt **Hybrid Connections** auf **Hybridverbindung hinzufügen**.

    ![Hybridverbindung hinzufügen](media/itsmc-connections/itsmc-new-hybrid-connection-add.png)

5. Klicken Sie auf dem Blatt **Hybridverbindungen hinzufügen** auf **Neue Hybridverbindung erstellen**.

    ![Neue Hybridverbindung](media/itsmc-connections/itsmc-create-new-hybrid-connection.png)

6. Geben Sie die folgenden Werte ein:

   - **Endpunktname**: Legen Sie einen Namen für die neue Hybridverbindung fest.
   - **Endpunkthost**: FQDN des Service Manager-Verwaltungsservers.
   - **Endpunktport**: Geben Sie 5724 ein.
   - **Service Bus-Namespace**: Verwenden Sie einen vorhandenen Service Bus-Namespace, oder erstellen Sie einen neuen.
   - **Standort:** Wählen Sie den Standort aus.
   - **Name**: Legen Sie einen Namen für den Service Bus fest, falls Sie ihn erstellen.

     ![Hybridverbindungswerte](media/itsmc-connections/itsmc-new-hybrid-connection-values.png)
6. Klicken Sie auf **OK**, um das Blatt **Hybridverbindung erstellen** zu schließen und mit der Erstellung der Hybridverbindung zu beginnen.

    Sobald die Hybridverbindung erstellt wurde, wird sie unter dem Blatt angezeigt.

7. Nachdem die Hybridverbindung erstellt wurde, wählen Sie die Verbindung aus, und klicken Sie auf **Ausgewählte Hybridverbindung hinzufügen**.

    ![Neue Hybridverbindung](media/itsmc-connections/itsmc-new-hybrid-connection-added.png)

### <a name="configure-the-listener-setup"></a>Konfigurieren der Listener-Einrichtung

Verwenden Sie das folgende Verfahren, um die Listener-Einrichtung für die Hybridverbindung zu konfigurieren.

1. Klicken Sie auf dem Blatt **Hybrid Connections** auf **Download the Connection Manager** (Verbindungs-Manager herunterladen), und installieren Sie ihn auf dem Computer, auf dem die System Center Service Manager-Instanz ausgeführt wird.

    Nach Abschluss der Installation ist die Option **Hybrid Connection Manager UI** (Benutzeroberfläche des Managers für Hybridverbindungen) im **Startmenü** verfügbar.

2. Wenn Sie auf **Hybrid Connection Manager UI** (Benutzeroberfläche des Managers für Hybridverbindungen) klicken, werden Sie zur Eingabe Ihrer Azure-Anmeldeinformationen aufgefordert.

3. Melden Sie sich mit Ihren Azure-Anmeldeinformationen an, und wählen Sie Ihr Abonnement aus, in dem die Hybridverbindung erstellt wurde.

4. Klicken Sie auf **Speichern**.

Ihre Hybridverbindung wurde hergestellt.

![erfolgreiche Hybridverbindung](media/itsmc-connections/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]
> 
> Nach dem Herstellen der Hybridverbindung überprüfen testen Sie die Verbindung, indem Sie die bereitgestellte Service Manager-Web-App aufrufen. Stellen Sie sicher, dass die Verbindung erfolgreich ist, bevor Sie versuchen, in Azure eine Verbindung mit dem ITSMC herzustellen.

Die folgende Beispielabbildung zeigt die Details einer erfolgreichen Verbindung:

![Hybridverbindungstest](media/itsmc-connections/itsmc-hybrid-connection-test.png)

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über den ITSM-Connector](itsmc-overview.md)
* [Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts)
* [Problembehandlung in ITSM-Connector](./itsmc-resync-servicenow.md)