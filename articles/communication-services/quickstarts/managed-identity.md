---
title: Verwenden von verwalteten Identitäten in Communication Services
titleSuffix: An Azure Communication Services quickstart
description: Mithilfe von verwalteten Identitäten können Sie den Zugriff auf Azure Communication Services aus Anwendungen autorisieren, die auf Azure-VMs, in Funktions-Apps und in anderen Ressourcen ausgeführt werden.
services: azure-communication-services
author: peiliu
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 03/10/2021
ms.author: peiliu
ms.reviewer: mikben
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: ffda88da451e25b79112a7adf85026158bd27acc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103492352"
---
# <a name="use-managed-identities"></a>Verwenden verwalteter Identitäten
Hier erfahren Sie mehr zu den ersten Schritten bei der Verwendung verwalteter Identitäten mit Azure Communication Services. Die Identitäts- und SMS-Clientbibliotheken von Communication Services unterstützen die Azure AD-Authentifizierung (Azure Active Directory) mit [verwalteten Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md).

In diesem Schnellstart erfahren Sie, wie Sie den Zugriff auf die Identitäts- und SMS-Clientbibliotheken aus einer Azure-Umgebung autorisieren, die verwaltete Identitäten unterstützt. Darüber hinaus erfahren Sie, wie Sie Ihren Code in einer Entwicklungsumgebung testen.

::: zone pivot="programming-language-csharp"
[!INCLUDE [.NET](./includes/managed-identity-net.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/managed-identity-js.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/managed-identity-java.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/managed-identity-python.md)]
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure](../../../articles/role-based-access-control/index.yml)
- [Weitere Informationen zur Azure-Identitätsbibliothek für .NET](/dotnet/api/overview/azure/identity-readme)
- [Erstellen von Benutzerzugriffstoken](../quickstarts/access-tokens.md)
- [Senden einer SMS](../quickstarts/telephony-sms/send.md)
- [Weitere Informationen zu SMS](../concepts/telephony-sms/concepts.md)
