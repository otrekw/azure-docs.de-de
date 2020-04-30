---
title: Erstellen einer technischen Azure-VM-Ressource
description: Dieser Artikel beschreibt die Schritte und Anforderungen für die Konfiguration eines Containerangebots im Azure Marketplace.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: e51b8c705533fd74a5e46eaa2570563fef485309
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81730640"
---
# <a name="create-an-azure-vm-technical-asset"></a>Erstellen einer technischen Azure-VM-Ressource

> [!IMPORTANT]
> Wir verlagern die Verwaltung Ihrer Azure-Containerangebote vom Cloud-Partnerportal ins Partner Center. Befolgen Sie zur Verwaltung Ihrer Angebote bis zur erfolgten Migration Ihrer Angebote die Anweisungen in [Vorbereiten Ihrer technischen Containerressourcen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/containers/cpp-create-technical-assets) für das Cloud-Partnerportal.

Dieser Artikel beschreibt die Schritte und Anforderungen für die Konfiguration eines Containerangebots im Azure Marketplace.

## <a name="before-you-begin"></a>Voraussetzungen

Informationen zu Schnellstarts, Tutorials und Beispiele finden Sie in der [Dokumentation zu Azure Container Instances](https://docs.microsoft.com/azure/container-instances).

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

## <a name="next-step"></a>Nächster Schritt

- [Erstellen eines neuen Containerangebots mit dem Cloud-Partnerportal](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-offer).
