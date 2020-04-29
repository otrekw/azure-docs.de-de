---
title: Verwenden des Azure-Portals zum Zuweisen einer RBAC-Rolle für den Datenzugriff
titleSuffix: Azure Storage
description: Hier erfahren Sie, wie Sie mit dem Azure-Portal Berechtigungen für einen Azure Active Directory-Sicherheitsprinzipal mit rollenbasierter Zugriffssteuerung (Role-Based Access Control, RBAC) zuweisen. Azure Storage unterstützt für die Authentifizierung über Azure AD integrierte und benutzerdefinierte RBAC-Rollen.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/31/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: d224bd9e9e7b1f8fc9eb45d85e78811d8642fc78
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80519563"
---
# <a name="use-the-azure-portal-to-assign-an-rbac-role-for-access-to-blob-and-queue-data"></a>Verwenden des Azure-Portals zum Zuweisen einer RBAC-Rolle für den Zugriff auf Blob- und Warteschlangendaten

Azure Active Directory (Azure AD) autorisiert Rechte für den Zugriff auf abgesicherte Ressourcen über die [rollenbasierte Zugriffssteuerung (RBAC)](../../role-based-access-control/overview.md). Azure Storage bietet eine Reihe integrierter RBAC-Rollen mit allgemeinen Berechtigungssätzen für den Zugriff auf Blob- und Warteschlangendaten.

Wenn einem Azure AD-Sicherheitsprinzipal eine RBAC-Rolle zugewiesen wird, gewährt Azure diesem Sicherheitsprinzipal Zugriff auf diese Ressourcen. Der Zugriff kann auf die Ebene des Abonnements, der Ressourcengruppe, des Speicherkontos oder eines einzelnen Containers oder einer Warteschlange begrenzt werden. Eine Azure AD-Sicherheitsprinzipal kann ein Benutzer, eine Gruppe, ein Anwendungsdienstprinzipal oder eine [verwaltete Identität für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md) sein.

