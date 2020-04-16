---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: a84f0a92703d1b626710cfc4dcfa2820bc58bda6
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673208"
---
## <a name="configure-your-local-environment"></a>Konfigurieren Ihrer lokalen Umgebung

Bevor Sie mit diesem Lernprogramm beginnen können, benötigen Sie Folgendes:

+ Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell"  
+ [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md#v2), Version 2.7.1846 oder höhere 2.x-Version.
::: zone-end  
::: zone pivot="programming-language-python"
+ Für Python 3.6 und 3.7 ist Version 2.7.1846 oder eine höhere 2.x-Version von [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md#v2) erforderlich. Für Python 3.8 ist [Version 3.x](../articles/azure-functions/functions-run-local.md#v2) von Core Tools erforderlich.
::: zone-end

+ [Azure CLI](/cli/azure/install-azure-cli), Version 2.0.76 oder höher. 
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node.js](https://nodejs.org/), Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1 empfohlen)
::: zone-end

::: zone pivot="programming-language-python"
+ [Python 3.8 (64 Bit)](https://www.python.org/downloads/release/python-382/), [Python 3.7 (64 Bit)](https://www.python.org/downloads/release/python-375/), [Python 3.6 (64 Bit)](https://www.python.org/downloads/release/python-368/). Diese Versionen werden von Azure Functions unterstützt. 
::: zone-end
::: zone pivot="programming-language-powershell"
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ [.NET Core SDK 2.2+](https://www.microsoft.com/net/download)
::: zone-end
::: zone pivot="programming-language-java"  
+ [Java Developer Kit](https://aka.ms/azure-jdks), Version 8

+ [Apache Maven](https://maven.apache.org), Version 3.0 oder höher

> [!IMPORTANT]
> Damit Sie diesen Schnellstart durchführen können, muss die Umgebungsvariable JAVA_HOME auf den Installationsspeicherort des JDK festgelegt sein.
::: zone-end