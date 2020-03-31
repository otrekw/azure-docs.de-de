---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: da3793c428c624ce3a224cbd7606ab26c4a50803
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "67178594"
---
Das Mobile Apps-Feature von Azure App Service verwendet [Azure Notification Hubs] für den Versand von Pushbenachrichtigungen. Deshalb müssen Sie einen Benachrichtigungs-Hub für Ihre mobile App konfigurieren.

1. Navigieren Sie im [Azure portal] zu **App Services**, und wählen Sie dann Ihr App-Back-End aus. Wählen Sie unter **Einstellungen** die Option **Push** aus.
2. Wählen Sie **Verbinden** aus, um der App eine Notification Hub-Ressource hinzuzufügen. Sie können einen Hub erstellen oder einen vorhandenen Hub verwenden.

    ![Konfigurieren eines Hubs](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Sie haben nun einen Benachrichtigungs-Hub mit Ihrem Mobile Apps-Back-End-Projekt verbunden. Später konfigurieren Sie diesen Notification Hub so, dass eine Verbindung mit einem Plattformbenachrichtigungssystem (Platform Notification System, PNS) hergestellt wird, um Pushbenachrichtigungen an Geräte zu senden.

[Azure portal]: https://portal.azure.com/
[Azure Notification Hubs]: https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/
