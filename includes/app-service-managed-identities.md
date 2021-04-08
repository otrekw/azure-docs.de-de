---
author: mattchenderson
ms.service: app-service
ms.topic: include
ms.date: 04/20/2020
ms.author: mahender
ms.openlocfilehash: 3b282a99bb7f6f107717d9c265a46d285d03b849
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "83649080"
---
Über eine verwaltete Identität aus Azure Active Directory (Azure AD) kann Ihre App einfach auf andere durch Azure AD geschützte Ressourcen wie Azure Key Vault zugreifen. Da die Identität von der Azure-Plattform verwaltet wird, müssen Sie keine Geheimnisse bereitstellen oder rotieren. Weitere Informationen zu verwalteten Identitäten in Azure AD finden Sie unter [Verwaltete Identitäten für Azure-Ressourcen](../articles/active-directory/managed-identities-azure-resources/overview.md).

Ihrer Anwendung können zwei Arten von Identitäten zugewiesen werden:

- Eine **systemseitig zugewiesene Identität** ist an Ihre Anwendung gebunden und wird gelöscht, wenn Ihre App gelöscht wird. Eine App kann nur über eine systemseitig zugewiesene Identität verfügen.
- Eine **benutzerseitig zugewiesene Identität** ist eine eigenständige Azure-Ressource, die Ihrer App zugewiesen werden kann. Eine App kann über mehrere benutzerseitig zugewiesene Identitäten verfügen.