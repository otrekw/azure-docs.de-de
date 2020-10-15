---
title: Apache Hive-Protokolle füllen den gesamten Speicherplatz – Azure HDInsight
description: Die Apache Hive-Protokolle füllen den gesamten Speicherplatz auf den Hauptknoten in Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
author: nisgoel
ms.author: nisgoel
ms.reviewer: jasonh
ms.date: 03/05/2020
ms.openlocfilehash: d843b942702d335065a5f3798572e34c71b4cd0e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "78943965"
---
# <a name="scenario-apache-hive-logs-are-filling-up-the-disk-space-on-the-head-nodes-in-azure-hdinsight"></a>Szenario: Apache Hive-Protokolle füllen den gesamten Speicherplatz auf den Hauptknoten in Azure HDInsight

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme durch nicht ausreichenden Speicherplatz auf den Hauptknoten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

In einem Apache Hive/LLAP-Cluster nehmen unerwünschte Protokolle den gesamten Speicherplatz auf den Hauptknoten ein. Aus diesem Grund können die folgenden Probleme auftreten.

1. Beim SSH-Zugriff tritt ein Fehler auf, weil auf dem Hauptknoten kein Speicherplatz mehr vorhanden ist.
2. Ambari gibt die Fehlermeldung *HTTP-FEHLER 503: Dienst nicht verfügbar* aus.

Die `ambari-agent`-Protokolle zeigen bei Auftreten des Problems Folgendes an.
```
ambari_agent - Controller.py - [54697] - Controller - ERROR - Error:[Errno 28] No space left on device
```
```
ambari_agent - HostCheckReportFileHandler.py - [54697] - ambari_agent.HostCheckReportFileHandler - ERROR - Can't write host check file at /var/lib/ambari-agent/data/hostcheck.result
```

## <a name="cause"></a>Ursache

In erweiterten Hive-log4j-Konfigurationen wird der Parameter *log4j.appender.RFA.MaxBackupIndex* ausgelassen. Dies bewirkt ein endloses Generieren von Protokolldateien.

## <a name="resolution"></a>Lösung

1. Navigieren Sie im Ambari-Portal zur Hive-Komponentenzusammenfassung, und klicken Sie auf die Registerkarte `Configs`.

2. Wechseln Sie in den erweiterten Einstellungen zum Abschnitt `Advanced hive-log4j`.

3. Legen Sie den Parameter `log4j.appender.RFA` als „RollingFileAppender“ fest. 

4. Legen Sie `log4j.appender.RFA.MaxFileSize` und `log4j.appender.RFA.MaxBackupIndex` wie folgt fest.

```
log4jhive.log.maxfilesize=1024MB
log4jhive.log.maxbackupindex=10

log4j.appender.RFA=org.apache.log4j.RollingFileAppender
log4j.appender.RFA.File=${hive.log.dir}/${hive.log.file}
log4j.appender.RFA.MaxFileSize=${log4jhive.log.maxfilesize}
log4j.appender.RFA.MaxBackupIndex=${log4jhive.log.maxbackupindex}
log4j.appender.RFA.layout=org.apache.log4j.PatternLayout
log4j.appender.RFA.layout.ConversionPattern=%d{ISO8601} %-5p [%t] %c{2}: %m%n
```
5. Legen Sie `hive.root.logger` wie folgt auf `INFO,RFA` fest. Die Standardeinstellung ist „DEBUG“, wodurch Protokolle sehr groß werden.

```
# Define some default values that can be overridden by system properties
hive.log.threshold=ALL
hive.root.logger=INFO,RFA
hive.log.dir=${java.io.tmpdir}/${user.name}
hive.log.file=hive.log
```

6. Speichern Sie die Konfigurationen, und starten Sie die erforderlichen Komponenten neu.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
