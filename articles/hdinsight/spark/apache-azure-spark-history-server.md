---
title: 'Azure HDInsight: Verwenden der erweiterten Features des Apache Spark-Verlaufsservers zum Debuggen von Apps'
description: 'Azure HDInsight: Verwenden der erweiterten Features des Apache Spark-Verlaufsservers zum Debuggen und Diagnostizieren von Spark-Anwendungen'
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/25/2019
ms.openlocfilehash: 1d53d9e9ee2a7fa6588ea0993b3bebdb2a287351
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98941039"
---
# <a name="use-the-extended-features-of-the-apache-spark-history-server-to-debug-and-diagnose-spark-applications"></a>Verwenden der erweiterten Features des Apache Spark-Verlaufsservers zum Debuggen und Diagnostizieren von Spark-Anwendungen

In diesem Artikel erfahren Sie, wie Sie die erweiterten Features des Apache Spark-Verlaufsservers verwenden, um abgeschlossene oder ausgeführte Spark-Anwendungen zu debuggen und zu diagnostizieren. Die Erweiterung umfasst folgende Registerkarten: **Daten**, **Graph** und **Diagnose**. Auf der Registerkarte **Daten** können Sie die Eingabe- und Ausgabedaten des Spark-Auftrags überprüfen. Auf der Registerkarte **Graph** können Sie den Datenfluss überprüfen und den Auftragsgraph wiedergeben. Auf der Registerkarte **Diagnose** finden Sie die Features **Datenschiefe**, **Zeitabweichung** und **Executor Usage Analysis** (Analyse zur Executorauslastung).

## <a name="get-access-to-the-spark-history-server"></a>Zugreifen auf den Spark-Verlaufsserver

Der Spark-Verlaufsserver ist die Webbenutzeroberfläche für abgeschlossene und ausgeführte Spark-Anwendungen. Sie können ihn entweder über das Azure-Portal oder über eine URL aufrufen.

