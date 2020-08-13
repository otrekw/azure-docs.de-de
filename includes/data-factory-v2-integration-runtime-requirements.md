---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 07/13/2020
ms.author: jingwang
ms.openlocfilehash: 6c348b3dd0005eeab154aa2d74abc617cbd1d0cb
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87529386"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Wenn sich Ihr Datenspeicher in einem lokalen Netzwerk, in einem virtuellen Azure-Netzwerk oder in einer virtuellen privaten Amazon-Cloud befindet, müssen Sie eine [selbstgehostete Integration Runtime](../articles/data-factory/create-self-hosted-integration-runtime.md) einrichten, um eine Verbindung herzustellen.

Handelt es sich bei Ihrem Datenspeicher um einen verwalteten Clouddatendienst, können Sie Azure Integration Runtime verwenden. Ist der Zugriff auf IP-Adressen beschränkt, die in der Whiteliste der Firewallregeln enthalten sind, können Sie [Azure Integration Runtime-IPs](../articles/data-factory/azure-integration-runtime-ip-addresses.md) zur Zulassungsliste hinzufügen. 

Weitere Informationen zu den von Data Factory unterstützten Netzwerksicherheitsmechanismen und -optionen finden Sie unter [Datenzugriffsstrategien](../articles/data-factory/data-access-strategies.md).
