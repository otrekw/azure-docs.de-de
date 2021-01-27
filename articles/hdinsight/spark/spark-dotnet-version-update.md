---
title: Aktualisieren von .NET für Apache Spark auf Version v1.0 in HDI
description: Hier erfahren Sie, wie Sie Ihre .NET für Apache Spark-Version in HDI auf 1.0 aktualisieren und wie sich dies auf Ihren vorhandenen Code und Ihre Cluster auswirkt.
author: Niharikadutta
ms.author: nidutta
ms.service: hdinsight
ms.topic: how-to
ms.date: 01/05/2021
ms.openlocfilehash: a1602f29a6d0066ec3c99e990532411621652c47
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98788128"
---
# <a name="updating-net-for-apache-spark-to-version-v10--in-hdinsight"></a>Aktualisieren von .NET für Apache Spark auf Version v1.0 in HDInsight

In diesem Dokument wird die erste Hauptversion von [.NET für Apache Spark](https://github.com/dotnet/spark) erläutert, und Sie erfahren, wie sie sich auf Ihre aktuellen Produktionspipelines in HDInsight-Clustern auswirken kann.

## <a name="about-net-for-apache-spark-version-100"></a>Informationen zu .NET für Apache Spark, Version 1.0.0

Dies ist die erste [offizielle Hauptversion](https://github.com/dotnet/spark/releases/tag/v1.0.0) von .NET für Apache Spark und bietet – zusammen mit anderen Features – DataFrame-API-Vollständigkeit für Spark 2.4.x und Spark 3.0.x. Eine vollständige Liste aller Features, Verbesserungen und Fehlerbehebungen finden Sie in den offiziellen [Versionshinweisen zu v1.0.0](https://github.com/dotnet/spark/blob/master/docs/release-notes/1.0.0/release-1.0.0.md).
Es ist wichtig zu wissen, dass diese Version mit früheren Versionen von `Microsoft.Spark` und `Microsoft.Spark.Worker` **nicht** kompatibel ist. Wenn Sie ein Upgrade Ihrer .NET für Apache Spark-Anwendung planen, damit sie mit v1.0.0 kompatibel ist, lesen Sie den [Migrationsleitfaden](https://github.com/dotnet/spark/blob/master/docs/migration-guide.md#upgrading-from-microsoftspark-0x-to-10).

## <a name="using-net-for-apache-spark-v10-in-hdinsight"></a>Verwenden von .NET für Apache Spark v1.0 in HDInsight

Während die aktuellen HDI-Cluster nicht betroffen sind (d. h., sie haben weiterhin dieselbe Version wie zuvor), erhalten neu erstellte HDI-Cluster diese neueste v1.0.0-Version von .NET für Apache Spark. Dies bedeutet, wenn Folgendes zutrifft:

- **Sie haben einen älteren HDI-Cluster**: Wenn Sie Ihre Spark .NET-Anwendung auf v1.0.0 aktualisieren möchten (empfohlen), müssen Sie die `Microsoft.Spark.Worker`-Version auf Ihrem HDI-Cluster aktualisieren. Weitere Informationen finden Sie im Abschnitt [Changing versions of .NET for Apache Spark on HDI cluster](#changing-net-for-apache-spark-version-on-hdinsight) (Ändern von Versionen von .NET für Apache Spark auf dem HDI-Cluster).
Wenn Sie die aktuelle Version von .NET für Apache Spark in Ihrer Anwendung nicht aktualisieren möchten, sind keine weiteren Schritte erforderlich.  

- **Sie haben einen neuen HDI-Cluster**: Wenn Sie Ihre Spark .NET-Anwendung auf v1.0.0 aktualisieren möchten (empfohlen), sind keine Schritte erforderlich, um den Worker auf HDI zu ändern. Sie müssen sich aber im [Migrationsleitfaden](https://github.com/dotnet/spark/blob/master/docs/migration-guide.md#upgrading-from-microsoftspark-0x-to-10) informieren, damit Sie die erforderlichen Schritte zum Aktualisieren Ihres Codes und Ihrer Pipelines verstehen.
Wenn Sie die aktuelle Version von .NET für Apache Spark in Ihrer Anwendung nicht ändern möchten, müssen Sie die Version auf Ihrem HDI-Cluster von v1.0 (Standard auf neuen Clustern) in die von Ihnen verwendete Version ändern. Weitere Informationen finden Sie im Abschnitt [Changing versions of .NET for Apache Spark on HDI cluster](spark-dotnet-version-update.md#changing-net-for-apache-spark-version-on-hdinsight) (Ändern von Versionen von .NET für Apache Spark auf dem HDI-Cluster).  

## <a name="changing-net-for-apache-spark-version-on-hdinsight"></a>Ändern der .NET für Apache Spark-Version in HDInsight

### <a name="deploy-microsoftsparkworker"></a>Bereitstellen von Microsoft.Spark.Worker

`Microsoft.Spark.Worker` ist eine Back-End-Komponente, die sich auf den einzelnen Workerknoten Ihres Spark-Clusters befindet. Wenn Sie eine benutzerdefinierte C#-Funktion (User-Defined Function, UDF) ausführen möchten, muss Spark verstehen, wie die .NET CLR zu deren Ausführung zu starten ist. `Microsoft.Spark.Worker` stellt eine Sammlung von Klassen für Spark bereit, die diese Funktionalität ermöglichen. Wählen Sie die Workerversion abhängig von der .NET für Apache Spark-Version aus, die Sie auf dem HDI-Cluster bereitstellen möchten.

1. Laden Sie das Microsoft.Spark.Worker Linux-Release Ihrer speziellen Version herunter. Wenn Sie beispielsweise `.NET for Apache Spark v1.0.0` haben möchten, müssten Sie [Microsoft.Spark.Worker.netcoreapp3.1.linux-x64-1.0.0.tar.gz](https://github.com/dotnet/spark/releases/tag/v1.0.0) herunterladen.  

2. Laden Sie das Skript [install-worker.sh](https://github.com/dotnet/spark/blob/master/deployment/install-worker.sh) herunter, um die in Schritt 1 heruntergeladenen Worker-Binärdateien auf allen Workerknoten Ihres HDI-Clusters zu installieren.  

3. Laden Sie die oben erwähnten Dateien in das Azure Storage Konto hoch, auf das Ihr Cluster zugreifen kann. Weitere Informationen können Sie im [Artikel zur Bereitstellung von .NET für Apache Spark HDI](/dotnet/spark/tutorials/hdinsight-deployment#upload-files-to-azure) finden.

4. Führen Sie das Skript `install-worker.sh` auf allen Workerknoten Ihres Clusters mithilfe von Skriptaktionen aus. Weitere Informationen finden Sie im [Artikel zur Bereitstellung von .NET für Apache Spark HDI](/dotnet/spark/tutorials/hdinsight-deployment#run-the-hdinsight-script-action).

### <a name="update-your-application-to-use-specific-version"></a>Aktualisieren Ihrer Anwendung zur Verwendung einer bestimmten Version

Sie können Ihre .NET für Apache Spark-Anwendung aktualisieren, damit sie eine bestimmte Version verwendet, indem Sie die erforderliche Version des [Microsoft.Spark-NuGet-Pakets](https://www.nuget.org/packages/Microsoft.Spark/) in Ihrem Projekt auswählen. Wenn Sie sich für eine Aktualisierung Ihrer Anwendung auf v1.0.0 entscheiden, lesen Sie unbedingt die Versionshinweise zu dieser Version und den oben erwähnten [Migrationsleitfaden](https://github.com/dotnet/spark/blob/master/docs/migration-guide.md#upgrading-from-microsoftspark-0x-to-10).

## <a name="faqs"></a>Häufig gestellte Fragen

### <a name="will-my-existing-hdi-cluster-with-version--100-start-failing-with-the-new-release"></a>Wird auf meinem vorhandenen HDI-Cluster mit Version < 1.0.0 bei dem neuen Release ein Fehler auftreten?

Vorhandene HDI-Cluster haben weiterhin dieselbe vorherige Version für .NET für Apache Spark, und Ihre vorhandene Anwendung (mit einer früheren Version von Spark .NET) ist von der Aktualisierung nicht betroffen.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen Ihrer .NET für Apache Spark-Anwendung in HDInsight](/dotnet/spark/tutorials/hdinsight-deployment)