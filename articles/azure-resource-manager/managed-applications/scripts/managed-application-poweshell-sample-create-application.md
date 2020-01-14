---
title: 'Azure PowerShell-Skriptbeispiel: Bereitstellen einer verwalteten Anwendung'
description: Hier finden Sie ein Azure PowerShell-Beispielskript, mit dem die Definition für eine verwaltete Anwendung im Abonnement bereitgestellt wird.
author: tfitzmac
ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: e076128a5be453028635b1657bf3f90980863148
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75648937"
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
| [New-AzManagedApplication](https://docs.microsoft.com/powershell/module/az.resources/new-azmanagedapplication) | Erstellen Sie eine verwaltete Anwendung. Geben Sie die Definitions-ID und die Parameter für die Vorlage an. |


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zu PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/get-started-azureps).
