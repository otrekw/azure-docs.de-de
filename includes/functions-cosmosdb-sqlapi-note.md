---
title: include file
description: include file
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 08/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f7495c52e36bdc207145f17ec72eb57b7ebf1784
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "76279552"
---
Azure Cosmos DB-Bindungen werden nur in Kombination mit der SQL-API unterstützt. Für alle anderen Azure Cosmos DB-APIs (einschließlich [Azure Cosmos DB-API für MongoDB](../articles/cosmos-db/mongodb-introduction.md), [Cassandra-API](../articles/cosmos-db/cassandra-introduction.md), [Gremlin-API](../articles/cosmos-db/graph-introduction.md) und [Tabellen-API](../articles/cosmos-db/table-introduction.md)) müssen Sie für den Datenbankzugriff aus Ihrer Funktion den statischen Client für Ihre API verwenden.
