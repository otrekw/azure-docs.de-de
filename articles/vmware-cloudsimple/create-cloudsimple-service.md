---
title: 'Azure-VMware-Lösungen (AVS): Erstellen eines AVS-Diensts'
description: In diesem Artikel wird das Erstellen des AVS-Diensts im Azure-Portal beschrieben.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 468ef8751438812422d7eee83d98acc9e3aedb82
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024822"
---
# <a name="create-the-azure-vmware-solutions-avs-service"></a>Erstellen des AVS-Diensts (Azure-VMware-Lösungen)

Erstellen Sie für den Einstieg mit AVS (Azure-VMware-Lösungen) den AVS-Dienst im Azure-Portal.

## <a name="before-you-begin"></a>Voraussetzungen

Ordnen Sie einen /28-CIDR-Block für das Gatewaysubnetz zu. Ein Gatewaysubnetz ist für jeden AVS-Dienst erforderlich und für die Region spezifisch, in der es erstellt wird. Das Gatewaysubnetz wird für Edge-Netzwerkdienste verwendet und erfordert einen CIDR-Block vom Typ „/28“. Der Adressraum des Gatewaysubnetzes muss eindeutig sein. Er darf sich nicht mit dem eines Netzwerks überschneiden, das mit der AVS-Umgebung kommuniziert. Zu den Netzwerken, die mit AVS kommunizieren, gehören unter anderem lokale Netzwerke und virtuelle Azure-Netzwerke.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-the-service"></a>Erstellen des Diensts

1. Wählen Sie **Alle Dienste** aus.
2. Suchen Sie nach **AVS Services** (AVS-Dienste).
    ![Suchen nach AVS-Diensten](media/create-cloudsimple-service-search.png)
3. Klicken Sie auf **AVS Services** (AVS-Dienste).
4. Klicken Sie auf **Hinzufügen**, um einen neuen Dienst zu erstellen.
    ![Hinzufügen von AVS-Diensten](media/create-cloudsimple-service-add.png)
5. Wählen Sie das Abonnement aus, in dem Sie den AVS-Dienst erstellen möchten.
6. Wählen Sie die Ressourcengruppe für den Dienst aus. Um eine neue Ressourcengruppe hinzuzufügen, klicken Sie auf **Neue erstellen**.
7. Geben Sie einen Namen ein, um den Dienst zu benennen.
8. Geben Sie das CIDR für das Dienstgateway ein. Geben Sie ein /28-Subnetz an, das mit keinem Ihrer lokalen Subnetze, Azure-Subnetzen oder geplanten AVS-Subnetzen überlappt. Sie können das CIDR nicht mehr ändern, nachdem der Dienst erstellt wurde.

    ![Erstellen des AVS-Diensts](media/create-cloudsimple-service.png)
9. Klicken Sie auf **OK**.

Der Dienst wird erstellt und zur Liste der Dienste hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Knoten bereitstellen](create-nodes.md).
* Informieren Sie sich über das [Erstellen einer privaten AVS-Cloud](create-private-cloud.md).
* Erkundigen Sie sich über das [Konfigurieren einer privaten AVS-Cloudumgebung](quickstart-create-private-cloud.md).
