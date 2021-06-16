---
title: Verwalten einer vom Benutzer zugewiesenen verwalteten Identität im Azure-Portal – Azure AD
description: Schrittweise Anweisungen zum Erstellen, Auflisten, Löschen und Zuweisen einer Rolle für eine vom Benutzer zugewiesene verwalteten Identität.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.custom: subject-rbac-steps
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/20/2021
ms.author: barclayn
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: 72a9cfe6142b4c82cd122068e096776c19c4e392
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112080901"
---
# <a name="create-list-delete-or-assign-a-role-to-a-user-assigned-managed-identity-using-the-azure-portal"></a>Erstellen, Auflisten, Löschen oder Zuweisen einer Rolle zu einer benutzerseitig zugewiesenen verwalteten Identität über das Azure-Portal

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität verwenden, um sich bei Diensten mit Unterstützung für die Azure AD-Authentifizierung zu authentifizieren, ohne dass Anmeldeinformationen in Ihren Code eingebettet werden müssen. 

In diesem Artikel erfahren Sie, wie Sie eine benutzerseitig zugewiesene verwaltete Identität über das Azure-Portal erstellen, auflisten, löschen oder zuweisen.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#managed-identity-types)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer benutzerseitig zugewiesenen verwalteten Identität

Ihrem Konto muss die Rolle [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität erstellt werden kann.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, um die vom Benutzer zugewiesene verwaltete Identität zu erstellen.
2. Geben Sie *Verwaltete Identitäten* im Suchfeld ein, und klicken Sie unter **Dienste** auf **Verwaltete Identitäten**.
3. Klicken Sie auf **Hinzufügen**, und geben Sie im Bereich **Vom Benutzer zugewiesene verwaltete Identität erstellen** in den folgenden Feldern Werte ein:
    - **Abonnement**: Wählen Sie das Abonnement aus, in dem die benutzerseitig zugewiesene verwaltete Identität erstellt werden soll.
    - **Ressourcengruppe**: Wählen Sie eine Ressourcengruppe aus, in der die benutzerseitig zugewiesene verwaltete Identität erstellt werden soll, oder klicken Sie auf **Neu erstellen**, um eine neue Ressourcengruppe zu erstellen.
    - **Region**: Wählen Sie eine Region aus, in der die benutzerseitig zugewiesene verwaltete Identität bereitgestellt werden soll, z. B. **USA, Westen**.
    - **Name**: Dies ist der Name für die vom Benutzer zugewiesene verwaltete Identität, z.B. „UAI1“.
    ![Erstellen einer benutzerseitig zugewiesenen verwalteten Identität](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)
4. Klicken Sie auf **Überprüfen + erstellen**, um die Änderungen zu überprüfen.
5. Klicken Sie auf **Erstellen**.

## <a name="list-user-assigned-managed-identities"></a>Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten

Ihrem Konto muss die Rolle [Operator für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-operator) oder [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität aufgelistet/gelesen werden kann.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, um die vom Benutzer zugewiesenen verwalteten Identitäten aufzulisten.
2. Geben Sie *Verwaltete Identitäten* im Suchfeld ein, und klicken Sie unter „Dienste“ auf **Verwaltete Identitäten**.
3. Eine Liste der vom Benutzer zugewiesenen verwalteten Identitäten für Ihr Abonnement wird zurückgegeben.  Durch Klicken auf den entsprechenden Namen können Sie die Details der vom Benutzer zugewiesenen verwalteten Identität anzeigen.

![Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Löschen einer vom Benutzer zugewiesenen verwalteten Identität

Ihrem Konto muss die Rolle [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität gelöscht werden kann.

Durch das Löschen wird eine vom Benutzer zugewiesene Identität nicht von der VM oder Ressource entfernt, der sie zugewiesen wurde.  Informationen zum Entfernen einer vom Benutzer zugewiesenen Identität von einem virtuellen Computer finden Sie unter [Entfernen einer vom Benutzer zugewiesenen verwalteten Identität von einer VM](./qs-configure-portal-windows-vm.md#remove-a-user-assigned-managed-identity-from-a-vm).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, um eine vom Benutzer zugewiesene verwaltete Identität zu löschen.
2. Wählen Sie die vom Benutzer zugewiesene verwaltete Identität aus, und klicken Sie auf **Löschen**.
3. Wählen Sie im Bestätigungsfeld **Ja** aus.

![Löschen einer vom Benutzer zugewiesenen verwalteten Identität](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)

## <a name="assign-a-role-to-a-user-assigned-managed-identity"></a>Zuweisen einer Rolle zu einer vom Benutzer zugewiesenen verwalteten Identität 

Ihrem Konto muss die Rolle [Benutzerzugriffsadministrator](../../role-based-access-control/built-in-roles.md#user-access-administrator) zugewiesen sein, damit einer benutzerseitig zugewiesenen verwalteten Identität eine Rolle zugewiesen werden kann.

Ausführliche Informationen finden Sie unter [Zuweisen von Azure-Rollen über das Azure-Portal](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen des Zugriffs auf eine Ressource für eine verwaltete Identität mithilfe der Azure CLI](howto-assign-access-cli.md)
- [Zuweisen des Zugriffs einer verwalteten Identität auf eine Ressource mithilfe von PowerShell](howto-assign-access-powershell.md)