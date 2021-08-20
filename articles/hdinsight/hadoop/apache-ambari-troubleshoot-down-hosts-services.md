---
title: Die Apache Ambari-Benutzeroberfläche zeigt ausgefallene Hosts und Dienste in Azure HDInsight
description: Problembehandlung bei einer Apache Ambari-Benutzeroberfläche, wenn ausgefallene Hosts und Dienste in Azure HDInsight angezeigt werden
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/02/2019
ms.openlocfilehash: b8cd9c4e00eb3dc454098dbd127622342bc690f7
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112291455"
---
# <a name="scenario-apache-ambari-ui-shows-down-hosts-and-services-in-azure-hdinsight"></a>Szenario: Die Apache Ambari-Benutzeroberfläche zeigt ausgefallene Hosts und Dienste in Azure HDInsight an

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Der Zugriff auf die Apache Ambari-Benutzeroberfläche ist möglich, aber die Benutzeroberfläche zeigt, dass fast alle Dienste ausgefallen sind, und alle Hosts zeigen, dass kein Heartbeat mehr empfangen wird.

## <a name="cause"></a>Ursache

In den meisten Szenarien tritt dieses Problem auf, wenn AmbariServer nicht auf dem aktiven Hauptknoten ausgeführt wird. Überprüfen Sie, welcher Hauptknoten der aktive Hauptknoten ist, und stellen Sie sicher, dass Ihr AmbariServer auf dem richtigen ausgeführt wird. Starten Sie AmbariServer nicht manuell, sondern legen Sie fest, dass der Failovercontrollerdienst für das Starten von AmbariServer auf dem richtigen Hauptknoten zuständig ist. Starten Sie den aktiven Hauptknoten neu, um ein Failover zu erzwingen.

Dieses Problem kann auch durch Netzwerkprobleme verursacht werden. Überprüfen Sie bei jedem Clusterknoten, ob Sie `headnodehost` pingen können. Es gibt eine seltene Situation, in der kein Clusterknoten eine Verbindung mit `headnodehost` herstellen kann:

```
$>telnet headnodehost 8440
... No route to host
```

## <a name="resolution"></a>Lösung

Normalerweise wird dieses Problem durch einen Neustart des aktiven Hauptknotens behoben. Falls nicht, wenden Sie sich an das Support-Team von HDInsight.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../includes/hdinsight-troubleshooting-next-steps.md)]