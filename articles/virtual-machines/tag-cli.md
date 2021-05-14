---
title: Markieren einer Azure-VM mithilfe der Azure CLI
description: Erfahren Sie etwas über das Markieren eines virtuellen Computers mithilfe der Azure CLI.
author: cynthn
ms.service: virtual-machines
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 11/11/2020
ms.author: cynthn
ms.custom: devx-track-azurecli
ms.openlocfilehash: 20bb4ab622a01646bcc61d0f691c514a25a06edc
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107502605"
---
# <a name="how-to-tag-a-vm-using-the-azure-cli"></a>Markieren einer VM mithilfe der Azure CLI

In diesem Artikel wird das Markieren einer VM mit der Azure CLI beschrieben. Tags sind benutzerdefinierte Schlüssel-Wert-Paare, die direkt auf einer Ressource oder einer Ressourcengruppe platziert werden können. Azure unterstützt derzeit bis zu 50 Tags pro Ressource und Ressourcengruppe. Tags können zum Zeitpunkt der Erstellung auf einer Ressource platziert werden oder zu einer vorhandenen Ressource hinzugefügt werden. Sie können eine VM auch mit [Azure PowerShell](tag-powershell.md) markieren.


Sie können alle Eigenschaften für einen bestimmten virtuellen Computer einschließlich der Tags mit `az vm show` anzeigen.

```azurecli-interactive
az vm show --resource-group myResourceGroup --name myVM --query tags
```

Zum Hinzufügen eines neuen VM-Tags über die Azure-Befehlszeilenschnittstelle können Sie den `azure vm update` -Befehl zusammen mit dem Tag-Parameter `--set`verwenden:

```azurecli-interactive
az vm update \
    --resource-group myResourceGroup \
    --name myVM \
    --set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2
```

Um Tags zu entfernen, können Sie den `--remove`-Parameter im `azure vm update`-Befehl verwenden.

```azurecli-interactive
az vm update \
   --resource-group myResourceGroup \
   --name myVM \
   --remove tags.myNewTagName1
```

Nachdem wir unseren Ressourcen über die Azure CLI und das Portal Tags zugewiesen haben, werfen wir einen Blick auf die Nutzungsdetails, um die Tags im Abrechnungsportal anzuzeigen.

### <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Markieren Ihrer Azure-Ressourcen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/management/overview.md) und [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../azure-resource-manager/management/tag-resources.md).
- Unter [Grundlegendes zu Ihrer Microsoft Azure-Rechnung und „Verwenden der Azure-Abrechnungs-APIs, um programmgesteuerte Einblicke in die Nutzung Ihrer Azure-Ressourcen zu erlangen“ erfahren Sie, wie Tags Sie bei der Verwaltung Ihrer Azure-Ressourcenverwendung unterstützen können.](../cost-management-billing/understand/review-individual-bill.md)
