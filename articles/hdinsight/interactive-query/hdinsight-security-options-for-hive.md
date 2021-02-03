---
title: Sicherheitsoptionen für Hive in Azure HDInsight
description: Sicherheitsoptionen für Hive in Standard- und ESP-Clustern
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/02/2020
ms.openlocfilehash: 1189a320d0dc700756c9f7664d0a6303be5dab51
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98940897"
---
# <a name="security-options-for-hive-in-azure-hdinsight"></a>Sicherheitsoptionen für Hive in Azure HDInsight

In diesem Dokument werden die empfohlenen Sicherheitsoptionen für Hive in HDInsight beschrieben. Diese Optionen können über Ambari konfiguriert werden.

![„Sicherheitsoptionen für Hive“](./media/hdinsight-security-options-for-hive/security-options-hive.png "Sicherheitsoptionen für Hive")

## <a name="hiveserver2-authentication"></a>HiveServer2-Authentifizierung

Für Standardcluster ist die empfohlene Einstellung für die HiveServer2-Authentifizierung die Standardeinstellung „Ohne“. Zum Aktivieren der Authentifizierung empfehlen wir ein Upgrade auf einen [ESP](../domain-joined/hdinsight-security-overview.md)-Cluster (Enterprise Security Package). 

Für ESP-Cluster ist standardmäßig [Kerberos](https://web.mit.edu/Kerberos/)-Authentifizierung aktiviert. PAMs (Pluggable Authentication Modules) und benutzerdefinierte Authentifizierungsschemas werden nicht unterstützt.

## <a name="hiveserver2-authorization"></a>HiveServer2-Autorisierung

Für Standardcluster ist die Standardeinstellung „Ohne“. [SqlStdAuth (SQL Standards Based Authorization, auf SQL-Standards basierende Autorisierung)](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+based+hive+authorization) kann aktiviert werden. Die Autorisierung mithilfe von [Apache Ranger](https://ranger.apache.org/) wird für Standardcluster nicht unterstützt. Wenn Ranger-Autorisierung verwendet werden soll, empfehlen wir das Upgrade auf einen ESP-Cluster. 

Für ESP-Cluster ist Autorisierung mithilfe von Ranger standardmäßig aktiviert. 


## <a name="ssl-encryption-for-hiveserver2"></a>SSL-Verschlüsselung für HiveServer2

Die Aktivierung von Hiveserver2-SSL wird weder für Standard- noch für ESP-Cluster empfohlen. Stattdessen wird SSL auf dem Gateway aktiviert. Die [Verschlüsselung während der Übertragung](../domain-joined/encryption-in-transit.md) kann aktiviert werden, um die Kommunikation zwischen den Clusterknoten mithilfe von [IPSec (Internet-Protokollsicherheit)](https://en.wikipedia.org/wiki/IPsec) zu verschlüsseln.


## <a name="next-steps"></a>Nächste Schritte
* [Übersicht zur HiveServer2-Authentifizierung](https://cwiki.apache.org/confluence/display/Hive/Setting+up+HiveServer2#SettingUpHiveServer2-Authentication/SecurityConfiguration)
* [Übersicht zur HiveServer2-Autorisierung](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Authorization)
* [Aktivieren der auf SQL-Standards basierenden Hive-Autorisierung](https://community.cloudera.com/t5/Community-Articles/Getting-started-with-SQLStdAuth/ta-p/244263)
* [Apache Ranger mit Hive](../domain-joined/apache-domain-joined-run-hive.md)
