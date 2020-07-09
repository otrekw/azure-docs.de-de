---
title: include file
description: include file
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 07/07/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 837e82131b17fa02fca87da067eac6d77903b31d
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86111655"
---
Diese Beispiele zeigen, wie Sie Azure Policy mit Abonnements verwenden, für die ein Onboarding in Azure Lighthouse durchgeführt wurde.

| **Vorlage** | **Beschreibung** |
|---------|---------|
| [policy-add-or-replace-tag](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-add-or-replace-tag) | Weist eine Richtlinie zu, die ein Tag (mit dem Modify-Effekt) einem delegierten Abonnement hinzufügt oder es daraus entfernt. Weitere Informationen finden Sie unter [Bereitstellen einer Richtlinie, die innerhalb eines delegierten Abonnements gewartet werden kann](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-audit-delegation](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-audit-delegation) | Weist eine Richtlinie zu, die Überprüfungen auf Delegierungszuweisungen durchführt. |
| [policy-enforce-keyvault-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-keyvault-monitoring) | Weist eine Richtlinie zu, die die Diagnose für Azure Key Vault-Ressourcen in einem delegierten Abonnement aktiviert (mit dem deployIfNotExists-Effekt). Weitere Informationen finden Sie unter [Bereitstellen einer Richtlinie, die innerhalb eines delegierten Abonnements gewartet werden kann](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-enforce-sub-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-sub-monitoring) | Weist mehrere Richtlinien zum Aktivieren der Diagnose für ein delegiertes Abonnement zu und verbindet alle virtuellen Windows- und Linux-Computer mit dem von der Richtlinie erstellten Log Analytics-Arbeitsbereich. Weitere Informationen finden Sie unter [Bereitstellen einer Richtlinie, die innerhalb eines delegierten Abonnements gewartet werden kann](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-initiative](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-initiative) | Wendet eine Initiative (mehrere verwandte Richtliniendefinitionen) auf ein delegiertes Abonnement an. |

