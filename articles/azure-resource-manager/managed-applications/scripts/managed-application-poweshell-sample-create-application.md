---
title: 'Azure PowerShell-Skriptbeispiel: Bereitstellen einer verwalteten Anwendung'
description: Hier finden Sie ein Azure PowerShell-Beispielskript, mit dem die Definition für eine verwaltete Anwendung im Abonnement bereitgestellt wird.
author: tfitzmac
ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: a2687e9c943df8454ff42a17f44866dcdb7f4730
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86055884"
---
# <a name="deploy-a-managed-application-for-a-service-catalog-with-powershell"></a>Bereitstellen einer verwalteten Anwendung für einen Dienstkatalog mit PowerShell

Mit diesem Skript wird eine Definition für eine verwaltete Anwendung aus dem Dienstkatalog bereitgestellt.


[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../../powershell_scripts/managed-applications/create-application/create-application.ps1 "Create application")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl zum Bereitstellen der verwalteten Anwendung. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzManagedApplication](/powershell/module/az.resources/new-azmanagedapplication) | Erstellen Sie eine verwaltete Anwendung. Geben Sie die Definitions-ID und die Parameter für die Vorlage an. |


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zu PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/get-started-azureps).
