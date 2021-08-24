---
title: Installieren der `arcdata`-Erweiterung
description: Installieren der `arcdata`-Erweiterung für Azure (az) CLI
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 07/30/2021
ms.topic: how-to
ms.openlocfilehash: 5e1ddfdc13ff7bf11a2a1293ebf39c4c527c6d0f
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355976"
---
# <a name="install-arcdata-azure-cli-extension"></a>Installieren der Azure CLI-Erweiterung `arcdata`

> [!IMPORTANT]
> Wenn Sie ein Update auf ein neues Release durchführen, stellen Sie sicher, dass Sie auch auf die neueste Version von Azure CLI und der `arcdata`-Erweiterung aktualisieren.


## <a name="install-latest-azure-cli"></a>Installation der neuesten Azure CLI 

Informationen, wie Sie die neueste Azure CLI erhalten, finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).


## <a name="add-arcdata-extension"></a>Hinzufügen der `arcdata`-Erweiterung

Führen Sie zum Hinzufügen der Erweiterung den folgenden Befehl aus: 

```azurecli
az extension add --name arcdata 
```

[Weitere Informationen zu Azure CLI-Erweiterungen](/cli/azure/azure-cli-extensions-overview).

## <a name="update-arcdata-extension"></a>Aktualisieren der `arcdata`-Erweiterung

Wenn Sie bereits über die Erweiterung verfügen, können Sie sie mit dem folgenden Befehl aktualisieren:

```azurecli
az extension update --name arcdata
```

## <a name="next-steps"></a>Nächste Schritte

[Erstellen des Azure Arc-Datencontrollers](create-data-controller.md)
