---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/06/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 57368331e83111836e13b6c3796cacad9816e318
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521287"
---
Führen Sie folgende Schritte durch, um die Zugriffsschlüssel oder Verbindungszeichenfolge Ihres Speicherkontos aus dem Azure-Portal anzuzeigen oder zu kopieren:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Suchen Sie nach Ihrem Speicherkonto.
3. Wählen Sie unter **Einstellungen** die Option **Zugriffsschlüssel** aus. Daraufhin werden Ihre Zugriffsschlüssel zusammen mit der jeweiligen vollständigen Verbindungszeichenfolge angezeigt.
4. Suchen Sie unter **key1** nach dem Wert für **Schlüssel**, und klicken Sie dann auf die Schaltfläche **Kopieren**, um den Kontoschlüssel zu kopieren.
5. Alternativ können Sie die gesamte Verbindungszeichenfolge kopieren. Suchen Sie unter **key1** nach dem Wert für die **Verbindungszeichenfolge**, und klicken Sie dann auf die Schaltfläche **Kopieren**, um die Verbindungszeichenfolge zu kopieren.

    ![Ein Screenshot, der zeigt, wie Sie Zugriffsschlüssel im Azure-Portal anzeigen](media/storage-view-keys-include/portal-connection-string.png)

Sie können einen der beiden Schlüssel verwenden, um auf Azure Storage zuzugreifen. Im Allgemeinen empfiehlt es sich jedoch, den ersten Schlüssel zu verwenden und die Verwendung des zweiten Schlüssels bei der Rotation von Schlüsseln zu reservieren.

Zum Anzeigen oder Lesen der Zugriffsschlüssel eines Kontos muss der Benutzer entweder ein Dienstadministrator sein oder ihm muss eine RBAC-Rolle zugewiesen sein, die **Microsoft.Storage/storageAccounts/listkeys/action** enthält. Einige integrierte RBAC-Rollen, die diese Aktion beinhalten, sind die Rollen **Besitzer**, **Mitwirkender** und **Dienstrolle „Speicherkonto-Schlüsseloperator“** . Weitere Informationen zur Dienstadministratorrolle finden Sie unter [Administratorrollen für klassische Abonnements, Azure RBAC-Rollen und Azure AD-Rollen](../articles/role-based-access-control/rbac-and-directory-admin-roles.md). Ausführliche Informationen zu integrierten Rollen für Azure Storage finden Sie im Artikel [In Azure integrierte Rollen für Azure RBAC](../articles/role-based-access-control/built-in-roles.md#storage) im Abschnitt **Storage**.
