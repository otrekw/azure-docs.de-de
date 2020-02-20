---
title: 'Überwachung: Apache Ambari- und Azure Monitor-Protokolle – Azure HDInsight'
description: Erfahren Sie, wie Sie mit Ambari und Azure Monitor-Protokollen die Integrität und Verfügbarkeit des Clusters überwachen können.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/06/2020
ms.openlocfilehash: 383366fa3e436c79bed28a7c47f1e9daa5f0d9de
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77060172"
---
# <a name="how-to-monitor-cluster-availability-with-apache-ambari-and-azure-monitor-logs"></a>Überwachen der Clusterverfügbarkeit mit Apache Ambari und Azure Monitor-Protokollen

HDInsight-Cluster zeichnen sich durch sowohl Apache Ambari, das auf einen Blick Integritätsinformationen und vordefinierte Warnungen liefert, als auch die Integration von Azure Monitor-Protokollen aus, die abfragbare Metriken und Protokolle sowie konfigurierbare Warnungen zur Verfügung stellt.

Dieses Dokument veranschaulicht, wie Sie die Tools verwenden, um Ihren Cluster zu überwachen. Es enthält einige Beispiele für die Konfiguration einer Ambari-Warnung, die Überwachung der Knotenverfügbarkeitsrate und die Erstellung einer Azure Monitor-Warnung, die ausgelöst wird, wenn von mindestens einem Knoten innerhalb von fünf Stunden kein Heartbeat empfangen wurde.

## <a name="ambari"></a>Ambari

### <a name="dashboard"></a>Dashboard

Sie greifen auf das Ambari-Dashboard zu, indem Sie im Azure-Portal im Abschnitt **Clusterdashboards** der HDInsight-Übersicht den Link für die **Ambari-Homepage** auswählen (siehe unten). Alternativ können Sie darauf zugreifen, indem Sie in einem Browser zu `https://CLUSTERNAME.azurehdinsight.net` navigieren, wobei CLUSTERNAME dem Namen Ihres Clusters entspricht.

![Ansicht des HDInsight-Ressourcenportals](media/hdinsight-cluster-availability/azure-portal-dashboard-ambari.png)

Anschließend werden Sie aufgefordert, einen Benutzernamen und ein Kennwort für die Anmeldung beim Cluster anzugeben. Geben Sie die Anmeldeinformationen ein, die Sie beim Erstellen des Clusters festgelegt haben.

Sie werden dann zum Ambari-Dashboard weitergeleitet, das Widgets mit einer Reihe von Metriken enthält, die Ihnen einen schnellen Überblick über die Integrität Ihres HDInsight-Clusters geben. Diese Widgets zeigen Metriken wie z.B. die Anzahl der aktiven DataNodes (Workerknoten) und JournalNodes (Zookeeper-Knoten), Betriebszeit von NameNodes (Hauptknoten) sowie Metriken, die spezifisch für bestimmte Clustertypen sind, wie die Betriebszeit von YARN ResourceManager für Spark- und Hadoop-Cluster.

![Apache Ambari – Verwenden der Dashboardanzeige](media/hdinsight-cluster-availability/apache-ambari-dashboard.png)

### <a name="hosts--view-individual-node-status"></a>Hosts – Anzeigen des Status einzelner Knoten

Sie können auch Statusinformationen für einzelne Knoten anzeigen. Wählen Sie die Registerkarte **Hosts** aus, um eine Liste aller Knoten in Ihrem Cluster und grundlegende Informationen zu jedem Knoten anzuzeigen. Das grüne Häkchen links neben einem Knotennamen gibt an, dass alle Komponenten auf dem Knoten betriebsbereit sind. Wenn eine Komponente auf einem Knoten ausgefallen ist, wird ein rotes Warndreieck anstelle des grünen Häkchens angezeigt.

![HDInsight Apache Ambari – Hostansicht](media/hdinsight-cluster-availability/apache-ambari-hosts1.png)

Sie können dann den **Namen** eines Knotens auswählen, um detailliertere Hostmetriken für diesen bestimmten Knoten anzuzeigen. Diese Ansicht zeigt den Status bzw. die Verfügbarkeit jeder einzelnen Komponente.

