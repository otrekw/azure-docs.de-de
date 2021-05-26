---
title: include file
description: include file
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 5e83acf3b2c0762a6e6622edaa345215faa6192e
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110073737"
---
Sie können nun mithilfe des Daten-Explorer-Tools im Azure-Portal eine Datenbank und einen Container erstellen. 

1. Wählen Sie **Daten-Explorer** > **Neuer Container** aus. 
    
    Der Bereich **Container hinzufügen** wird ganz rechts angezeigt. Möglicherweise müssen Sie nach rechts scrollen, damit Sie ihn sehen.

    :::image type="content" source="./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png" alt-text="Daten-Explorer im Azure-Portal, Bereich „Container hinzufügen“":::

2. Geben Sie auf der Seite **Container hinzufügen** die Einstellungen für den neuen Container ein.

    |Einstellung|Vorgeschlagener Wert|BESCHREIBUNG
    |---|---|---|
    |**Datenbank-ID**|ToDoList|Geben Sie *Tasks* als Namen für die neue Datenbank ein. Datenbanknamen müssen zwischen 1 und 255 Zeichen lang sein und dürfen weder `/, \\, #, ?` noch nachgestellte Leerzeichen enthalten. Aktivieren Sie die Option **Durchsatz über Container hinweg teilen**. Diese Option ermöglicht es Ihnen, den für die Datenbank bereitgestellten Durchsatz auf alle Container in der Datenbank zu verteilen. Darüber hinaus hilft sie Ihnen dabei, Kosten zu sparen. |
    | **Datenbank-Durchsatz**| Sie können **automatische Skalierung** oder **manuellen** Durchsatz bereitstellen. Mit dem manuellen Durchsatz können Sie RU/s selbst skalieren, während der durch autoskalierungsbasierte Durchsatz dem System ermöglicht, RU/s basierend auf der Nutzung zu skalieren. Wählen Sie für dieses Beispiel **Manuell** aus. <br><br> Belassen Sie den Durchsatz bei 400 Anforderungseinheiten pro Sekunde (RU/s). Wenn Sie die Latenz reduzieren möchten, können Sie den Durchsatz später hochskalieren, indem Sie die erforderlichen RU/s mit dem [Kapazitätsrechner](../articles/cosmos-db/estimate-ru-with-capacity-planner.md) schätzen.<br><br>**Hinweis**: Diese Einstellung ist nicht verfügbar, wenn ein neuer Container in einem serverlosen Konto erstellt wird. |
    |**Container-ID**|Items|Geben Sie *Items* als Namen für den neuen Container ein. Für Container-IDs gelten dieselben Zeichenanforderungen wie für Datenbanknamen.|
    |**Partitionsschlüssel**| /category| Das in diesem Artikel beschriebene Beispiel verwendet */category* als Partitionsschlüssel.|

    Fügen Sie für dieses Beispiel keine **eindeutigen Schlüssel** hinzu, oder aktivieren Sie den **Analysespeicher** nicht. Mit eindeutigen Schlüsseln können Sie die Datenbank mit einer Datenintegritätsebene versehen, indem Sie die Eindeutigkeit von Werten per Partitionsschlüssel sicherstellen. Weitere Informationen finden Sie unter [Eindeutige Schlüssel in Azure Cosmos DB.](../articles/cosmos-db/unique-keys.md) [Der Analysespeicher](../articles/cosmos-db/analytical-store-introduction.md) wird verwendet, um umfangreiche Analysen für operative Daten ohne Auswirkungen auf Ihre Transaktionsworkloads zu ermöglichen.
    
    Klicken Sie auf **OK**. Im Daten-Explorer werden die neue Datenbank und der neue Container angezeigt.