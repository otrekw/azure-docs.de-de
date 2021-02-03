---
title: Anmeldung beim Azure HDInsight-Cluster nicht möglich
description: Problembehandlung, wenn das Anmelden beim Apache Hadoop-Cluster in Azure HDInsight nicht möglich ist
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/31/2019
ms.openlocfilehash: 03657606f7e24436ff5a851fe7eef1e0c4d5dc24
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98944311"
---
# <a name="scenario-unable-to-log-into-azure-hdinsight-cluster"></a>Szenario: Anmeldung beim Azure HDInsight-Cluster nicht möglich

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Anmeldung beim Azure HDInsight-Cluster nicht möglich.

## <a name="cause"></a>Ursache

Die Gründe können variieren. Verwenden Sie für die Anmeldung beim Cluster oder bei den App-Dashboards Ihre „Clusteranmeldedaten“ oder HTTP-Anmeldeinformationen. Bei einer Remoteverbindung verwenden Sie Ihre Secure Shell (SSH)- oder Remotedesktop-Anmeldeinformationen.

## <a name="resolution"></a>Lösung

Probieren Sie zum Beheben allgemeiner Probleme die folgenden Schritte aus:

* Versuchen Sie, das Clusterdashboard auf einer neuen Browserregisterkarte im Datenschutzmodus zu öffnen.

* Falls Sie sich nicht mehr an Ihre SSH-Anmeldeinformationen erinnern, können Sie die [Anmeldeinformationen auf der Ambari-Benutzeroberfläche zurücksetzen](../hdinsight-administer-use-portal-linux.md#change-passwords).

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]