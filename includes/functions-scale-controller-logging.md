---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/15/2020
ms.author: glenga
ms.openlocfilehash: 49818cf59da2d63cef4bb0bdca38d38a2feafca5
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86169906"
---
| | |
|--|--|
|**`<DESTINATION>`**| Das Ziel, an das Protokolle gesendet werden. Gültige Werte sind `AppInsights` und `Blob`.<br/>Wenn Sie `AppInsights` verwenden, vergewissern Sie sich, dass [Application Insights in der Funktions-App aktiviert ist](../articles/azure-functions/functions-monitoring.md#enable-application-insights-integration).<br/>Wenn Sie das Ziel auf `Blob` festgelegt haben, werden Protokolle in einem Blobcontainer mit dem Namen `azure-functions-scale-controller` in dem Standardspeicherkonto erstellt, das in der Anwendungseinstellung `AzureWebJobsStorage` festgelegt ist. |
|**`<VERBOSITY>`** | Gibt die Protokollierungsstufe an. Unterstützte Werte sind `None`, `Warning` und `Verbose`.<br/>Wenn diese Einstellung auf `Verbose` festgelegt ist, protokolliert der Skalierungscontroller einen Grund für jede Änderung an der Workeranzahl sowie Informationen zu den Triggern, die diese Entscheidungen beeinflussen. Ausführliche Protokolle enthalten Triggerwarnungen und die Hashes, die von den Triggern vor und nach dem Ausführen des Skalierungscontrollers verwendet werden. |

> [!CAUTION]
> Lassen Sie die Protokollierung des Skalierungscontrollers nicht aktiviert. Aktivieren Sie die Protokollierung, bis Sie genügend Daten gesammelt haben, um das Verhalten des Skalierungscontrollers nachzuvollziehen, und deaktivieren Sie sie dann.