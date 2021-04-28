---
title: Referenz zu den Azure HDInsight-Konfigurationseinstellungen
description: Hier finden Sie eine Einführung in die Konfiguration der Azure HDInsight-Erweiterung.
ms.service: hdinsight
ms.topic: how-to
ms.date: 04/07/2021
ms.custom: devx-track-python
ms.openlocfilehash: e5bfa66f7c7d22617c17c6bd57eff373574021f4
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108142061"
---
# <a name="azure-hdinsight-configuration-settings-reference"></a>Referenz zu den Azure HDInsight-Konfigurationseinstellungen

Die Erweiterung „Spark & Hive Tools“ für Visual Studio Code ist umfassend konfigurierbar. Auf dieser Seite werden die wichtigsten Einstellungen beschrieben, mit denen Sie arbeiten können.  

Allgemeine Informationen zum Arbeiten mit Einstellungen in Visual Studio Code (VS Code) finden Sie unter [Benutzer- und Arbeitsbereichseinstellungen](https://code.visualstudio.com/docs/getstarted/settings), und Informationen zur Unterstützung vordefinierter Variablen finden Sie in der [Referenz zu Variablen](https://code.visualstudio.com/docs/editor/variables-reference).

## <a name="open-the-azure-hdinsight-configuration"></a>Öffnen der Azure HDInsight-Konfiguration

1. Öffnen Sie zunächst einen Ordner, um Arbeitsbereichseinstellungen zu erstellen.
2. Drücken Sie **STRG+UMSCHALT+P**, oder navigieren Sie zu **Ansicht** -> **Befehlspalette**, um alle Befehle anzuzeigen.
3. Suchen Sie **Konfiguration festlegen**.
4. Erweitern Sie im linken Verzeichnis die Option **Erweiterungen**, und wählen Sie **HDInsight-Konfiguration** aus.

 ![Screenshot der HDInsight-Konfiguration](./media/HDInsight-config-for-vscode/HDInsight-config-for-vscode.png)

## <a name="general-settings"></a>Allgemeine Einstellungen   

|  Eigenschaft   | Standard | BESCHREIBUNG   |
| ----- | ----- |----- |
| HDInsight: Azure-Umgebung | Azure | Azure-Umgebung |
| HDInsight: Link zum Öffnen der Umfrage deaktivieren | Überprüft | Aktivieren/Deaktivieren des Öffnens einer HDInsight-Umfrage |
| HDInsight: Überspringen der PySpark-Installation aktivieren | Deaktiviert | Aktivieren/Deaktivieren des Überspringens der PySpark-Installation |
| HDInsight: Anmeldetipps aktivieren | Deaktiviert | Wenn diese Option aktiviert ist, wird beim Anmelden bei Azure eine Eingabeaufforderung angezeigt. |
| HDInsight: Vorherige Erweiterungsversion | Anzeigen der Versionsnummer der aktuellen Erweiterung | Anzeigen der vorherigen Erweiterungsversion|
| HDInsight: Schriftfamilie für Ergebnisse | -apple-system,BlinkMacSystemFont,Segoe WPC,Segoe UI,HelveticaNeue-Light,Ubuntu,Droid Sans,sans-serif | Legen Sie die Schriftfamilie für das Ergebnisraster fest. Lassen Sie die Einstellung leer, um die Editor-Schriftart zu verwenden. |
| HDInsight: Schriftgrad für Ergebnisse | 13 |Legen Sie den Schriftgrad für das Ergebnisraster fest. Lassen Sie die Einstellung leer, um die Editor-Größe zu verwenden. |
| HDInsight-Cluster: Verknüpfter Cluster | -- | URLs verknüpfter Cluster. Sie können auch die JSON-Datei bearbeiten, um dies festzulegen. |
| HDInsight Hive: Lokalisierung anwenden | Deaktiviert | [Optional] Konfigurationsoptionen für die Lokalisierung in das von VS Code konfigurierte Gebietsschema (VS Code muss neu gestartet werden, damit diese Einstellungen wirksam werden.)|
| HDInsight Hive: Header kopieren/einschließen | Deaktiviert | [Optional] Konfigurationsoption für das Kopieren von Ergebnissen aus der Ergebnisansicht |
| HDInsight Hive: Neue Zeile kopieren/entfernen | Überprüft | [Optional] Konfigurationsoptionen für das Kopieren von mehrzeiligen Ergebnissen aus der Ergebnisansicht |
| HDInsight Hive › Format: Spaltendefinitionen in Spalten ausrichten | Deaktiviert | Gibt an, ob Spaltendefinitionen ausgerichtet werden sollen |
| HDInsight Hive › Format: Groß-/Kleinschreibung von Datentypen | Keine | Gibt an, ob Datentypen in Großbuchstaben, Kleinbuchstaben oder gar nicht formatiert werden sollen |
| HDInsight Hive › Format: Groß-/Kleinschreibung von Schlüsselwörtern | Keine | Gibt an, ob Schlüsselwörter in Großbuchstaben, Kleinbuchstaben oder gar nicht formatiert werden sollen |
| HDInsight Hive › Format: Kommas vor der nächsten Anweisung platzieren | Deaktiviert | Gibt an, ob Kommas in einer Liste am Anfang der einzelnen Anweisungen (z. B. „, mycolumn2“) und nicht am Ende („mycolumn1,“) platziert werden sollen|
| HDInsight Hive › Format: SELECT-Anweisungsverweise in neuer Zeile platzieren | Deaktiviert | Sollen Verweise auf Objekte in einer SELECT-Anweisung in separate Zeilen aufgeteilt werden? Beispielsweise werden bei „SELECT C1, C2 FROM T1“ sowohl C1 als auch C2 in separaten Zeilen stehen.
| HDInsight Hive: Debuginformationen protokollieren | Deaktiviert | [Optional] Protokollieren Sie die Debugausgabe in der VS Code-Konsole (Hilfe > Entwicklertools umschalten). 
| HDInsight Hive: Nachrichten standardmäßig geöffnet | Überprüft | Bei Festlegung auf TRUE wird der Nachrichtenbereich standardmäßig geöffnet, bei Festlegung auf FALSE ist er geschlossen.|
| HDInsight Hive: Schriftfamilie für Ergebnisse | -apple-system,BlinkMacSystemFont,Segoe WPC,Segoe UI,HelveticaNeue-Light,Ubuntu,Droid Sans,sans-serif | Legen Sie die Schriftfamilie für das Ergebnisraster fest. Lassen Sie die Einstellung leer, um die Editor-Schriftart zu verwenden. |
| HDInsight Hive: Schriftgrad für Ergebnisse | 13 | Legen Sie den Schriftgrad für das Ergebnisraster fest. Lassen Sie die Einstellung leer, um die Editor-Größe zu verwenden. |
| HDInsight Hive › Als CSV speichern: Header einschließen | Überprüft | [Optional] Bei Festlegung auf TRUE werden beim Speichern der Ergebnisse als CSV auch die Spaltenüberschriften einbezogen. |
| HDInsight Hive: Tastenkombinationen | -- | Tastenkombinationen für das Ergebnisfenster |
| HDInsight Hive: Batchzeit anzeigen| Deaktiviert | [Optional] Gibt an, ob die Ausführungszeit für einzelne Batches angezeigt werden soll |
| HDInsight Hive: Teilbereichsauswahl | Weiter | [Optional] Konfigurationsoptionen zur Festlegung, in welcher Spalte neue Ergebnisbereiche geöffnet werden sollen |
| HDInsight-Auftragsübermittlung: Clusterkonfiguration | -- | Clusterkonfiguration |
| HDInsight-Auftragsübermittlung: Livy-Konfiguration | -- | Livy-Konfiguration. POST/Batches |
| HDInsight Jupyter: Ergebnisse anfügen| Überprüft | Gibt an, ob die Ergebnisse an das Ergebnisfenster angefügt werden sollen. Ansonsten werden sie gelöscht, und das Fenster wird angezeigt. |
| HDInsight Jupyter: Sprachen | -- | Standardeinstellungen pro Sprache. |
| HDInsight Jupyter › Protokoll: Ausführlich | Deaktiviert | Gibt an, ob die ausführliche Protokollierung aktiviert werden soll |
| HDInsight Jupyter › Notebook: Mit Argumenten starten | Element kann hinzugefügt werden | „Jupyter Notebook“-Befehlszeilenargumente. Jedes Argument ist ein separates Element im Array. Geben Sie zum Abrufen einer vollständige Liste in ein Terminalfenster den Befehl „jupyter notebook--help“ ein. |
| HDInsight Jupyter › Notebook: Startordner | ${workspaceRoot} |-- |
| HDInsight Jupyter: Python-Erweiterung aktiviert | Überprüft | Verwenden Sie das interaktive Python-Fenster der Microsoft Python-Erweiterung, wenn Sie interaktive PySpark-Aufträge übermitteln. Verwenden Sie andernfalls unser eigenes Jupyter-Fenster. |
| HDInsight Spark.NET: 7z | C:\Programme\7-Zip | <Pfad zur Datei „7z.exe“> |
| HDInsight Spark.NET: HADOOP_HOME | D:\winutils | <Pfad zu „bin\winutils.exe“> nur unter Windows-Betriebssystemen |
| HDInsight Spark.NET: JAVA_HOME | C:\Programme\Java\jdk1.8.0_201\ | Pfad des Java-Basisverzeichnisses|
| HDInsight Spark.NET: SCALA_HOME | C:\Programme (x86)\scala\ | Pfad des Scala-Basisverzeichnisses |
| HDInsight Spark.NET: SPARK_HOME | D:\spark-2.3.3-bin-hadoop2.7\ | Pfad des Spark-Basisverzeichnisses |
| Hive: Abfrageergebnisregisterkarten beibehalten | Deaktiviert | Hive PersistQueryResultTabs |
| Hive: Teilbereichsauswahl | Weiter | [Optional] Konfigurationsoptionen zur Festlegung, in welcher Spalte neue Ergebnisbereiche geöffnet werden sollen |
| Hive Interactive: Ausführbaren Ordner kopieren | Deaktiviert | Gibt an, ob der Hive Interactive Service Runtime-Ordner in den temporären Ordner des Benutzers kopiert werden soll |
| HQL Interactive Server: Port des Wrappers | 13424 | Interaktiver Hive-Dienstport |
| HQL-Sprachserver: Port des Sprachwrappers | 12342 | Hive-Sprachdienstport, den die Server überwachen. |
| HQL-Sprachserver: Maximale Anzahl von Problemen | 100 | Steuert die maximale Anzahl von Problemen, die vom Server erzeugt werden. |
| Synapse Spark Compute: Azure-Umgebung für Synapse Spark Compute | leer | Azure-Umgebung für Synapse Spark Compute |
| Auftragsübermittlung für Synapse Spark-Pool: Livy-Konfig | -- | Livy-Konfiguration. POST/Batches
| Auftragsübermittlung für Synapse Spark-Pool: Synapse Spark Pool-Clusterkonfig | -- | Konfiguration des Synapse Spark-Pools |


## <a name="next-steps"></a>Nächste Schritte

- Informationen zu Azure HDInsight für VS Code finden Sie unter [Spark & Hive Tools für Visual Studio Code](/sql/big-data-cluster/spark-hive-tools-vscode).
- Ein Video, in dem die Verwendung von Spark und Hive für Visual Studio Code gezeigt wird, finden Sie unter [Spark und Hive für Visual Studio Code](https://go.microsoft.com/fwlink/?linkid=858706).