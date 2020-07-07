---
title: Ändern der Lebenszykluseinstellungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Erfahren Sie, wie Sie die Lebenszykluseinstellungen für ein Zugriffspaket in der Azure Active Directory-Berechtigungsverwaltung ändern.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/18/2020
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 538ed31c1f3bbca8c2151a6ac0d2a3ad561bc6a7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85078782"
---
# <a name="change-lifecycle-settings-for-an-access-package-in-azure-ad-entitlement-management"></a>Ändern der Lebenszykluseinstellungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung

Als Zugriffspaket-Manager können Sie die Lebenszykluseinstellungen für ein Zugriffspaket jederzeit ändern, indem Sie eine vorhandene Richtlinie bearbeiten. Wenn Sie das Ablaufdatum für eine Richtlinie ändern, ändert sich das Ablaufdatum für Anforderungen mit ausstehender Genehmigung oder genehmigte Anforderungen nicht.

In diesem Artikel wird beschrieben, wie die Lebenszykluseinstellungen für ein vorhandenes Zugriffspaket geändert werden.

## <a name="open-lifecycle-settings"></a>Öffnen der Lebenszykluseinstellungen

Wenn Sie die Lebenszykluseinstellungen für ein Zugriffspaket ändern möchten, müssen Sie die entsprechende Richtlinie öffnen. Führen Sie die folgenden Schritte aus, um die Lebenszykluseinstellungen für ein Zugriffspaket zu öffnen.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie auf **Richtlinien**, und klicken Sie dann auf die Richtlinie mit den Lebenszykluseinstellungen, die Sie bearbeiten möchten.

    Der Bereich „Richtliniendetails“ wird unten auf der Seite geöffnet.

    ![Zugriffspaket: Bereich „Richtliniendetails“](./media/entitlement-management-shared/policy-details.png)

1. Klicken Sie auf **Bearbeiten**, um die Richtlinie zu bearbeiten.

    ![Zugriffspaket: Richtlinie bearbeiten](./media/entitlement-management-shared/policy-edit.png)

1. Klicken Sie auf die Registerkarte **Lebenszyklus**, um die Lebenszykluseinstellungen zu öffnen.

[!INCLUDE [Entitlement management lifecycle policy](../../../includes/active-directory-entitlement-management-lifecycle-policy.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Ändern der Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket](entitlement-management-access-package-request-policy.md)