---
title: 'Azure PowerShell-Skriptbeispiel: Abonnieren einer Ressourcengruppe | Microsoft-Dokumentation'
description: In diesem Artikel wird anhand eines Azure PowerShell-Beispielskripts das Abonnieren von Event Grid-Ereignissen für eine Ressourcengruppe veranschaulicht.
services: event-grid
documentationcenter: na
author: spelluru
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: e285f628d1ec3afce55dc8fe4efa50a361dab5ed
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "76720804"
---
# <a name="subscribe-to-events-for-a-resource-group-with-powershell"></a>Abonnieren von Ereignissen für eine Ressourcengruppe mit PowerShell

Dieses Skript erstellt ein Event Grid-Abonnement für die Ereignisse für eine Ressourcengruppe.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Für das Vorschaubeispielskript ist das Event Grid-Modul erforderlich. Führen Sie zum Installieren `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery` aus.

## <a name="sample-script---stable"></a>Beispielskript: stabil

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-resource-group/subscribe-to-resource-group.ps1 "Subscribe to resource group")]

## <a name="sample-script---preview-module"></a>Beispielskript: Vorschaumodul

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-resource-group-preview/subscribe-to-resource-group-preview.ps1 "Subscribe to resource group")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl, um das Ereignisabonnement zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzEventGridSubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/new-azeventgridsubscription) | Erstellen Sie ein Event Grid-Abonnement. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zu PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/get-started-azureps).
