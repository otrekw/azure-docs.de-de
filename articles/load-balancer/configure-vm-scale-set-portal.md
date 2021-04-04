---
title: Konfigurieren von VM-Skalierungsgruppen mit einer vorhandenen Azure Load Balancer-Instanz – Azure-Portal
description: Erfahren Sie, wie Sie eine VM-Skalierungsgruppe mit einer vorhandenen Azure Load Balancer-Instanz mithilfe des Azure-Portals konfigurieren.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: how-to
ms.date: 03/25/2020
ms.openlocfilehash: cb7bfb9ac4b10b807ac186d087b0037953abd559
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91439521"
---
# <a name="configure-a-virtual-machine-scale-set-with-an-existing-azure-load-balancer-using-the-azure-portal"></a>Konfigurieren einer VM-Skalierungsgruppe mit einer vorhandenen Azure Load Balancer-Instanz mithilfe des Azure-Portals

In diesem Artikel erfahren Sie, wie Sie eine VM-Skalierungsgruppe mit einer vorhandenen Azure Load Balancer-Instanz konfigurieren. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement.
- Ein vorhandener Standard-SKU-Lastenausgleich in dem Abonnement, in dem die VM-Skalierungsgruppe bereitgestellt werden soll.
- Eine Azure Virtual Network-Instanz für die VM-Skalierungsgruppe.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.



## <a name="deploy-virtual-machine-scale-set-with-existing-load-balancer"></a>Bereitstellen einer VM-Skalierungsgruppe mit einem vorhandenen Lastenausgleich

In diesem Abschnitt erstellen Sie eine VM-Skalierungsgruppe im Azure-Portal mit einem vorhandenen Azure-Lastenausgleich.

> [!NOTE]
> Die folgenden Schritte gehen davon aus, dass ein virtuelles Netzwerk namens **myVNet** und ein Azure-Lastenausgleich namens **myLoadBalancer** zuvor bereitgestellt wurden.

1. Klicken Sie oben links auf dem Bildschirm auf **Ressource erstellen** > **Compute** > **VM-Skalierungsgruppe**, oder suchen Sie in der Marketplace-Suche nach **VM-Skalierungsgruppe**.

2. Klicken Sie auf **Erstellen**.

3. Geben Sie in **VM-Skalierungsgruppe erstellen** ein, oder wählen Sie diese Informationen auf der Registerkarte **Grundlagen** aus:

    | Einstellung                        | Wert                                                                                                 |
    |--------------------------------|-------------------------------------------------------------------------------------------------------|
    | **Projektdetails**            |                                                                                                       |
    | Abonnement                   | Auswählen des Azure-Abonnements                                                                        |
    | Ressourcengruppe                 | Wählen Sie „Neue erstellen“ aus, geben Sie **myResourceGroup** ein, und wählen Sie dann „OK“ oder eine vorhandene Ressourcengruppe aus. |
    | **Details zur Skalierungsgruppe**          |                                                                                                       |
    | Name der VM-Skalierungsgruppe | Geben Sie **myVMSS** ein.                                                                                      |
    | Region                         | Wählen Sie **USA, Osten 2** aus.                                                                                    |
    | Verfügbarkeitszone              | Wählen Sie **Keine** aus.                                                                                       |
    | **Instanzendetails**           |                                                                                                       |
    | Image                          | Klicken Sie auf **Ubuntu Server 18.04 LTS**.                                                                    |
    | Azure Spot-Instanz            | Wählen Sie **Nein** aus.                                                                                         |
    | Size                           | Belassen Sie den Standardwert.                                                                                      |
    | **Administratorkonto**      |                                                                                                       |
    | Authentifizierungsart            | Wählen Sie **Kennwort** aus.                                                                                   |
    | Username                       | Geben Sie Ihren Administratorbenutzernamen ein.        |
    | Kennwort                       | Geben Sie Ihr Administratorkennwort ein.    |
    | Kennwort bestätigen               | Geben Sie Ihr Administratorkennwort erneut ein. |


    :::image type="content" source="./media/vm-scale-sets/create-vm-scale-set-01.png" alt-text="Screenshot: Registerkarte „Grundlagen“ zum Erstellen von VM-Skalierungsgruppen." border="true":::

4. Wählen Sie die Registerkarte **Netzwerk** aus.

5. Geben Sie diese Informationen auf der Registerkarte **Netzwerk** ein oder wählen Sie sie aus:

     Einstellung                           | Wert                                                    |
    |-----------------------------------|----------------------------------------------------------|
    | **Konfiguration von virtuellen Netzwerken** |                                                          |
    | Virtuelles Netzwerk                   | Wählen Sie **myVNet** oder Ihr bestehendes virtuelles Netzwerk aus.      |
    | **Lastenausgleich**                |                                                          |
    | Verwenden Sie einen Lastenausgleich.               | Klicken Sie auf **Ja**.                                           |
    | **Lastenausgleichseinstellungen**       |                                                          |
    | Optionen für den Lastenausgleich            | Wählen Sie **Azure-Lastenausgleich** aus.                           |
    | Wählen Sie einen Lastenausgleich aus.            | Wählen Sie **myLoadBalancer** oder Ihren vorhandenen Lastenausgleich aus. |
    | Wählen Sie einen Back-End-Pool aus.             | Wählen Sie **myBackendPool** oder Ihren vorhandenen Back-End-Pool aus.  |

    :::image type="content" source="./media/vm-scale-sets/create-vm-scale-set-02.png" alt-text="Screenshot: Registerkarte „Netzwerk“ zum Erstellen von VM-Skalierungsgruppen." border="true":::

6. Wählen Sie die Registerkarte **Verwaltung** aus.

7. Legen Sie auf der Registerkarte **Verwaltung** die Option **Startdiagnose** auf **Aus** fest.

8. Wählen Sie die blaue Schaltfläche **Überprüfen + erstellen** aus.

9. Überprüfen Sie die Einstellungen, und wählen Sie die Schaltfläche **Erstellen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine VM-Skalierungsgruppe mit einer vorhandenen Azure Load Balancer-Instanz konfiguriert.  Weitere Informationen über VM-Skalierungsgruppen und Lastenausgleiche finden Sie unter:

- [Was versteht man unter Azure Load Balancer?](load-balancer-overview.md)
- [Was sind Skalierungsgruppen für virtuelle Computer?](../virtual-machine-scale-sets/overview.md)
