---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: 5e3c4622131528fc2c40a1510aeea3092018d182
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "77474220"
---
### <a name="install-via-composer"></a>Installation mithilfe von Composer
1. Erstellen Sie im Stammverzeichnis Ihres Projekts eine Datei namens **composer.json** , und fügen Sie zu dieser den folgenden Code hinzu:
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "*"
      }
    }
    ```
2. Laden Sie **[composer.phar][composer-phar]** in das Stammverzeichnis Ihres Projekts herunter.
3. Öffnen Sie eine Eingabeaufforderung und führen Sie in Ihrem Projektverzeichnis folgenden Befehl aus
   
    ```
    php composer.phar install
    ```

Navigieren Sie alternativ zur [Azure Storage PHP-Clientbibliothek][php-sdk-github] auf GitHub, um den Quellcode zu klonen.

[php-sdk-github]: https://github.com/Azure/azure-storage-php
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: https://github.com/Azure/azure-sdk-for-php
[composer-phar]: http://getcomposer.org/composer.phar
