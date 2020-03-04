---
title: 'Häufig gestellte Fragen zu Azure Security Center: Fragen zu Berechtigungen'
description: Diese FAQs beantworten Fragen zu Berechtigungen in Azure Security Center, einem Produkt, das Ihnen hilft, Bedrohungen zu verhindern, zu erkennen und darauf zu reagieren.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 66a260fbb03f770ee98ec29a5f5e15e3d7dd1310
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77599663"
---
# <a name="permissions"></a>Berechtigungen

## <a name="how-do-permissions-work-in-azure-security-center"></a>Wie funktionieren Berechtigungen in Azure Security Center?

Azure Security Center verwendet die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](../role-based-access-control/role-assignments-portal.md). Diese stellt [integrierte Rollen](../role-based-access-control/built-in-roles.md) bereit, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können.

Security Center bewertet die Konfiguration Ihrer Ressourcen, um die Sicherheitsprobleme und Sicherheitsrisiken zu identifizieren. In Security Center werden Ihnen nur dann Informationen zu einer Ressource angezeigt, wenn Ihnen für das Abonnement oder die Ressourcengruppe, der eine Ressource angehört, die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ zugewiesen ist.

Informationen zu Rollen und zulässigen Aktionen in Security Center finden Sie unter [Permissions in Azure Security Center](security-center-permissions.md) (Berechtigungen in Azure Security Center).



## <a name="who-can-modify-a-security-policy"></a>Wie kann ich eine Sicherheitsrichtlinie ändern?

Damit Sie eine Sicherheitsrichtlinie ändern können, müssen Sie Systemadministrator oder Besitzer bzw. Mitwirkender dieses Abonnements sein.

Weitere Informationen dazu, wie eine Sicherheitsrichtlinie konfiguriert wird, finden Sie unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](tutorial-security-policy.md).