---
title: 'Übersicht zur Migration: LUIS'
description: Informationen zu den Elementen in einem Migrationspfad
ms.topic: how-to
ms.date: 05/22/2020
ms.openlocfilehash: 4080dd27414c2da551b60ad430278c18768337c9
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88587819"
---
# <a name="migration-in-luis"></a>Migration in LUIS

In einem Migrationspfad befinden sich mehrere Elemente. Anhand der folgenden Tabelle können Sie nachvollziehen, welche Elemente betroffen sind und wann Sie die Migration abschließen müssen:

|Bereich|BESCHREIBUNG|Migrationsabschlussdatum|
|--|--|--|
|[Vorhersage-APIs](luis-migration-api-v3.md)|Migrieren zur V3-API|TBD|
|[Erstellungs-APIs](luis-migration-authoring-entities.md)|Migrieren zur V3-API|TBD|
|[Erstellungsressource](luis-migration-authoring.md)|Migrieren von einer Umgebung ohne Erstellungsressource zu einer LUIS-Erstellungsressource im LUIS-Portal|2\. November 2020 |
|[Erforderliches Feature](luis-migration-authoring-entities.md#api-change-constraint-replaced-with-required-feature)|Diese Änderung wurde im Mai 2020 auf der //Build-Konferenz vorgenommen und gilt nur für die V3-Erstellungs-APIs, bei denen eine App ein eingeschränktes Feature verwendet.|19. Juni 2020|
|[Entität vom Typ „Composite“](migrate-from-composite-entity.md)|Führen Sie ein Upgrade einer Entität vom Typ „Composite“ zu einer Machine Learning-Entität aus, um eine Entität zu erstellen, die ausführlichere, besser aufschlüsselbare Vorhersagen für das Debuggen der Entität erhält.|TBD|
