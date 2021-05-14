---
title: Transport Layer Security in Azure HDInsight
description: Transport Layer Security (TLS) und Secure Sockets Layer (SSL) sind kryptografische Protokolle, die Kommunikationssicherheit über ein Computernetzwerk bereitstellen.
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: 57dd7cbfe83b22c0221193af3c6ac2912b40499c
ms.sourcegitcommit: 43be2ce9bf6d1186795609c99b6b8f6bb4676f47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2021
ms.locfileid: "108278594"
---
# <a name="transport-layer-security-in-azure-hdinsight"></a>Transport Layer Security in Azure HDInsight

Verbindungen mit dem HDInsight-Cluster über den öffentlichen Clusterendpunkt `https://CLUSTERNAME.azurehdinsight.net` werden über Clustergatewayknoten hergestellt, die als Proxy fungieren. Diese Verbindungen werden mit einem Protokoll namens TLS geschützt. Die Erzwingung höherer TLS-Versionen für Gateways erhöhen die Sicherheit dieser Verbindungen. Weitere Informationen dazu, warum Sie neuere Versionen von TLS verwenden sollten, finden Sie unter [Beheben des Problems mit TLS 1.0](/security/solving-tls1-problem).

Azure HDInsight-Cluster akzeptieren standardmäßig TLS 1.2-Verbindungen an öffentlichen HTTPS-Endpunkten und ältere Versionen aus Gründen der Abwärtskompatibilität. Sie können die TLS-Mindestversion, die auf den Gatewayknoten während der Clustererstellung unterstützt wird, entweder im Azure-Portal oder über eine Resource Manager-Vorlage festlegen. Wählen Sie im Portal während der Clustererstellung auf der Registerkarte **Sicherheit + Netzwerk** die TLS-Version aus. Verwenden Sie für eine Resource Manager-Vorlage zum Zeitpunkt der Bereitstellung die Eigenschaft **minSupportedTlsVersion**. Eine Beispielvorlage finden Sie unter [Bereitstellen eines HDInsight-Clusters, für den mindestens Version 1.2. des TLS-Protokolls erzwungen wird](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.hdinsight/hdinsight-minimum-tls/azuredeploy.json). Diese Eigenschaft unterstützt drei Werte: 1.0, 1.1 und 1.2. Diese entsprechen jeweils TLS 1.0 oder höher, TLS 1.1 oder höher und TLS 1.2 oder höher.


## <a name="next-steps"></a>Nächste Schritte

* [Planen eines virtuellen Netzwerks für Azure HDInsight](./hdinsight-plan-virtual-network-deployment.md)
* [Erstellen von virtuellen Netzwerken für Azure HDInsight-Cluster](hdinsight-create-virtual-network.md).
* [Netzwerksicherheitsgruppen](../virtual-network/network-security-groups-overview.md).