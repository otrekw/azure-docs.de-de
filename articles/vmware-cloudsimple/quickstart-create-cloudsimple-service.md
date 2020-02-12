---
title: 'Schnellstart für Azure VMware Solutions (AVS): Erstellen des Diensts'
description: Erfahren Sie, wie der AVS-Dienst erstellt wird und wie Knoten erworben und reserviert werden.
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e7eb414e51ca38f524ab83bfb51f80f771524287
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024431"
---
# <a name="quickstart---create-azure-vmware-solutions-avs-service"></a>Schnellstart: Erstellen des AVS-Diensts (Azure VMware Solutions)

Erstellen Sie zunächst Azure VMware Solutions (AVS) im Azure-Portal.

## <a name="vmware-solutions-avs---service-overview"></a>VMware Solutions (AVS): Dienstübersicht

Der AVS-Dienst ermöglicht Ihnen, Azure VMware Solutions (AVS) zu nutzen. Nach dem Erstellen des Diensts können Sie Knoten bereitstellen und reservieren sowie private AVS-Clouds erstellen. Sie fügen den AVS-Dienst in jeder Azure-Region hinzu, in der der AVS-Dienst verfügbar ist. Der Dienst definiert das Umkreisnetzwerk von Azure VMware Solutions (AVS). Dieses Edgenetzwerk wird für Dienste verwendet, zu denen VPN, ExpressRoute und Internetkonnektivität mit Ihren privaten AVS-Clouds gehören.

Um den AVS-Dienst hinzuzufügen, müssen Sie ein Gatewaysubnetz erstellen. Das Gatewaysubnetz wird beim Erstellen des Umkreisnetzwerks verwendet und erfordert einen CIDR-Block vom Typ „/28“. Der Adressraum des Gatewaysubnetzes muss eindeutig sein. Er darf nicht mit einem Ihrer lokalen Netzwerkadressräume oder mit einem Adressraum des virtuellen Azure-Netzwerk überlappen.

## <a name="before-you-begin"></a>Voraussetzungen

Ordnen Sie einen /28-CIDR-Block für das Gatewaysubnetz zu. Ein Gatewaysubnetz ist für jeden einzelnen AVS-Dienst erforderlich und für die Region spezifisch, in der es erstellt wird. Das Gatewaysubnetz wird für Edge-Netzwerkdienste von Azure VMware Solutions (AVS) verwendet und erfordert einen CIDR-Block vom Typ „/28“. Der Adressraum des Gatewaysubnetzes muss eindeutig sein. Er darf sich nicht mit dem eines Netzwerks überschneiden, das mit der AVS-Umgebung kommuniziert. Zu den Netzwerken, die mit AVS kommunizieren, gehören unter anderem lokale Netzwerke und virtuelle Azure-Netzwerke.

Lesen Sie [Voraussetzungen für Netzwerke](cloudsimple-network-checklist.md). 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-the-service"></a>Erstellen des Diensts

1. Wählen Sie **Alle Dienste** aus.
2. Suchen Sie nach **AVS Service** (AVS-Dienst).

    ![Suchen nach AVS-Dienst](media/create-cloudsimple-service-search.png)

3. Wählen Sie **AVS Services** (AVS-Dienste) aus.
4. Klicken Sie auf **Hinzufügen**, um einen neuen Dienst zu erstellen.

    ![Hinzufügen des AVS-Diensts](media/create-cloudsimple-service-add.png)

5. Wählen Sie das Abonnement aus, in dem Sie den AVS-Dienst erstellen möchten.
6. Wählen Sie die Ressourcengruppe für den Dienst aus. Um eine neue Ressourcengruppe hinzuzufügen, klicken Sie auf **Neue erstellen**.
7. Geben Sie einen Namen ein, um den Dienst zu benennen.
8. Geben Sie das CIDR für das Dienstgateway ein. Geben Sie ein /28-Subnetz an, das mit keinem Ihrer lokalen Subnetze, Azure-Subnetzen oder geplanten AVS-Subnetzen überlappt. Sie können das CIDR nicht mehr ändern, nachdem der Dienst erstellt wurde.

    ![Erstellen des AVS-Diensts](media/create-cloudsimple-service.png)

9. Klicken Sie auf **OK**.

Der Dienst wird erstellt und zur Liste der Dienste hinzugefügt.

## <a name="provision-nodes"></a>Bereitstellen von Knoten

Um für die Umgebung einer privaten AVS-Cloud Kapazität mit nutzungsbasierter Bezahlung einzurichten, müssen Sie zuerst Knoten im Azure-Portal bereitstellen.

1. Wählen Sie **Alle Dienste** aus.
2. Suchen Sie nach **AVS Nodes** (AVS-Knoten).

    ![Suchen nach AVS-Knoten](media/create-cloudsimple-node-search.png)

3. Wählen Sie **AVS Nodes** (AVS-Knoten) aus.
4. Klicken Sie auf **Hinzufügen**, um Knoten zu erstellen.

    ![Hinzufügen von AVS-Knoten](media/create-cloudsimple-node-add.png)

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

* [Erstellen einer privaten AVS-Cloud und Konfigurieren der Umgebung](quickstart-create-private-cloud.md)
* Weitere Informationen zum [AVS-Dienst](https://docs.azure.cloudsimple.com/cloudsimple-service)
