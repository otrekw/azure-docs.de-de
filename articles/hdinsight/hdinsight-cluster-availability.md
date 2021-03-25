---
title: Überwachen der Clusterverfügbarkeit mit Apache Ambari in Azure HDInsight
description: Hier erfahren Sie, wie Sie die Clusterintegrität und -verfügbarkeit mithilfe von Apache Ambari überwachen können.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 05/01/2020
ms.openlocfilehash: dcbe44defeb89ad2f67833b263e5f4983070a46c
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104863529"
---
# <a name="how-to-monitor-cluster-availability-with-apache-ambari-in-azure-hdinsight"></a>Überwachen der Clusterverfügbarkeit mit Apache Ambari in Azure HDInsight

HDInsight-Cluster enthalten Apache Ambari, das Integritätsinformationen auf einen Blick und vordefinierte Warnungen bereitstellt.

In diesem Artikel wird gezeigt, wie Sie Ihren Cluster mithilfe von Apache Ambari überwachen. Er enthält einige Beispiele für die Konfiguration einer Ambari-Warnung, die Überwachung der Knotenverfügbarkeitsrate und die Erstellung einer Azure Monitor-Warnung, die ausgelöst wird, wenn innerhalb von fünf Stunden von mindestens einem Knoten kein Heartbeat empfangen wurde.

## <a name="dashboard"></a>Dashboard

Sie greifen auf das Ambari-Dashboard zu, indem Sie im Azure-Portal im Abschnitt **Clusterdashboards** der HDInsight-Übersicht den Link für die **Ambari-Homepage** auswählen (siehe unten). Alternativ können Sie darauf zugreifen, indem Sie in einem Browser zu `https://CLUSTERNAME.azurehdinsight.net` navigieren, wobei CLUSTERNAME dem Namen Ihres Clusters entspricht.

:::image type="content" source="media/hdinsight-cluster-availability/azure-portal-dashboard-ambari.png" alt-text="Ansicht des HDInsight-Ressourcenportals":::

Anschließend werden Sie aufgefordert, einen Benutzernamen und ein Kennwort für die Anmeldung beim Cluster anzugeben. Geben Sie die Anmeldeinformationen ein, die Sie beim Erstellen des Clusters festgelegt haben.

Sie werden dann zum Ambari-Dashboard weitergeleitet, das Widgets mit einer Reihe von Metriken enthält, die Ihnen einen schnellen Überblick über die Integrität Ihres HDInsight-Clusters geben. Diese Widgets zeigen Metriken wie z.B. die Anzahl der aktiven DataNodes (Workerknoten) und JournalNodes (Zookeeper-Knoten), Betriebszeit von NameNodes (Hauptknoten) sowie Metriken, die spezifisch für bestimmte Clustertypen sind, wie die Betriebszeit von YARN ResourceManager für Spark- und Hadoop-Cluster.

:::image type="content" source="media/hdinsight-cluster-availability/apache-ambari-dashboard.png" alt-text="Apache Ambari – Verwenden der Dashboardanzeige":::

## <a name="hosts--view-individual-node-status"></a>Hosts – Anzeigen des Status einzelner Knoten

Sie können auch Statusinformationen für einzelne Knoten anzeigen. Wählen Sie die Registerkarte **Hosts** aus, um eine Liste aller Knoten in Ihrem Cluster und grundlegende Informationen zu jedem Knoten anzuzeigen. Das grüne Häkchen links neben einem Knotennamen gibt an, dass alle Komponenten auf dem Knoten betriebsbereit sind. Wenn eine Komponente auf einem Knoten ausgefallen ist, wird ein rotes Warndreieck anstelle des grünen Häkchens angezeigt.

:::image type="content" source="media/hdinsight-cluster-availability/apache-ambari-hosts1.png" alt-text="HDInsight Apache Ambari – Hostansicht":::

Sie können dann den **Namen** eines Knotens auswählen, um detailliertere Hostmetriken für diesen bestimmten Knoten anzuzeigen. Diese Ansicht zeigt den Status bzw. die Verfügbarkeit jeder einzelnen Komponente.

:::image type="content" source="media/hdinsight-cluster-availability/apache-ambari-hosts-node.png" alt-text="Apache Ambari – Einzelknotenansicht für Hosts":::

## <a name="ambari-alerts"></a>Ambari-Warnungen

Ambari bietet außerdem mehrere konfigurierbare Warnungen, die Benutzer bei bestimmten Ereignissen benachrichtigen. Wenn Warnungen ausgelöst werden, werden sie in der linken oberen Ecke von Ambari in einem roten Badge mit der Anzahl der Warnungen angezeigt. Wenn Sie diesen Badge auswählen, wird eine Liste der aktuellen Warnungen angezeigt.

