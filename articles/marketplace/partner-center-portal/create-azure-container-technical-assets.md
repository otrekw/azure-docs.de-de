---
title: Technische Konzepte für Azure-Containerangebote – kommerzieller Microsoft-Marketplace
description: Technische Ressourcen und Anleitungen, die Ihnen bei der Konfiguration eines Containerangebots im Azure Marketplace helfen.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: 87c915330fdc0d59d0d32c0747c7dc7647c342ba
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83700986"
---
# <a name="create-an-azure-container-offer"></a>Erstellen eines Azure-Containerangebots

Dieser Artikel enthält technische Ressourcen und Empfehlungen, die Ihnen helfen, ein Containerangebot im Azure Marketplace zu erstellen.

## <a name="before-you-begin"></a>Voraussetzungen

Schnellstarts, Tutorials und Beispiele finden Sie in der [Dokumentation zu Azure Container Instances](https://docs.microsoft.com/azure/container-instances).

## <a name="fundamental-technical-knowledge"></a>Grundlegende technische Kenntnisse

Das Entwerfen, Erstellen und Testen dieser Ressourcen dauert lange, und es sind technische Kenntnisse in Bezug auf die Azure-Plattform und die Technologien erforderlich, die verwendet werden, um das Angebot zu erstellen.

Zusätzlich zur Vertrautheit mit Ihrer Lösungsdomäne sollte sich Ihr Engineeringteam mit den folgenden Microsoft-Technologien auskennen:

- Grundkenntnisse in Bezug auf [Azure-Dienste](https://azure.microsoft.com/services/)
- [Entwerfen und Erstellen der Architektur für Azure-Anwendungen](https://azure.microsoft.com/solutions/architecture/)
- Erweiterte Grundkenntnisse in Bezug auf [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) und [Azure-Netzwerk](https://azure.microsoft.com/services/?filter=networking)
- Erweiterte Grundkenntnisse in Bezug auf [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
- Fundierte Kenntnisse zu [JSON](https://www.json.org/).

## <a name="suggested-tools"></a>Vorgeschlagene Tools

Wählen Sie eine oder beide der folgenden Skriptumgebungen als Unterstützung bei der Verwaltung Ihres Containerimages aus:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/?view=azps-3.7.0&viewFallbackFrom=azps-3.6.1)
- [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

Sie sollten Ihrer Entwicklungsumgebung diese Tools hinzufügen:

- [Azure Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)
- [Visual Studio Code](https://code.visualstudio.com/)
  - Erweiterung: [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Erweiterung: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Erweiterung: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json).

Machen Sie sich mit den verfügbaren Tools auf der Seite [Azure-Entwicklertools](https://azure.microsoft.com/) vertraut. Wenn Sie Visual Studio verwenden, machen Sie sich mit den im [Visual Studio Marketplace](https://marketplace.visualstudio.com/) verfügbaren Tools vertraut.

## <a name="create-the-container-image"></a>Erstellen des Containerimages

Weitere Informationen finden Sie in folgenden Tutorials:

- [Tutorial: Erstellen eines Containerimages für die Bereitstellung in Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-tutorial-prepare-app)
- [Tutorial: Erstellen und Bereitstellen von Containerimages in der Cloud mit Azure Container Registry Tasks](https://docs.microsoft.com/azure/container-registry/container-registry-tutorial-quick-task).

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie Ihr Containerangebot](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-offer).
