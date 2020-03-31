---
title: Apache Hive-Bibliotheken während der Clustererstellung – Azure HDInsight
description: Erfahren Sie, wie Sie während der Erstellung des Clusters Apache Hive-Bibliotheken (JAR-Dateien) zu einem HDInsight-Cluster hinzufügen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.date: 02/14/2020
ms.openlocfilehash: 0b746963cea5a950ba47d8b4dfeb074cb0910436
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471022"
---
# <a name="add-custom-apache-hive-libraries-when-creating-your-hdinsight-cluster"></a>Hinzufügen benutzerdefinierter Apache Hive-Bibliotheken beim Erstellen des HDInsight-Clusters

Erfahren Sie, wie Sie [Apache Hive](https://hive.apache.org/)-Bibliotheken in HDInsight vorab laden. In diesem Dokument erfahren Sie, wie Sie eine Skriptaktion zum Vorabladen von Bibliotheken während der Clustererstellung verwenden. Bibliotheken, die anhand der Schritte in diesem Dokument hinzugefügt werden, stehen global in Hive zur Verfügung. Sie müssen zum Laden nicht [ADD JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) verwenden.

## <a name="how-it-works"></a>Funktionsweise

Beim Erstellen eines Clusters können Sie eine Skriptaktion verwenden, um Clusterknoten während der Erstellung zu bearbeiten. Das Skript in diesem Dokument akzeptiert einen einzelnen Parameter, der den Speicherort der Bibliotheken darstellt. Dieser Speicherort muss sich in einem Azure-Speicherkonto befinden. Die Bibliotheken müssen als JAR-Dateien gespeichert werden.

Während der Clustererstellung zählt das Skript die Dateien auf, kopiert diese auf Haupt- und Workerknoten in das Verzeichnis `/usr/lib/customhivelibs/` und fügt sie dann der `hive.aux.jars.path`-Eigenschaft in der Datei `core-site.xml` hinzu. Bei Linux-basierten Clustern aktualisiert es außerdem die Datei `hive-env.sh` mit dem Speicherort der Dateien.

Mithilfe der Skriptaktion in diesem Artikel werden die Bibliotheken zur Verfügung gestellt, wenn ein Hive-Client für **WebHCat** und **HiveServer2** verwendet wird.

## <a name="the-script"></a>Das Skript

**Skriptspeicherort**

[https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

### <a name="requirements"></a>Requirements (Anforderungen)

* Die Skripts müssen sowohl auf die **Hauptknoten** als auch die **Workerknoten** angewendet werden.

* Die JAR-Dateien, die Sie installieren möchten, müssen in Azure Blob Storage in einem **einzelnen Container** gespeichert werden.

* Das Speicherkonto mit der Bibliothek von JAR-Dateien **muss** bei der Erstellung mit dem HDInsight-Cluster verknüpft werden. Dies muss entweder das Standardspeicherkonto sein oder ein über die __Speicherkontoeinstellungen__ hinzugefügtes Konto.

* Der WASB-Pfad zum Container muss als Parameter für die Skriptaktion angegeben werden. Wenn sich die JAR-Dateien beispielsweise in einem Container namens **libs** in einem Speicherkonto namens **mystorage** befinden, lautet der Parameter `wasbs://libs@mystorage.blob.core.windows.net/`.

  > [!NOTE]  
  > Es wird vorausgesetzt, dass Sie bereits ein Speicherkonto und einen Blobcontainer erstellt und die Dateien darin hochgeladen haben.
  >
  > Wenn Sie noch kein Speicherkonto erstellt haben, ist dies über das [Azure-Portal](https://portal.azure.com) möglich. Anschließend können Sie in einem Hilfsprogramm wie dem [Azure Storage-Explorer](https://storageexplorer.com/) einen Container im Konto erstellen und Dateien in diesen hochladen.

## <a name="create-a-cluster-using-the-script"></a>Erstellen eines Clusters mithilfe des Skripts

1. Beginnen Sie die Bereitstellung eines Clusters anhand der Schritte in [Bereitstellen von HDInsight-Clustern unter Linux](hdinsight-hadoop-provision-linux-clusters.md), schließen Sie sie jedoch nicht ab. Sie können auch Azure PowerShell oder das HDInsight .NET SDK zum Erstellen eines Clusters mit diesem Skript verwenden. Weitere Informationen zur Verwendung dieser Methoden finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md). Wählen Sie für das Azure-Portal auf der Registerkarte **Konfiguration + Preise** die Option **+ Skriptaktion hinzufügen** aus.

1. Wenn sich für **Storage** das Speicherkonto, das die Bibliothek der JAR-Dateien enthält, von dem für den Cluster verwendeten Konto unterscheidet, füllen Sie **Zusätzliche Speicherkonten** aus.

1. Geben Sie für **Skriptaktionen** die folgenden Informationen an:

    |Eigenschaft |value |
    |---|---|
    |Skripttyp|--Benutzerdefiniert|
    |Name|Bibliotheken |
    |Bash-Skript-URI|`https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh`|
    |Knotentyp(en)|Hauptknoten, Workerknoten|
    |Parameter|Geben Sie die WASB-Adresse des Containers und Speicherkontos an, das die JAR-Dateien enthält. Beispiel: `wasbs://libs@mystorage.blob.core.windows.net/`.|

    > [!NOTE]
    > Verwenden Sie für Apache Spark 2.1 den folgenden Bash-Skript-URI: `https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v00.sh`.

1. Setzen Sie die Bereitstellung des Clusters entsprechend der Beschreibung unter [Bereitstellen von HDInsight-Clustern unter Linux](hdinsight-hadoop-provision-linux-clusters.md) fort.

Nach der Erstellung des Clusters können Sie die durch dieses Skript hinzugefügten JAR-Dateien in Hive nutzen, ohne die `ADD JAR`-Anweisung verwenden zu müssen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Arbeiten mit Hive finden Sie unter [Verwenden von Apache Hive mit HDInsight](hadoop/hdinsight-use-hive.md)
