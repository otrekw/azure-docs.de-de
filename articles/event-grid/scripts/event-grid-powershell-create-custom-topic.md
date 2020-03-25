---
title: Azure PowerShell-Skriptbeispiel – Erstellen eines benutzerdefinierten Themas | Microsoft-Dokumentation
description: In diesem Artikel wird anhand eines Azure PowerShell-Beispielskripts das Erstellen eines benutzerdefinierten Event Grid-Themas veranschaulicht.
services: event-grid
documentationcenter: na
author: spelluru
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2020
ms.author: tomfitz
ms.openlocfilehash: 900f31ff87e6a588fc1d1e3efd854437d2c8c1b9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "76719941"
---
# <a name="create-event-grid-custom-topic-with-powershell"></a>Erstellen eines benutzerdefinierten Event Grid-Themas mit PowerShell

Dieses Skript erstellt ein benutzerdefiniertes Event Grid-Thema.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/event-grid/create-custom-topic/create-custom-topic.ps1 "Create custom topic")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl zum Erstellen des benutzerdefinierten Themas. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzEventGridTopic](https://docs.microsoft.com/powershell/module/az.eventgrid/new-azeventgridtopic) | Erstellt ein benutzerdefiniertes Event Grid-Thema. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zu PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/get-started-azureps).