### <a name="open-the-spark-history-server-web-ui-from-the-azure-portal"></a>Öffnen der Webbenutzeroberfläche des Spark-Verlaufsservers über das Azure-Portal

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) den Spark-Cluster. Weitere Informationen finden Sie unter [Auflisten und Anzeigen von Clustern](../hdinsight-administer-use-portal-linux.md#showClusters).
2. Wählen Sie unter **Cluster-Dashboards** die Option **Spark-Verlaufsserver** aus. Geben Sie bei Aufforderung die Anmeldeinformationen für den Spark-Cluster ein.

    ![Spark-Verlaufsserver über das Azure-Portal starten](./media/apache-azure-spark-history-server/azure-portal-dashboard-spark-history.png "Spark History Server")

### <a name="open-the-spark-history-server-web-ui-by-url"></a>Öffnen der Webbenutzeroberfläche des Spark-Verlaufsservers über eine URL

Öffnen Sie den Spark-Verlaufsserver, indem Sie zu `https://CLUSTERNAME.azurehdinsight.net/sparkhistory` navigieren. Dabei entspricht **CLUSTERNAME** dem Namen Ihres Spark-Clusters.

Die Webbenutzeroberfläche des Spark-Verlaufsservers ähnelt der folgenden Abbildung:

![Seite des Spark-Verlaufsservers](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)

## <a name="use-the-data-tab-in-the-spark-history-server"></a>Verwenden der Registerkarte „Daten“ des Spark-Verlaufsservers

Wählen Sie die Auftrags-ID aus, und klicken Sie dann im Toolmenü auf **Daten**, um die Datenansicht anzuzeigen.

+ Überprüfen Sie die **Eingaben**, **Ausgaben** und **Tabellenvorgänge**, indem Sie die jeweiligen Registerkarten auswählen.

    ![Datenregisterkarten auf der Seite „Data for Spark Application“ (Daten für Spark-Anwendung)](./media/apache-azure-spark-history-server/apache-spark-data-tabs.png)

+ Kopieren Sie alle Zeilen, indem Sie auf **Kopieren** klicken.

    ![Daten auf der Spark-Anwendungsseite kopieren](./media/apache-azure-spark-history-server/apache-spark-data-copy.png)

+ Speichern Sie alle Daten als eine CSV-Datei, indem Sie auf die **CSV**-Schaltfläche klicken.

    ![Daten der Seite „Data for Spark Application“ (Daten für Spark-Anwendung) als CSV-Datei speichern](./media/apache-azure-spark-history-server/apache-spark-data-save.png)

+ Durchsuchen Sie die Daten, indem Sie Schlüsselwörter in das Feld **Suche** eingeben. Die Suchergebnisse werden sofort angezeigt.

    ![Daten auf der Seite „Data for Spark Application“ (Daten für Spark-Anwendung) suchen](./media/apache-azure-spark-history-server/apache-spark-data-search.png)

+ Klicken Sie auf die Spaltenüberschrift, um die Tabelle zu sortieren. Klicken Sie auf das Pluszeichen, um eine Zeile zu erweitern und somit weitere Details anzuzeigen. Klicken Sie auf das Minuszeichen, um eine Zeile zu reduzieren.

    ![Datentabelle auf der Seite „Data for Spark Application“ (Daten für Spark-Anwendung)](./media/apache-azure-spark-history-server/apache-spark-data-table.png)

+ Laden Sie ein einzelne Datei herunter, indem Sie rechts auf **Unvollständiger Download** klicken. Die ausgewählte Datei wird lokal heruntergeladen. Wenn die Datei nicht mehr vorhanden ist, wird eine neue Registerkarte geöffnet, auf der die Fehlermeldungen angezeigt werden.

    ![Datendownloadzeile auf der Seite „Data for Spark Application“ (Daten für Spark-Anwendung)](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Kopieren Sie einen vollständigen oder relativen Pfad, indem Sie entweder **Vollständigen Pfad kopieren** oder **Relativen Pfad kopieren** im erweiterten Downloadmenü auswählen. Klicken Sie bei Azure Data Lake Storage-Dateien auf **In Azure Storage-Explorer öffnen**, um den Azure Storage-Explorer zu starte und den Ordner nach den Anmeldung aufzurufen.

    ![Die Optionen „Vollständigen Pfad kopieren“ und „Relativen Pfad kopieren“ auf der Seite „Data for Spark Application“ (Daten für Spark-Anwendung)](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Wenn so viele Zeilen vorhanden sind, dass sie nicht auf einer einzelnen Seite angezeigt werden können, klicken Sie zum Navigieren auf die Seitenzahlen unten in der Tabelle.

    ![Seitenzahlen auf der Seite „Data for Spark Application“ (Daten für Spark-Anwendung)](./media/apache-azure-spark-history-server/apache-spark-data-page.png)

+ Für weitere Informationen zeigen oder klicken Sie auf das Fragezeichen neben **Daten für Spark-Anwendung**, um die QuickInfo anzuzeigen.

    ![Weitere Informationen von der Seite „Data for Spark Application“ (Daten für Spark-Anwendung) abrufen](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+  Klicken Sie auf **Feedback senden**, um Feedback zu Problemen zu übermitteln.

    ![Feedback über die Seite „Data for Spark Application“ (Daten für Spark-Anwendung) übermitteln](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="use-the-graph-tab-in-the-spark-history-server"></a>Verwenden der Registerkarte „Graph“ des Spark-Verlaufsservers

+ Wählen Sie die Auftrags-ID aus, und klicken Sie dann im Toolmenü auf **Graph**, um den Auftragsgraph anzuzeigen. Der Graph zeigt standardmäßig alle Aufträge an. Filtern Sie die Ergebnisse mithilfe des Dropdownmenüs **Auftrags-ID**.

    ![Das Auftrags-ID-Dropdownmenü auf der Seite „Spark Application & Job Graph“ (Spark-Anwendung und Auftragsgraph)](./media/apache-azure-spark-history-server/apache-spark-graph-jobid.png)

+ Standardmäßig ist die Anzeigeoption **Fortschritt** ausgewählt. Überprüfen Sie den Datenfluss, indem Sie **Read** (Gelesen) oder **Written** (Geschrieben) im Dropdownmenü **Anzeigen** auswählen.

    ![Datenfluss auf der Seite „Spark Application & Job Graph“ (Spark-Anwendung und Auftragsgraph) überprüfen](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

+ Die Hintergrundfarbe eines jeden Auftrags entspricht einem Wärmebild.

   ![Wärmebild auf der Seite „Spark Application & Job Graph“ (Spark-Anwendung und Auftragsgraph)](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)


    |Color |BESCHREIBUNG |
    |---|---|
    |Grün|die abgeschlossen wurden.|
    |Orange|Die Aufgabe ist fehlgeschlagen. Dies wirkt sich jedoch nicht auf das Endergebnis des Auftrags aus. Für diese Aufgaben gibt es doppelte oder Wiederholungsinstanzen, die später erfolgreich durchgeführt werden können.|
    |Blau|die gerade ausgeführt werden.|
    |White|deren Ausführung noch ansteht oder bei denen eine Phase übersprungen wurde.|
    |Red|fehlgeschlagene Aufträge.|

     ![Ausführen einer Aufgabe auf der Seite „Spark Application & Job Graph“ (Spark-Anwendung und Auftragsgraph)](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)

     Übersprungene Phasen werden in weiß angezeigt.
    ![Eine übersprungene Aufgabe auf der Seite „Spark Application & Job Graph“ (Spark-Anwendung und Auftragsgraph)](./media/apache-azure-spark-history-server/sparkui-graph-color-skip.png)

    ![Eine fehlgeschlagene Aufgabe auf der Seite „Spark Application & Job Graph“ (Spark-Anwendung und Auftragsgraph)](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)

     > [!NOTE]  
     > Für abgeschlossene Aufträge ist die Wiedergabefunktion verfügbar. Klicken Sie auf die Schaltfläche **Wiedergabe**, um den Auftrag wiederzugeben. Sie können den Auftrag jederzeit mit der Stoppschaltfläche beenden. Bei der Wiedergabe eines Auftrags wird der Status der einzelnen Aufgaben durch die Farbe angegeben. Für unvollständige Aufträge wird die Wiedergabe nicht unterstützt.

+ Scrollen Sie, um in den Auftragsgraph hinein oder heraus zu zoomen, oder klicken Sie auf **Zoom anpassen**, um den Graph an die Anzeige anzupassen.

    ![Schaltfläche „Zoom anpassen“ auf der Seite „Spark Application & Job Graph“ (Spark-Anwendung und Auftragsgraph)](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Wenn Aufgaben fehlschlagen, können Sie auf den Graphknoten zeigen, um die QuickInfo anzuzeigen. Klicken Sie anschließend auf die Phase, um sie in einer neuen Seite zu öffnen.

    ![QuickInfo auf der Seite „Spark Application und Job Graph“ (Spark-Anwendung und Auftragsgraph) anzeigen](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ Die Phasen auf der Seite „Spark Application & Job Graph“ (Spark-Anwendung & Auftragsgraph) zeigen QuickInfos und kleine Symbole an, wenn die Aufgaben die folgenden Bedingungen erfüllen:
  + Datenschiefe: Größe der gelesenen Daten > durchschnittliche Größe der gelesenen Daten aller Aufträge innerhalb dieser Phase × 2 *und* Größe der gelesenen Daten > 10 MB
  + Zeitabweichung: Ausführungszeit > durchschnittliche Ausführungszeit aller Aufgaben in dieser Phase × 2 *und* Ausführungszeit > 2 Minuten

    ![Das Symbol für Aufgaben mit Abweichungen auf der Seite „Spark Application & Job Graph“ (Spark-Anwendung und Auftragsgraph)](./media/apache-azure-spark-history-server/sparkui-graph-skew-icon.png)

+ Der Auftragsgraphknoten zeigt die folgenden Informationen zu den einzelnen Phasen an:
  + id
  + Name oder Beschreibung
  + Anzahl der Aufgaben insgesamt
  + Gelesene Daten: die Summe der Eingabegröße und der Shuffle-Lesegröße
  + Geschriebene Daten: die Summe der Ausgabegröße und der Shuffle-Schreibgröße
  + Ausführungszeit: die Zeit zwischen der Startzeit des ersten Versuchs und der Endzeit des letzten Versuchs
  + Zeilenanzahl: die Summe der Eingabe- und Ausgabedatensätze sowie die Datensätze der Shuffle-Lesevorgänge und -Schreibvorgänge
  + Status

    > [!NOTE]  
    > Standardmäßig zeigt der Auftragsdiagrammknoten Informationen zum letzten Versuch der einzelnen Phasen an (mit Ausnahme der Phase „Ausführungszeit“). Während der Wiedergabe zeigt der Auftragsgraphknoten jedoch Informationen zu jedem Versuch an.

    > [!NOTE]  
    > Für die Datengrößen bei Lese- und Schreibvorgänge wird 1 MB = 1.000 KB = 1.000 × 1.000 Byte verwendet.

+ Senden Sie Ihr Feedback zu Problemen, indem Sie auf **Feedback senden** klicken.

    ![Die Feedbackoption auf der Seite „Spark Application & Job Graph“ (Spark-Anwendung und Auftragsgraph)](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="use-the-diagnosis-tab-in-the-spark-history-server"></a>Verwenden der Registerkarte „Diagnose“ des Spark-Verlaufsservers

Wählen Sie die Auftrags-ID und dann im Menü „Extras“ die Option **Diagnose** aus, um die Ansicht der Auftragsdiagnose aufzurufen. Die Registerkarte **Diagnose** umfasst die Registerkarten **Datenschiefe**, **Zeitabweichung** und **Executor Usage Analysis** (Analyse zur Executorauslastung).

+ Überprüfen Sie die **Datenschiefe**, **Zeitabweichung** und **Executor-Nutzungsanalyse**, indem Sie die entsprechenden Registerkarten auswählen.

    ![Die Registerkarte „Datenschiefe“ auf der Registerkarte „Diagnose“](./media/apache-azure-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Datenschiefe

Klicken Sie auf die Registerkarte **Datenschiefe**. Daraufhin werden die entsprechenden Aufgaben mit Abweichungen basierend auf den angegebenen Parametern angezeigt.

#### <a name="specify-parameters"></a>Angeben von Parametern

Im Abschnitt **Parameter festlegen** werden die Parameter angezeigt, die zum Ermitteln der Datenschiefe verwendet werden. Die Standardregel lautet wie folgt: Die gelesenen Aufgabendaten ist dreimal so groß wie die durchschnittlichen gelesenen Aufgabendaten, und die gelesenen Aufgabendaten sind größer als 10 MB. Wenn Sie Ihre eigene Regel für Aufgaben mit Abweichungen definieren möchten, können Sie die Parameter selbst festlegen. Die Abschnitte **Schiefe Phase** und **Skew Chart** (Abweichungsdiagramm) werden entsprechend aktualisiert.

#### <a name="skewed-stage"></a>Schiefe Phase

Im Abschnitt **Schiefe Phase** werden Phasen angezeigt, die Aufgaben mit Abweichungen enthalten, die die angegebenen Kriterien erfüllen. Wenn eine Phase mehrere Aufgaben mit Abweichungen enthält, zeigt der Abschnitt **Schiefe Phase** nur die Aufgabe mit der größten Abweichung an (d. h. die größten Daten für die Datenschiefe).

![Größere Ansicht der Registerkarte „Datenschiefe“ in der Registerkarte „Diagnose“](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

##### <a name="skew-chart"></a>Skew Chart (Abweichungsdiagramm)

Wenn Sie eine Zeile in der Tabelle **Schiefe Phase** auswählen, zeigt das **Abweichungsdiagramm** basierend auf den gelesenen Daten und der Ausführungszeit mehr Informationen zur Aufgabenverteilung an. Die Aufgaben mit Abweichungen sind rot und die normalen Aufgaben sind blau markiert. Aus Leistungsgründen werden im Diagramm bis zu 100 Beispielaufgaben angezeigt. Die Aufgabendetails werden im Bereich unten rechts angezeigt.

![Das Abweichungsdiagramm für Phase 10 in der Spark-Benutzeroberfläche](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Zeitabweichung

Auf der Registerkarte **Time Skew** (Zeitabweichung) werden schiefe Aufträge basierend auf ihrer Ausführungszeit angezeigt.

#### <a name="specify-parameters"></a>Angeben von Parametern

Im Abschnitt **Parameter festlegen** werden die Parameter angezeigt, die zum Ermitteln der Zeitabweichung verwendet werden. Die Standardregel lautet wie folgt: Die Aufgabenausführungszeit ist dreimal so lang wie die durchschnittliche Ausführungszeit, und die Aufgabenausführungszeit ist länger als 30 Sekunden. Sie können die Parameter Ihren Anforderungen entsprechend anpassen. Die **schiefe Phase** und das **Abweichungsdiagramm** zeigen die entsprechenden Phasen- und Aufgabeninformationen wie auf der Registerkarte **Datenschiefe** an.

Wenn Sie die **Zeitabweichung** auswählen, wird das gefilterte Ergebnis im Abschnitt **Schiefe Phase** entsprechend der Parameter angezeigt, die im Abschnitt **Parameter festlegen** festgelegt wurden. Wenn Sie ein Element im Abschnitt **Schiefe Phase** auswählen, wird das entsprechende Diagramm im dritten Abschnitt und die Aufgabendetails werden im Bereich unten rechts angezeigt.

![Registerkarte „Zeitabweichung“ auf der Registerkarte „Diagnose“](./media/apache-azure-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis-graphs"></a>Graph für die Analyse der Executorauslastung

Der **Graph für die Executorauslastung** zeigt die tatsächliche Executorzuteilung und den Ausführungsstatus des Auftrags an.  

Wenn Sie die **Analyse der Executorauslastung** aufrufen, werden vier verschiedene Funktionsgraphen zur Executorauslastung dargestellt: **Allocated Executors** (Zugewiesene Executors), **Running Executors** (Ausgeführte Executors), **Idle Executors** (Excutors im Leerlauf) und **Max Executor Instances** (Höchstanzahl der Executorinstanzen). Durch jedes Ereignis, bei dem ein **Executor hinzugefügt** oder ein **Executor entfernt** wird, wird die Anzahl der zugewiesenen Executors erhöht oder verringert. Weitere Vergleiche finden Sie im **Ereignisverlauf** auf der Registerkarte **Aufträge**.

![Die Registerkarte „Analyse zur Executorauslastung“ auf der Registerkarte „Diagnose“](./media/apache-azure-spark-history-server/sparkui-diagnosis-executors.png)

Wählen Sie das Farbsymbol aus, um den entsprechenden Inhalt in allen Entwürfen auszuwählen oder die Auswahl aufzuheben.

 ![Diagramm auf der Registerkarte „Analyse zur Executorauslastung“ auswählen](./media/apache-azure-spark-history-server/sparkui-diagnosis-select-chart.png)

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="how-do-i-revert-to-the-community-version"></a>Wie stelle ich die Communityversion wieder her?

Führen Sie die folgenden Schritte aus, um die Communityversion wiederherzustellen.

1. Öffnen Sie den Cluster in Ambari.
1. Navigieren Sie zu **Spark2** > **Configs**.
1. Wählen Sie **Custom Spark2-defaults** aus.
1. Klicken Sie auf **Add Property ...** (Eigenschaft hinzufügen).
1. Fügen Sie **spark.ui.enhancement.enabled=false** hinzu, und speichern Sie diese Änderung.
1. Die Eigenschaft ist jetzt auf **false** festgelegt.
1. Wählen Sie zum Speichern der Konfiguration **Speichern** aus.

    ![Feature in Apache Ambari deaktivieren](./media/apache-azure-spark-history-server/apache-spark-turn-off.png)

1. Klicken Sie im linken Bereich auf **Spark2**. Klicken Sie dann auf der Registerkarte **Summary** (Zusammenfassung) auf **Spark2 History Server** (Spark2-Verlaufsserver).

    ![Zusammenfassungsansicht in Apache Ambari](./media/apache-azure-spark-history-server/apache-spark-restart1.png)

1. Klicken Sie rechts neben **Spark2 History Server** (Spark2-Verlaufsserver) auf die Schaltfläche **Started** (Gestartet), und wählen Sie dann im Dropdownmenü die Option **Restart** (Neu starten) aus, um den Spark-Verlaufsserver neu zu starten.

    ![Spark-Verlaufsserver in Apache Ambari neu starten](./media/apache-azure-spark-history-server/apache-spark-restart2.png)  

1. Aktualisieren Sie die Webbenutzeroberfläche des Spark-Verlaufsservers. Dadurch wird die Communityversion wiederhergestellt.

### <a name="how-do-i-upload-a-spark-history-server-event-to-report-it-as-an-issue"></a>Wie lade ich ein Spark-Verlaufsserverereignis hoch, um es als Problem zu melden?

Führen Sie die folgenden Schritte zum Melden des Ereignisses durch, wenn ein Fehler beim Spark-Verlaufsserver auftritt:

1. Laden Sie das Ereignis herunter, indem Sie in der Webbenutzeroberfläche des Spark-Verlaufsservers auf **Download** klicken.

    ![Ereignis über die Benutzeroberfläche des Spark-Verlaufsservers herunterladen](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. Klicken Sie auf der Seite **Spark Application & Job Graph** (Spark-Anwendung und Auftragsgraph) auf **Feedback senden**.

    ![Feedback auf der Seite „Spark Application & Job Graph“ (Spark-Anwendung und Auftragsgraph) senden](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Geben Sie einen Titel und eine Beschreibung für den Fehler an. Ziehen Sie dann die ZIP-Datei auf das Feld „Bearbeiten“, und wählen Sie **Submit new issue** (Neues Problem melden) aus.

    ![Neues Problem hochladen und übermitteln](./media/apache-azure-spark-history-server/apache-spark-file-issue.png)

### <a name="how-do-i-upgrade-a-jar-file-in-a-hotfix-scenario"></a>Wie führe ich in einem Hotfixszenario ein Upgrade für eine JAR-Datei durch?

Wenn Sie ein Upgrade mit einem Hotfix durchführen möchten, verwenden Sie das folgende Skript, das ein Upgrade für `spark-enhancement.jar*` durchführt.

**upgrade_spark_enhancement.sh**:

   ```bash
    #!/usr/bin/env bash

    # Copyright (C) Microsoft Corporation. All rights reserved.

    # Arguments:
    # $1 Enhancement jar path

    if [ "$#" -ne 1 ]; then
        >&2 echo "Please provide the upgrade jar path."
        exit 1
    fi

    install_jar() {
        tmp_jar_path="/tmp/spark-enhancement-hotfix-$( date +%s )"

        if wget -O "$tmp_jar_path" "$2"; then
            for FILE in "$1"/spark-enhancement*.jar
            do
                back_up_path="$FILE.original.$( date +%s )"
                echo "Back up $FILE to $back_up_path"
                mv "$FILE" "$back_up_path"
                echo "Copy the hotfix jar file from $tmp_jar_path   to $FILE"
                cp "$tmp_jar_path" "$FILE"

                "Hotfix done."
                break
            done
        else    
            >&2 echo "Download jar file failed."
            exit 1
        fi
    }

    jars_folder="/usr/hdp/current/spark2-client/jars"
    jar_path=$1

    if ls ${jars_folder}/spark-enhancement*.jar 1>/dev/null 2>&1;   then
        install_jar "$jars_folder" "$jar_path"
    else
        >&2 echo "There is no target jar on this node. Exit with no action."
        exit 0
    fi
   ```

#### <a name="usage"></a>Verwendung

`upgrade_spark_enhancement.sh https://${jar_path}`

#### <a name="example"></a>Beispiel

`upgrade_spark_enhancement.sh https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`

#### <a name="use-the-bash-file-from-the-azure-portal"></a>Verwenden der Bash-Datei über das Azure-Portal

1. Starten Sie das [Azure-Portal](https://ms.portal.azure.com), und wählen Sie dann Ihren Cluster aus.
2. Führen Sie eine [Skriptaktion](../hdinsight-hadoop-customize-cluster-linux.md) mit den folgenden Parametern aus:

    |Eigenschaft |Wert |
    |---|---|
    |Skripttyp|--Benutzerdefiniert|
    |Name|UpgradeJar|
    |Bash-Skript-URI|`https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh`|
    |Knotentyp(en)|Hauptknoten, Workerknoten|
    |Parameter|`https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`|

     ![Azure-Portal – Aktion „Skript übermitteln“](./media/apache-azure-spark-history-server/apache-spark-upload1.png)

## <a name="known-issues"></a>Bekannte Probleme

+ Derzeit funktioniert der Spark-Verlaufsserver nur mit Spark 2.3 und 2.4.

+ Eingabe- und Ausgabedaten, die RDD nutzen, werden nicht auf der Registerkarte **Daten** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

+ [Verwalten von Ressourcen für den Apache Spark-Cluster unter HDInsight](apache-spark-resource-manager.md)
+ [Konfigurieren von Apache Spark-Einstellungen](apache-spark-settings.md)

## <a name="suggestions"></a>Vorschläge

Wenn Sie Feedback abgeben möchten oder bei der Verwendung des Tools Probleme auftreten, senden Sie eine E-Mail an [hdivstool@microsoft.com](mailto:hdivstool@microsoft.com).
