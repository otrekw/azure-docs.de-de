---
title: Löschen einer privaten AVS-Cloud in Azure-VMware-Lösungen
description: In diesem Artikel wird beschrieben, wie Sie eine private AVS-Cloud löschen.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 77cbfb19c3861bac517142f7491e6b1a5fb4ca27
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024754"
---
# <a name="delete-an-avs-private-cloud"></a>Löschen einer privaten AVS-Cloud

AVS bietet die Flexibilität, eine private AVS-Cloud zu löschen. Eine private AVS-Cloud besteht aus einem oder mehreren vSphere-Clustern. Jeder Cluster kann 3 bis 16 Knoten enthalten. Wenn Sie eine private AVS-Cloud löschen, werden alle Cluster gelöscht.

## <a name="before-you-begin"></a>Voraussetzungen

Beim Löschen einer privaten AVS-Cloud wird die gesamte private AVS-Cloud gelöscht. Alle Komponenten der privaten AVS-Cloud werden gelöscht. Wenn Sie einige Daten beibehalten möchten, müssen Sie sicherstellen, dass Sie diese Daten im lokalen Speicher oder in Azure Storage gesichert haben.

Zu den Komponenten einer privaten AVS-Cloud gehören folgende:

* AVS-Knoten
* Virtuelle Computer
* VLANs/Subnetze
* alle Benutzerdaten, die in der privaten AVS-Cloud gespeichert sind
* Alle Firewallregelanlagen für ein VLAN/Subnetz

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="delete-an-avs-private-cloud"></a>Löschen einer privaten AVS-Cloud

1. [Greifen Sie auf das AVS-Portal zu.](access-cloudsimple-portal.md)

2. Öffnen Sie die Seite **Ressourcen**.

3. Klicken Sie auf die zu löschende private AVS-Cloud.

4. Klicken Sie auf der Zusammenfassungsseite auf **Löschen**.

    ![Löschen einer privaten AVS-Cloud](media/delete-private-cloud.png)

5. Geben Sie auf der Bestätigungsseite den Namen der privaten AVS-Cloud ein, und klicken Sie auf **Löschen**. 

    ![Löschen einer privaten AVS-Cloud: Bestätigung](media/delete-private-cloud-confirm.png)

Die private AVS-Cloud wird zum Löschen markiert. Der Löschvorgang beginnt nach drei Stunden und löscht die private AVS-Cloud.

> [!CAUTION]
> Knoten müssen nach dem Löschen der privaten AVS-Cloud gelöscht werden. Die Messung der Knoten wird fortgesetzt, bis die Knoten aus Ihrem Abonnement gelöscht wurden.

## <a name="next-steps"></a>Nächste Schritte

* [Löschen von Knoten](delete-nodes.md)
