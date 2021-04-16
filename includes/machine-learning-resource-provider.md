---
author: blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/09/2020
ms.author: larryfr
ms.openlocfilehash: 8fd774f8a3a73ceaffa7902b35e1b1dff12ef5af
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "75893973"
---
Wenn Sie einen Azure Machine Learning-Arbeitsbereich oder eine vom Arbeitsbereich verwendete Ressource erstellen, erhalten Sie möglicherweise eine Fehlermeldung ähnlich der folgenden:

* `No registered resource provider found for location {location}`
* `The subscription is not registered to use namespace {resource-provider-namespace}`

Die meisten Ressourcenanbieter werden automatisch registriert, aber nicht alle. Wenn Sie diese Meldung erhalten, müssen Sie den erwähnten Anbieter registrieren.

Weitere Informationen zum Registrieren eines Ressourcenanbieters finden Sie unter [Beheben von Fehlern bei der Ressourcenanbieterregistrierung](../articles/azure-resource-manager/templates/error-register-resource-provider.md).