---
title: Bereitstellen von Azure SQL Edge für Windturbinen einer Contoso Wind Farm
description: In diesem Tutorial nutzen Sie Azure SQL Edge für die Aktivierungserkennung der Windturbinen einer Contoso Wind Farm.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: tutorial
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 12/18/2020
ms.openlocfilehash: e966d756744210dc8f6739b96501dd91f0d8d1b5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97723472"
---
# <a name="using-azure-sql-edge-to-build-smarter-renewable-resources"></a>Verwenden von Azure SQL Edge zum Erstellen von intelligenteren Ressourcen für erneuerbare Energien

Diese Azure SQL Edge-Demo basiert auf einer Contoso-Ressource für den Bereich „Erneuerbare Energien“: eine Farm mit Windturbinen, bei der SQL DB Edge für die Datenverarbeitung auf dem Generator genutzt wird. 

In dieser Demo wird Schritt für Schritt beschrieben, wie Sie die Ursache einer Warnung beheben, die aufgrund der Erkennung von Windturbulenzen auf dem Gerät ausgelöst wird. Sie trainieren ein Modell und stellen es in SQL DB Edge bereit, um aufgrund der Winderkennung eine Korrektur durchzuführen und so die Leistung zu optimieren.

Azure SQL Edge: Channel 9-Video mit einer Demo im Bereich „Erneuerbaren Energien“:
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Edge-Demo-Renewable-Energy/player]

## <a name="setting-up-the-demo-on-your-local-computer"></a>Einrichten der Demo auf Ihrem lokalen Computer
Git wird genutzt, um alle Dateien aus der Demo auf Ihren lokalen Computer zu kopieren. 

1. Führen Sie die Installation von Git [hier](https://git-scm.com/download) durch.
2. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zu einem Ordner, in den das Repository heruntergeladen werden soll. 
3. Führen Sie den Befehl https://github.com/microsoft/sql-server-samples.git aus.
4. Navigieren Sie an dem Speicherort, an dem das Repository geklont wurde, zu **'sql-server-samples\samples\demos\azure-sql-edge-demos\Wind Turbine Demo'** .
5. Befolgen Sie die Anleitung in der Datei „README.md“, um die Demoumgebung einzurichten und die Demo auszuführen.