In diesem Artikel wird beschrieben, wie Sie im Azure-Portal RBAC-Rollen zuweisen. Das Azure-Portal bietet eine einfache Benutzeroberfläche für die Zuweisung von RBAC-Rollen und die Verwaltung des Zugriffs auf Ihre Speicherressourcen. Sie können RBAC-Rollen auch für Blob- und Warteschlangenressourcen mithilfe von Azure-Befehlszeilentools oder Azure Storage-Verwaltungs-APIs zuweisen. Weitere Informationen zu RBAC-Rollen für Speicherressourcen finden Sie unter [Authentifizieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory](storage-auth-aad.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC-Rollen für Blobs und Warteschlangen

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Bestimmen des Ressourcenumfangs

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="assign-rbac-roles-using-the-azure-portal"></a>Zuweisen von RBAC-Rollen im Azure-Portal

Nachdem Sie den gewünschten Umfang für eine Rollenzuweisung festgelegt haben, navigieren Sie im Azure-Portal zur entsprechenden Ressource. Zeigen Sie die Einstellungen für die **Zugriffssteuerung (IAM)** für die Ressource an, und befolgen Sie diese Anweisungen zum Verwalten von Rollenzuweisungen:

1. Weisen Sie die entsprechende Azure Storage-RBAC-Rolle zu, um einem Azure AD-Sicherheitsprinzipal Zugriff zu gewähren.

1. Weisen Sie die Azure Resource Manager-Rolle [Leser](../../role-based-access-control/built-in-roles.md#reader) Benutzern zu, die über das Azure-Portal mit ihren Azure AD-Anmeldeinformationen auf Container oder Warteschlangen zugreifen müssen. 

In den folgenden Abschnitten werden diese Schritte ausführlicher beschrieben.

> [!NOTE]
> Als Besitzer Ihres Azure Storage-Kontos erhalten Sie nicht automatisch Berechtigungen für den Zugriff auf Daten. Sie müssen sich selbst explizit eine RBAC-Rolle für Azure Storage zuweisen. Sie können sie auf der Ebene Ihres Abonnements, einer Ressourcengruppe, eines Speicherkontos oder eines Containers oder einer Warteschlange zuordnen.
>
> Sie können einem Container oder einer Warteschlange keine Rolle zuweisen, wenn in Ihrem Speicherkonto ein hierarchischer Namespace aktiviert ist.

### <a name="assign-a-built-in-rbac-role"></a>Zuweisen einer integrierten RBAC-Rolle

Bevor Sie einem Sicherheitsprinzipal eine Rolle zuweisen, stellen Sie sicher, dass Sie den Umfang der Berechtigungen berücksichtigen, die Sie erteilen. Legen Sie mithilfe des Abschnitts [Bestimmen des Ressourcenumfangs](#determine-resource-scope) den gewünschten Umfang fest.

Die hier gezeigte Vorgehensweise weist eine auf einen Container begrenzte Rolle zu, aber Sie können die gleichen Schritte ausführen, um eine auf eine Warteschlange begrenzte Rolle zuzuweisen:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Speicherkonto, und zeigen Sie die **Übersicht** für das Konto an.
1. Wählen Sie unter „Dienste“ die Option **Blobs** aus.
1. Navigieren Sie zum Container, dem Sie eine Rolle zuweisen möchten, und zeigen Sie die Einstellungen des Containers an.
1. Wählen Sie **Zugriffssteuerung (IAM)** aus, um Zugriffssteuerungseinstellungen für den Container anzuzeigen. Wählen Sie die Registerkarte **Rollenzuweisungen** aus, um die Liste mit den Rollenzuweisungen anzuzeigen.

    ![Screenshot mit Zugriffssteuerungseinstellungen für den Container](media/storage-auth-aad-rbac-portal/portal-access-control-for-storage.png)

1. Klicken Sie auf die Schaltfläche **Rollenzuweisung hinzufügen**, um eine neue Rolle hinzuzufügen.
1. Wählen Sie im Fenster **Rollenzuweisung hinzufügen** die Azure Storage-Rolle aus, die Sie zuweisen möchten. Suchen Sie dann den Sicherheitsprinzipal, dem Sie diese Rolle zuweisen möchten.

    ![Screenshot: Zuweisen einer RBAC-Rolle](media/storage-auth-aad-rbac-portal/add-rbac-role.png)

1. Klicken Sie auf **Speichern**. Die Identität, der Sie die Rolle zugewiesen haben, wird unter dieser Rolle angezeigt. Die folgende Abbildung zeigt z.B., dass der hinzugefügte Benutzer nun Leseberechtigungen für Daten im Container *sample-container* hat.

    ![Screenshot mit einer Liste von Benutzern, denen eine Rolle zugewiesen ist](media/storage-auth-aad-rbac-portal/container-scoped-role.png)

Sie können ähnliche Schritte ausführen, um eine Rolle zuzuweisen, deren Umfang für ein Speicherkonto, eine Ressourcengruppe oder ein Abonnement gilt.

### <a name="assign-the-reader-role-for-portal-access"></a>Zuweisen der Rolle „Leser“ für den Portalzugriff

Wenn Sie einem Sicherheitsprinzipal eine integrierte oder benutzerdefinierte Rolle für Azure Storage zuweisen, erteilen Sie diesem Sicherheitsprinzipal die Berechtigung, Vorgänge auf Daten in Ihrem Speicherkonto anzuwenden. Die integrierten Rollen des Typs **Datenleser** bieten Leserechte für die Daten in einem Container oder einer Warteschlange, während die integrierten Rollen des Typs **Mitwirkender an Daten** Lese-, Schreib- und Löschrechte für einen Container oder eine Warteschlange erteilen. Berechtigungen beziehen sich auf die angegebene Ressource.  
Wenn Sie beispielsweise der Benutzerin Mary die Rolle **Mitwirkender an Storage-Warteschlangendaten** auf der Ebene eines Containers namens **sample-container** zuweisen, erhält Mary Lese-, Schreib- und Löschzugriff auf alle Blobs in diesem Container.

Wenn Mary jedoch ein Blob im Azure-Portal anzeigen möchte, bietet die Rolle **Mitwirkender an Storage-Warteschlangendaten** allein nicht genügend Berechtigungen, um im Portal zum Blob zu navigieren, um ihn anzuzeigen. Zusätzliche Azure AD-Berechtigungen sind erforderlich, um durch das Portal zu navigieren und die anderen Ressourcen anzuzeigen, die dort sichtbar sind.

Wenn Ihre Benutzer auf Blobs im Azure-Portal zugreifen können müssen, weisen Sie ihnen auf der Ebene des Speicherkontos oder höher die zusätzliche RBAC-Rolle [Leser](../../role-based-access-control/built-in-roles.md#reader) zu. Die Rolle **Leser** ist eine Azure Resource Manager-Rolle, die es Benutzern ermöglicht, Ressourcen im Speicherkonto anzuzeigen, ohne sie ändern zu können. Sie bietet keine Leseberechtigungen für Daten in Azure Storage, sondern nur für Ressourcen zur Kontoverwaltung.

Führen Sie diese Schritte aus, um die Rolle **Leser** so zuzuweisen, dass ein Benutzer im Azure-Portal auf Blobs zugreifen kann. In diesem Beispiel ist die Zuweisung auf das Speicherkonto beschränkt:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Speicherkonto.
1. Wählen Sie **Zugriffssteuerung (IAM)** aus, um Zugriffssteuerungseinstellungen für das Speicherkonto anzuzeigen. Wählen Sie die Registerkarte **Rollenzuweisungen** aus, um die Liste mit den Rollenzuweisungen anzuzeigen.
1. Wählen Sie im Fenster **Rollenzuweisung hinzufügen** die Rolle **Leser** aus. 
1. Wählen Sie im Feld **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal** aus.
1. Suchen Sie den Sicherheitsprinzipal, dem Sie die Rolle zuweisen möchten.
1. Speichern Sie die Rollenzuweisung.

Die Zuweisung der Rolle **Leser** ist nur für Benutzer erforderlich, die über das Azure-Portal auf Blobs oder Warteschlangen zugreifen müssen.

> [!IMPORTANT]
> Die Vorschauversion von Azure Storage-Explorer im Azure-Portal unterstützt nicht die Verwendung von Azure AD-Anmeldeinformationen zum Anzeigen und Ändern von Blob- und Warteschlangendaten. Storage-Explorer im Azure-Portal verwendet immer die Kontoschlüssel für den Zugriff auf Daten. Um Storage-Explorer im Azure-Portal verwenden zu können, muss Ihnen eine Rolle zugewiesen werden, in der **Microsoft.Storage/storageAccounts/listkeys/action** enthalten ist.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu RBAC-Rollen für Speicherressourcen finden Sie unter [Authentifizieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory](storage-auth-aad.md). 
- Weitere Informationen zur rollenbasierte Zugriffssteuerung finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)?](../../role-based-access-control/overview.md).
- Informationen zum Zuweisen und Verwalten von RBAC-Rollenzuweisungen mit Azure PowerShell, Azure CLI oder der REST-API finden Sie in diesen Artikeln:
    - [Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
    - [Verwalten der rollenbasierten Zugriffssteuerung mit der Azure-Befehlszeilenschnittstelle](../../role-based-access-control/role-assignments-cli.md)
    - [Verwalten der rollenbasierten Zugriffssteuerung mit der REST-API](../../role-based-access-control/role-assignments-rest.md)
- Informationen zum Autorisieren des Zugriffs auf Container und Warteschlangen in Ihren Speicheranwendungen finden Sie unter [Verwenden von Azure AD mit Azure Storage-Anwendungen](storage-auth-aad-app.md).
