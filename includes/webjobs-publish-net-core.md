---
title: Datei einfügen
description: include file
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
ms.date: 06/26/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 9013095a974a86d34399ab566bcc3fea440f2dc7
ms.sourcegitcommit: 695a33a2123429289ac316028265711a79542b1c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2021
ms.locfileid: "113125953"
---
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus.

1. Klicken Sie im Dialogfeld **Veröffentlichen** unter **Ziel** erst auf **Azure** und dann auf **Weiter**. 

1. Wählen Sie **Azure WebJobs** als **Bestimmtes Ziel** aus, und klicken Sie dann auf **Weiter**.

1. Wählen Sie über den **App Service Instanzen** die Taste mit dem Pluszeichen ( **+** ) aus, um **einen neuen Azure WebJob zu erstellen**.

1. Verwenden Sie im Dialogfeld **App Service (Windows)** die Hostingeinstellungen aus der folgenden Tabelle.

    | Einstellung      | Vorgeschlagener Wert  | Beschreibung                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | Global eindeutiger Name | Name, der Ihre neue Funktions-App eindeutig identifiziert. |
    | **Abonnement** | Auswählen Ihres Abonnements | Das zu verwendende Azure-Abonnement. |
    | **[Ressourcengruppe](../articles/azure-resource-manager/management/overview.md)** | myResourceGroup |  Der Name der Ressourcengruppe, in der Ihre Funktions-App erstellt werden soll. Wählen Sie **Neu** aus, um eine neue Ressourcengruppe zu erstellen.|
    | **[Hostingplan](../articles/app-service/overview-hosting-plans.md)** | App Service-Plan | Ein [App Service-Plan](../articles/app-service/overview-hosting-plans.md) gibt den Standort, die Größe und die Funktionen der Webserverfarm an, die Ihre App hostet. Beim Hosten mehrerer Apps können Sie Geld sparen, indem Sie die Web-Apps für die gemeinsame Verwendung eines einzelnen App Service-Plans konfigurieren. App Service-Pläne definieren Region, Instanzgröße, Skalierung und SKU (Free, Shared, Basic, Standard oder Premium). Wählen Sie **New** aus, um einen neuen App Service-Plan zu erstellen. Die kostenlosen und einfachen Ebenen unterstützen die Option „Always On“ nicht, damit Ihre Website kontinuierlich ausgeführt wird. |

    ![Dialogfeld „App Service erstellen“](./media/webjobs-publish-netcore/app-service-dialog.png)

1. Klicken Sie auf **Erstellen**, um eine WebJob-Instanz und zugehörige Ressourcen in Azure mit diesen Einstellungen zu erstellen und Ihren Projektcode bereitzustellen.

1. Wählen Sie **Fertig stellen** aus, um zur Seite **Veröffentlichen** zurückzukehren.  