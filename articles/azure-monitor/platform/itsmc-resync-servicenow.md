---
title: Manuelles Beheben von ServiceNow-Synchronisierungsproblemen
description: Setzen Sie die Verbindung mit ServiceNow zurück, damit Warnungen in Microsoft Azure ServiceNow wieder aufrufen können.
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: cea4503c4e3b9dd58cc475aaec355a2bb2e0bd29
ms.sourcegitcommit: 2488894b8ece49d493399d2ed7c98d29b53a5599
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98065184"
---
# <a name="troubleshooting-problems-in-itsm-connector"></a>Problembehandlung in ITSM-Connector

In diesem Artikel werden häufige Probleme mit dem ITSM-Connector und das Beheben dieser Probleme erläutert.

Azure Monitor-Warnungen informieren Sie proaktiv, wenn wichtige Bedingungen in Ihren Überwachungsdaten gefunden werden. Sie ermöglichen es Ihnen, Probleme zu identifizieren und zu beheben, bevor die Benutzer Ihres Systems sie bemerken. Weitere Informationen über Warnungen finden Sie unter Überblick über Warnungen in Microsoft Azure.
Der Kunde kann auswählen, wie er über die Warnung benachrichtigt werden möchte, sei es per E-Mail, SMS, Webhook oder sogar zur Automatisierung einer Lösung. Eine weitere Option für Benachrichtigungen ist die Verwendung von ITSM.
ITSM bietet Ihnen die Möglichkeit, die Warnungen an ein externes Ticketsystem wie ServiceNow zu senden.

## <a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Visualisieren und Analysieren der Incident- und Änderungsanforderungsdaten

Basierend auf Ihrer Konfiguration, die Sie beim Einrichten einer Verbindung vorgenommenen haben, kann der ITSM-Connector bis zu 120 Tage an Incident- und Änderungsanforderungsdaten synchronisieren. Das Schema für den Protokolldatensatz dieser Daten finden Sie im [Abschnitt „Zusätzliche Informationen“](./itsmc-synced-data.md) dieses Artikels.

Sie können die Incident- und die Änderungsanforderungsdaten über das Dashboard des ITSM-Connectors visualisieren:

![Screenshot des Dashboards des ITSM-Connectors](media/itsmc-overview/itsmc-overview-sample-log-analytics.png)

Das Dashboard bietet darüber hinaus Informationen zum Connectorstatus, die als Ausgangspunkt bei der Analyse von Verbindungsproblemen verwendet werden können.

### <a name="error-investigation-using-the-dashboard"></a>Untersuchung von Fehlern auf dem Dashboard

Gehen Sie folgendermaßen vor, um Fehler auf dem Dashboard anzuzeigen:

1. Suchen Sie in **Alle Ressourcen** nach **ServiceDesk(*IhrArbeitsbereichsname*)** :

   ![Screenshot der zuletzt verwendeten Ressourcen im Azure-Portal](media/itsmc-definition/create-new-connection-from-resource.png)

2. Wählen Sie im linken Bereich unter **Arbeitsbereichsdatenquellen** die Option **ITSM-Verbindungen** aus:

   ![Screenshot des Menüelements „ITSM-Verbindungen“](media/itsmc-overview/add-new-itsm-connection.png)

3. Wählen Sie unter **Zusammenfassung** im linken Feld **ITSM-Connector** die Option **Zusammenfassung anzeigen** aus:

    ![Screenshot der Zusammenfassungsansicht](media/itsmc-resync-servicenow/dashboard-view-summary.png)

4. Klicken Sie unter **Zusammenfassung** im linken Feld **ITSM-Connector** auf das Diagramm:

    ![Screenshot des Klicks auf das Diagramm](media/itsmc-resync-servicenow/dashboard-graph-click.png)

5. Auf diesem Dashboard können Sie den Status und die Fehler in Ihrem Connector überprüfen.
    ![Screenshot mit dem Connectorstatus](media/itsmc-resync-servicenow/connector-dashboard.png)

### <a name="service-map"></a>Dienstzuordnung

Zudem lassen sich die mit den betroffenen Computern synchronisierten Incidents innerhalb der Dienstzuordnungslösung visuell darstellen.

Service Map ermittelt automatisch die Anwendungskomponenten auf Windows- und Linux-Systemen und stellt die Kommunikation zwischen Diensten dar. In dieser Lösung können Sie die Server nach ihrer Funktion anzeigen: als verbundene Systeme, die wichtige Dienste bereitstellen. Die Dienstzuordnungslösung zeigt Verbindungen zwischen Servern, Prozessen und Ports über die gesamte TCP-Verbindungsarchitektur an. Außer der Installation eines Agents ist keine Konfiguration erforderlich. Weitere Informationen finden Sie unter [Verwenden der Dienstzuordnung](../insights/service-map.md).

Wenn Sie die Dienstzuordnungslösung verwenden, können Sie die in ITSM-Lösungen erstellten Service Desk-Elemente wie im Folgenden gezeigt anzeigen:

![Screenshot des Log Analytics-Bildschirms](media/itsmc-overview/itsmc-overview-integrated-solutions.png)

## <a name="troubleshoot-itsm-connections"></a>Problembehandlung bei ITSM-Verbindungen

