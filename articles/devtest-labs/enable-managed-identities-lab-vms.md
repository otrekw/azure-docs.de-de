---
title: Aktivieren von verwalteten Identitäten auf Ihren Lab-VMs in Azure DevTest Labs
description: In diesem Artikel wird gezeigt, wie ein Lab-Besitzer benutzerseitig zugewiesene verwaltete Identitäten auf Ihren virtuellen Lab-Computern aktivieren kann.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: b4bf2900acebaeecd5cbc4cb65635aee6de87dda
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88717630"
---
# <a name="enable-user-assigned-managed-identities-on-lab-virtual-machines-in-azure-devtest-labs"></a>Aktivieren von benutzerseitig zugewiesenen verwalteten Identitäten auf virtuellen Lab-Computern in Azure DevTest Labs
Als Lab-Besitzer können Sie benutzerseitig zugewiesene verwaltete Identitäten auf Ihren virtuellen Lab-Computern in Azure DevTest Labs aktivieren.

Eine verwaltete Identität kann für die Authentifizierung bei jedem Dienst verwendet werden, der die Azure Active Directory-Authentifizierung, einschließlich Key Vault, unterstützt, ohne dabei Anmeldeinformationen im Code übergeben zu müssen. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).

Mit dieser Funktion können Lab-Benutzer Azure-Ressourcen wie Azure SQL-Datenbank im Kontext des Labs freigeben. Die Authentifizierung bei der Ressource wird von der Identität selbst übernommen. Nach der Konfiguration wird jede vorhandene/neu erstellte Lab-VM mit dieser Identität aktiviert. Lab-Benutzer können auf Ressourcen zugreifen, sobald sie bei ihren Computern angemeldet sind.

> [!NOTE]
> Sie können mehrere benutzerseitig zugewiesene verwaltete Identitäten hinzufügen, die auf Ihren Lab-VMs aktiviert werden sollen.

## <a name="use-azure-portal"></a>Verwenden des Azure-Portals
Um eine benutzerseitig zugewiesene verwaltete Identität für Lab-VMs hinzuzufügen, führen Sie diese Schritte aus:

1. [Erstellen Sie eine benutzerseitig zugewiesene verwaltete Identität in Ihrem Abonnement](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).
1. Navigieren Sie zu der Seite **Konfiguration und Richtlinien** Ihres Labs.
1. Wählen Sie im linken Menü **Identität (Preview)** aus.
1. Wählen Sie die Registerkarte **Virtueller Computer** aus.
1. Wählen Sie **Hinzufügen** aus, um eine vorhandene Identität aus einer voraufgefüllten Dropdownliste auszuwählen. 

    > [!div class="mx-imgBorder"]
    > ![Schaltfläche „Identität hinzufügen“](./media/enable-managed-identities-lab-vms/add-identity-button.png)
1. Wählen Sie eine vorhandene **benutzerseitig verwaltete Identität** aus der Dropdownliste aus, und wählen Sie **OK**aus. 

    > [!div class="mx-imgBorder"]
    > ![Identität hinzufügen](./media/enable-managed-identities-lab-vms/add-identity.png)

## <a name="use-api"></a>Verwenden der API

1.  Notieren Sie sich nach dem Erstellen einer Identität die Ressourcen-ID der Identität. Sie sollte in etwa wie das folgende Beispiel aussehen: 

    [https://login.microsoftonline.com/consumers/](`/subscriptions/0000000000-0000-0000-0000-00000000000000/resourceGroups/<RESOURCE GROUP NAME> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/<NAME of USER IDENTITY>`).
2. Führen Sie eine PUT HTTPS-Methode aus, um dem Lab eine neue **ServiceRunner**-Ressource hinzuzufügen, wie im folgenden Beispiel gezeigt. 

    Die Service Runner-Ressource ist eine Proxyressource zum Verwalten und Steuern verwalteter Identitäten in DevTest Labs. Der Name des Service Runners kann ein beliebiger gültiger Name sein, aber es wird empfohlen, dass Sie den Namen der verwalteten Identitätsressource verwenden.

    ```json
    {
        "identity": {
            "type": "userAssigned",
            "userAssignedIdentities": { 
                "[userAssignedIdentityResourceId]": {}
            }
            },
        "location": "southeastasia",
        "properties": {
            "identityUsageType": "VirtualMachine"
        }
    }
    ```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).







