---
title: 'Tutorial: Freigeben von Ankern mit Azure Cosmos DB'
description: In diesem Tutorial wird beschrieben, wie Sie Azure Spatial Anchors-Bezeichner zwischen Android/iOS-Geräten in Unity mit einem Back-End-Dienst und Azure Cosmos DB freigeben.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 07/31/2020
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 8ff431c27dd53e82f9003b658c82ceb3efb5d320
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810019"
---
# <a name="tutorial-sharing-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Tutorial: Sitzungs- und geräteübergreifendes Freigeben von Azure Spatial Anchors mit einem Azure Cosmos DB-Back-End

Dieses Tutorial ist eine Fortsetzung des Tutorials [Sitzungs- und geräteübergreifendes Freigeben von Azure Spatial Anchors](../../../articles/spatial-anchors/tutorials/tutorial-share-anchors-across-devices.md). Hier werden die Schritte zum Hinzufügen weiterer Funktionen erläutert, um Azure Cosmos DB als Back-End-Speicher bereitzustellen und Azure Spatial Anchors über Sitzungen und Geräte hinweg freizugeben.

![GIF zur Veranschaulichung der Objektpersistenz](./media/persistence.gif)

Beachten Sie Folgendes: Sie verwenden in diesem Tutorial zwar Unity und Azure Cosmos DB, aber dies dient nur als Beispiel für die geräteübergreifende Freigabe von Spatial Anchors-Bezeichnern. Sie können auch andere Sprachen und Back-End-Technologien verwenden, um dieses Ziel zu erreichen.

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

Fügen Sie der zuvor erstellten Ressourcengruppe eine Azure Cosmos-Datenbank hinzu.

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Kopieren Sie den `Connection String`, da Sie ihn benötigen.

## <a name="make-minor-changes-to-the-sharingservice-files"></a>Vornehmen kleinerer Änderungen an den SharingService-Dateien

Öffnen Sie `SharingService\Startup.cs` im **Projektmappen-Explorer**.

Suchen Sie oben in der Datei nach `#define INMEMORY_DEMO`, und kommentieren Sie diese Zeile aus. Speichern Sie die Datei .

Öffnen Sie `SharingService\appsettings.json` im **Projektmappen-Explorer**.

Suchen Sie nach der `StorageConnectionString`-Eigenschaft, und legen Sie den Wert auf den `Connection String`-Text fest, den Sie im Schritt [Erstellen eines Datenbankkontos](#create-a-database-account) kopiert haben. Speichern Sie die Datei .

Sie können den Freigabedienst erneut veröffentlichen und die Beispiel-App ausführen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Azure Cosmos DB verwendet, um Ankerbezeichner geräteübergreifend freizugeben. Weitere Informationen zur Verwendung von Azure Spatial Anchors in einer neuen Unity HoloLens-App finden Sie im nächsten Tutorial.

> [!div class="nextstepaction"]
> [Starten einer neuen Unity HoloLens-App](./tutorial-new-unity-hololens-app.md)
