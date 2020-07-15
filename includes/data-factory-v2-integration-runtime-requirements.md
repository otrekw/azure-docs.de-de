---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: 0b03957178af0578d2c6cd91d7377c93f413cec3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84629537"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Wurde Ihr Datenspeicher mit einer der folgenden Methoden konfiguriert, müssen Sie eine [selbstgehostete Integration Runtime](../articles/data-factory/create-self-hosted-integration-runtime.md) einrichten, um eine Verbindung mit diesem Datenspeicher herstellen zu können:

- Der Datenspeicher befindet sich in einem lokalen Netzwerk, in einem virtuellen Azure-Netzwerk oder innerhalb der virtuellen privaten Amazon-Cloud.
- Der Datenspeicher ist ein verwalteter Clouddatendienst, bei dem der Zugriff auf IP-Adressen beschränkt ist, die in der Whiteliste der Firewallregeln enthalten sind.
