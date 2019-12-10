---
title: 'Azure PowerShell: Abonnieren eines Azure-Abonnements'
description: 'Azure PowerShell- und Azure Event Grid-Skriptbeispiel: Abonnieren eines Azure-Abonnements'
services: event-grid
documentationcenter: na
author: spelluru
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/22/2019
ms.author: spelluru
ms.openlocfilehash: 8c90e39cef1420ec06b7f62ebf9982720ffebf94
ms.sourcegitcommit: 57eb9acf6507d746289efa317a1a5210bd32ca2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2019
ms.locfileid: "74664130"
---
# <a name="subscribe-to-events-for-an-azure-subscription-with-powershell"></a>Abonnieren von Ereignissen für ein Azure-Abonnement mit PowerShell

Dieses Skript erstellt ein Event Grid-Abonnement für die Ereignisse für ein Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script---stable"></a>Beispielskript: stabil

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-azure-subscription/subscribe-to-azure-subscription.ps1 "Subscribe to Azure subscription")]

## <a name="sample-script---preview-module"></a>Beispielskript: Vorschaumodul

Für dieses Vorschaubeispielskript ist das Event Grid-Modul erforderlich. Führen Sie zum Installieren `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery` aus.

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-azure-subscription-preview/subscribe-to-azure-subscription-preview.ps1 "Subscribe to Azure subscription")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl, um das Ereignisabonnement zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzEventGridSubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/new-azeventgridsubscription) | Erstellen Sie ein Event Grid-Abonnement. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zu PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/get-started-azureps).
