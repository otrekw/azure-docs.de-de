---
title: Erstellen der technischen Ressourcen eines Containerimages | Azure Marketplace
description: Erstellen Sie eine technische Ressourcen für einen Azure-Container.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: dsindona
ms.openlocfilehash: 68db606c9a01c4b1122f9b0cce620762485ca40a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82148273"
---
# <a name="prepare-your-container-technical-assets"></a>Vorbereiten Ihrer technischen Containerressourcen

> [!IMPORTANT]
> Ab dem 13. April 2020 beginnen wir mit der Umstellung der Verwaltung Ihrer Azure-Containerangebote auf Partner Center. Nach der Migration erstellen und verwalten Sie Ihre Angebote im Partner Center. Befolgen Sie zum Verwalten Ihrer migrierten Angebote die Anweisungen unter [Vorbereiten Ihrer technischen Containerressourcen](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-offer).

Dieser Artikel beschreibt die Schritte und Anforderungen für die Konfiguration eines Containerangebots im Azure Marketplace.

## <a name="before-you-begin"></a>Voraussetzungen

Lesen Sie die Dokumentation [Azure Container Instances](https://docs.microsoft.com/azure/container-instances). Darin finden Sie außerdem Schnellstarts, Tutorials und Beispiele.

## <a name="fundamental-technical-knowledge"></a>Grundlegende technische Kenntnisse

Das Entwerfen, Erstellen und Testen dieser Ressourcen dauert lange, und es sind technische Kenntnisse in Bezug auf die Azure Platform und die Technologien erforderlich, um das Angebot zu erstellen.
 
Zusätzlich zur Vertrautheit mit Ihrer Lösungsdomäne sollte sich Ihr Engineering Team mit den folgenden Microsoft-Technologien auskennen:

-    Grundkenntnisse in Bezug auf [Azure-Dienste](https://azure.microsoft.com/services/) 
-    [Entwerfen und Erstellen der Architektur für Azure-Anwendungen](https://azure.microsoft.com/solutions/architecture/)
-    Erweiterte Grundkenntnisse in Bezug auf [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) und das [Azure-Netzwerk](https://azure.microsoft.com/services/?filter=networking)
-    Erweiterte Grundkenntnisse in Bezug auf [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
-    Erweiterte [JSON](https://www.json.org/)-Grundkenntnisse

## <a name="suggested-tools"></a>Vorgeschlagene Tools

Wählen Sie eine oder beide der folgenden Skriptumgebungen als Unterstützung bei der Verwaltung Ihres Containerimages aus:

-    [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
-    [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure)

Außerdem empfehlen wir Ihnen, Ihrer Entwicklungsumgebung die folgenden Tools hinzuzufügen:

-    [Azure Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
-    [Visual Studio Code](https://code.visualstudio.com/)
    *    Erweiterung: [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    *    Erweiterung: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    *    Erweiterung: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

Außerdem empfehlen wir Ihnen, sich die verfügbaren Tools auf der Seite [Azure-Entwicklungstools](https://azure.microsoft.com/tools/) und – bei Verwendung von Visual Studio – den [Visual Studio Marketplace](https://marketplace.visualstudio.com/) anzusehen.

## <a name="create-the-container-image"></a>Erstellen des Containerimages

Weitere Informationen finden Sie unter den folgenden Links:

* [Tutorial: Erstellen eines Containerimages für die Bereitstellung in Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-tutorial-prepare-app)
* [Tutorial: Erstellen und Bereitstellen von Containerimages in der Cloud mit Azure Container Registry Tasks](https://docs.microsoft.com/azure/container-registry/container-registry-tutorial-quick-task)

## <a name="next-steps"></a>Nächste Schritte

[Erstellen Ihres Containerangebots](./cpp-create-offer.md)
