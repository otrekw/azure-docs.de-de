---
title: Verschieben einer Service Fabric Mesh-Anwendung in eine andere Region
description: Sie können Service Fabric Mesh-Ressourcen verschieben, indem Sie eine Kopie Ihrer aktuellen Vorlage in einer neuen Azure-Region bereitstellen.
author: erikadoyle
ms.author: edoyle
ms.topic: how-to
ms.date: 01/14/2020
ms.custom: subject-moving-resources
ms.openlocfilehash: 376808a6d8f61d4dc03d17061323a473d48053a6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "76907341"
---
# <a name="move-a-service-fabric-mesh-application-to-another-azure-region"></a>Verschieben einer Service Fabric Mesh-Anwendung in eine andere Azure-Region

In diesem Artikel wird beschrieben, wie Sie Ihre Service Fabric Mesh-Anwendung und ihre Ressourcen in eine andere Azure-Region verschieben. Sie können Ihre Ressourcen aus verschiedenen Gründen in eine andere Region verschieben. Beispielsweise als Reaktion auf Ausfälle, um Features oder Dienste zu nutzen, die nur in bestimmten Regionen verfügbar sind, um die internen Richtlinien- und Governanceanforderungen zu erfüllen, oder als Reaktion auf Anforderungen an die Kapazitätsplanung.

 Service Fabric Mesh unterstützt [nicht](../azure-resource-manager/management/region-move-support.md#microsoftservicefabricmesh) die Möglichkeit, Ressourcen direkt in Azure-Regionen zu verschieben. Sie können Ressourcen jedoch indirekt verschieben, indem Sie eine Kopie der aktuellen Azure Resource Manager-Vorlage in der neuen Zielregion bereitstellen und dann den eingehenden Datenverkehr und die Abhängigkeiten an die neu erstellte Service Fabric Mesh-Anwendung umleiten.

## <a name="prerequisites"></a>Voraussetzungen

* Eingangscontroller (z. B. [Application Gateway](https://docs.microsoft.com/azure/application-gateway/)), der als Vermittler für das Routing von Datenverkehr zwischen Clients und Ihrer Service Fabric Mesh-Anwendung dient.
* Verfügbarkeit von Service Fabric Mesh (Vorschau) in der Azure-Zielregion (`westus`, `eastus` oder `westeurope`).

## <a name="prepare"></a>Vorbereiten

1. Erstellen Sie eine „Momentaufnahme“ des aktuellen Status Ihrer Service Fabric Mesh-Anwendung, indem Sie die Azure Resource Manager-Vorlage und die Parameter aus der aktuellen Bereitstellung exportieren. Befolgen Sie hierzu die Schritte unter [Exportieren einer Vorlage nach der Bereitstellung](../azure-resource-manager/templates/export-template-portal.md#export-template-after-deployment) mithilfe des Azure-Portals. Sie können auch die [Azure CLI](../azure-resource-manager/management/manage-resource-groups-cli.md#export-resource-groups-to-templates), [Azure PowerShell](../azure-resource-manager/management/manage-resource-groups-powershell.md#export-resource-groups-to-templates) oder die [REST-API](https://docs.microsoft.com/rest/api/resources/resourcegroups/exporttemplate) verwenden.

2. [Exportieren Sie ggf. andere Ressourcen in derselben Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal#export-template-from-a-resource-group) für eine erneute Bereitstellung in der Zielregion.

3. Überprüfen Sie die exportierte Vorlage (und bearbeiten Sie sie bei Bedarf), um sicherzustellen, dass die vorhandenen Eigenschaftswerte den Werten entsprechen, die Sie in der Zielregion verwenden möchten. Das neue `location`-Element (Azure-Region) ist ein Parameter, den Sie während der erneuten Bereitstellung angeben.

## <a name="move"></a>Move

1. Erstellen Sie eine neue Ressourcengruppe in der Zielregion (oder verwenden Sie eine bereits vorhandene Ressourcengruppe).

2. Führen Sie mit der exportierten Vorlage die Schritte in [Bereitstellen von Ressourcen aus einer benutzerdefinierten Vorlage](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-portal#deploy-resources-from-custom-template) mithilfe des Azure-Portals aus. Sie können auch die [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-cli), [Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-powershell) oder die [REST-API](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-rest) verwenden.

3. Anleitungen zum Verschieben verwandter Ressourcen (z. B. [Azure Storage Konten](../storage/common/storage-account-move.md)) finden Sie in den Anweisungen für einzelne Dienste, die unter dem Thema [Regionsübergreifendes Verschieben von Azure-Ressourcen](../azure-resource-manager/management/move-region.md) aufgeführt werden.

## <a name="verify"></a>Überprüfen

1. Testen Sie nach Abschluss der Bereitstellung die Anwendungsendpunkte, um die Funktionalität der Anwendung zu überprüfen.

2. Sie können den Status der Anwendung auch überprüfen, indem Sie den Anwendungsstatus überprüfen ([az mesh app show](https://docs.microsoft.com/cli/azure/ext/mesh/mesh/app?view=azure-cli-latest#ext-mesh-az-mesh-app-show)) und die Anwendungsprotokolle und -Befehle mithilfe der [Azure Service Fabric Mesh CLI](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-quickstart-deploy-container#set-up-service-fabric-mesh-cli) überprüfen ([az mesh code-package-log](https://docs.microsoft.com/cli/azure/ext/mesh/mesh/code-package-log?view=azure-cli-latest)).

## <a name="commit"></a>Commit

Nachdem Sie die äquivalente Funktionalität Ihrer Service Fabric Mesh-Anwendung in der Zielregion bestätigt haben, konfigurieren Sie den Eingangscontroller (z. B. [Application Gateway](../application-gateway/redirect-overview.md)), um den Datenverkehr an die neue Anwendung umzuleiten.

## <a name="clean-up-source-resources"></a>Bereinigen der Quellressourcen

Um das Verschieben der Service Fabric Mesh-Anwendung abzuschließen, [löschen Sie die Quellanwendung und/oder die übergeordnete Ressourcengruppe](../azure-resource-manager/management/delete-resource-group.md).

## <a name="next-steps"></a>Nächste Schritte

* [Verschieben von Azure-Ressourcen zwischen Regionen](../azure-resource-manager/management/move-region.md)
* [Unterstützung für das regionsübergreifende Verschieben von Azure-Ressourcen](../azure-resource-manager/management/region-move-support.md)
* [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/management/move-resource-group-and-subscription.md)
* [Unterstützung des Verschiebevorgangs für Ressourcen](../azure-resource-manager/management/move-support-resources.md
)