![Apache Ambari – Einzelknotenansicht für Hosts](media/hdinsight-cluster-availability/apache-ambari-hosts-node.png)

### <a name="ambari-alerts"></a>Ambari-Warnungen

Ambari bietet außerdem mehrere konfigurierbare Warnungen, die Benutzer bei bestimmten Ereignissen benachrichtigen. Wenn Warnungen ausgelöst werden, werden sie in der linken oberen Ecke von Ambari in einem roten Badge mit der Anzahl der Warnungen angezeigt. Wenn Sie diesen Badge auswählen, wird eine Liste der aktuellen Warnungen angezeigt.

![Apache Ambari – Aktuelle Anzahl von Warnungen](media/hdinsight-cluster-availability/apache-ambari-alerts.png)

Um eine Liste der Warnungsdefinitionen und deren Status anzuzeigen, wählen Sie die Registerkarte **Alerts** (Warnungen) aus (siehe unten).

![Ambari-Warnungen: Ansicht der Definitionen](media/hdinsight-cluster-availability/ambari-alerts-definitions.png)

Ambari bietet viele vordefinierte Warnungen in Bezug auf Verfügbarkeit, wie z.B.:

| Name der Warnung                        | BESCHREIBUNG   |
|---|---|
| DataNode Health Summary (DataNode-Integritätszusammenfassung)           | Diese Warnung auf Dienstebene wird ausgelöst, wenn es fehlerhafte DataNodes gibt.|
| NameNode High Availability Health (NameNode-Hochverfügbarkeitsintegrität) | Diese Warnung auf Dienstebene wird ausgelöst, wenn entweder der aktive NameNode oder der Standby-NameNode nicht ausgeführt wird.|
| Percent JournalNodes Available (Prozentsatz verfügbarer JournalNodes)    | Diese Warnung wird ausgelöst, wenn die Anzahl der ausgefallenen JournalNodes im Cluster höher ist als der konfigurierte kritische Schwellenwert. Sie aggregiert die Ergebnisse der JournalNode-Prozessüberprüfungen. |
| Percent DataNodes Available (Prozentsatz verfügbarer DataNodes)       | Diese Warnung wird ausgelöst, wenn die Anzahl der ausgefallenen DataNodes im Cluster höher ist als der konfigurierte kritische Schwellenwert. Sie aggregiert die Ergebnisse der DataNode-Prozessüberprüfungen.|

