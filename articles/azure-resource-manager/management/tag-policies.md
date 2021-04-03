---
title: Richtlinien für das Taggen von Ressourcen
description: In diesem Artikel werden die Azure-Richtlinien beschrieben, die Sie zuweisen können, um Konformität in Bezug auf Tags sicherzustellen.
ms.topic: conceptual
ms.date: 03/20/2020
ms.openlocfilehash: c6867bc01306ac3c08a9797ece0567a45e060af2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89293733"
---
# <a name="assign-policies-for-tag-compliance"></a>Zuweisen von Richtlinien für Konformität in Bezug auf Tags

Sie verwenden [Azure Policy](../../governance/policy/overview.md), um Taggingregeln und -konventionen zu erzwingen. Durch die Erstellung einer Richtlinie vermeiden Sie das Szenario, dass Ressourcen in Ihrem Abonnement bereitgestellt werden, die nicht über die erwarteten Tags für Ihr Unternehmen verfügen. Anstelle der manuellen Anwendung von Tags oder der Suche nach Ressourcen, die nicht konform sind, erstellen Sie eine Richtlinie, die die erforderlichen Tags während der Bereitstellung automatisch anwendet. Tags können jetzt auch auf vorhandene Ressourcen mit dem neuen Effekt [Ändern](../../governance/policy/concepts/effects.md#modify) und einer [Korrekturaufgabe](../../governance/policy/how-to/remediate-resources.md) angewendet werden. Im folgenden Abschnitt werden Beispielrichtlinien für Tags dargestellt.

## <a name="policies"></a>Richtlinien

[!INCLUDE [Tag policies](../../../includes/policy/reference/bycat/policies-tags.md)]

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Taggen von Ressourcen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](tag-resources.md).
* Nicht alle Ressourcentypen unterstützen Tags. Um festzustellen, ob Sie ein Tag auf einen Ressourcentyp anwenden können, lesen Sie [Tagunterstützung für Azure-Ressourcen](tag-support.md).
