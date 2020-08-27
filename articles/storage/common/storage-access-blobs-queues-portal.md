---
title: Auswählen der Autorisierung des Zugriffs auf Blob- oder Warteschlangendaten im Azure-Portal
titleSuffix: Azure Storage
description: Wenn Sie über das Azure-Portal auf Blob- oder Warteschlangendaten zugreifen, richtet das Portal tatsächlich Anforderungen an Azure Storage. Diese Anforderungen an Azure Storage können entweder mit Ihrem Azure AD-Konto oder Zugriffsschlüssel für das Speicherkonto authentifiziert und autorisiert werden.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 08/18/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.custom: contperfq1
ms.openlocfilehash: 7ab0899f49fc28d787e6b53945558709353c4512
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88691444"
---
# <a name="choose-how-to-authorize-access-to-blob-or-queue-data-in-the-azure-portal"></a>Auswählen der Autorisierung des Zugriffs auf Blob- oder Warteschlangendaten im Azure-Portal

Wenn Sie über das [Azure-Portal](https://portal.azure.com) auf Blob- oder Warteschlangendaten zugreifen, richtet das Portal tatsächlich Anforderungen an Azure Storage. Eine Anforderung an Azure Storage kann entweder mit Ihrem Azure AD-Konto oder Zugriffsschlüssel für das Speicherkonto autorisiert werden. Das Portal zeigt an, welche Methode Sie verwenden, und ermöglicht Ihnen, zwischen den beiden zu wechseln, sofern Sie die entsprechenden Berechtigungen haben.  

Sie können auch angeben, wie ein einzelner Blob-Uploadvorgang im Azure-Portal autorisiert werden soll. Das Portal verwendet standardmäßig die Methode, die Sie zum Autorisieren von Blob-Uploadvorgängen verwenden. Sie haben jedoch die Möglichkeit, diese Einstellung beim Hochladen eines Blobs zu ändern.

## <a name="permissions-needed-to-access-blob-or-queue-data"></a>Für den Zugriff auf Blob- oder Warteschlangendaten benötigte Berechtigungen

Je nachdem, wie Sie den Zugriff auf Blob- oder Warteschlangendaten im Azure-Portal autorisieren möchten, benötigen Sie spezielle Berechtigungen. Meist werden diese Berechtigungen über die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) bereitgestellt. Weitere Informationen zu RBAC finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../../role-based-access-control/overview.md).

### <a name="use-the-account-access-key"></a>Verwenden des Kontozugriffsschlüssels

Für den Zugriff auf Blob- und Warteschlangendaten mit dem Kontozugriffsschlüssel muss Ihnen eine Azure-Rolle zugewiesen sein, die die RBAC-Aktion **Microsoft.Storage/storageAccounts/listkeys/action** einschließt. Bei dieser Azure-Rolle kann es sich um eine integrierte oder benutzerdefinierte Rolle handeln. Es folgen integrierte Rollen mit Unterstützung für **Microsoft.Storage/storageAccounts/listkeys/action**:

