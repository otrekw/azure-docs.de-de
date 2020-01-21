---
title: API Management-Beispielrichtlinie – Filtern nach IP-Adresse bei Verwendung von Application Gateway
titleSuffix: Azure API Management
description: 'API Management-Beispielrichtlinie: Demonstriert, wie Sie nach der IP-Adresse einer Anforderung filtern, wenn ein Application Gateway verwendet wird.'
services: api-management
documentationcenter: ''
author: jftl6y
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/13/2020
ms.author: joscot
ms.custom: fasttrack-new
ms.openlocfilehash: 45e16c9aa9e4b04e7225320951e9f839fae75ba3
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "75942363"
---
# <a name="filter-on-request-ip-address-when-using-an-application-gateway"></a>Filtern nach der IP-Adresse einer Anforderung bei Verwendung von Application Gateway

Dieser Artikel zeigt ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie auf die IP-Adresse einer Anforderung gefiltert wird, wenn über ein Application Gateway oder einen anderen Mittler auf die API Management-Instanz zugegriffen wird. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte unter [Festlegen oder Bearbeiten von Azure API Management-Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [API Management-Richtlinienbeispiele](../policy-samples.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/examples/Filter on IP Address when using Application Gateway.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu APIM-Richtlinien:

+ [API Management-Richtlinien für die Zugriffsbeschränkung](../api-management-access-restriction-policies.md)
+ [API Management-Richtlinienbeispiele](../policy-samples.md)
