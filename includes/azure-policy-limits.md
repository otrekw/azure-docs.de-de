---
title: include file
description: include file
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/30/2020
ms.author: dacoulte
ms.openlocfilehash: f3f706789e14cb20214bf17fd91f6ec1e503848f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91606007"
---
Für jeden Objekttyp für Azure Policy gilt eine maximale Anzahl. Bei Definitionen gilt ein _Scope_-Eintrag (Bereich) entweder für die [Verwaltungsgruppe](../articles/governance/management-groups/overview.md) oder das Abonnement.
Bei Zuweisungen und Ausnahmen gilt ein _Scope_-Eintrag für die [Verwaltungsgruppe](../articles/governance/management-groups/overview.md), das Abonnement, die Ressourcengruppe oder eine einzelne Ressource.

| Hierbei gilt: | Was | Maximale Anzahl |
|---|---|---|
| `Scope` | Richtliniendefinitionen | 500 |
| `Scope` | Initiativdefinitionen | 200 |
| Tenant | Initiativdefinitionen | 2\.500 |
| `Scope` | Richtlinien- oder Initiativenzuweisungen | 200 |
| Bereich | Ausnahmen | 1000 |
| Richtliniendefinition | Parameter | 20 |
| Initiativdefinition | Richtlinien | 1000 |
| Initiativdefinition | Parameter | 100 |
| Richtlinien- oder Initiativenzuweisungen | Ausschlüsse (notScopes) | 400 |
| Richtlinienregel | Geschachtelte konditionelle Abschnitte | 512 |
| Wartungstask | Ressourcen | 500 |