- Die Azure Resource Manager-Rolle [Besitzer](../../role-based-access-control/built-in-roles.md#owner)
- Die Azure Resource Manager-Rolle [Mitwirkender](../../role-based-access-control/built-in-roles.md#contributor)
- Die Rolle [Speicherkontomitwirkender](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

Wenn Sie versuchen, im Azure-Portal auf Blob- oder Warteschlangendaten zuzugreifen, prüft das Portal zunächst, ob Ihnen eine Rolle mit **Microsoft.Storage/storageAccounts/listkeys/action** zugewiesen ist. Wenn Ihnen eine Rolle mit dieser Aktion zugewiesen wurde, verwendet das Portal den Kontoschlüssel für den Zugriff auf Blob- und Warteschlangendaten. Wenn Ihnen keine Rolle mit dieser Aktion zugewiesen wurde, versucht das Portal, über Ihr Azure AD-Konto auf Daten zuzugreifen.

> [!NOTE]
> Die zu „Administrator für klassisches Abonnement“ gehörigen Rollen „Dienstadministrator“ und „Co-Administrator“ schließen die Entsprechung der Azure Resource Manager-Rolle [Besitzer](../../role-based-access-control/built-in-roles.md#owner) ein. Die Rolle **Besitzer** schließt alle Aktionen einschließlich **Microsoft.Storage/storageAccounts/listkeys/action** ein, sodass ein Benutzer mit einer dieser Administratorrollen auch mit dem Kontoschlüssel auf Blob- und Warteschlangendaten zugreifen kann. Weitere Informationen finden Sie unter [Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Rollen](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles).

### <a name="use-your-azure-ad-account"></a>Verwenden des Azure AD-Kontos

Für den Zugriff auf Blob- oder Warteschlangendaten im Azure-Portal über Ihr Azure AD-Konto müssen Sie die beiden folgenden Voraussetzungen erfüllen:

- Ihnen wurde die Azure Resource Manager-Rolle [Leser](../../role-based-access-control/built-in-roles.md#reader) mindestens bis zur Ebene des Speicherkontos oder höher zugewiesen. Die Rolle **Leser** erteilt die am stärksten eingeschränkten Berechtigungen. Eine andere Azure Resource Manager-Rolle, die den Zugriff auf Ressourcen zur Verwaltung von Speicherkonten gewährt, ist jedoch ebenfalls akzeptabel.
- Ihnen wurde entweder eine integrierte oder benutzerdefinierte Rolle zugewiesen, die den Zugriff auf Blob- oder Warteschlangendaten ermöglicht.

Die Zuweisung der Rolle **Leser** oder einer anderen Azure Resource Manager-Rollen ist notwendig, damit der Benutzer die Ressourcen der Speicherkontenverwaltung im Azure-Portal anzeigen und nutzen kann. Die Azure-Rollen, die Zugriff auf Blob- oder Warteschlangendaten gewähren, lassen keinen Zugriff auf Ressourcen zur Verwaltung von Speicherkonten zu. Um im Portal auf Blob- oder Warteschlangendaten zugreifen zu können, benötigt der Benutzer Berechtigungen zum Navigieren durch die Ressourcen des Speicherkontos. Weitere Informationen zu dieser Anforderung finden Sie unter [Zuweisen der Rolle „Leser“ für den Zugriff auf das Portal](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access).

Es folgen die integrierten Rollen, die den Zugriff auf Ihre Blob- oder Warteschlangendaten unterstützen:

- [Besitzer von Speicherblobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): Dient zur Steuerung des POSIX-Zugriffs für Azure Data Lake Storage Gen2.
- [Mitwirkender an Speicherblobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Weist Lese-, Schreib- und Löschberechtigungen für Blobs zu.
- [Leser von Speicherblobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): Weist ausschließlich Leseberechtigungen für Blobs zu.
- [Mitwirkender an Storage-Warteschlangendaten](../../role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Weist Lese-, Schreib- und Löschberechtigungen für Warteschlangen zu.
- [Storage-Warteschlangendatenleser](../../role-based-access-control/built-in-roles.md#storage-queue-data-reader): Weist ausschließlich Leseberechtigungen für Warteschlangen zu.

Benutzerdefinierte Rollen können verschiedene Kombinationen der von den integrierten Rollen gebotenen Berechtigungen unterstützen. Weitere Informationen zum Erstellen benutzerdefinierter Azure-Rollen finden Sie unter [Benutzerdefinierte Azure-Rollen](../../role-based-access-control/custom-roles.md) und [Grundlegendes zu Rollendefinitionen für Azure-Ressourcen](../../role-based-access-control/role-definitions.md).

Das Auflisten von Warteschlangen wird von der Rolle „Administrator für klassisches Abonnement“ nicht unterstützt. Um Warteschlangen auflisten zu können, muss einem Benutzer die Azure Resource Manager-Rolle **Leser**, die Rolle **Storage-Warteschlangendatenleser** oder die Rolle **Mitwirkender an Storage-Warteschlangendaten** zugewiesen sein.

> [!IMPORTANT]
> Die Vorschauversion von Azure Storage-Explorer im Azure-Portal unterstützt nicht die Verwendung von Azure AD-Anmeldeinformationen zum Anzeigen und Ändern von Blob- und Warteschlangendaten. Storage-Explorer im Azure-Portal verwendet immer die Kontoschlüssel für den Zugriff auf Daten. Um Storage-Explorer im Azure-Portal verwenden zu können, muss Ihnen eine Rolle zugewiesen werden, in der **Microsoft.Storage/storageAccounts/listkeys/action** enthalten ist.

## <a name="navigate-to-blobs-or-queues-in-the-portal"></a>Navigieren zu Blobs oder Warteschlangen im Portal

Um Blob- oder Warteschlangendaten im Portal anzuzeigen, navigieren Sie zur **Übersicht** Ihres Speicherkontos und klicken auf die Links zu **Blobs** oder **Warteschlangen**. Alternativ können Sie im Menü zu den Abschnitten **Blob-Dienst** und **Warteschlangendienst** navigieren. 

![Navigieren zu Blob- oder Warteschlangendaten im Azure-Portal](media/storage-access-blobs-queues-portal/blob-queue-access.png)

## <a name="determine-the-current-authentication-method"></a>Bestimmen der aktuellen Authentifizierungsmethode

Wenn Sie zu einem Container oder einer Warteschlange navigieren, zeigt das Azure-Portal an, ob Sie zur Authentifizierung derzeit den Kontozugriffsschlüssel oder Ihr Azure AD-Konto verwenden.

Die Beispiele in diesem Abschnitt zeigen den Zugriff auf einen Container und dessen Blobs, aber das Portal zeigt die gleiche Meldung an, wenn Sie auf eine Warteschlange und deren Nachrichten zugreifen oder Warteschlangen auflisten.

### <a name="authenticate-with-the-account-access-key"></a>Authentifizierung mit dem Kontozugriffsschlüssel

Wenn Sie sich mit dem Kontozugriffsschlüssel authentifizieren, wird im Portal **Zugriffsschlüssel** als Authentifizierungsmethode angegeben:

![Derzeit erfolgt der Zugriff auf Containerdaten mit dem Kontoschlüssel](media/storage-access-blobs-queues-portal/auth-method-access-key.png)

Um zur Verwendung des Azure AD-Kontos zu wechseln, klicken Sie auf den in der Abbildung hervorgehobenen Link. Wenn Sie über die Ihnen zugewiesenen Azure-Rollen die entsprechenden Berechtigungen haben, können Sie fortfahren. Wenn Ihnen jedoch die benötigten Berechtigungen fehlen, erhalten Sie eine Fehlermeldung wie die folgende:

![Angezeigter Fehler, wenn das Azure AD-Konto den Zugriff nicht unterstützt](media/storage-access-blobs-queues-portal/auth-error-azure-ad.png)

Beachten Sie, dass in der Liste keine Blobs enthalten sind, wenn Ihrem Azure AD-Konto die Berechtigung zu ihrer Anzeige fehlt. Klicken Sie auf den Link **Zum Zugriffsschlüssel wechseln**, um erneut den Zugriffsschlüssel für die Authentifizierung zu nutzen.

### <a name="authenticate-with-your-azure-ad-account"></a>Authentifizierung mit dem Azure AD-Konto

Wenn Sie sich mit Ihrem Azure AD-Konto authentifizieren, wird im Portal **Azure AD-Benutzerkonto** als Authentifizierungsmethode angegeben:

![Derzeit erfolgt der Zugriff auf Containerdaten mit dem Azure AD-Konto](media/storage-access-blobs-queues-portal/auth-method-azure-ad.png)

Um zur Verwendung des Zugriffsschlüssels zu wechseln, klicken Sie auf den in der Abbildung hervorgehobenen Link. Wenn Sie Zugriff auf den Kontoschlüssel haben, können Sie fortfahren. Wenn Sie keinen Zugriff auf den Kontoschlüssel haben, erhalten Sie eine Fehlermeldung wie die folgende:

![Angezeigter Fehler, wenn Sie keinen Zugriff auf den Kontoschlüssel haben](media/storage-access-blobs-queues-portal/auth-error-access-key.png)

Beachten Sie, dass in der Liste keine Blobs enthalten sind, wenn Sie keinen Zugriff auf die Kontoschlüssel haben. Klicken Sie auf den Link **Zum Azure AD-Benutzerkonto wechseln**, um erneut das Azure AD-Konto für die Authentifizierung zu nutzen.

## <a name="specify-how-to-authorize-a-blob-upload-operation"></a>Angeben, wie ein Blob-Uploadvorgang autorisiert werden soll

Wenn Sie ein Blob aus dem Azure-Portal hochladen, können Sie angeben, ob Sie diesen Vorgang mit dem Kontozugriffsschlüssel oder mit Ihren Azure AD-Anmeldeinformationen authentifizieren und autorisieren möchten. Das Portal verwendet wie unter [Bestimmen der aktuellen Authentifizierungsmethode](#determine-the-current-authentication-method) beschrieben standardmäßig die aktuelle Authentifizierungsmethode.

So geben Sie an, wie ein Blob-Uploadvorgang autorisiert werden soll

1. Navigieren Sie im Azure-Portal zu dem Container, in den Sie ein Blob hochladen möchten.
1. Wählen Sie die Schaltfläche **Hochladen**.
1. Erweitern Sie den Abschnitt **Erweitert**, um die erweiterten Eigenschaften für das Blob anzuzeigen.
1. Geben Sie im Feld **Authentifizierungstyp** an, ob Sie den Uploadvorgang mithilfe Ihres Azure AD-Kontos oder mit dem Kontozugriffsschlüssel autorisieren möchten, wie in der folgenden Abbildung dargestellt:

    :::image type="content" source="media/storage-access-blobs-queues-portal/auth-blob-upload.png" alt-text="Screenshot: Ändern der Autorisierungsmethode beim Hochladen von Blobs":::

## <a name="next-steps"></a>Nächste Schritte

- [Authentifizieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory](storage-auth-aad.md)
- [Gewähren von Zugriff auf Azure-Container und -Warteschlangen im Azure-Portal mithilfe von RBAC](storage-auth-aad-rbac-portal.md)
- [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über die Azure CLI](storage-auth-aad-rbac-cli.md)
- [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über PowerShell](storage-auth-aad-rbac-powershell.md)
