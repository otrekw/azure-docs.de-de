---
title: 'Problembehandlung: Hive-Protokolle belegen den gesamten Speicherplatz - Azure HDInsight'
description: In diesem Artikel finden Sie Schritte zur Problembehandlung, wenn Apache Hive-Protokolle den gesamten Speicherplatz auf den Hauptknoten in Azure HDInsight belegen.
ms.service: hdinsight
ms.topic: troubleshooting
author: kcheeeung
ms.author: kecheung
ms.date: 05/21/2021
ms.openlocfilehash: f6e2deac6c5f5807618225f4afc208e091e7e004
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110578687"
---
# <a name="scenario-apache-hive-logs-are-filling-up-the-disk-space-on-the-head-nodes-in-azure-hdinsight"></a>Szenario: Apache Hive-Protokolle belegen den gesamten Speicherplatz auf den Hauptknoten in Azure HDInsight

In diesem Artikel werden die Problembehandlungsschritte sowie mögliche Problemlösungen bei nicht ausreichendem Speicherplatz auf den Hauptknoten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

In einem HDI 4.0 Apache Hive/LLAP-Cluster nehmen unerwünschte Protokolle den gesamten Speicherplatz auf den Hauptknoten ein. Diese Bedingung kann folgende Probleme verursachen:

- Beim SSH-Zugriff tritt ein Fehler auf, weil auf dem Hauptknoten kein Speicherplatz mehr vorhanden ist.
- Fehler beim Neustart von HiveServer2 Interactive.

## <a name="cause"></a>Ursache

Das automatische Löschen von Hive-Protokollen ist in den erweiterten hive-log4j2-Konfigurationen nicht konfiguriert. Die Standardgrößenbeschränkung von 60 GB benötigt zu viel Speicherplatz für das Nutzungsmuster des Kunden.

## <a name="resolution"></a>Lösung

1. Navigieren Sie im Ambari-Portal zur Hive-Komponentenübersicht, und wählen Sie die Registerkarte **Konfigurationen** aus.

2. Wechseln Sie unter **Erweiterte Einstellungen** zum Abschnitt `Advanced hive-log4j2`.

3. Stellen Sie sicher, dass Sie über diese Einstellungen verfügen. Wenn keine zugehörigen Einstellungen angezeigt werden, fügen Sie die folgenden Einstellungen an:
    ```
    # automatically delete hive log
    appender.RFA.strategy.action.type = Delete
    appender.RFA.strategy.action.basePath = ${sys:hive.log.dir}
    appender.RFA.strategy.action.condition.type = IfFileName
    appender.RFA.strategy.action.condition.regex = hive*.*log.*
    appender.RFA.strategy.action.condition.nested_condition.type = IfAny
    # Deletes logs based on total accumulated size, keeping the most recent
    appender.RFA.strategy.action.condition.nested_condition.fileSize.type = IfAccumulatedFileSize
    appender.RFA.strategy.action.condition.nested_condition.fileSize.exceeds = 60GB
    # Deletes logs IfLastModified date is greater than number of days
    #appender.RFA.strategy.action.condition.nested_condition.lastMod.type = IfLastModified
    #appender.RFA.strategy.action.condition.nested_condition.lastMod.age = 30D
    ```

4. Wir werden drei grundlegende Optionen mit Löschvorgängen basierend auf folgenden Optionen durchgehen:
- **Gesamtgröße**
    - Ändern Sie `appender.RFA.strategy.action.condition.nested_condition.fileSize.exceeds` in eine Größenbeschränkung Ihrer Wahl.

- **Datum**
    - Sie können auch die Auskommentierung aufheben und die Bedingungen wechseln. Ändern Sie dann `appender.RFA.strategy.action.condition.nested_condition.lastMod.age` in ein Alter Ihrer Wahl.

    ```
    # Deletes logs based on total accumulated size, keeping the most recent 
    #appender.RFA.strategy.action.condition.nested_condition.fileSize.type = IfAccumulatedFileSize 
    #appender.RFA.strategy.action.condition.nested_condition.fileSize.exceeds = 60GB
    # Deletes logs IfLastModified date is greater than number of days 
    appender.RFA.strategy.action.condition.nested_condition.lastMod.type = IfLastModified 
    appender.RFA.strategy.action.condition.nested_condition.lastMod.age = 30D
    ```

- **Kombination aus Gesamtgröße und Datum**
    - Sie können beide Optionen kombinieren, indem Sie die Auskommentierung wie unten gezeigt aufheben. Log4j2 verhält sich dann so: Beginnen Sie mit dem Löschen von Protokollen, wenn eine der Bedingungen erfüllt ist.
    
    ```
    # Deletes logs based on total accumulated size, keeping the most recent 
    appender.RFA.strategy.action.condition.nested_condition.fileSize.type = IfAccumulatedFileSize 
    appender.RFA.strategy.action.condition.nested_condition.fileSize.exceeds = 60GB
    # Deletes logs IfLastModified date is greater than number of days 
    appender.RFA.strategy.action.condition.nested_condition.lastMod.type = IfLastModified 
    appender.RFA.strategy.action.condition.nested_condition.lastMod.age = 30D
    ```
5. Speichern Sie die Konfigurationen, und starten Sie die erforderlichen Komponenten neu.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]
