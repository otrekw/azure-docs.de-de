---
title: include file
description: include file
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/23/2020
ms.author: dacoulte
ms.openlocfilehash: e9faea1d5913a19dfdeff662e26992529dc1b22d
ms.sourcegitcommit: ba8df8424d73c8c4ac43602678dae4273af8b336
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2020
ms.locfileid: "84466941"
---
Für jeden Objekttyp für Azure Policy gilt eine maximale Anzahl. Ein _Scope_-Eintrag (Bereich) gilt entweder für das Abonnement oder die [Verwaltungsgruppe](../articles/governance/management-groups/overview.md).

| Hierbei gilt: | Was | Maximale Anzahl |
|---|---|---|
| `Scope` | Richtliniendefinitionen | 500 |
| `Scope` | Initiativdefinitionen | 100 |
| Tenant | Initiativdefinitionen | 2\.500 |
| `Scope` | Richtlinien- oder Initiativenzuweisungen | 100 |
| Richtliniendefinition | Parameter | 20 |
| Initiativdefinition | Richtlinien | 100 |
| Initiativdefinition | Parameter | 100 |
| Richtlinien- oder Initiativenzuweisungen | Ausschlüsse (notScopes) | 400 |
| Richtlinienregel | Geschachtelte konditionelle Abschnitte | 512 |
| Wartungstask | Ressourcen | 500 |
