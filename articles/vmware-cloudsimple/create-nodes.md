---
title: Bereitstellen von Knoten für VMware Solution by CloudSimple – Azure
description: Hier erfahren Sie, wie Sie im Azure-Portal mit der CloudSimple-Bereitstellung Knoten zu Ihrer VMWare-Lösung hinzufügen. Sie können für Ihre private Cloudumgebung eine Kapazität mit nutzungsbasierter Bezahlung einrichten.
author: dikamath
ms.author: dikamath
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ccff5b4dc6ed5a571e25ab0569b9fa92674e1340
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88140733"
---
# <a name="provision-nodes-for-azure-vmware-solution-by-cloudsimple"></a>Bereitstellen von Knoten für Azure VMware Solution by CloudSimple

Stellen Sie Knoten im Azure-Portal bereit. Danach können Sie Kapazität mit nutzungsbasierter Bezahlung für die Umgebung Ihrer privaten CloudSimple-Cloud einrichten.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="add-a-node-to-your-cloudsimple-private-cloud"></a>Hinzufügen eines Knotens zu Ihrer privaten CloudSimple-Cloud

1. Wählen Sie **Alle Dienste** aus.
2. Suchen Sie nach **CloudSimple-Knoten**.

   ![Suchen nach CloudSimple-Knoten](media/create-cloudsimple-node-search.png)

3. Wählen Sie **CloudSimple-Knoten** aus.
4. Klicken Sie auf **Hinzufügen**, um Knoten zu erstellen.

    ![Hinzufügen von CloudSimple-Knoten](media/create-cloudsimple-node-add.png)

5. Wählen Sie das Abonnement aus, in dem Sie CloudSimple-Knoten bereitstellen möchten.
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

* [Erstellen einer privaten Cloud](create-private-cloud.md)
