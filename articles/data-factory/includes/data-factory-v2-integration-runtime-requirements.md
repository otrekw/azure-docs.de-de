---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 07/13/2020
ms.author: jingwang
ms.openlocfilehash: f2d93449744ea7993ec22f200c51fea8280d1576
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108747795"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Wenn sich Ihr Datenspeicher in einem lokalen Netzwerk, in einem virtuellen Azure-Netzwerk oder in einer virtuellen privaten Amazon-Cloud befindet, müssen Sie eine [selbstgehostete Integration Runtime](../create-self-hosted-integration-runtime.md) konfigurieren, um eine Verbindung herzustellen.

Handelt es sich bei Ihrem Datenspeicher um einen verwalteten Clouddatendienst, können Sie alternativ Azure Integration Runtime verwenden. Ist der Zugriff auf IP-Adressen beschränkt, die in den Firewallregeln genehmigt sind, können Sie [Azure Integration Runtime-IPs](../azure-integration-runtime-ip-addresses.md) zur Positivliste hinzufügen. 

Weitere Informationen zu den von Data Factory unterstützten Netzwerksicherheitsmechanismen und -optionen finden Sie unter [Datenzugriffsstrategien](../data-access-strategies.md).
