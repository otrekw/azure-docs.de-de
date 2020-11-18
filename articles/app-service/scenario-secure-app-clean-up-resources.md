---
title: 'Tutorial: Bereinigen von Ressourcen | Azure'
description: In diesem Tutorial erfahren Sie, wie Sie die Azure-Ressourcen bereinigen, die beim Erstellen der Web-App zugeordnet wurden.
services: storage, app-service-web
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 10/27/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.openlocfilehash: ab91ea7aa6e621dabc5cac83fe818dbf175214b6
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2020
ms.locfileid: "94428274"
---
# <a name="tutorial-clean-up-resources"></a>Tutorial: Bereinigen von Ressourcen

Wenn Sie alle Schritte in diesem mehrteiligen Tutorial ausgeführt haben, haben Sie einen App-Dienst, einen App Service-Hostingplan und ein Speicherkonto in einer Ressourcengruppe erstellt.  Außerdem haben Sie eine App-Registrierung in Azure AD erstellt.  Wenn Sie diese Ressourcen nicht mehr benötigen, löschen Sie diese und die App-Registrierung, sodass keine Gebühren mehr anfallen.  

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Löschen der Azure-Ressourcen, die im Rahmen des Tutorials erstellt wurden

## <a name="delete-the-resource-group"></a>Löschen der Ressourcengruppe
Wählen Sie im [Azure-Portal](https://portal.azure.com) im Portalmenü die Option **Ressourcengruppen** und dann die Ressourcengruppe aus, die Ihren App-Dienst und den App Service-Plan enthält.

Wählen Sie **Ressourcengruppe löschen** aus, um die Ressourcengruppe und alle Ressourcen zu löschen.

:::image type="content" alt-text="Ressourcengruppe löschen" source="./media/scenario-secure-app-clean-up-resources/delete-resource-group.png":::

Die Ausführung dieses Befehls kann einige Minuten in Anspruch nehmen.

## <a name="delete-the-app-registration"></a>Löschen der App-Registrierung
Wählen Sie im Portalmenü die Optionen **Azure Active Directory** > **App-Registrierungen** und anschließend die von Ihnen erstellte Anwendung aus.
:::image type="content" alt-text="Auswählen der App-Registrierung" source="./media/scenario-secure-app-clean-up-resources/select-app-registration.png":::

Wählen Sie in der Übersicht der App-Registrierungen die Option **Löschen** aus.
:::image type="content" alt-text="Löschen einer App-Registrierung" source="./media/scenario-secure-app-clean-up-resources/delete-app-registration.png":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
>
> * Löschen der Azure-Ressourcen, die im Rahmen des Tutorials erstellt wurden

Informieren Sie sich darüber, wie Sie für eine [.NET Core-App](tutorial-dotnetcore-sqldb-app.md), [Python-App](tutorial-python-postgresql-app.md), [Java-App](tutorial-java-spring-cosmosdb.md) oder [Node.js-App](tutorial-nodejs-mongodb-app.md) eine Verbindung mit einer Datenbank herstellen.