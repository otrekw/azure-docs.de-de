---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 07/13/2020
ms.author: jingwang
ms.openlocfilehash: 66b7159d19838cf08158d794f9b8e8819efc3f41
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112041709"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime setup from connector articles.
-->
Wenn sich Ihr Datenspeicher in einem lokalen Netzwerk, in einem virtuellen Azure-Netzwerk oder in einer virtuellen privaten Amazon-Cloud befindet, müssen Sie eine [selbstgehostete Integration Runtime](../create-self-hosted-integration-runtime.md) konfigurieren, um eine Verbindung herzustellen.

Handelt es sich bei Ihrem Datenspeicher um einen verwalteten Clouddatendienst, können Sie die Azure Integration Runtime verwenden. Ist der Zugriff auf IP-Adressen beschränkt, die in den Firewallregeln genehmigt sind, können Sie [Azure Integration Runtime-IPs](../azure-integration-runtime-ip-addresses.md) zur Positivliste hinzufügen. 

Sie können auch das Feature [managed virtual network integration runtime](../tutorial-managed-virtual-network-on-premise-sql-server.md) (Integration Runtime für verwaltete virtuelle Netzwerke) in Azure Data Factory verwenden, um auf das lokale Netzwerk zuzugreifen, ohne eine selbstgehostete Integration Runtime zu installieren und zu konfigurieren.

Weitere Informationen zu den von Data Factory unterstützten Netzwerksicherheitsmechanismen und -optionen finden Sie unter [Datenzugriffsstrategien](../data-access-strategies.md).