:::image type="content" source="media/hdinsight-cluster-availability/apache-ambari-alerts.png" alt-text="Apache Ambari – Aktuelle Anzahl von Warnungen":::

Um eine Liste der Warnungsdefinitionen und deren Status anzuzeigen, wählen Sie die Registerkarte **Alerts** (Warnungen) aus (siehe unten).

:::image type="content" source="media/hdinsight-cluster-availability/ambari-alerts-definitions.png" alt-text="Ambari-Warnungen: Ansicht der Definitionen":::

Ambari bietet viele vordefinierte Warnungen in Bezug auf Verfügbarkeit, wie z.B.:

| Name der Warnung                        | BESCHREIBUNG   |
|---|---|
| DataNode Health Summary (DataNode-Integritätszusammenfassung)           | Diese Warnung auf Dienstebene wird ausgelöst, wenn es fehlerhafte DataNodes gibt.|
| NameNode High Availability Health (NameNode-Hochverfügbarkeitsintegrität) | Diese Warnung auf Dienstebene wird ausgelöst, wenn entweder der aktive NameNode oder der Standby-NameNode nicht ausgeführt wird.|
| Percent JournalNodes Available (Prozentsatz verfügbarer JournalNodes)    | Diese Warnung wird ausgelöst, wenn die Anzahl der ausgefallenen JournalNodes im Cluster höher ist als der konfigurierte kritische Schwellenwert. Sie aggregiert die Ergebnisse der JournalNode-Prozessüberprüfungen. |
| Percent DataNodes Available (Prozentsatz verfügbarer DataNodes)       | Diese Warnung wird ausgelöst, wenn die Anzahl der ausgefallenen DataNodes im Cluster höher ist als der konfigurierte kritische Schwellenwert. Sie aggregiert die Ergebnisse der DataNode-Prozessüberprüfungen.|


Um Details zu einer Warnung anzuzeigen oder Kriterien zu ändern, wählen Sie den **Namen** der Warnung aus. Nehmen Sie **DataNode Health Summary** (DataNode-Integritätszusammenfassung) als Beispiel. Sie sehen eine Beschreibung der Warnung sowie die spezifischen Kriterien, die eine „Warnung“ oder „kritische“ Warnung auslösen, und das Überprüfungsintervall für die Kriterien. Um die Konfiguration zu bearbeiten, wählen Sie rechts oben im Konfigurationsfeld die Schaltfläche **Edit** (Bearbeiten) aus.

:::image type="content" source="media/hdinsight-cluster-availability/ambari-alert-configuration.png" alt-text="Apache Ambari – Warnungskonfiguration":::

Hier können Sie die Beschreibung und vor allem das Überprüfungsintervall und die Schwellenwerte für Warnungen bzw. kritische Warnungen bearbeiten.

:::image type="content" source="media/hdinsight-cluster-availability/ambari-alert-configuration-edit.png" alt-text="Bearbeitungsansicht der Ambari-Warnungskonfigurationen":::

Bei diesem Beispiel könnten Sie veranlassen, dass zwei fehlerhafte DataNodes eine kritische Warnung auslösen und ein fehlerhafter DataNode nur eine Warnung auslöst. Wählen Sie **Save** (Speichern) aus, wenn Sie die Bearbeitung abgeschlossen haben.

## <a name="email-notifications"></a>E-Mail-Benachrichtigungen

Sie können optional auch E-Mail-Benachrichtigungen für Ambari-Warnungen konfigurieren. Klicken Sie dazu auf der Registerkarte **Alerts** (Warnungen) links oben auf die Schaltfläche **Actions** (Aktionen) und dann auf **Manage Notifications** (Benachrichtigungen verwalten).

:::image type="content" source="media/hdinsight-cluster-availability/ambari-manage-notifications.png" alt-text="Ambari: Verwalten der Benachrichtigungsaktion":::

Ein Dialogfeld für die Verwaltung von Benachrichtigungen zu Warnungen wird geöffnet. Wählen Sie unten im Dialogfeld **+** aus, und füllen Sie die Pflichtfelder aus, um in Ambari die Details zum E-Mail-Server anzugeben, von dem E-Mails gesendet werden sollen.

> [!TIP]
> Die Einrichtung von Ambari-E-Mail-Benachrichtigungen kann eine gute Möglichkeit sein, bei Verwaltung vieler HDInsight-Cluster Benachrichtigungen zentral zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- [Verfügbarkeit und Zuverlässigkeit von Apache Hadoop-Clustern in HDInsight](./hdinsight-business-continuity.md)
- [Clusterverfügbarkeit – Azure Monitor-Protokolle](./cluster-availability-monitor-logs.md)
- [Verwenden von Azure Monitor-Protokollen](hdinsight-hadoop-oms-log-analytics-tutorial.md)
- [Apache Ambari-E-Mail-Benachrichtigungen](apache-ambari-email.md)
