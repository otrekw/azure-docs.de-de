---
title: include file
description: include file
services: active-directory
author: daveba
ms.service: active-directory
ms.subservice: msi
ms.topic: include
ms.date: 05/31/2018
ms.author: daveba
ms.custom: include file
ms.openlocfilehash: dc7849e52b3e00c87d271ee8aef8788d816dff7a
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90971040"
---
- Jede verwaltete Identität wird auf den Objektkontingentgrenzwert in einem Azure AD-Mandanten angerechnet, wie unter [Dienst- und andere Einschränkungen für Azure AD](../articles/active-directory/users-groups-roles/directory-service-limits-restrictions.md) beschrieben.
-   Für die Rate, mit der verwaltete Identitäten erstellt werden können, gelten die folgenden Grenzwerte:

    1. Pro Azure AD-Mandant und Azure-Region: 200 Erstellungsvorgänge pro 20 Sekunden
    2. Pro Azure-Abonnement und Azure-Region: 40 Erstellungsvorgänge pro 20 Sekunden

- Wenn Sie durch den Benutzer zugewiesene verwaltete Identitäten erstellen, werden nur alphanumerische Zeichen (0-9, a-z und A-Z) und der Bindestrich (-) unterstützt. Damit die Zuweisung zu einem virtuellen Computer oder einer VM-Skalierungsgruppe ordnungsgemäß funktioniert, darf der Name höchstens 24 Zeichen lang sein.
