---
title: Benutzerdefinierte Apache Ambari-Datenbank in Azure HDInsight
description: Erfahren Sie, wie Sie HDInsight-Cluster mit einer eigenen benutzerdefinierten Apache Ambari-Datenbank erstellen.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: how-to
ms.date: 06/24/2019
ms.author: hrasheed
ms.openlocfilehash: 1858e06567a0ab0907e6d2cb60358ff4ac00f9a2
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086346"
---
# <a name="set-up-hdinsight-clusters-with-a-custom-ambari-db"></a>Einrichten von HDInsight-Clustern mit einer benutzerdefinierten Ambari-Datenbank

Apache Ambari vereinfacht die Verwaltung und Überwachung von Apache Hadoop-Clustern. Ambari bietet eine benutzerfreundliche Webbenutzeroberfläche und REST-API. Ambari ist in HDInsight-Clustern enthalten und wird verwendet, um den Cluster zu überwachen und Konfigurationsänderungen vorzunehmen.

Bei der normalen Clustererstellung, die in anderen Artikeln beschrieben wird, z. B. unter [Einrichten von Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md), wird Ambari in einer [Azure SQL-Datenbank mit Computegröße S0](../azure-sql/database/resource-limits-dtu-single-databases.md#standard-service-tier) bereitgestellt, die in HDInsight verwaltet wird und für Benutzer nicht zugänglich ist.

Mit der Funktion der benutzerdefinierten Ambari-Datenbank können Sie einen neuen Cluster bereitstellen und Ambari in einer externen Datenbank einrichten, die Sie verwalten. Die Bereitstellung erfolgt über eine Azure Resource Manager-Vorlage. Diese Funktion bietet folgende Vorteile:

- Anpassung: Sie wählen die Größe und die Verarbeitungskapazität der Datenbank aus. Bei der Verarbeitung datenintensiver Workloads in großen Clustern kann eine Ambari-Datenbank mit niedrigeren Spezifikationen leicht zu einem Engpass bei Verwaltungsvorgängen führen.
- Flexibilität: Sie können die Datenbank entsprechend Ihren Anforderungen skalieren.
- Steuerung: Sie können Sicherungen und Sicherheit für die Datenbank entsprechend den Anforderung Ihrer Organisation verwalten.

Im weiteren Verlauf dieses Artikels werden folgende Themen behandelt:

- Anforderungen für die Verwendung der Funktion der benutzerdefinierten Ambari-Datenbank
- Erforderliche Schritte zum Bereitstellen von HDInsight-Clustern unter Verwendung einer eigenen externen Datenbank für Apache Ambari

## <a name="custom-ambari-db-requirements"></a>Anforderungen an benutzerdefinierte Ambari-Datenbanken

Sie können eine benutzerdefinierte Ambari-Datenbank mit allen Clustertypen und -versionen bereitstellen. Dieselbe Ambari-Datenbank kann nicht in mehreren Clustern verwendet werden.

Für die benutzerdefinierte Ambari-Datenbank gelten folgende weitere Voraussetzungen:

- Der Name der Datenbank darf keine Bindestriche oder Leerzeichen enthalten.
- Sie müssen über einen Azure SQL-Datenbank-Server und eine Azure SQL-Datenbank-Instanz verfügen.
- Die für die Einrichtung von Ambari bereitgestellte Datenbank muss leer sein. Das dbo-Standardschema darf keine Tabellen enthalten.
- Der Benutzer, der zum Herstellen der Verbindung mit der Datenbank verwendet wird, muss über die Berechtigungen SELECT, CREATE TABLE und INSERT für die Datenbank verfügen.
- Aktivieren Sie die Option [Zugriff auf Azure-Dienste erlauben](../azure-sql/database/vnet-service-endpoint-rule-overview.md#azure-portal-steps) auf dem Server, auf dem Ambari gehostet wird.
- Verwaltungs-IP-Adressen vom HDInsight-Dienst müssen in der Firewallregel zulässig sein. Eine Liste der IP-Adressen, die der Firewallregel auf Serverebene hinzugefügt werden müssen, finden Sie unter [HDInsight-Verwaltungs-IP-Adressen](hdinsight-management-ip-addresses.md).

Beachten Sie beim Hosten der Apache Ambari-Datenbank in einer externen Datenbank die folgenden Punkte:

- Für die Azure SQL-Datenbank, die Ambari umfasst, fallen zusätzliche Kosten für Sie an.
- Sichern Sie die benutzerdefinierte Ambari-Datenbank regelmäßig. Die Azure SQL-Datenbank generiert Sicherungen automatisch, der Aufbewahrungszeitraum der Sicherungen variiert jedoch. Weitere Informationen finden Sie unter [Informationen zu automatischen Sicherungen von SQL-Datenbank](../azure-sql/database/automated-backups-overview.md).

## <a name="deploy-clusters-with-a-custom-ambari-db"></a>Bereitstellen von HDInsight-Clustern mit einer benutzerdefinierten Ambari-Datenbank

Verwenden Sie die [Schnellstartvorlage für die benutzerdefinierte Ambari-Datenbank](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-custom-ambari-db), um einen HDInsight-Cluster zu erstellen, in dem Ihre benutzerdefinierte externe Ambari-Datenbank verwendet wird.

Bearbeiten Sie die Parameter in `azuredeploy.parameters.json`, um Informationen zum neuen Cluster und der Datenbank anzugeben, die Ambari enthält.

Sie können die Bereitstellung über die Azure-Befehlszeilenschnittstelle starten. Ersetzen Sie `<RESOURCEGROUPNAME>` durch die Ressourcengruppe, in der Sie den Cluster bereitstellen möchten.

```azurecli
az group deployment create --name HDInsightAmbariDBDeployment \
    --resource-group <RESOURCEGROUPNAME> \
    --template-file azuredeploy.json \
    --parameters azuredeploy.parameters.json
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von externen Metadatenspeichern in Azure HDInsight](hdinsight-use-external-metadata-stores.md)
