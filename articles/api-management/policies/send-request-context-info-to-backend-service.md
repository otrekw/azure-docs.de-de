---
title: API Management-Beispielrichtlinie –Senden von Anforderungskontextinformationen an den Back-End-Dienst
titleSuffix: Azure API Management
description: 'Beispiel für eine Azure API Management-Richtlinie: Veranschaulicht, wie Anforderungskontextinformationen an den Back-End-Dienst gesendet werden.'
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 7782af3c8a533ceb3a6d2bd3b412c21469f9a021
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92078815"
---
# <a name="send-request-context-information-to-the-backend-service"></a>Senden von Anforderungskontextinformationen an den Back-End-Dienst

Dieser Artikel enthält ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Anforderungskontextinformationen an den Back-End-Dienst gesendet werden. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte unter [Festlegen oder Bearbeiten von Azure API Management-Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [API Management-Richtlinienbeispiele](../policy-reference.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/examples/Send request context information to the backend service.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [API Management-Richtlinienbeispiele](../policy-reference.md)