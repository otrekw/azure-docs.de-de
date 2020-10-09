---
title: include file
description: include file
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 09/24/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: a9c9b5081232988cb4ab80bfcbb9f2d92d1d025f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91336570"
---
Diese Beispiele zeigen, wie Sie Azure Policy mit Abonnements verwenden, für die ein Onboarding in Azure Lighthouse durchgeführt wurde.

| **Vorlage** | **Beschreibung** |
|---------|---------|
| [policy-add-or-replace-tag](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-add-or-replace-tag) | Weist eine Richtlinie zu, die ein Tag (mit dem Modify-Effekt) einem delegierten Abonnement hinzufügt oder es daraus entfernt. Weitere Informationen finden Sie unter [Bereitstellen einer Richtlinie, die innerhalb eines delegierten Abonnements gewartet werden kann](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-audit-delegation](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-audit-delegation) | Weist eine Richtlinie zu, die Überprüfungen auf Delegierungszuweisungen durchführt. |
| [policy-delegate-management-groups](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-delegate-management-groups) | Weist eine Richtlinie zu, um zu bestätigen, dass Abonnements innerhalb einer Verwaltungsgruppe an einen Verwaltungsmandanten delegiert wurden, und erstellt andernfalls die Zuweisung.
| [policy-enforce-keyvault-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-keyvault-monitoring) | Weist eine Richtlinie zu, die die Diagnose für Azure Key Vault-Ressourcen in einem delegierten Abonnement aktiviert (mit dem deployIfNotExists-Effekt). Weitere Informationen finden Sie unter [Bereitstellen einer Richtlinie, die innerhalb eines delegierten Abonnements gewartet werden kann](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-enforce-sub-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-sub-monitoring) | Weist mehrere Richtlinien zum Aktivieren der Diagnose für ein delegiertes Abonnement zu und verbindet alle virtuellen Windows- und Linux-Computer mit dem von der Richtlinie erstellten Log Analytics-Arbeitsbereich. Weitere Informationen finden Sie unter [Bereitstellen einer Richtlinie, die innerhalb eines delegierten Abonnements gewartet werden kann](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-initiative](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-initiative) | Wendet eine Initiative (mehrere verwandte Richtliniendefinitionen) auf ein delegiertes Abonnement an. |

