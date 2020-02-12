---
title: Bereitstellen von Knoten für VMware-Lösungen (AVS) – Azure
description: Erfahren Sie, wie Sie mit AVS-Bereitstellung Knoten zu Ihrer VMware-Lösung hinzufügen können.
author: dikamath
ms.author: dikamath
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 94ca681153f1e3ccd9ff628b41a9d0e547be5802
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024805"
---
# <a name="provision-nodes-for-azure-vmware-solutions-avs"></a>Bereitstellen von Knoten für Azure-VMware-Lösungen (AVS)

Stellen Sie Knoten im Azure-Portal bereit. Danach können Sie für Ihre private AVS-Cloudumgebung eine Kapazität mit nutzungsbasierter Bezahlung einrichten.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="add-a-node-to-your-avs-private-cloud"></a>Hinzufügen eines Knotens zu Ihrer privaten AVS-Cloud

1. Wählen Sie **Alle Dienste** aus.
2. Suchen Sie nach **AVS Nodes** (AVS-Knoten).

   ![Suche AVS Nodes (AVS-Knoten)](media/create-cloudsimple-node-search.png)

3. Wählen Sie **AVS Nodes** (AVS-Knoten) aus.
4. Klicken Sie auf **Hinzufügen**, um Knoten zu erstellen.

    ![AVS-Knoten hinzufügen](media/create-cloudsimple-node-add.png)

5. Wählen Sie das Abonnement aus, in dem Sie AVS-Knoten bereitstellen möchten.
6. Wählen Sie die Ressourcengruppe für die Knoten aus. Um eine neue Ressourcengruppe hinzuzufügen, klicken Sie auf **Neue erstellen**.
7. Geben Sie das Präfix ein, um die Knoten zu kennzeichnen.
8. Wählen Sie den Standort für die Knotenressourcen aus.
9. Wählen Sie den dedizierten Standort (Dedicated location) aus, in dem die Knotenressourcen gehostet werden sollen.
10. Wählen Sie den [Knotentyp](cloudsimple-node.md) aus.
11. Wählen Sie die Anzahl der bereitzustellenden Knoten aus.
12. Klicken Sie auf **Überprüfen + erstellen**.
13. Überprüfen Sie die Einstellungen. Wenn Sie irgendwelche Einstellungen ändern möchten, klicken Sie auf **Zurück**.
14. Klicken Sie auf **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer privaten AVS-Cloud](create-private-cloud.md)
