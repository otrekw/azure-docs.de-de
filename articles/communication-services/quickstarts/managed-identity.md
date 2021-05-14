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
ms.openlocfilehash: aedf54c8c958e96b2bbfa31652b4861ff452f75a
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107307438"
---
# <a name="use-managed-identities"></a>Verwenden verwalteter Identitäten
Hier erfahren Sie mehr zu den ersten Schritten bei der Verwendung verwalteter Identitäten mit Azure Communication Services. Die Identitäts- und SMS-SDKs von Communication Services unterstützen die Azure AD-Authentifizierung (Azure Active Directory) mit [verwalteten Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md).

In diesem Schnellstart erfahren Sie, wie Sie den Zugriff auf die Identitäts- und SMS-SDKs aus einer Azure-Umgebung autorisieren, die verwaltete Identitäten unterstützt. Darüber hinaus erfahren Sie, wie Sie Ihren Code in einer Entwicklungsumgebung testen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. [Kostenlos ein Konto erstellen](https://azure.microsoft.com/free)
- Eine aktive Azure Communication Services-Ressource. Informationen zum Erstellen einer Communication Services-Ressource finden Sie bei Bedarf [hier](./create-communication-resource.md).
- Eine [Telefonnummer](./telephony-sms/get-phone-number.md) zum Senden einer SMS.
- Eine verwaltete Setupidentität für eine Entwicklungsumgebung. Weitere Informationen finden Sie unter [Autorisieren des Zugriffs mit der verwalteten Identität auf die Kommunikationsressource in Ihrer Entwicklungsumgebung](./managed-identity-from-cli.md).

::: zone pivot="programming-language-csharp"
[!INCLUDE [.NET](./includes/managed-identity/managed-identity-net.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/managed-identity/managed-identity-js.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/managed-identity/managed-identity-java.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/managed-identity/managed-identity-python.md)]
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure](../../../articles/role-based-access-control/index.yml)
- [Weitere Informationen zur Azure-Identitätsbibliothek für .NET](/dotnet/api/overview/azure/identity-readme)
- [Erstellen von Benutzerzugriffstoken](../quickstarts/access-tokens.md)
- [Senden einer SMS](../quickstarts/telephony-sms/send.md)
- [Weitere Informationen zu SMS](../concepts/telephony-sms/concepts.md)
