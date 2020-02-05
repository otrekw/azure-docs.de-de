---
title: Veraltete Apache Ambari-Warnungen in Azure HDInsight
description: Erörterung und Analyse möglicher Ursachen und Lösungen für veraltete Apache Ambari-Warnungen in Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/22/2020
ms.openlocfilehash: f19d499b5e50fbb5030a0f396296eed46fc6eee3
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722666"
---
# <a name="scenario-apache-ambari-stale-alerts-in-azure-hdinsight"></a>Szenario: Veraltete Apache Ambari-Warnungen in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Auf der Apache Ambari-Benutzeroberfläche wird möglicherweise eine Warnung angezeigt, die der folgenden Abbildung ähnelt:

![Beispiel für veraltete Apache Ambari-Warnung](./media/apache-ambari-troubleshoot-stale-alerts/ambari-stale-alerts-example.png)

## <a name="cause"></a>Ursache

Ambari-Agents führen kontinuierlich Integritätsprüfungen aus, um die Integrität vieler Ressourcen zu überwachen. Jede Warnung ist zur Ausführung in vordefinierten Zeitintervallen konfiguriert. Nach der Ausführung der einzelnen Warnungen melden Ambari-Agents den Status an den Ambari-Server zurück. Wenn der Ambari-Server dabei erkennt, dass eine der Warnungen nicht rechtzeitig ausgeführt wurde, löst er „Ambari Server Alerts“ (Ambari-Serverwarnungen) aus. Es gibt verschiedene Gründe, warum eine Integritätsprüfung möglicherweise nicht im festgelegten Intervall ausgeführt wird:

* Wenn Hosts stark ausgelastet sind (hohe CPU-Auslastung), besteht die Möglichkeit, dass dem Ambari-Agent nicht genügend Systemressourcen zum rechtzeitigen Ausführen der Warnungen zur Verfügung standen.

* Der Cluster ist während Zeiten mit hoher Auslastung mit der Ausführung vieler Aufträge/Dienste beschäftigt.

* Auf wenigen Hosts im Cluster werden möglicherweise viele Komponenten gehostet, sodass viele Warnungen ausgeführt werden müssen. Wenn eine große Anzahl von Komponenten vorhanden ist, kann es sein, dass Warnungsaufträge nicht in den geplanten Intervallen ausgeführt werden.

## <a name="resolution"></a>Lösung

### <a name="increase-alert-interval-time"></a>Erhöhen des Warnungsintervalls

Sie können den Wert eines einzelnen Warnungsintervalls basierend auf der Antwortzeit Ihres Clusters und seiner Auslastung erhöhen.

1. Wählen Sie auf der Apache Ambari-Benutzeroberfläche die Registerkarte **Alerts** (Warnungen) aus.
1. Wählen Sie den Namen der gewünschten Warnungsdefinition aus.
1. Wählen Sie in der Definition die Option **Edit** (Bearbeiten) aus.
1. Ändern Sie den Wert für **Check Interval** (Überprüfungsintervall) wie gewünscht, und wählen Sie dann **Save** (Speichern) aus.

### <a name="increase-alert-interval-time-for-ambari-server-alerts"></a>Erhöhen des Warnungsintervalls für „Ambari Server Alerts“ (Ambari-Serverwarnungen)

1. Wählen Sie auf der Apache Ambari-Benutzeroberfläche die Registerkarte **Alerts** (Warnungen) aus.
1. Wählen Sie in der Dropdownliste **Groups** (Guppen) die Option **AMBARI Default** (Ambari-Standard) aus.
1. Wählen Sie die Warnung **Ambari Server Alerts** (Ambari-Serverwarnungen) aus.
1. Wählen Sie in der Definition die Option **Edit** (Bearbeiten) aus.
1. Ändern Sie den Wert für **Check Interval** (Überprüfungsintervall) wie gewünscht.
1. Ändern Sie den Wert für **Interval Multiplier** (Intervallmultiplikator) wie gewünscht, und wählen Sie dann **Save** (Speichern) aus.

### <a name="disable-and-enable-the-alert"></a>Deaktivieren und Aktivieren der Warnung

Sie können die Warnung deaktivieren und dann wieder aktivieren, um veraltete Warnungen zu verwerfen.

1. Wählen Sie auf der Apache Ambari-Benutzeroberfläche die Registerkarte **Alerts** (Warnungen) aus.
1. Wählen Sie den Namen der gewünschten Warnungsdefinition aus.
1. Wählen Sie in der Definition ganz rechts die Option **Enabled** (Aktiviert) aus.
1. Wählen Sie im Popupfenster **Confirmation** (Bestätigung) die Option **Confirm Disable** (Deaktivierung bestätigen) aus.
1. Warten Sie einige Sekunden, bis alle auf der Seite angezeigten „Instanzen“ der Warnung gelöscht wurden.
1. Wählen Sie in der Definition ganz rechts die Option **Disabled** (Deaktiviert) aus.
1. Wählen Sie im Popupfenster **Confirmation** (Bestätigung) die Option **Confirm Enable** (Aktivierung bestätigen) aus.

### <a name="increase-alert-grace-time"></a>Erhöhen der Karenzzeit für die Warnung

Bevor der Ambari-Agent meldet, dass eine konfigurierte Warnung den Zeitplan nicht eingehalten hat, gibt es eine Karenzzeit. Wenn die Warnung den geplanten Zeitpunkt verpasst hat, aber innerhalb der Karenzzeit ausgelöst wurde, wird die veraltete Warnung nicht ausgelöst.

Der Standardwert für `alert_grace_period` beträgt 5 Sekunden. Diese `alert_grace_period`-Einstellung kann in `/etc/ambari-agent/conf/ambari-agent.ini` konfiguriert werden. Versuchen Sie bei den Hosts, von denen die veralteten Warnungen in regelmäßigen Intervallen ausgelöst werden, den Wert auf 10 zu erhöhen. Starten Sie den Ambari-Agent dann neu.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