Eine vollständige Liste der Ambari-Warnungen, die helfen, die Verfügbarkeit eines Clusters zu überwachen, finden Sie [hier](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-linux#ambari-web-ui).

Um Details zu einer Warnung anzuzeigen oder Kriterien zu ändern, wählen Sie den **Namen** der Warnung aus. Nehmen Sie **DataNode Health Summary** (DataNode-Integritätszusammenfassung) als Beispiel. Sie sehen eine Beschreibung der Warnung sowie die spezifischen Kriterien, die eine „Warnung“ oder „kritische“ Warnung auslösen, und das Überprüfungsintervall für die Kriterien. Um die Konfiguration zu bearbeiten, wählen Sie rechts oben im Konfigurationsfeld die Schaltfläche **Edit** (Bearbeiten) aus.

![Apache Ambari – Warnungskonfiguration](media/hdinsight-cluster-availability/ambari-alert-configuration.png)

Hier können Sie die Beschreibung und vor allem das Überprüfungsintervall und die Schwellenwerte für Warnungen bzw. kritische Warnungen bearbeiten.

![Bearbeitungsansicht der Ambari-Warnungskonfigurationen](media/hdinsight-cluster-availability/ambari-alert-configuration-edit.png)

Bei diesem Beispiel könnten Sie veranlassen, dass zwei fehlerhafte DataNodes eine kritische Warnung auslösen und ein fehlerhafter DataNode nur eine Warnung auslöst. Wählen Sie **Save** (Speichern) aus, wenn Sie die Bearbeitung abgeschlossen haben.

### <a name="email-notifications"></a>E-Mail-Benachrichtigungen

Sie können optional auch E-Mail-Benachrichtigungen für Ambari-Warnungen konfigurieren. Klicken Sie dazu auf der Registerkarte **Alerts** (Warnungen) links oben auf die Schaltfläche **Actions** (Aktionen) und dann auf **Manage Notifications** (Benachrichtigungen verwalten).

![Ambari: Verwalten der Benachrichtigungsaktion](media/hdinsight-cluster-availability/ambari-manage-notifications.png)

Ein Dialogfeld für die Verwaltung von Benachrichtigungen zu Warnungen wird geöffnet. Wählen Sie unten im Dialogfeld **+** aus, und füllen Sie die Pflichtfelder aus, um in Ambari die Details zum E-Mail-Server anzugeben, von dem E-Mails gesendet werden sollen.

> [!TIP]
> Die Einrichtung von Ambari-E-Mail-Benachrichtigungen kann eine gute Möglichkeit sein, bei Verwaltung vieler HDInsight-Cluster Benachrichtigungen zentral zu erhalten.

## <a name="azure-monitor-logs-integration"></a>Integration von Azure Monitor-Protokollen

Mit Azure Monitor-Protokollen können Daten, die von mehreren Ressourcen wie z. B. HDInsight-Clustern generiert wurden, zentral gesammelt und zusammengefasst werden, um eine einheitliche Überwachungsumgebung zu schaffen.

Als Voraussetzung benötigen Sie einen Log Analytics-Arbeitsbereich, um die gesammelten Daten zu speichern. Wenn Sie noch keinen eingerichtet haben, befolgen Sie die nachstehenden Anweisungen: [Erstellen eines Log Analytics-Arbeitsbereichs](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

### <a name="enable-hdinsight-azure-monitor-logs-integration"></a>Aktivieren der Integration von Azure Monitor-Protokollen in HDInsight

Wählen Sie im Portal auf der Seite mit den HDInsight-Clusterressourcen **Azure Monitor** aus. Wählen Sie dann **Aktivieren** und Ihren Log Analytics-Arbeitsbereich aus der Dropdownliste aus.

![HDInsight Operations Management Suite](media/hdinsight-cluster-availability/azure-portal-monitoring.png)

### <a name="query-metrics-and-logs-tables"></a>Abfragemetriken und Protokolltabellen

Sobald die Integration von Azure Monitor-Protokollen aktiviert ist (was einige Minuten dauern kann), navigieren Sie zu Ihrer Ressource **Log Analytics-Arbeitsbereich** und wählen **Protokolle** aus.

![Log Analytics-Arbeitsbereich – Protokolle](media/hdinsight-cluster-availability/hdinsight-portal-logs.png)

In den Protokollen sind eine Reihe von Beispielabfragen aufgeführt, wie z. B.:

| Abfragename                      | BESCHREIBUNG                                                               |
|---------------------------------|---------------------------------------------------------------------------|
| Computer availability today (Heute verfügbare Computer)    | Diagramm der Anzahl von Computern, die Protokolle senden, stündlich                     |
| List heartbeats (Heartbeat auflisten)                 | Listet alle Heartbeats von Computern der letzten Stunde auf                           |
| Last heartbeat of each computer (Letzter Heartbeat jedes Computers) | Zeigt den letzten von jedem Computer gesendeten Heartbeat                             |
| Unavailable computers (Nicht verfügbare Computer)           | Listet alle bekannten Computer auf, die in den letzten 5 Stunden keinen Heartbeat gesendet haben |
| Availability rate (Verfügbarkeitsrate)               | Berechnet die Verfügbarkeitsrate jedes angebundenen Computers                |

Sie können die Beispielabfrage **Availability rate** (Verfügbarkeitsrate) ausführen, indem Sie für die Abfrage **Run** (Ausführen) auswählen, wie im Screenshot oben dargestellt. Dadurch wird die Verfügbarkeitsrate jedes Knotens in Ihrem Cluster als Prozentsatz angezeigt. Wenn Sie mehrere HDInsight-Cluster für das Senden von Metriken an denselben Log Analytics-Arbeitsbereich aktiviert haben, wird die Verfügbarkeitsrate für alle Knoten in diesen Clustern angezeigt.

![Protokolle im Log Analytics-Arbeitsbereich mit Beispielabfrage zur Verfügbarkeitsrate](media/hdinsight-cluster-availability/portal-availability-rate.png)

> [!NOTE]  
> Die Verfügbarkeitsrate wird über einen Zeitraum von 24 Stunden gemessen, sodass Ihr Cluster mindestens 24 Stunden ausgeführt werden muss, bevor Sie genaue Verfügbarkeitsraten sehen.

Sie können diese Tabelle an ein freigegebenes Dashboard anheften, indem Sie rechts oben auf **Anheften** klicken. Wenn Sie über kein beschreibbares freigegebenes Dashboard verfügen, erfahren Sie hier, wie Sie ein Dashboard erstellen: [Erstellen und Freigeben von Dashboards im Azure-Portal](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards#publish-and-share-a-dashboard).

### <a name="azure-monitor-alerts"></a>Azure Monitor-Warnungen

Sie können auch Azure Monitor-Warnungen einrichten, die ausgelöst werden, wenn der Wert einer Metrik oder die Ergebnisse einer Abfrage bestimmte Bedingungen erfüllen. Als Beispiel erstellen wir eine Warnung zum Senden einer E-Mail, wenn mindestens von einem Knoten innerhalb von 5 Stunden kein Heartbeat gesendet wurde (d. h. es wird angenommen, dass er nicht verfügbar ist).

Führen Sie unter **Logs** (Protokolle) die Beispielabfrage **Unavailable computers** (Nicht verfügbare Computer) aus, indem Sie für diese Abfrage **Run** (Ausführen) auswählen, wie unten dargestellt.

![Protokolle im Log Analytics-Arbeitsbereich mit Beispiel zu nicht verfügbaren Computern](media/hdinsight-cluster-availability/portal-unavailable-computers.png)

Wenn alle Knoten verfügbar sind, sollte diese Abfrage vorerst 0 (null) Ergebnisse zurückgeben. Klicken Sie auf **Neue Warnungsregel**, um mit der Konfiguration Ihrer Warnung für diese Abfrage zu beginnen.

![Log Analytics-Arbeitsbereich mit neuer Warnungsregel](media/hdinsight-cluster-availability/portal-logs-new-alert-rule.png)

Eine Warnung besteht aus drei Komponenten: der *Ressource*, für die die Regel erstellt werden soll (in diesem Fall der Log Analytics-Arbeitsbereich), der *Bedingung* zum Auslösen der Warnung und den *Aktionsgruppen*, die bestimmen, was passieren soll, wenn die Warnung ausgelöst wird.
Klicken Sie auf den **Titel der Bedingung**, wie unten gezeigt, um die Konfiguration der Signallogik abzuschließen.

![Portalwarnung – Erstellen einer Regelbedingung](media/hdinsight-cluster-availability/portal-condition-title.png)

Dadurch wird **Signallogik konfigurieren** geöffnet.

Legen Sie den Abschnitt **Warnungslogik** wie folgt fest:

*Basierend auf: Anzahl von Ergebnissen, Bedingung: Größer als, Schwellenwert: 0.*

Da diese Abfrage nur nicht verfügbare Knoten als Ergebnis zurückgibt, sollte die Warnung immer ausgelöst werden, wenn die Anzahl der Ergebnisse größer als 0 ist.

Legen Sie im Abschnitt **Auswertung basierend auf** den **Zeitraum** und die **Häufigkeit** basierend darauf fest, wie oft Sie die Prüfung auf nicht verfügbare Knoten erfolgen soll.

Bei dieser Warnung möchten Sie sicherstellen, dass sich **Zeitraum und Häufigkeit** entsprechen. Weitere Informationen zu Zeitraum, Häufigkeit und anderen Warnungsparametern finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log#log-search-alert-rule---definition-and-types).

Wählen Sie **Fertig** aus, wenn die Konfiguration der Signallogik abgeschlossen ist.

![Warnungsregel zur Konfiguration von Signallogik](media/hdinsight-cluster-availability/portal-configure-signal-logic.png)

Wenn Sie noch nicht über eine Aktionsgruppe verfügen, klicken Sie im Abschnitt **Aktionsgruppen** auf **Neu erstellen**.

![Warnungsregel zur Erstellung einer neuen Aktionsgruppe](media/hdinsight-cluster-availability/portal-create-new-action-group.png)

Dadurch wird **Aktionsgruppe hinzufügen** geöffnet. Wählen Sie einen **Aktionsgruppennamen**, **Kurznamen**, ein **Abonnement** und eine **Ressourcengruppe**. Wählen Sie im Abschnitt **Aktionen** einen **Aktionsnamen** und **E-Mail/SMS/Push/Sprachanruf** als **Aktionstyp** aus.

> [!NOTE]
> Es gibt mehrere andere Aktionen, die neben „E-Mail/SMS/Push/Sprachanruf“ eine Warnung auslösen können, wie beispielsweise eine Azure-Funktion, LogicApp, Webhook, ITSM und Automation Runbook. [Weitere Informationen](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#action-specific-information).

Dadurch wird **E-Mail/SMS/Push/Sprachanruf** geöffnet. Wählen Sie einen **Namen** für den Empfänger, **aktivieren** Sie das Feld **E-Mail**, und geben Sie eine E-Mail-Adresse ein, an die die Warnung gesendet werden soll. Wählen Sie unter **E-Mail/SMS/Push/Sprachanruf** und unter **Aktionsgruppe hinzufügen** die Option **OK** aus, um die Konfiguration der Aktionsgruppe abzuschließen.

![Erstellen einer Warnungsregel – Hinzufügen einer Aktionsgruppe](media/hdinsight-cluster-availability/portal-add-action-group.png)

Nachdem diese Blätter geschlossen wurden, sollte Ihre Aktionsgruppe unter dem Abschnitt **Aktionsgruppen** aufgeführt sein. Geben Sie abschließend im Abschnitt **Warnungsdetails** den **Namen der Warnungsregel** und eine **Beschreibung** ein, und wählen Sie einen **Schweregrad**. Klicken Sie im letzten Schritt auf **Warnungsregel erstellen**.

![Portal – Fertigstellen der Warnungsregel](media/hdinsight-cluster-availability/portal-create-alert-rule-finish.png)

> [!TIP]
> Die Möglichkeit, einen **Schweregrad** anzugeben, ist ein leistungsstarkes Instrument, das bei der Erstellung mehrerer Benachrichtigungen verwendet werden kann. Sie können beispielsweise eine Warnung mit Schweregrad 1 auszulösen, wenn ein einzelner Hauptknoten ausfällt, und eine weitere kritische Warnung mit Schweregrad 0 für den unwahrscheinlichen Fall erstellen, dass beide Hauptknoten ausfallen.

Wenn die Bedingung für diese Warnung erfüllt ist, wird die Warnung ausgelöst, und Sie erhalten eine E-Mail mit den entsprechenden Details:

![Beispiel für eine Azure Monitor-Warnungs-E-Mail](media/hdinsight-cluster-availability/portal-oms-alert-email.png)

Sie können auch alle ausgelösten Warnungen gruppiert nach Schweregrad anzeigen, indem Sie im **Log Analytics-Arbeitsbereich** zu **Warnungen** wechseln.

![Warnungen im Log Analytics-Arbeitsbereich](media/hdinsight-cluster-availability/hdi-portal-oms-alerts.png)

Wenn Sie eine Gruppierung nach Schweregrad auswählen (z. B. **Sev 1**, wie oben hervorgehoben), werden Einträge für alle Warnungen dieses Schweregrads angezeigt, die ausgelöst wurden (siehe unten):

![Log Analytics-Arbeitsbereich mit Warnungen des Schweregrads 1](media/hdinsight-cluster-availability/portal-oms-alerts-sev1.png)

## <a name="next-steps"></a>Nächste Schritte

- [Verfügbarkeit und Zuverlässigkeit von Apache Hadoop-Clustern in HDInsight](hdinsight-high-availability-linux.md)
