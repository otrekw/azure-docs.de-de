---
title: 'Zuweisen von Azure-Rollen zu einer verwalteten Identität (Vorschau): Azure RBAC'
description: Erfahren Sie, wie Sie Azure-Rollen zuweisen, indem Sie mit der verwalteten Identität beginnen und dann im Azure-Portal und über die rollenbasierte Zugriffssteuerung in Azure (Azure RBAC) eine Rolle und den zugehörigen Gültigkeitsbereich auswählen.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 02/15/2021
ms.author: rolyon
ms.openlocfilehash: 57c8c00a64996bc6223fbe7e514db9db38ccdcc2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100556846"
---
# <a name="assign-azure-roles-to-a-managed-identity-preview"></a>Zuweisen von Azure-Rollen zu einer verwalteten Identität (Vorschau)

Sie können einer verwalteten Identität über die Seite **Zugriffssteuerung (IAM)** eine Rolle hinzufügen, wie unter [Zuweisen von Azure-Rollen über das Azure-Portal](role-assignments-portal.md) beschrieben wird. Dabei beginnen Sie auf der Seite „Zugriffssteuerung (IAM)“ zunächst mit dem Bereich und wählen anschließend die verwaltete Identität und die Rolle aus. In diesem Artikel wird eine alternative Vorgehensweise zum Zuweisen von Rollen für eine verwaltete Identität beschrieben. Dabei beginnen Sie mit der verwalteten Identität und wählen anschließend den Bereich und die Rolle aus.

> [!IMPORTANT]
> Das Zuweisen einer Rolle zu einer verwalteten Identität befindet sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Azure role assignment prerequisites](../../includes/role-based-access-control/prerequisites-role-assignments.md)]

## <a name="system-assigned-managed-identity"></a>Systemseitig zugewiesene verwaltete Identität

Führen Sie die folgenden Schritte aus, um einer systemseitig zugewiesenen verwalteten Identität eine Rolle zuzuweisen und dabei mit der verwalteten Identität zu beginnen:

1. Öffnen Sie im Azure-Portal eine systemseitig zugewiesene verwaltete Identität.

1. Klicken Sie im linken Menü auf **Identität**.

    ![Systemseitig zugewiesene verwaltete Identität](./media/shared/identity-system-assigned.png)

1. Klicken Sie unter **Berechtigungen** auf **Azure-Rollenzuweisungen**.

    Wurden der ausgewählten systemseitig zugewiesenen verwalteten Identität bereits Rollen zugewiesen, wird die Liste der Rollenzuweisungen angezeigt. Diese Liste enthält alle Rollenzuweisungen, für die Sie über Leseberechtigungen verfügen.

    ![Rollenzuweisungen für eine systemseitig zugewiesene verwaltete Identität](./media/shared/role-assignments-system-assigned.png)

1. Klicken Sie auf die Liste **Abonnement**, um das Abonnement zu ändern.

1. Klicken Sie auf **Rollenzuweisung hinzufügen (Vorschau)** .

1. Wählen Sie in den Dropdownlisten die Ressourcen aus, für die die Rollenzuweisung gelten soll, z. B. **Abonnement**, **Ressourcengruppe** oder Ressource.

    Wenn Sie für den ausgewählten Bereich nicht über Schreibberechtigungen für die Rollenzuweisung verfügen, wird eine Inlinemeldung angezeigt. 

1. Wählen Sie in der Dropdownliste **Rolle** eine Rolle aus, etwa **Mitwirkender für virtuelle Computer**.

   ![Bereich „Rollenzuweisung hinzufügen“ für eine systemseitig zugewiesene verwaltete Identität](./media/role-assignments-portal-managed-identity/add-role-assignment-with-scope.png)

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird der verwalteten Identität die Rolle für den ausgewählten Bereich zugewiesen.

## <a name="user-assigned-managed-identity"></a>Benutzerseitig zugewiesene verwaltete Identität

Führen Sie die folgenden Schritte aus, um einer benutzerseitig zugewiesenen verwalteten Identität eine Rolle zuzuweisen und dabei mit der verwalteten Identität zu beginnen:

1. Öffnen Sie im Azure-Portal eine benutzerseitig zugewiesene verwaltete Identität.

1. Klicken Sie im Menü auf der linken Seite auf **Azure-Rollenzuweisungen**.

    Wurden der ausgewählten benutzerseitig zugewiesenen verwalteten Identität bereits Rollen zugewiesen, wird die Liste der Rollenzuweisungen angezeigt. Diese Liste enthält alle Rollenzuweisungen, für die Sie über Leseberechtigungen verfügen.

    ![Rollenzuweisungen für eine benutzerseitig zugewiesene verwaltete Identität](./media/shared/role-assignments-user-assigned.png)

1. Klicken Sie auf die Liste **Abonnement**, um das Abonnement zu ändern.

1. Klicken Sie auf **Rollenzuweisung hinzufügen (Vorschau)** .

1. Wählen Sie in den Dropdownlisten die Ressourcen aus, für die die Rollenzuweisung gelten soll, z. B. **Abonnement**, **Ressourcengruppe** oder Ressource.

    Wenn Sie für den ausgewählten Bereich nicht über Schreibberechtigungen für die Rollenzuweisung verfügen, wird eine Inlinemeldung angezeigt. 

1. Wählen Sie in der Dropdownliste **Rolle** eine Rolle aus, etwa **Mitwirkender für virtuelle Computer**.

   ![Bereich „Rollenzuweisung hinzufügen“ für eine benutzerseitig zugewiesene verwaltete Identität](./media/role-assignments-portal-managed-identity/add-role-assignment-with-scope.png)

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird der verwalteten Identität die Rolle für den ausgewählten Bereich zugewiesen.

## <a name="next-steps"></a>Nächste Schritte

- [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md)
- [Zuweisen von Azure-Rollen über das Azure-Portal](role-assignments-portal.md)
- [Auflisten von Azure-Rollenzuweisungen über das Azure-Portal](role-assignments-list-portal.md)
