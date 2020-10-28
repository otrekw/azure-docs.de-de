---
title: Sicherheitsoptionen für Hive in Azure HDInsight
description: Sicherheitsoptionen für Hive in Standard- und ESP-Clustern
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/02/2020
ms.openlocfilehash: 14a41365640439ff99861bbb22cc04a40f35da5e
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92221713"
---
# <a name="security-options-for-hive-in-azure-hdinsight"></a>Sicherheitsoptionen für Hive in Azure HDInsight

In diesem Dokument werden die empfohlenen Sicherheitsoptionen für Hive in HDInsight beschrieben. Diese Optionen können über Ambari konfiguriert werden.

![„Sicherheitsoptionen für Hive“](./media/hdinsight-security-options-for-hive/security-options-hive.png "Sicherheitsoptionen für Hive")

## <a name="hiveserver2-authentication"></a>HiveServer2-Authentifizierung

Für Standardcluster ist die empfohlene Einstellung für die HiveServer2-Authentifizierung die Standardeinstellung „Ohne“. Zum Aktivieren der Authentifizierung empfehlen wir ein Upgrade auf einen [ESP](https://docs.microsoft.com/azure/hdinsight/domain-joined/hdinsight-security-overview)-Cluster (Enterprise Security Package). 

Für ESP-Cluster ist standardmäßig [Kerberos](https://web.mit.edu/Kerberos/)-Authentifizierung aktiviert. PAMs (Pluggable Authentication Modules) und benutzerdefinierte Authentifizierungsschemas werden nicht unterstützt.

## <a name="hiveserver2-authorization"></a>HiveServer2-Autorisierung

Für Standardcluster ist die Standardeinstellung „Ohne“. [SqlStdAuth (SQL Standards Based Authorization, auf SQL-Standards basierende Autorisierung)](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+based+hive+authorization) kann aktiviert werden. Die Autorisierung mithilfe von [Apache Ranger](https://ranger.apache.org/) wird für Standardcluster nicht unterstützt. Wenn Ranger-Autorisierung verwendet werden soll, empfehlen wir das Upgrade auf einen ESP-Cluster. 

Für ESP-Cluster ist Autorisierung mithilfe von Ranger standardmäßig aktiviert. 


## <a name="ssl-encryption-for-hiveserver2"></a>SSL-Verschlüsselung für HiveServer2

Die Aktivierung von Hiveserver2-SSL wird weder für Standard- noch für ESP-Cluster empfohlen. Stattdessen wird SSL auf dem Gateway aktiviert. Die [Verschlüsselung während der Übertragung](https://docs.microsoft.com/azure/hdinsight/domain-joined/encryption-in-transit) kann aktiviert werden, um die Kommunikation zwischen den Clusterknoten mithilfe von [IPSec (Internet-Protokollsicherheit)](https://en.wikipedia.org/wiki/IPsec) zu verschlüsseln.


## <a name="next-steps"></a>Nächste Schritte
* [Übersicht zur HiveServer2-Authentifizierung](https://cwiki.apache.org/confluence/display/Hive/Setting+up+HiveServer2#SettingUpHiveServer2-Authentication/SecurityConfiguration)
* [Übersicht zur HiveServer2-Autorisierung](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Authorization#:~:text=%20Overview%20of%20Authorization%20Modes%20%201%201,and%20Apache%20Sentry%20are%20apache%20projects...%20More%20)
* [Aktivieren der auf SQL-Standards basierenden Hive-Autorisierung](https://community.cloudera.com/t5/Community-Articles/Getting-started-with-SQLStdAuth/ta-p/244263)
* [Apache Ranger mit Hive](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-run-hive#:~:text=Create%20Hive%20ODBC%20data%20source%20%20%20,Enter%20hiveuser1%40contoso158.onmicrosoft.c%20...%20%205%20more%20rows%20)