- Wenn beim Herstellen der Verbindung mit dem ITSM-System ein Fehler auftritt und die Fehlermeldung **Fehler beim Speichern der Verbindung** angezeigt wird, gehen Sie folgendermaßen vor:
   - Bei ServiceNow-, Cherwell- und Provance-Verbindungen:  
     - Stellen Sie sicher, dass Sie Benutzername, Kennwort, Client-ID und geheimen Clientschlüssel für jede der Verbindungen richtig eingegeben haben.  
     - Stellen Sie sicher, dass Sie über ausreichende Berechtigungen für das entsprechende ITSM-Produkt verfügen, um die Verbindung herzustellen.  
   - Bei Service Manager-Verbindungen:  
     - Stellen Sie sicher, dass die Web-App erfolgreich bereitgestellt und die Hybridverbindung erstellt wird. Um zu überprüfen, ob die Verbindung mit dem lokalen Service Manager-Computer erfolgreich hergestellt wird, navigieren Sie zur Web-App-URL, wie in der Dokumentation zum Herstellen der [Hybridverbindung](./itsmc-connections-scsm.md#configure-the-hybrid-connection) erläutert.  

- Wenn Daten von ServiceNow nicht in Log Analytics synchronisiert werden, stellen Sie sicher, dass sich die ServiceNow-Instanz nicht im Energiesparmodus befindet. ServiceNow-Entwicklungsinstanzen wechseln manchmal nach längerem Leerlauf in den Energiesparmodus. Wenn dies nicht der Fall ist, melden Sie das Problem.
- Wenn Log Analytics-Warnungen ausgelöst werden, aber keine Arbeitselemente im ITSM-Produkt oder keine Konfigurationselemente erstellt bzw. diese nicht mit Arbeitselementen verknüpft werden, nutzen Sie folgende Quellen. Dort finden Sie darüber hinaus allgemeine Informationen:
   -  ITSMC: Die Lösung zeigt eine Zusammenfassung der Verbindungen, Arbeitselemente, Computer usw. an. Wählen Sie die Kachel mit der Bezeichnung **Connectorstatus** aus. Dadurch wechseln Sie zur **Protokollsuche** mit der zugehörigen Abfrage. Weitere Informationen finden Sie in den Protokolldatensätzen mit dem `LogType_S` `ERROR`.
   - Seite **Protokollsuche**: Sie können die Fehler und zugehörigen Informationen direkt mithilfe der Abfrage `*ServiceDeskLog_CL*` anzeigen.

### <a name="troubleshoot-service-manager-web-app-deployment"></a>Beheben von Problemen bei der Service Manager-Web-App-Bereitstellung

-   Stellen Sie bei Problemen mit der Web-App-Bereitstellung sicher, dass Sie im angegebenen Abonnement über ausreichende Berechtigungen zum Erstellen/Bereitstellen von Ressourcen verfügen.
-   Wenn die Fehlermeldung **Objektverweis ist nicht auf eine Instanz eines Objekts festgelegt** angezeigt wird, während Sie das [Skript](itsmc-service-manager-script.md) ausführen, vergewissern Sie sich, dass Sie im Abschnitt **Benutzerkonfiguration** gültige Werte eingegeben haben.
-   Wenn Sie den Service Bus-Relaynamespace nicht erstellen können, stellen Sie sicher, dass der erforderliche Ressourcenanbieter im Abonnement registriert ist. Wenn er nicht registriert ist, erstellen Sie den Service Bus-Relaynamespace manuell über das Azure-Portal. Sie können ihn auch beim [Erstellen der Hybridverbindung](./itsmc-connections-scsm.md#configure-the-hybrid-connection) über das Azure-Portal erstellen.

### <a name="how-to-manually-fix-sync-problems"></a>Manuelles Beheben von Synchronisierungsproblemen

Azure Monitor kann Verbindungen mit ITSM-Drittanbietern (IT Service Management) herstellen. ServiceNow ist einer dieser Anbieter.

Aus Sicherheitsgründen müssen Sie möglicherweise das Authentifizierungstoken für die Verbindung mit ServiceNow aktualisieren.
Verwenden Sie das folgende Synchronisierungsverfahren, um die Verbindung erneut zu aktivieren und das Token zu aktualisieren:


1. Suchen Sie die Lösung im oberen Suchbanner, und wählen Sie dann die relevanten Lösungen aus.

    ![Screenshot: Oberes Suchbanner und Auswählen der relevanten Lösungen](media/itsmc-resync-servicenow/solution-search-8bit.png)

1. Wählen Sie auf dem Bildschirm der Lösung im Abonnementfilter „Alles auswählen“ aus, und filtern Sie dann nach „ServiceDesk“.

    ![Screenshot, der zeigt, wo Sie „Alles auswählen“ auswählen und wo Sie nach ServiceDesk filtern](media/itsmc-resync-servicenow/solutions-list-8bit.png)

1. Wählen Sie die Lösung Ihrer ITSM-Verbindung aus.
1. Wählen Sie im linken Banner die ITSM-Verbindung aus.

    ![Screenshot, der zeigt, wo Sie ITSM-Verbindungen auswählen](media/itsmc-resync-servicenow/itsm-connector-8bit.png)

1. Wählen Sie in der Liste jeden Connector aus. 
    1. Klicken Sie jeweils auf den Namen des Connectors, um ihn zu konfigurieren.
    1. Löschen Sie alle Connectors, die nicht mehr verwendet werden.

    1. Aktualisieren Sie die Felder entsprechend [diesen Definitionen](./itsmc-connections.md) anhand Ihres Partnertyps.

    1. Klicken Sie auf „Synchronisieren“.

       ![Screenshot, auf dem die Schaltfläche „Synchronisieren“ hervorgehoben ist](media/itsmc-resync-servicenow/resync-8bit2.png)

    1. Klicken Sie auf „Speichern“.

        ![Neue Verbindung](media/itsmc-resync-servicenow/save-8bit.png)

f.    Überprüfen Sie die Benachrichtigungen, um festzustellen, ob der Vorgang gestartet wurde.
