---
title: 'Erstellen der Definition für die verwaltete Anwendung: Azure PowerShell'
description: Hier finden Sie ein Azure PowerShell-Skriptbeispiel, mit dem die Definition für eine verwaltete Anwendung im Azure-Abonnement erstellt wird.
author: tfitzmac
ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: b605d63db09b81bfe73eca8f4abd7a38d2325004
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86056020"
---
# <a name="create-a-managed-application-definition-with-powershell"></a>Erstellen einer Definition für die verwaltete Anwendung mit PowerShell

Mit diesem Skript wird eine Definition für die verwaltete Anwendung in einem Dienstkatalog veröffentlicht.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../../powershell_scripts/managed-applications/create-definition/create-definition.ps1 "Create definition")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl zum Erstellen der Definition für die verwaltete Anwendung. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzManagedApplicationDefinition](/powershell/module/az.resources/new-azmanagedapplicationdefinition) | Erstellen Sie eine Definition für die verwaltete Anwendung. Stellen Sie das Paket bereit, das die erforderlichen Dateien enthält. |


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zu PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/get-started-azureps).
