---
author: baanders
description: Includedatei, die eine Codelösung für die mandantenübergreifende Einschränkung von Azure Digital Twins beschreibt.
ms.service: digital-twins
ms.topic: include
ms.date: 4/13/2021
ms.author: baanders
ms.openlocfilehash: 2702f3c70d9e6f1a0bdad8695414e2d5fab02411
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107589337"
---
Das folgende Beispiel zeigt, wie eine Mandanten-ID für `InteractiveBrowserTenantId` in den `DefaultAzureCredential`-Optionen festgelegt wird:

:::image type="content" source="../articles/digital-twins/media/troubleshoot-error-404/defaultazurecredentialoptions.png" alt-text="Screenshot: Code, der die Methode „DefaultAzureCredentialOptions“ zeigt. Der Wert von „InteractiveBrowserTenantId“ wird auf einen Beispielwert für die Mandanten-ID festgelegt.":::

In Visual Studio und Visual Studio Code stehen ähnliche Optionen zum Festlegen eines Mandanten für die Authentifizierung zur Verfügung. Weitere Informationen zu den verfügbaren Optionen finden Sie in der [DefaultAzureCredentialOptions-Dokumentation](/dotnet/api/azure.identity.defaultazurecredentialoptions?view=azure-dotnet&preserve-view=true).