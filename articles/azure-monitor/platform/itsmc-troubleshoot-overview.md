---
title: Problembehandlung in ITSM-Connector
description: Problembehandlung im ITSM-Connector
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: 2ffe7c8994d32917a08896c7d25f20d4adf09066
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98601894"
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

Weitere Informationen zur Dashboarduntersuchung finden Sie unter [Untersuchung von Fehlern auf dem Dashboard](./itsmc-dashboard.md).

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

- Wenn Log Analytics-Warnungen ausgelöst werden, aber keine Arbeitselemente im ITSM-Produkt oder keine Konfigurationselemente erstellt bzw. diese nicht mit Arbeitselementen verknüpft werden, nutzen Sie folgende Quellen. Dort finden Sie darüber hinaus allgemeine Informationen:
   -  ITSMC: Die Lösung zeigt eine [Zusammenfassung der Verbindungen](itsmc-dashboard.md), Arbeitselemente, Computer usw. an. Wählen Sie die Kachel mit der Bezeichnung **Connectorstatus** aus. Dadurch wechseln Sie zur **Protokollsuche** mit der zugehörigen Abfrage. Weitere Informationen finden Sie in den Protokolldatensätzen mit dem `LogType_S` `ERROR`.
   Details zu den Meldungen in der Tabelle finden Sie [hier](itsmc-dashboard-errors.md).
   - Seite **Protokollsuche**: Sie können die Fehler und zugehörigen Informationen direkt mithilfe der Abfrage `*ServiceDeskLog_CL*` anzeigen.

## <a name="common-symptoms---how-it-should-be-resolved"></a>Allgemeine Symptome und mögliche Problemlösung

Die folgende Liste enthält allgemeine Symptome und mögliche Lösungen des Problems:

* **Symptom**: Doppelte Arbeitselemente werden erstellt.

    **Ursache**: Es können zwei Ursachen vorliegen:
    * Für die Warnung ist mehr als eine ITSM-Aktion definiert.
    * Die Warnung wurde aufgelöst.

    **Lösung:** Es gibt zwei mögliche Lösungen:
    * Stellen Sie sicher, dass pro Warnung nur eine einzige ITSM-Aktionsgruppe vorhanden ist.
    * Der ITSM-Connector unterstützt Statusaktualisierungen bei übereinstimmenden Arbeitselementen nicht, wenn eine Warnung aufgelöst wurde. Es wird ein neues Arbeitselement erstellt.
* **Symptom**: Es werden keine Arbeitselemente erstellt.

    **Ursache:** Für dieses Symptom kann es eine Reihe von Gründen geben:
    * Der Code wurde seitens ServiceNow geändert.
    * Berechtigungen sind fehlerhaft konfiguriert.
    * ServiceNow-Ratenbegrenzungen sind zu hoch oder zu niedrig.
    * Das Aktualisierungstoken ist abgelaufen.
    * Der ITSM-Connector wurde gelöscht.

    **Lösung:** Sie können die Fehler im Abschnitt zum Connectorstatus auf dem [Dashboard](itsmc-dashboard.md) überprüfen. Sehen Sie sich die [allgemeinen Fehler](itsmc-dashboard-errors.md) an, und finden Sie heraus, wie der Fehler behoben werden kann.

* **Symptom**: Für eine Aktionsgruppe kann keine ITSM-Aktion erstellt werden.

    **Ursache**: Für einen neu erstellten ITSM-Connector muss die anfängliche Synchronisierung noch abgeschlossen werden.

    **Lösung**: Sehen Sie sich die [allgemeinen Benutzeroberflächenfehler](itsmc-dashboard-errors.md#ui-common-errors) an, und finden Sie heraus, wie der Fehler behoben werden kann.