---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.openlocfilehash: 9c972508c98e87771154bd26cc166c6bea4201ee
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279614"
---
## <a name="test"></a>Überprüfen der Funktion in Azure

Sie können die bereitgestellte Funktion mithilfe eines Webbrowsers überprüfen.  Kopieren Sie die URL (einschließlich des Funktionsschlüssels) in die Adressleiste des Webbrowsers. Hängen Sie die Abfragezeichenfolge `&name=<yourname>` an die URL an, bevor Sie die Anforderung ausführen.

![Verwenden eines Webbrowsers zum Aufrufen der Funktion](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  

Alternativ können Sie die bereitgestellte Funktion mit cURL überprüfen. Fügen Sie unter Verwendung der URL (einschließlich des Funktionsschlüssels), die Sie im vorherigen Schritt kopiert haben, die Abfragezeichenfolge `&name=<yourname>` an die URL an.

![Verwenden von cURL zum Aufrufen der Funktion in Azure](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

