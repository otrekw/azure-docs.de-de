---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 4ace70abe0112e0fe27d177c02bcb697746c92cc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "78262021"
---
### <a name="set-the-storage-account-connection"></a>Festlegen der Speicherkontoverbindung

Öffnen Sie die Datei „local.settings.json“, und kopieren Sie den Wert von `AzureWebJobsStorage`, wobei es sich um die Verbindungszeichenfolge des Speicherkontos handelt. Legen Sie die Umgebungsvariable `AZURE_STORAGE_CONNECTION_STRING` mit dem folgenden Bash-Befehl auf die Verbindungszeichenfolge fest:

```bash
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

Wenn Sie die Verbindungszeichenfolge in der Umgebungsvariablen `AZURE_STORAGE_CONNECTION_STRING` festlegen, können Sie auf Ihr Speicherkonto zugreifen, ohne sich jedes Mal authentifizieren zu müssen.