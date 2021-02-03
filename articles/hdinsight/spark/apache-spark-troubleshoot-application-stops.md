---
title: Eine Apache Spark-Streaminganwendung wird nach 24 Tagen in Azure HDInsight beendet
description: Eine Apache Spark-Streaminganwendung wird beendet, nachdem sie 24 Tage lang ausgeführt wurde. In den Protokolldateien werden keine Fehler angezeigt.
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/29/2019
ms.openlocfilehash: b702cbf915e4991df4c202564677ea7e0a02f9c4
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98929461"
---
# <a name="scenario-apache-spark-streaming-application-stops-after-executing-for-24-days-in-azure-hdinsight"></a>Szenario: Eine Apache Spark-Streaminganwendung wird beendet, nachdem sie 24 Tage lang in Azure HDInsight ausgeführt wurde.

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Eine Apache Spark-Streaminganwendung wird beendet, nachdem sie 24 Tage lang ausgeführt wurde. In den Protokolldateien werden keine Fehler angezeigt.

## <a name="cause"></a>Ursache

Der Wert von `livy.server.session.timeout` steuert, wie lange Apache Livy warten soll, bis eine Sitzung abgeschlossen wird. Wenn die Sitzungslänge den `session.timeout`-Wert erreicht hat, werden die Livy-Sitzung und die Anwendung automatisch beendet.

## <a name="resolution"></a>Lösung

Erhöhen Sie für zeitintensive Aufträge den Wert `livy.server.session.timeout` auf der Ambari-Benutzeroberfläche. Mithilfe der URL `https://<yourclustername>.azurehdinsight.net/#/main/services/LIVY/configs` können Sie über die Ambari-Benutzeroberfläche auf die Livy-Konfiguration zugreifen.

Ersetzen Sie `<yourclustername>` durch den Namen Ihres HDInsight-Clusters, wie er im Portal angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]