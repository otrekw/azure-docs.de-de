---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: a5f829fc0e153c3a427fd1c7cc7d5c613cd624f6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "83648269"
---
Azure Functions unterstützt jetzt die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS). CORS wird [im Portal](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors) und über die [Azure-Befehlszeilenschnittstelle](/cli/azure/functionapp/cors) konfiguriert. Die CORS-Liste der zulässigen Ursprünge gilt auf Funktions-App-Ebene. Wenn CORS aktiviert ist, enthalten Antworten den `Access-Control-Allow-Origin`-Header. Weitere Informationen finden Sie unter [Cross-Origin Resource Sharing (CORS)](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors). 