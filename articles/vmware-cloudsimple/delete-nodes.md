---
title: Löschen von Knoten für VMware-Lösungen (AVS) – Azure
description: Erfahren Sie, wie Sie mit AVS-Bereitstellung Knoten aus Ihrer VMware-Lösung löschen können.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 30d128d6bd2f2e1e2705a7b742f02d11fd947a03
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024737"
---
# <a name="delete-nodes-from-azure-vmware-solution-by-avs"></a>Löschen von Knoten aus einer Azure-VMware-Lösung von AVS

AVS-Knoten werden ab der Erstellung abgerechnet. Die Knoten müssen gelöscht werden, um die Abrechnung zu beenden. Sie löschen die nicht mehr verwendeten Knoten im Azure-Portal.

## <a name="before-you-begin"></a>Voraussetzungen

Ein Knoten kann nur unter folgenden Bedingungen gelöscht werden:

* Eine mit den Knoten erstellte private AVS-Cloud wird gelöscht. Informationen zum Löschen einer privaten AVS-Cloud finden Sie unter [Löschen einer Azure-VMware-Lösung bei einer privaten AVS-Cloud](delete-private-cloud.md).
* Der Knoten wurde durch Verkleinern der privaten AVS-Cloud aus der privaten AVS-Cloud entfernt. Informationen zum Verkleinern einer privaten AVS-Cloud finden Sie unter [Verkleinern einer Azure-VMware-Lösung bei einer privaten AVS-Cloud](shrink-private-cloud.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="delete-avs-node"></a>Löschen von AVS-Knoten

1. Wählen Sie **Alle Dienste** aus.

2. Suchen Sie nach **AVS Nodes** (AVS-Knoten).

   ![Suche AVS Nodes (AVS-Knoten)](media/create-cloudsimple-node-search.png)

3. Wählen Sie **AVS Nodes** (AVS-Knoten) aus.

4. Wählen Sie Knoten, die nicht zu einer privaten AVS-Cloud gehören, zum Löschen aus. Die Spalte **AVS PRIVATE CLOUD NAME** (NAME DER PRIVATEN AVS-CLOUD) enthält den Namen der privaten AVS-Cloud, zu der ein Knoten gehört. Wenn ein Knoten von keiner privaten AVS-Cloud verwendet wird, ist dieser Wert leer. 

    ![AVS-Knoten auswählen](media/select-delete-cloudsimple-node.png)

> [!NOTE]
> Nur Knoten, die nicht Teil der privaten AVS-Cloud sind, können gelöscht werden.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [private AVS-Clouds](cloudsimple-private-cloud.md).
