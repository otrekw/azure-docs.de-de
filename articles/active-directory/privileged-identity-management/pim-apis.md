---
title: 'Microsoft Graph-APIs für PIM (Vorschauversion): Azure AD | Microsoft-Dokumentation'
description: Hier erhalten Sie Informationen zur Verwendung der Microsoft Graph-APIs für Azure AD Privileged Identity Management (PIM) (Vorschauversion).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: how-to
ms.date: 01/02/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea3e307df95fe4cb18986ed8e68a7f3381c5fbe7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84742895"
---
# <a name="microsoft-graph-apis-for-privileged-identity-management-preview"></a>Microsoft Graph-APIs für Privileged Identity Management (Vorschauversion)

Privileged Identity Management-Aufgaben können mithilfe der [Microsoft Graph-APIs](https://developer.microsoft.com/graph/docs/concepts/overview) für Azure Active Directory ausgeführt werden. In diesem Artikel werden wichtige Konzepte für die Verwendung der Microsoft Graph-APIs für Privileged Identity Management beschrieben.

Weitere Informationen zu den Microsoft Graph-APIs finden Sie in der [Referenz zur Azure AD Privileged Identity Management-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root).

> [!IMPORTANT]
> APIs in der Betaversion in Microsoft Graph befinden sich in der Vorschauphase und ändern sich möglicherweise noch. Die Verwendung dieser APIs in Produktionsanwendungen wird nicht unterstützt.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Zum Aufrufen der Microsoft Graph-APIs für Privileged Identity Management benötigen Sie **mindestens eine** der folgenden Berechtigungen:

- `Directory.AccessAsUser.All`
- `Directory.Read.All`
- `Directory.ReadWrite.All`
- `PrivilegedAccess.ReadWrite.AzureAD`

### <a name="set-permissions"></a>Festlegen von Berechtigungen

Damit Anwendungen die Microsoft Graph-APIs für Privileged Identity Management aufrufen können, benötigen sie die erforderlichen Berechtigungen. Die erforderlichen Berechtigungen werden am einfachsten über das [Azure AD-Zustimmungsframework](../develop/consent-framework.md) angegeben.

### <a name="set-permissions-in-graph-explorer"></a>Festlegen von Berechtigungen in Graph-Tester

Beim Testen Ihrer Aufrufe mit Graph-Tester können Sie die Berechtigungen im Tool angeben.

1. Melden Sie sich bei [Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) als globaler Administrator an.

1. Klicken Sie auf **Berechtigungen ändern**.

    ![Graph-Tester: Berechtigungen ändern](./media/pim-apis/graph-explorer.png)

1. Aktivieren Sie die Kontrollkästchen neben den gewünschten Berechtigungen. `PrivilegedAccess.ReadWrite.AzureAD` ist in Graph-Tester noch nicht verfügbar.

    ![Graph-Tester: Berechtigungen ändern](./media/pim-apis/graph-explorer-modify-permissions.png)

1. Klicken Sie auf **Berechtigungen ändern**, um die Änderungen der Berechtigungen zu übernehmen.

## <a name="next-steps"></a>Nächste Schritte

- [Referenz zur Azure AD Privileged Identity Management-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)
