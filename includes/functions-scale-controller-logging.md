---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/15/2020
ms.author: glenga
ms.openlocfilehash: 906413d0a6702e6146779f79d628b5cebf383af1
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2020
ms.locfileid: "92165763"
---
| | |
|--|--|
|**`<DESTINATION>`**| Das Ziel, an das Protokolle gesendet werden. Gültige Werte sind `AppInsights` und `Blob`.<br/>Wenn Sie `AppInsights` verwenden, vergewissern Sie sich, dass [Application Insights in der Funktions-App aktiviert ist](../articles/azure-functions/configure-monitoring.md#enable-application-insights-integration).<br/>Wenn Sie das Ziel auf `Blob` festgelegt haben, werden Protokolle in einem Blobcontainer mit dem Namen `azure-functions-scale-controller` in dem Standardspeicherkonto erstellt, das in der Anwendungseinstellung `AzureWebJobsStorage` festgelegt ist. |
|**`<VERBOSITY>`** | Gibt die Protokollierungsstufe an. Unterstützte Werte sind `None`, `Warning` und `Verbose`.<br/>Wenn diese Einstellung auf `Verbose` festgelegt ist, protokolliert der Skalierungscontroller einen Grund für jede Änderung an der Workeranzahl sowie Informationen zu den Triggern, die diese Entscheidungen beeinflussen. Ausführliche Protokolle enthalten Triggerwarnungen und die Hashes, die von den Triggern vor und nach dem Ausführen des Skalierungscontrollers verwendet werden. |

> [!TIP]
> Beachten Sie, dass es Auswirkungen auf die [potenziellen Kosten für die Überwachung Ihrer Funktions-App](../articles/azure-functions/functions-monitoring.md#application-insights-pricing-and-limits) haben kann, wenn die Protokollierung des Skalierungscontrollers weiterhin aktiviert ist. Erwägen Sie, die Protokollierung zu aktivieren, bis Sie genügend Daten gesammelt haben, um das Verhalten des Skalierungscontrollers nachzuvollziehen, und deaktivieren Sie sie dann.