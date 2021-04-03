---
title: include file
description: include file
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
ms.date: 06/26/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: e208b52c67f173bd0d289715b63562df656b1ec9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88009762"
---
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus.

1. Klicken Sie im Dialogfeld **Veröffentlichen** unter **Ziel** erst auf **Azure** und dann auf **Weiter**. 

1. Wählen Sie **Azure WebJobs** als **Bestimmtes Ziel** aus, und klicken Sie dann auf **Weiter**.

1. Klicken Sie auf **Create a new Azure WebJob** (Neue Azure WebJob-Instanz erstellen).

   ![Auswählen eines Veröffentlichungsziels](./media/webjobs-publish-netcore/pick-publish-target.png)

1. Verwenden Sie im Dialogfeld **App Service (Windows)** die Hostingeinstellungen aus der folgenden Tabelle.

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | Global eindeutiger Name | Name, der Ihre neue Funktions-App eindeutig identifiziert. |
    | **Abonnement** | Auswählen Ihres Abonnements | Das zu verwendende Azure-Abonnement. |
    | **[Ressourcengruppe](../articles/azure-resource-manager/management/overview.md)** | myResourceGroup |  Der Name der Ressourcengruppe, in der Ihre Funktions-App erstellt werden soll. Wählen Sie **Neu** aus, um eine neue Ressourcengruppe zu erstellen.|
    | **[Hostingplan](../articles/app-service/overview-hosting-plans.md)** | App Service-Plan | Ein [App Service-Plan](../articles/app-service/overview-hosting-plans.md) gibt den Standort, die Größe und die Funktionen der Webserverfarm an, die Ihre App hostet. Beim Hosten mehrerer Apps können Sie Geld sparen, indem Sie die Web-Apps für die gemeinsame Verwendung eines einzelnen App Service-Plans konfigurieren. App Service-Pläne definieren Region, Instanzgröße, Skalierung und SKU (Free, Shared, Basic, Standard oder Premium). Wählen Sie **New** aus, um einen neuen App Service-Plan zu erstellen. |

    ![Dialogfeld „App Service erstellen“](./media/webjobs-publish-netcore/app-service-dialog.png)

1. Klicken Sie auf **Erstellen**, um eine WebJob-Instanz und zugehörige Ressourcen in Azure mit diesen Einstellungen zu erstellen und Ihren Projektcode bereitzustellen.