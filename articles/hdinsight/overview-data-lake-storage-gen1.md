---
title: Übersicht über Azure Data Lake Storage Gen1 in HDInsight
description: Übersicht über Data Lake Storage Gen1 in HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: b598494500124ca4a70e8e2d7bfef023fbf7d498
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92328775"
---
# <a name="azure-data-lake-storage-gen1-overview-in-hdinsight"></a>Übersicht über Azure Data Lake Storage Gen1 in HDInsight

Azure Data Lake Storage Gen1 ist ein unternehmensweites Hyperscale-Repository für Big Data-Analyseworkloads. Mit Azure Data Lake können Sie Daten von beliebiger Größe, Art und Erfassungsgeschwindigkeit erfassen, und zwar an einer einzelnen Stelle zur Durchführung operativer und explorativer Analysen.

Greifen Sie über Hadoop (verfügbar mit einem HDInsight-Cluster) mithilfe der WebHDFS-kompatiblen REST-APIs auf Data Lake Storage Gen1 zu. Data Lake Storage Gen1 wurde speziell für Analysen der gespeicherten Daten konzipiert und ist für eine hohe Leistung in Datenanalyseszenarien optimiert. Gen1 umfasst bereits vorkonfiguriert Funktionen, die für echte Anwendungsfälle von Unternehmen unerlässlich sind. Zu den Funktionen gehören Sicherheit, Verwaltbarkeit, Flexibilität, Zuverlässigkeit und Verfügbarkeit.

Weitere Informationen zu Data Lake Storage Gen1 finden Sie in der ausführlichen [Übersicht über Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md).

Data Lake Storage Gen1 bietet u.a. die folgenden wichtigen Funktionen.

## <a name="compatibility-with-hadoop"></a>Kompatibilität mit Hadoop

Data Lake Storage Gen1 ist ein Apache Hadoop-Dateisystem, das mit HDFS und der Hadoop-Umgebung kompatibel ist.  HDInsight-Anwendungen oder -Dienste, die die WebHDFS-API verwenden, können problemlos in Data Lake Storage Gen1 integriert werden. Data Lake Storage Gen1 macht auch eine WebHDFS-kompatible REST-Schnittstelle für Anwendungen verfügbar.

In Data Lake Storage Gen1 gespeicherte Daten können mühelos mit Hadoop-Analyseframeworks wie MapReduce oder Hive analysiert werden. Azure HDInsight-Cluster können für den direkten Zugriff auf in Data Lake Storage Gen1 gespeicherte Daten bereitgestellt und konfiguriert werden.

## <a name="unlimited-storage-petabyte-files"></a>Unbegrenzter Speicher, Dateigrößen bis in den Petabytebereich

Data Lake Storage Gen1 bietet unbegrenzten Speicher und eignet sich zum Speichern verschiedener Arten von Daten für Analysezwecke. Es gibt keine Einschränkungen für Kontogrößen, Dateigrößen oder die Menge an Daten, die in einem Data Lake gespeichert werden kann. Data Lake Storage Gen1 unterstützt Dateigrößen vom Kilobyte- bis in den Petabytebereich und ist somit eine gute Wahl für die Speicherung von Daten jeglicher Art. Daten werden dauerhaft gespeichert, indem mehrere Kopien angefertigt werden. Und diese können dann für unbegrenzte Zeit im Data Lake verbleiben.

## <a name="performance-tuning-for-big-data-analytics"></a>Leistungsoptimierung für Big Data-Analysen

Data Lake Storage Gen1 ist für analytische Systeme konzipiert, die einen enormen Durchsatz erfordern, um große Datenmengen abzufragen und zu analysieren. Der Data Lake verteilt Teile einer Datei auf mehrere einzelne Speicherserver. Dieses Setup verbessert beim Analysieren der Daten den Lesedurchsatz, wenn die Datei parallel gelesen wird.

## <a name="readiness-for-enterprise-highly-available-and-secure"></a>Bereitschaft für Unternehmen: Hoch verfügbar und sicher

Data Lake Storage Gen1 bietet Verfügbarkeit und Zuverlässigkeit nach Branchenstandard. Datenressourcen werden dauerhaft gespeichert: Redundante Kopien schützen vor unerwarteten Fehlern. Unternehmen können Data Lake Storage Gen1 in ihren Lösungen als wesentlichen Bestandteil ihrer vorhandenen Datenplattform einsetzen.

