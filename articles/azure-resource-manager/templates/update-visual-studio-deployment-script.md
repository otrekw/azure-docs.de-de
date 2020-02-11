---
title: Aktualisieren des Vorlagenbereitstellungsskripts von Visual Studio für die Verwendung von Az PowerShell
description: Aktualisieren des Visual Studio-Vorlagenbereitstellungsskripts von AzureRM auf Az PowerShell
author: cweining
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: cweining
ms.openlocfilehash: 357e0289f3237ed32b0801280316225ba5530282
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2020
ms.locfileid: "76963868"
---
# <a name="update-visual-studio-template-deployment-script-to-use-az-powershell-module"></a>Aktualisieren des Vorlagenbereitstellungsskripts von Visual Studio für die Verwendung des Az PowerShell-Moduls

Visual Studio 16.4 unterstützt die Verwendung des Az PowerShell-Moduls im Vorlagenbereitstellungsskript. Visual Studio installiert dieses Modul aber nicht automatisch. Um das Az-Modul zu verwenden, müssen Sie vier Schritte ausführen:

1. [Deinstallieren des AzureRM-Moduls](/powershell/azure/uninstall-az-ps#uninstall-the-azurerm-module)
1. [Installieren des Az-Moduls](/powershell/azure/install-az-ps)
1. Aktualisieren von Visual Studio auf Version 16.4
1. Aktualisieren des Bereitstellungsskripts in Ihrem Projekt

## <a name="update-visual-studio-to-164"></a>Aktualisieren von Visual Studio auf Version 16.4

Aktualisieren Ihrer Installation von Visual Studio auf Version 16.4 oder höher. Stellen Sie während des Upgrades sicher, dass die Azure PowerShell-Komponente nicht aktiviert ist. Da Sie das Az-Modul über den Katalog installiert haben, sollten Sie das AzureRM-Modul nicht erneut installieren.

Wenn Sie bereits ein Upgrade auf 16.4 ausgeführt haben und die Azure PowerShell-Komponente aktiviert war, können Sie sie deinstallieren, indem Sie dazu den Visual Studio-Installer ausführen. Aktivieren Sie nicht die Azure PowerShell-Komponente in der Azure-Workload bzw. auf der Seite der einzelnen Komponenten.

## <a name="update-the-deployment-script-in-your-project"></a>Aktualisieren des Bereitstellungsskripts in Ihrem Projekt

Ersetzen Sie alle Vorkommen der Zeichenfolge „AzureRm“ im Bereitstellungsskript durch „Az“. Sie können die Überarbeitungen in diesem [Gist](https://gist.github.com/cweining/d2da2479418ea403499c4306dcf4f619) einsehen, um die Änderungen zu verfolgen. Weitere Informationen zum Upgrade von Skripts auf das Az-Modul finden Sie unter [Migrieren der Azure PowerShell von AzureRM zu Az](/powershell/azure/migrate-from-azurerm-to-az).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Verwenden des Visual Studio-Projekts finden Sie unter [Erstellen und Bereitstellen von Azure-Ressourcengruppenprojekten über Visual Studio](create-visual-studio-deployment-project.md).
