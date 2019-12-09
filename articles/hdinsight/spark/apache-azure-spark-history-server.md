---
title: Erweiterter Spark-Verlaufsservers zum Debuggen von Apps – Azure HDInsight
description: Verwenden des erweiterten Spark-Verlaufsservers zum Debuggen und Diagnostizieren von Spark-Anwendungen – Azure HDInsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/25/2019
ms.openlocfilehash: 7e9ab0e41086a4c9478f95c5a56754640feeab4e
ms.sourcegitcommit: c31dbf646682c0f9d731f8df8cfd43d36a041f85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2019
ms.locfileid: "74561829"
---
# <a name="use-extended-apache-spark-history-server-to-debug-and-diagnose-apache-spark-applications"></a>Verwenden des erweiterten Apache Spark-Verlaufsservers zum Debuggen und Diagnostizieren von Apache Spark-Anwendungen

Dieser Artikel enthält Anleitungen zur Verwendung des erweiterten Apache Spark-Verlaufsservers zum Debuggen und Diagnostizieren abgeschlossener und aktiver Spark-Anwendungen. Die Erweiterung enthält die Registerkarten „Daten“, „Diagramm“ und „Diagnose“. Auf der Registerkarte **Daten** können Benutzer die Eingabe- und Ausgabedaten des Spark-Auftrags überprüfen. Auf der Registerkarte **Diagramm** können Benutzer den Datenfluss überprüfen und das Auftragsdiagramm wiedergeben. Auf der Registerkarte **Diagnose** finden Benutzer die Registerkarten **Datenschiefe**, **Zeitabweichung** und **Executor Usage Analysis** (Executor-Nutzungsanalyse).

## <a name="get-access-to-apache-spark-history-server"></a>Zugreifen auf den Apache Spark-Verlaufsserver

Der Apache Spark-Verlaufsserver ist die Webbenutzeroberfläche für abgeschlossene und ausgeführte Spark-Anwendungen.