Data Lake Storage Gen1 bietet außerdem Sicherheit auf Unternehmensniveau für gespeicherte Daten. Weitere Informationen finden Sie unter [Schützen von Daten in Data Lake Storage Gen1](#data-security-in-data-lake-storage-gen1).

## <a name="flexible-data-structures"></a>Flexible Datenstrukturen

Data Lake Storage Gen1 kann beliebige Daten ohne vorherige Umwandlung im nativen Format speichern. Es muss für Data Lake Storage Gen1 kein Schema definiert werden, bevor die Daten geladen werden. Das jeweilige Analyseframework interpretiert die Daten und definiert ein Schema zum Zeitpunkt der Analyse. Data Lake Storage Gen1 kann strukturierte, semistrukturierte und unstrukturierte Daten verarbeiten.

Data Lake Storage Gen1-Container für Daten sind im Wesentlichen Ordner und Dateien. Sie arbeiten mit den gespeicherten Daten über SDKs, das Azure-Portal und Azure PowerShell. Daten, die mit diesen Schnittstellen und Containern in den Speicher eingefügt werden, können beliebige Datentypen enthalten. Data Lake Storage Gen1 führt keine spezielle Verarbeitung von Daten basierend auf dem Datentyp durch.

## <a name="data-security-in-data-lake-storage-gen1"></a>Datensicherheit in Data Lake Storage Gen1

Data Lake Storage Gen1 verwendet Azure Active Directory zur Authentifizierung und Zugriffssteuerungslisten (ACLs) zum Verwalten des Zugriffs auf Ihre Daten.

| **Feature** | **Beschreibung** |
| --- | --- |
| Authentifizierung |Zur Identitäts- und Zugriffsverwaltung für alle in Data Lake Storage Gen1 gespeicherten Daten ist Data Lake Storage Gen1 in Azure Active Directory (Azure AD) integriert. Aufgrund dieser Integration profitiert Data Lake Storage Gen1 von allen Azure AD-Features. Zu diesen Features zählen mehrstufige Authentifizierung, bedingter Zugriff und rollenbasierte Zugriffssteuerung in Azure. Überwachung der Anwendungsnutzung, Sicherheitsüberwachung und -warnungen usw. Data Lake Storage Gen1 unterstützt das OAuth 2.0-Protokoll für die Authentifizierung mit der REST-Schnittstelle. Weitere Informationen finden Sie unter [Authentifizierung bei Azure Data Lake Storage Gen1 mit Azure Active Directory](../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md).|
| Zugriffssteuerung |Data Lake Storage Gen1 ermöglicht eine Zugriffssteuerung durch die Unterstützung von POSIX-Berechtigungen, die durch das WebHDFS-Protokoll verfügbar gemacht werden. ACLs können für Stammordner, Unterordner und einzelne Dateien aktiviert werden. Weitere Informationen zur Funktionsweise von ACLs im Kontext von Data Lake Storage Gen1 finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md). |
| Verschlüsselung |Data Lake Storage Gen1 bietet auch eine Verschlüsselung für Daten, die im Konto gespeichert sind. Beim Erstellen eines Data Lake Storage Gen1-Kontos geben Sie die Einstellungen für die Verschlüsselung an. Sie können auswählen, ob Ihre Daten verschlüsselt werden sollen. Weitere Informationen finden Sie unter [Verschlüsselung von Daten in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-encryption.md). Anweisungen zur Bereitstellung einer verschlüsselungsbezogenen Konfiguration finden Sie unter [Erste Schritte mit Azure Data Lake Storage Gen1 über das Azure-Portal](../data-lake-store/data-lake-store-get-started-portal.md). |

Weitere Informationen zum Schutz von Daten in Data Lake Storage Gen1 finden Sie unter [Sichern von in Azure Data Lake Storage Gen1 gespeicherten Daten](../data-lake-store/data-lake-store-secure-data.md).

## <a name="applications-that-are-compatible-with-data-lake-storage-gen1"></a>Mit Data Lake Storage Gen1 kompatible Anwendungen

Data Lake Storage Gen1 ist mit den meisten Open Source-Komponenten in der Hadoop-Umgebung kompatibel. Außerdem ist eine problemlose Integration in andere Azure-Dienste möglich.  Über die folgenden Links erhalten Sie weitere Informationen zur Verwendung von Data Lake Storage Gen1 mit Open Source-Komponenten und anderen Azure-Diensten.

* Siehe [Open Source-Big Data-Anwendungen, die mit Azure Data Lake Storage Gen1 verwendet werden können](../data-lake-store/data-lake-store-compatible-oss-other-applications.md).
* Informationen dazu, wie Data Lake Storage Gen1 mit anderen Azure-Diensten verwendet werden kann, um ein breiteres Spektrum von Szenarien zu ermöglichen, finden Sie unter [Integration von Azure Data Lake Storage Gen1 in andere Azure-Dienste](../data-lake-store/data-lake-store-integrate-with-other-services.md).
* Siehe [Verwenden von Azure Data Lake Storage Gen1 für Big Data-Anforderungen](../data-lake-store/data-lake-store-data-scenarios.md).

## <a name="data-lake-storage-gen1-file-system-adl"></a>Data Lake Storage Gen1-Dateisystem (adl://)

In Hadoop-Umgebungen kann über das neue Dateisystem AzureDataLakeFilesystem (adl://) auf Data Lake Storage Gen1 zugegriffen werden. Für die Optimierung der Leistung von Anwendungen und Diensten, die `adl://` verwenden, stehen Möglichkeiten zur Verfügung, die derzeit in WebHDFS nicht verfügbar sind. Daher erhalten Sie die Flexibilität, mit adl:// die optimale Leistung zu nutzen (empfohlen) oder vorhandenen Code beizubehalten, indem Sie die WebHDFS-API weiterhin direkt verwenden. Azure HDInsight nutzt die Möglichkeiten des AzureDataLakeFilesystem voll aus, um eine optimale Leistung für Data Lake Storage Gen1 bereitzustellen.

Sie können mithilfe des folgenden URI auf Ihre Daten in Data Lake Storage Gen1 zugreifen:

`adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`

Weitere Informationen zum Zugriff auf die Daten in Data Lake Storage Gen1 finden Sie unter [Verfügbare Aktionen mit den gespeicherten Daten](../data-lake-store/data-lake-store-get-started-portal.md#properties).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
* [Einführung in Azure Storage](../storage/common/storage-introduction.md)
* [Übersicht über Azure Data Lake Storage Gen2](./overview-data-lake-storage-gen2.md)