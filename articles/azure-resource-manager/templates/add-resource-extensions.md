---
title: Konfigurationen nach der Bereitstellung mithilfe von Erweiterungen
description: Hier erfahren Sie, wie Sie die Erweiterungen der Azure Resource Manager-Vorlage (ARM-Vorlage) verwenden, um Konfigurationen nach der Bereitstellung bereitzustellen.
author: mumian
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: 23d5a9aaa546542218a6f839ab415184ff309928
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97934321"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>Bereitstellen von Konfigurationen nach der Bereitstellung mithilfe von Erweiterungen

Die Erweiterungen der Azure Resource Manager-Vorlage (ARM-Vorlage) sind kleine Anwendungen, die Konfigurations- und Automatisierungsaufgaben auf Azure-Ressourcen nach der Bereitstellung ermöglichen. Die beliebtesten sind VM-Erweiterungen. Weitere Informationen finden Sie unter [Erweiterungen und Features für virtuelle Computer (Windows)](../../virtual-machines/extensions/features-windows.md) sowie unter [Erweiterungen und Features für virtuelle Computer (Linux)](../../virtual-machines/extensions/features-linux.md).

## <a name="extensions"></a>Erweiterungen

Die vorhandenen Erweiterungen sind:

- [Microsoft.Compute/virtualMachines/extensions](/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft.Compute virtualMachineScaleSets/extensions](/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Microsoft.HDInsight clusters/extensions](/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters)
- [Microsoft.Sql servers/databases/extensions](/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions)
- [Microsoft.Web/sites/siteextensions](/azure/templates/microsoft.web/2016-08-01/sites/siteextensions)

Um die verfügbaren Erweiterungen herauszufinden, navigieren Sie zur [Vorlagenreferenz](/azure/templates/). Geben Sie in **Nach Titel filtern** die **Erweiterung** ein.

Informationen zur Verwendung dieser Erweiterungen finden Sie unter:

- [Tutorial: Bereitstellen von VM-Erweiterungen mit ARM-Vorlagen](template-tutorial-deploy-vm-extensions.md).
- [Tutorial: Importieren von SQL-BACPAC-Dateien mit ARM-Vorlagen](template-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen von VM-Erweiterungen mit ARM-Vorlagen](template-tutorial-deploy-vm-extensions.md)
