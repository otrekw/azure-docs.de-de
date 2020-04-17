---
title: Herstellen der Verbindung mit Azure Cosmos DB mit Compass
description: Erfahren Sie, wie Sie über MongoDB Compass Daten in Azure Cosmos DB speichern und verwalten.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/20/2020
author: LuisBosquez
ms.author: lbosq
ms.openlocfilehash: c683ec0c4b3a536b0627a7c1c8abf28ee4f83663
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80757033"
---
# <a name="use-mongodb-compass-to-connect-to-azure-cosmos-dbs-api-for-mongodb"></a>Herstellen einer Verbindung mit der Azure Cosmos DB-API für MongoDB mithilfe von MongoDB Compass

Dieses Tutorial zeigt, wie Sie mit dem [MongoDB Compass](https://www.mongodb.com/products/compass) Daten in Cosmos DB speichern und/oder verwalten können. Für diese exemplarische Vorgehensweise wird die API für MongoDB von Azure Cosmos DB verwendet. Für diejenigen unter Ihnen, die es noch nicht kennen: Compass ist grafische Benutzeroberfläche für MongoDB. Es wird häufig zur Visualisierung und Verwaltung Ihrer Daten und zur Ausführung von Ad-hoc-Abfragen verwendet.

Cosmos DB ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel-Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Cosmos DB zugrunde liegen.

## <a name="pre-requisites"></a>Voraussetzungen

Um mithilfe von MongoDB Compass eine Verbindung mit einem Cosmos DB-Konto herzustellen, müssen Sie die folgenden Aktionen ausführen:

* Herunterladen und Installieren von [Compass](https://www.mongodb.com/download-center/compass?jmp=hero)
* Abrufen der Informationen zur Cosmos DB-[Verbindungszeichenfolge](connect-mongodb-account.md)

> [!NOTE]
> Derzeit wird die API von Azure Cosmos DB für MongoDB Server Version 3.2 mit MongoDB Compass unterstützt.

## <a name="connect-to-cosmos-dbs-api-for-mongodb"></a>Herstellen einer Verbindung mit der Azure Cosmos DB-API für MongoDB

Um Ihr Cosmos DB-Konto mit Compass zu verbinden, können Sie die folgenden Schritte ausführen:

1. Rufen Sie die Verbindungsinformationen für Ihr Cosmos-Konto ab, das mit der API für MongoDB von Azure Cosmos DB konfiguriert wurde. Verwenden Sie dazu die [hier](connect-mongodb-account.md) beschriebenen Anweisungen.

    ![Screenshot des Blatts „Verbindungszeichenfolge“](./media/mongodb-compass/mongodb-compass-connection.png)

2. Klicken Sie auf die Schaltfläche **In Zwischenablage kopieren** neben Ihrer **Primären/Sekundär en Verbindungszeichenfolge** in Cosmos DB. Wenn Sie auf diese Schaltfläche klicken, wird Ihre gesamte Verbindungszeichenfolge in die Zwischenablage kopiert.

    ![Screenshot der auf die Schaltfläche zum Kopieren in die Zwischenablage](./media/mongodb-compass/mongodb-connection-copy.png)

3. Öffnen Sie Compass auf Ihrem Desktop/Computer, und klicken Sie auf **Verbinden** und dann auf **Verbinden mit...** .

4. Compass erkennt automatisch eine Verbindungszeichenfolge in der Zwischenablage und fragt, ob Sie diese für die Verbindung verwenden möchten. Klicken Sie auf **Ja** wie im folgenden Screenshot gezeigt:

    ![Screenshot der Compass-Aufforderung zum Herstellen einer Verbindung](./media/mongodb-compass/mongodb-compass-detect.png)

5. Wenn Sie im obigen Schritt auf **Ja** klicken, werden Ihre Angaben aus der Verbindungszeichenfolge automatisch ausgefüllt. Entfernen Sie den Wert, der automatisch im Feld **Replikatgruppenname** eingetragen wird, um sicherzustellen, dass er leer bleibt.

    ![Screenshot der Compass-Aufforderung zum Herstellen einer Verbindung](./media/mongodb-compass/mongodb-compass-replica.png)

6. Klicken Sie unten auf der Seite auf **Verbinden**. Ihr Cosmos DB-Konto und Ihre Datenbanken sollten nun in MongoDB Compass angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Studio 3T](mongodb-mongochef.md) mit der API für MongoDB von Azure Cosmos DB verwenden.
- Untersuchen Sie MongoDB-[Beispiele](mongodb-samples.md) mit der API für MongoDB von Azure Cosmos DB.
