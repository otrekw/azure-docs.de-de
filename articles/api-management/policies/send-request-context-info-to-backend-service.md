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
ms.openlocfilehash: 8cef989ca7ce8ee649c4f4aeb579c43bf8a8d6ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75442402"
---
# <a name="send-request-context-information-to-the-backend-service"></a>Senden von Anforderungskontextinformationen an den Back-End-Dienst

Dieser Artikel enthält ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Anforderungskontextinformationen an den Back-End-Dienst gesendet werden. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte unter [Festlegen oder Bearbeiten von Azure API Management-Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [API Management-Richtlinienbeispiele](../policy-samples.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/examples/Send request context information to the backend service.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [API Management-Richtlinienbeispiele](../policy-samples.md)

