---
title: Problembehandlung für Apache Oozie in Azure HDInsight
description: Behandeln Sie Probleme für bestimmte Apache Oozie-Fehler in Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/27/2020
ms.openlocfilehash: 18831832f82cdbc8cec69e368f006f7acd4836c1
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204879"
---
# <a name="troubleshoot-apache-oozie-in-azure-hdinsight"></a>Problembehandlung für Apache Oozie in Azure HDInsight

Mithilfe der Apache Oozie-Benutzeroberfläche können Sie die Oozie-Protokolle anzeigen. Die Oozie-Benutzeroberfläche enthält darüber hinaus Links zu den JobTracker-Protokollen für die MapReduce-Aufgaben, die vom Workflow gestartet werden. Im Allgemeinen sollten Sie zur Problembehandlung wie folgt vorgehen:

1. Zeigen Sie den Auftrag auf der Oozie-Webbenutzeroberfläche an.

2. Wenn eine bestimmte Aktion nicht erfolgreich war, wählen Sie die Aktion aus, um festzustellen, ob das Feld **Fehlermeldung** weitere Informationen zum Fehler enthält.

3. Falls verfügbar, verwenden Sie die URL der Aktion, um weitere Details (z.B. JobTracker-Protokolle) für die Aktion anzuzeigen.

Im Folgenden sehen Sie Fehlermeldungen, die auftreten können, und Möglichkeiten zur Behebung.

## <a name="ja009-cant-initialize-cluster"></a>JA009: Cluster kann nicht initialisiert werden.

### <a name="issue"></a>Problem

Der Auftragsstatus ändert sich in **SUSPENDED**. In den Auftragsdetails wird der Status von `RunHiveScript` als **START_MANUAL** angezeigt. Bei Auswahl der Aktion wird die folgende Fehlermeldung angezeigt:

    JA009: Cannot initialize Cluster. Please check your configuration for map

### <a name="cause"></a>Ursache

Die in der Datei **job.xml** verwendeten Azure-Blobspeicheradressen enthalten nicht den Namen des Speichercontainers oder des Speicherkontos. Das Format der Blob-Speicheradresse muss `wasbs://containername@storageaccountname.blob.core.windows.net` sein.

### <a name="resolution"></a>Lösung

Ändern Sie die Blobspeicheradressen für den Auftrag.

---

## <a name="ja002-oozie-isnt-allowed-to-impersonate-ltusergt"></a>JA002: Oozie darf nicht die Identität von &lt;USER&gt; annehmen.

### <a name="issue"></a>Problem

Der Auftragsstatus ändert sich in **SUSPENDED**. In den Auftragsdetails wird der Status von `RunHiveScript` als **START_MANUAL** angezeigt. Wenn Sie die Aktion auswählen, wird die folgende Fehlermeldung angezeigt:

    JA002: User: oozie is not allowed to impersonate <USER>

### <a name="cause"></a>Ursache

Die aktuellen Berechtigungseinstellungen lassen nicht zu, dass Oozie die Identität des angegebenen Benutzerkontos annimmt.

### <a name="resolution"></a>Lösung

Oozie kann die Identität von Benutzern in der Gruppe **`users`** annehmen. Verwenden Sie `groups USERNAME` , um die Gruppen anzuzeigen, denen das Benutzerkonto als Mitglied angehört. Wenn der Benutzer nicht Mitglied der Gruppe **`users`** ist, verwenden Sie den folgenden Befehl, um den Benutzer der Gruppe hinzuzufügen:

    sudo adduser USERNAME users

> [!NOTE]  
> Es kann einige Minuten dauern, bis HDInsight erkennt, dass der Benutzer der Gruppe hinzugefügt wurde.

---

## <a name="launcher-error-sqoop"></a>Launcher ERROR (Sqoop)

### <a name="issue"></a>Problem

Der Auftragsstatus ändert sich in **KILLED**. In den Auftragsdetails wird der Status von `RunSqoopExport` als **ERROR** (Fehler) angezeigt. Wenn Sie die Aktion auswählen, wird die folgende Fehlermeldung angezeigt:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

### <a name="cause"></a>Ursache

Sqoop kann den Datenbanktreiber nicht laden, der für den Zugriff auf die Datenbank erforderlich ist.

### <a name="resolution"></a>Lösung

Bei Verwendung von Sqoop in einem Oozie-Auftrag müssen Sie den Datenbanktreiber zusammen mit den anderen vom Auftrag verwendeten Ressourcen (beispielsweise die Datei „workflow.xml“) angeben. Verweisen Sie im Abschnitt `<sqoop>...</sqoop>` von „workflow.xml“ auf das Archiv mit dem Datenbanktreiber.

Für das Auftragsbeispiel aus [Verwenden von Hadoop Oozie-Workflows](hdinsight-use-oozie-linux-mac.md) würden Sie z. B. die folgenden Schritte verwenden:

1. Kopieren Sie die Datei `mssql-jdbc-7.0.0.jre8.jar` in das Verzeichnis **/tutorials/useoozie**:

    ```bash
    hdfs dfs -put /usr/share/java/sqljdbc_7.0/enu/mssql-jdbc-7.0.0.jre8.jar /tutorials/useoozie/mssql-jdbc-7.0.0.jre8.jar
    ```

2. Ändern Sie die Datei `workflow.xml`, indem Sie den folgenden XML-Code einer neuen Zeile oberhalb von `</sqoop>` hinzufügen:

    ```xml
    <archive>mssql-jdbc-7.0.0.jre8.jar</archive>
    ```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
