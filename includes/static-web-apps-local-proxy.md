---
author: burkeholland
ms.service: static-web-apps
ms.topic: include
ms.date: 05/08/2020
ms.author: buhollan
ms.openlocfilehash: 879d0c2e8c4cd66ce04c0298d00f7d6a1987acf5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83594399"
---
#### <a name="local-proxy"></a>Lokaler Proxy

Sie können einen Proxy für die Live Server-Erweiterung für Visual Studio Code konfigurieren, von dem alle an `/api` gerichteten Anforderungen an den aktiven API-Endpunkt unter `http://127.0.0.1:7071/api` weitergeleitet werden.

1. Öffnen Sie die Datei _.vscode/settings.json_.

1. Fügen Sie die folgenden Einstellungen hinzu, um einen Proxy für Live Server anzugeben.

   ```json
   "liveServer.settings.proxy": {
      "enable": true,
      "baseUri": "/api",
      "proxyUri": "http://127.0.0.1:7071/api"
   }
   ```

   Es empfiehlt sich, diese Konfiguration nicht in der Datei mit den Benutzereinstellungen, sondern in der Datei mit den Projekteinstellungen zu speichern.

   Durch die Verwendung von Projekteinstellungen wird sichergestellt, dass der Proxy nicht für alle anderen Projekte verwendet wird, die in Visual Studio Code geöffnet sind.
