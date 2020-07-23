---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 07/13/2020
ms.author: jingwang
ms.openlocfilehash: 994b8285999bfa52e2aea9a57ad832aefddcfb76
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86544937"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Wenn sich Ihr Datenspeicher in einem lokalen Netzwerk, in einem virtuellen Azure-Netzwerk oder in einer virtuellen privaten Amazon-Cloud befindet, müssen Sie eine [selbstgehostete Integration Runtime](../articles/data-factory/create-self-hosted-integration-runtime.md) einrichten, um eine Verbindung herzustellen.

Handelt es sich bei Ihrem Datenspeicher um einen verwalteten Clouddatendienst, bei dem der Zugriff auf IP-Adressen beschränkt ist, die in der Whiteliste der Firewallregeln enthalten sind, können Sie Azure Integration Runtime verwenden und [ihre IP-Adressen](../articles/data-factory/azure-integration-runtime-ip-addresses.md) zur Zulassungsliste hinzufügen. 

Informationen zu den Netzwerksicherheitsmechanismen, die von Data Factory für den Zugriff auf Datenspeicher im Allgemeinen unterstützt werden, finden Sie unter [Datenzugriffsstrategien](../articles/data-factory/data-access-strategies.md).
