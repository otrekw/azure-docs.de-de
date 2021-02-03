---
title: Übersicht über Azure Data Lake Storage Gen2 in HDInsight
description: Übersicht über Data Lake Storage Gen2 in HDInsight.
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: e69838f18efc08d0f64dd9ea904f502617073a8f
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98938855"
---
# <a name="azure-data-lake-storage-gen2-overview-in-hdinsight"></a>Übersicht über Azure Data Lake Storage Gen2 in HDInsight

Azure Data Lake Storage Gen2 integriert Kernfeatures von Azure Data Lake Storage Gen1 in Azure Blob Storage. Zu diesen Features gehört ein Dateisystem, das mit Hadoop, Azure Active Directory (Azure AD) und POSIX-basierten Zugriffssteuerungslisten (ACLs) kompatibel ist. Mit dieser Kombination können Sie von der Leistung von Azure Data Lake Storage Gen1 profitieren, während Sie gleichzeitig das Tiering und die Datenlebenszyklusverwaltung von Blob-Speicher nutzen.

Weitere Informationen zu Data Lake Storage Gen2 finden Sie in der [Einführung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md).

## <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Kernfunktionen von Azure Data Lake Storage Gen2

* **Mit Hadoop kompatibler Zugriff:** In Azure Data Lake Storage Gen2 können Sie ebenso auf Daten zugreifen und diese verwalten wie mit einem Hadoop Distributed File System (HDFS). Der ABFS-Treiber (Azure Blob File System) ist in allen Apache Hadoop-Umgebungen verfügbar, einschließlich Azure HDInsight und Azure Databricks. Mit ABFS können Sie auf die in Data Lake Storage Gen2 gespeicherten Daten zugreifen.

* **Obermenge von POSIX-Berechtigungen:** Das Sicherheitsmodell für Data Lake Gen2 unterstützt Zugriffssteuerungslisten und POSIX-Berechtigungen sowie zusätzliche Granularität speziell für Data Lake Storage Gen2. Die Einstellungen können über Verwaltungstools oder Frameworks wie Apache Hive und Apache Spark konfiguriert werden.

* **Kosteneffizienz:** Data Lake Storage Gen2 bietet kostengünstige Speicherkapazität und -transaktionen. Mit den Lebenszyklen von Azure Blob Storage lassen sich die Kosten senken, indem die Abrechnungsraten angepasst werden, während die Daten den Lebenszyklus durchlaufen.

* **Kompatibilität mit Blob Storage-Tools, -Frameworks und -Apps:** Data Lake Storage Gen2 funktioniert weiterhin mit einer Vielzahl von Tools, Frameworks und Anwendungen für Blob Storage.

* **Optimierter Treiber:** Der ABFS-Treiber ist speziell für Big Data-Analysen optimiert. Die entsprechenden REST-APIs werden über den DFS-Endpunkt (Distributed File System, verteiltes Dateisystem) dfs.core.windows.net bereitgestellt.

## <a name="whats-new-for-azure-data-lake-storage-gen-2"></a>Neues bei Azure Data Lake Storage Gen2

### <a name="managed-identities-for-secure-file-access"></a>Verwaltete Identitäten für sicheren Dateizugriff

Azure HDInsight verwendet verwaltete Identitäten, um den Clusterzugriff auf Dateien in Azure Data Lake Storage Gen2 zu sichern. Verwaltete Identitäten sind ein Feature von Azure Active Directory, das Azure-Diensten eine Reihe von automatisch verwalteten Anmeldeinformationen zur Verfügung stellt. Diese Anmeldeinformationen können zur Authentifizierung bei jedem Dienst verwendet werden, der Active Directory-Authentifizierung unterstützt. Die Verwendung verwalteter Identitäten erfordert nicht, dass Sie Anmeldeinformationen in Code- oder Konfigurationsdateien speichern.

Weitere Informationen finden Sie unter [Verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md).

### <a name="azure-blob-file-system-driver"></a>Azure Blob File System-Treiber

Apache Hadoop-Anwendungen erwarten nativ, dass sie Daten von lokalen Datenträgerspeichern lesen und auf diese schreiben. Ein Hadoop-Dateisystemtreiber wie ABFS ermöglicht es Hadoop-Anwendungen, mit Cloudspeicher zu arbeiten. Dazu werden reguläre Hadoop-Dateisystemvorgänge emuliert. Der Treiber wandelt diese anschließend in Vorgänge um, die von der eigentlichen Cloudspeicherplattform interpretiert werden können.

Bisher wurden mit dem Hadoop-Dateisystemtreiber alle Dateisystemoperationen auf Clientseite in Azure Storage-REST-API-Aufrufe umgewandelt. Anschließend wurde die REST-API aufgerufen. Diese clientseitige Konvertierung führte jedoch zu mehreren REST-API-Aufrufen für einzelne Dateisystemvorgänge wie das Umbenennen einer Datei. Mit ABFS wurde die Hadoop-Dateisystemlogik von der Clientseite zur Serverseite verschoben. Die Azure Data Lake Storage Gen2-API wird jetzt parallel zur Blob-API ausgeführt. Diese Migration verbessert die Leistung, da jetzt gängige Hadoop-Dateisystemvorgänge mit einem REST-API-Aufruf ausgeführt werden können.

Weitere Informationen finden Sie unter [Azure-Blobdateisystemtreiber (ABFS): Ein dedizierter Azure Storage-Treiber für Hadoop](../storage/blobs/data-lake-storage-abfs-driver.md).

### <a name="uri-scheme-for-azure-data-lake-storage-gen-2"></a>URI-Schema für Azure Data Lake Storage Gen2

Azure Data Lake Storage Gen2 verwendet für den Zugriff auf Dateien in Azure Storage aus HDInsight ein neues URI-Schema:

`abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

Das URI-Schema bietet SSL-verschlüsselten Zugriff.

`<FILE_SYSTEM_NAME>` gibt den Pfad des Dateisystems in Data Lake Storage Gen2 an.

`<ACCOUNT_NAME>` gibt den Namen des Azure Storage-Kontos an. Ein vollqualifizierter Domänenname (FQDN) ist erforderlich.

`<PATH>` ist der HDFS-Pfadname für die Datei oder das Verzeichnis.

Wenn keine Werte für `<FILE_SYSTEM_NAME>` und `<ACCOUNT_NAME>` angegeben sind, wird das Standarddateisystem verwendet. Für die Dateien im Standarddateisystem können Sie relative oder absolute Pfade verwenden. Auf die Datei `hadoop-mapreduce-examples.jar`, die sich in HDInsight-Clustern befindet, kann z. B. mithilfe eines der folgenden Pfade verwiesen werden:

```
abfs://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfs:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]
> In HDInsight-Clustern der Version 2.1 und 1.6 lautet der Dateiname `hadoop-examples.jar`. Wenn Sie Dateien außerhalb von HDInsight verwenden, wird das ABFS-Format von den meisten Hilfsprogrammen nicht erkannt. Stattdessen wird ein einfaches Pfadformat wie `example/jars/hadoop-mapreduce-examples.jar` erwartet.

Weitere Informationen finden Sie unter [Verwenden des Azure Data Lake Storage Gen2-URI](../storage/blobs/data-lake-storage-introduction-abfs-uri.md).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
* [Einführung in Azure Storage](../storage/common/storage-introduction.md)
* [Übersicht über Azure Data Lake Storage Gen1](./overview-data-lake-storage-gen1.md)