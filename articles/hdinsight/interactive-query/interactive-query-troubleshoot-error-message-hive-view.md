---
title: 'Nicht in der Apache Hive-Ansicht angezeigte Fehlermeldung: Azure HDInsight'
description: Bei der Abfrage tritt in der Apache Hive-Ansicht ein Fehler auf. Es werden keine Informationen zum Azure HDInsight-Cluster angezeigt.
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/30/2019
ms.openlocfilehash: c60e06e8f37e7aed0d0a0df661690a2b1f32dbd5
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98931006"
---
# <a name="scenario-query-error-message-not-displayed-in-apache-hive-view-in-azure-hdinsight"></a>Szenario: Abfragefehlermeldung wird in Apache Hive-Ansicht in Azure HDInsight nicht angezeigt

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Die Abfragefehlermeldung in der Apache Hive-Ansicht sieht in etwa wie folgt aus und enthält keine weiteren Informationen:

```
"Failed to execute query. <a href="#/messages/1">(details)</a>"
```

## <a name="cause"></a>Ursache

Manchmal ist die Fehlermeldung eines Abfragefehlers ggf. zu groß, um auf der Hauptseite der Hive-Ansicht angezeigt zu werden.

## <a name="resolution"></a>Lösung

Überprüfen Sie die Registerkarte „Benachrichtigungen“ (rechts oben in der Hive-Ansicht), um sich die vollständige Stapelüberwachung und die gesamte Fehlermeldung anzusehen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]