### <a name="open-the-apache-spark-history-server-web-ui-from-azure-portal"></a>Öffnen der Webbenutzeroberfläche des Apache Spark-Verlaufsservers über das Azure-Portal

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) den Spark-Cluster. Weitere Informationen finden Sie unter [Auflisten und Anzeigen von Clustern](../hdinsight-administer-use-portal-linux.md#showClusters).
2. Wählen Sie unter **Cluster-Dashboards** die Option **Spark-Verlaufsserver** aus. Geben Sie bei Aufforderung die Anmeldeinformationen für den Spark-Cluster ein.

    ![Portal – Starten des Spark-Verlaufsservers](./media/apache-azure-spark-history-server/azure-portal-dashboard-spark-history.png "Spark-Verlaufsserver")

### <a name="open-the-spark-history-server-web-ui-by-url"></a>Öffnen der Webbenutzeroberfläche des Spark-Verlaufsservers mit einer URL

Öffnen Sie den Spark-Verlaufsserver, indem Sie zu `https://CLUSTERNAME.azurehdinsight.net/sparkhistory` navigieren. Dabei ist CLUSTERNAME der Name Ihres Spark-Clusters.

Die Webbenutzeroberfläche des Spark-Verlaufsservers ähnelt der folgenden Ansicht:

![HDInsight Spark-Verlaufsserver](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)

## <a name="data-tab-in-spark-history-server"></a>Registerkarte „Daten“ im Spark-Verlaufsserver

Wählen Sie die Auftrags-ID und dann im Menü „Extras“ die Option **Daten** aus, um die Datenansicht aufzurufen.

+ Überprüfen Sie die **Eingaben**, **Ausgaben** und **Tabellenvorgänge**, indem Sie diese Registerkarten getrennt auswählen.

    ![Daten für Spark-Anwendung – Registerkarten](./media/apache-azure-spark-history-server/apache-spark-data-tabs.png)

+ Kopieren Sie alle Zeilen, indem Sie die Schaltfläche **Kopieren** auswählen.

    ![Daten für Spark-Anwendung – Kopieren](./media/apache-azure-spark-history-server/apache-spark-data-copy.png)

+ Speichern Sie alle Daten als CSV-Datei, indem Sie die Schaltfläche **CSV** auswählen.

    ![Daten für Spark-Anwendung – Speichern](./media/apache-azure-spark-history-server/apache-spark-data-save.png)

+ Suchen Sie durch Eingeben von Schlüsselwörtern in das Feld **Suche**. Das Suchergebnis wird sofort angezeigt.

    ![Daten für Spark-Anwendung – Suchen](./media/apache-azure-spark-history-server/apache-spark-data-search.png)

+ Wählen Sie die Spaltenüberschrift aus, um die Tabelle zu sortieren, wählen Sie das Pluszeichen aus, um eine Zeile zu erweitern und weitere Details anzuzeigen, oder wählen Sie das Minuszeichen aus, um eine Zeile zu reduzieren.

    ![Daten für Spark-Anwendung – Tabelle](./media/apache-azure-spark-history-server/apache-spark-data-table.png)

+ Laden Sie eine einzelne Datei herunter, indem Sie rechts die Schaltfläche **Unvollständiger Download** auswählen. Die ausgewählte Datei wird dann lokal heruntergeladen. Wenn die Datei nicht mehr vorhanden ist, wird eine neue Registerkarte geöffnet, um die Fehlermeldungen anzuzeigen.

    ![Daten für Spark-Anwendung – Zeile herunterladen](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Kopieren Sie den vollständigen oder relativen Pfad durch Auswählen von **Vollständigen Pfad kopieren** oder **Relativen Pfad kopieren** aus dem Downloadmenü. Für Azure Data Lake Storage-Dateien wird mit **Im Azure Storage-Explorer öffnen** der Azure Storage-Explorer gestartet und der Ordner beim Anmelden aufgerufen.

    ![Daten für Spark-Anwendung – Pfad kopieren](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Wählen Sie die Zahlen unter der Tabelle aus, um durch Seiten zu navigieren, wenn zu viele Zeilen für die Anzeige auf einer Seite vorhanden sind.

    ![Daten für Spark-Anwendung – Seite](./media/apache-azure-spark-history-server/apache-spark-data-page.png)

+ Zeigen Sie auf das Fragezeichen neben den Daten, um die QuickInfo anzuzeigen, oder wählen Sie das Fragezeichen aus, um weitere Informationen zu erhalten.

    ![Daten für Spark-Anwendung – Weitere Informationen](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+ Senden Sie Feedback mit Problemen, indem Sie auf **Feedback abgeben** klicken.

    ![Spark-Diagramm – Feedback abgeben, Forts.](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="graph-tab-in-apache-spark-history-server"></a>Registerkarte „Graph“ im Apache Spark-Verlaufsserver

Wählen Sie die Auftrags-ID aus, und klicken Sie im Toolmenü auf **Diagramm**, um die Ansicht mit dem Auftragsdiagramm aufzurufen.

+ Überprüfen Sie die Übersicht über den Auftrag anhand des generierten Auftragsdiagramms.

+ Standardmäßig werden alle Aufträge angezeigt. Die Ansicht kann nach **Auftrags-ID** gefiltert werden.

    ![Spark-Anwendung und Auftragsdiagramm – Auftrags-ID](./media/apache-azure-spark-history-server/apache-spark-graph-jobid.png)

+ Standardmäßig ist **Fortschritt** ausgewählt. Der Benutzer kann den Datenfluss überprüfen, indem er **Gelesen/Geschrieben** in der Dropdownliste **Anzeigen** auswählt.

    ![Spark-Anwendung und Auftragsdiagramm – Anzeigen](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

    Die Diagrammknoten werden in Farben angezeigt, die ein Wärmebild darstellen.

    ![Spark-Anwendung und Auftragsdiagramm – Wärmebild](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)

+ Geben Sie den Auftrag wieder, indem Sie die Schaltfläche **Wiedergabe** auswählen. Sie können die Wiedergabe jederzeit beenden, indem Sie die Schaltfläche „Beenden“ auswählen. Die Aufgaben werden in Farbe angezeigt, um verschiedene Status bei der Wiedergabe darzustellen:

    |Farbe |BESCHREIBUNG |
    |---|---|
    |Grün|Der Auftrag wurde erfolgreich abgeschlossen.|
    |Orange|Instanzen fehlerhafter Aufgaben, die jedoch keine Auswirkungen auf das Endergebnis des Auftrags haben. Für diese Aufgaben gibt es doppelte oder Wiederholungsinstanzen, die später möglicherweise erfolgreich sind.|
    |Blau|Die Aufgabe wird ausgeführt.|
    |Weiß|Die Aufgabe wartet auf die Ausführung, oder die Phase wurde übersprungen.|
    |Rot|Die Aufgabe ist fehlgeschlagen.|

    ![Spark-Anwendung und Auftragsdiagramm – Farbbeispiel: Ausführung](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)

    Die übersprungene Phase wird in weiß angezeigt.
    ![Spark-Anwendung und Auftragsdiagramm – Farbbeispiel: Übersprungen](./media/apache-azure-spark-history-server/sparkui-graph-color-skip.png)

    ![Spark-Anwendung und Auftragsdiagramm – Farbbeispiel: Fehler](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)

    > [!NOTE]  
    > Die Wiedergabe ist für jeden Auftrag zulässig. Für einen unvollständigen Auftrag wird die Wiedergabe nicht unterstützt.

+ Vergrößern/verkleinern Sie das Auftragsdiagramm mit dem Mausrad, oder klicken Sie auf **Mit Zoom anpassen**, um es an die Bildschirmgröße anzupassen.

    ![Spark-Anwendung und Auftragsdiagramm – Mit Zoom Anpassen](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Zeigen Sie auf den Diagrammknoten, um die QuickInfo anzuzeigen, wenn es fehlgeschlagene Aufgaben gibt. Klicken Sie auf die Phase, um die Phasenseite zu öffnen.

    ![Spark-Anwendung und Auftragsdiagramm – QuickInfo](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ Auf der Registerkarte „Auftragsgraph“ wird für die Phasen eine QuickInfo und ein kleines Symbol angezeigt, wenn sie Aufgaben enthalten, die den folgenden Bedingungen entsprechen:
  + Datenschiefe: Größe der gelesenen Daten > Durchschnittliche Größe der gelesenen Daten aller Aufgaben innerhalb dieser Phase x 2 und Größe der gelesenen Daten > 10 MB.
  + Zeitabweichung: Ausführungszeit > durchschnittliche Ausführungszeit aller Aufgaben innerhalb dieser Phase x 2 und Ausführungszeit > 2 Minuten.

    ![Spark-Anwendung und Auftragsdiagramm – Symbol für Schiefe/Abweichung](./media/apache-azure-spark-history-server/sparkui-graph-skew-icon.png)

+ Der Diagrammknoten des Auftrags zeigt die folgenden Informationen der einzelnen Phasen an:
  + ID
  + Name oder Beschreibung
  + Gesamtanzahl der Aufgaben
  + Gelesene Daten: die Summe der Eingabegröße und der Shuffle-Lesegröße
  + Geschriebene Daten: die Summe der Ausgabegröße und der Shuffle-Schreibgröße
  + Ausführungszeit: die Zeit zwischen der Startzeit des ersten Versuchs und Abschlusszeit des letzten Versuchs
  + Zeilenanzahl: die Summe der Eingabedatensätze, Ausgabedatensätze, Shuffle-Lesedatensätze und Shuffle-Schreibdatensätze
  + Fortschritt

    > [!NOTE]  
    > Standardmäßig zeigt der Diagrammknoten des Auftrags Informationen zum letzten Versuch der einzelnen Phasen an (mit Ausnahme der Zeit für die Phasenausführung), aber während der Wiedergabe zeigt der Diagrammknoten Informationen zu jedem Versuch an.

    > [!NOTE]  
    > Für die Datengröße von Lese- und Schreibvorgängen verwenden wir 1 MB = 1000 KB = 1000 * 1000 Bytes.

+ Senden Sie Feedback mit Problemen, indem Sie **Provide us feedback** (Feedback abgeben) auswählen.

    ![Spark-Anwendung und Auftragsdiagramm – Feedback](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="diagnosis-tab-in-apache-spark-history-server"></a>Registerkarte „Diagnose“ im Apache Spark-Verlaufsserver

Wählen Sie die Auftrags-ID und dann im Menü „Extras“ die Option **Diagnose** aus, um die Ansicht der Auftragsdiagnose aufzurufen. Die Registerkarte „Diagnose“ enthält die Registerkarten **Datenschiefe**, **Zeitabweichung** und **Executor-Nutzungsanalyse**.

+ Überprüfen Sie die **Datenschiefe**, **Zeitabweichung** und **Executor-Nutzungsanalyse**, indem Sie die entsprechenden Registerkarten auswählen.

    ![SparkUI-Diagnoseregisterkarte „Datenschiefe“, Forts.](./media/apache-azure-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Datenschiefe

Wählen Sie die Registerkarte **Datenschiefe** aus, um die entsprechenden Aufgaben mit Abweichungen basierend auf den angegebenen Parametern anzuzeigen.

+ **Parameter festlegen:** Im ersten Abschnitt werden die Parameter angezeigt, die zum Erkennen von Datenschiefe verwendet werden. Die integrierte Regel lautet: Task-Datenlesevorgänge > 3 durchschnittliche Task-Datenlesevorgänge und Task-Datenlesevorgänge > 10 MB. Wenn Sie eine eigene Regel für Aufgaben mit Abweichungen definieren möchten, können Sie Ihre Parameter auswählen, und die Abschnitte **Schiefe Phase** und **Abweichungsdiagramm Char** werden entsprechend aktualisiert.

+ **Schiefe Phase:** Im zweiten Abschnitt werden die Phasen angezeigt, die Aufgaben mit Abweichungen entsprechend den oben angegebenen Kriterien enthalten. Wenn in einer Phase mehrere Aufgaben mit Abweichungen vorhanden sind, wird in der Tabelle der schiefen Phasen nur die Aufgabe mit der größten Abweichung angezeigt (z. B. mit dem größten Wert der Datenschiefe).

    ![SparkUI-Diagnoseregisterkarte „Datenschiefe“](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

+ **Abweichungsdiagramm**: Wenn eine Zeile in der Tabelle „Schiefe Phase“ ausgewählt ist, zeigt das Abweichungsdiagramm weitere Details zur Aufgabenverteilung basierend auf den gelesenen Daten und der Ausführungszeit an. Die Aufgaben mit Abweichungen werden in rot markiert, und die normalen Aufgaben in blau. Aus Performancegründen zeigt das Diagramm nur bis zu 100 Beispielaufgaben an. Die Aufgabendetails werden im unteren rechten Bereich angezeigt.

    ![SparkUI – Datenschiefediagramm für Phase 10](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Zeitabweichung

Auf der Registerkarte **Zeitabweichung** werden Aufgaben mit Abweichungen basierend auf der Ausführungszeit der Aufgabe angezeigt.

+ **Parameter festlegen:** Im ersten Abschnitt werden die Parameter angezeigt, die zum Erkennen der Zeitabweichung verwendet werden. Die Standardkriterien zum Erkennen von Zeitabweichungen sind: Ausführungszeit der Aufgabe > 3-mal durchschnittliche Ausführungszeit und Ausführungszeit der Aufgabe > 30 Sekunden. Sie können die Parameter basierend auf Ihren Anforderungen ändern. Unter **Schiefe Phase** und **Abweichungsdiagramm** werden, genau wie auf der Registerkarte **Datenschiefe** oben, die entsprechenden Phasen und Aufgabeninformationen angezeigt.

+ Wenn Sie **Zeitabweichung** auswählen, wird das gefilterte Ergebnis im Abschnitt **Schiefe Phase** entsprechend den im Abschnitt **Parameter festlegen** angegebenen Parametern angezeigt. Wenn Sie ein Element im Abschnitt **Schiefe Phase** auswählen, wird das entsprechende Diagramm in Abschnitt 3 entworfen, und die Aufgabendetails werden im rechten unteren Bereich angezeigt.

    ![SparkUI-Diagnose – Abschnitt „Zeitabweichung“](./media/apache-azure-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis"></a>Executor-Nutzungsanalyse

Das Executor-Nutzungsdiagramm visualisiert die tatsächliche Executor-Zuordnung und den Ausführungsstatus des Spark-Auftrags.  

+ Wenn Sie **Executor Usage Analysis** (Executor-Nutzungsanalyse) auswählen, werden vier Kurventypen zur Executor-Nutzung entworfen: die **zugewiesenen Executors**, die **ausgeführten Executors**, die **Executors im Leerlauf** und die **Max. Executor-Instanzen**. Was die zugewiesenen Executors betrifft, so erhöht bzw. verringert jedes Ereignis "Executor hinzugefügt" oder "Executor entfernt" die Anzahl der zugewiesenen Executors. Weitere Vergleiche finden Sie unter "Ereigniszeitachse" auf der Registerkarte "Aufträge".

    ![SparkUI-Diagnose – Registerkarte „Executors“](./media/apache-azure-spark-history-server/sparkui-diagnosis-executors.png)

+ Wählen Sie das Farbsymbol aus, um den entsprechenden Inhalt in allen Entwürfen auszuwählen oder die Auswahl aufzuheben.

    ![SparkUI-Diagnose – Auswählen von Diagramminhalten](./media/apache-azure-spark-history-server/sparkui-diagnosis-select-chart.png)

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="1-revert-to-community-version"></a>1. Wiederherstellen der Communityversion

Um die Communityversion wiederherzustellen, führen Sie die folgenden Schritte aus:

1. Öffnen Sie den Cluster in Ambari.
1. Navigieren Sie zu **Spark2** > **Configs** > **Custom spark2-defaults**.
1. Wählen Sie **Add Property** (Eigenschaft hinzufügen) aus, fügen Sie **spark.ui.enhancement.enabled=false** hinzu, und speichern Sie.
1. Die Eigenschaft ist jetzt auf **false** festgelegt.
1. Wählen Sie zum Speichern der Konfiguration **Speichern** aus.

    ![Die Apache Ambari-Funktion wird deaktiviert.](./media/apache-azure-spark-history-server/apache-spark-turn-off.png)

1. Wählen Sie im linken Bereich **Spark2** und auf der Registerkarte **Summary** (Zusammenfassung) die Option **Spark2 History Server** aus.

    ![Apache Ambari Spark2 – Zusammenfassung](./media/apache-azure-spark-history-server/apache-spark-restart1.png)

1. Starten Sie den Verlaufsserver neu, indem Sie für **Spark2 History Server** die Option **Restart** (Neu starten) auswählen.

    ![Apache Ambari Spark2-Verlaufsserver – Neu starten](./media/apache-azure-spark-history-server/apache-spark-restart2.png)  
1. Durch Aktualisieren der Webbenutzeroberfläche des Spark-Verlaufsservers wird die Communityversion wiederhergestellt.

### <a name="2-upload-history-server-event"></a>2. Hochladen von Verlaufsserverereignissen

Führen Sie bei einem Verlaufsserverfehler die Schritte zum Angeben des Ereignisses aus:

1. Laden Sie das Ereignis durch Auswählen von **Download** in der Webbenutzeroberfläche des Verlaufsservers herunter.

    ![Spark2-Verlaufsserver – Herunterladen](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. Wählen Sie auf der Registerkarte „Daten/Diagramm“ die Option **Provide us feedback** (Feedback abgeben) aus.

    ![Spark-Diagramm – Feedback abgeben](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Geben Sie den Titel und eine Beschreibung des Fehlers an, ziehen Sie die ZIP-Datei in das Bearbeitungsfeld, und klicken Sie dann auf **Neues Problem melden**.

    ![Apache Spark-Problem – Beispiel für Datei](./media/apache-azure-spark-history-server/apache-spark-file-issue.png)

### <a name="3-upgrade-jar-file-for-hotfix-scenario"></a>3. Aktualisieren der JAR-Datei für das Hotfixszenario

Wenn Sie mit einem Hotfix eine Aktualisierung durchführen möchten, verwenden Sie das folgende Skript, mit dem „spark-enhancement.jar*“ aktualisiert wird.

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

**Verwendung**:

`upgrade_spark_enhancement.sh https://${jar_path}`

**Beispiel**:

`upgrade_spark_enhancement.sh https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`

**So verwenden Sie die Bash-Datei aus dem Azure-Portal**

1. Starten Sie das [Azure-Portal](https://ms.portal.azure.com), und wählen Sie Ihren Cluster aus.
2. Führen Sie eine [Skriptaktion](../hdinsight-hadoop-customize-cluster-linux.md) mit den folgenden Parametern aus:

    |Eigenschaft |Wert |
    |---|---|
    |Skripttyp|--Benutzerdefiniert|
    |NAME|UpgradeJar|
    |Bash-Skript-URI|`https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh`|
    |Knotentyp(en)|Hauptknoten, Workerknoten|
    |Parameter|`https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`|

     ![Azure-Portal – Aktion „Skript übermitteln“](./media/apache-azure-spark-history-server/apache-spark-upload1.png)

## <a name="known-issues"></a>Bekannte Probleme

+ Gegenwärtig können nur Spark 2.3- und 2.4-Cluster verwendet werden.

+ Ein-/Ausgabedaten über RDD werden auf der Registerkarte „Daten“ nicht angezeigt.

## <a name="next-steps"></a>Nächste Schritte

+ [Verwalten von Ressourcen für den Apache Spark-Cluster unter HDInsight](apache-spark-resource-manager.md)
+ [Konfigurieren von Apache Spark-Einstellungen](apache-spark-settings.md)

## <a name="contact-us"></a>Kontakt

Wenn Sie Feedback abgeben möchten oder bei der Verwendung des Tools Probleme auftreten, senden Sie eine E-Mail an [hdivstool@microsoft.com](mailto:hdivstool@microsoft.com).
