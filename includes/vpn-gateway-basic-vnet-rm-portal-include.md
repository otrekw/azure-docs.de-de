---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/25/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 872ba86c9e43b1f6642331908eb829605f6c19bd
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77619698"
---
Sie können ein VNET wie im Folgenden beschrieben mit dem Resource Manager-Bereitstellungsmodell im Azure-Portal erstellen. Weitere Informationen zu virtuellen Netzwerken finden Sie in der [Übersicht über Azure Virtual Network](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Um das VNET mit einem lokalen Standort zu verbinden, legen Sie in Zusammenarbeit mit dem Administrator des lokalen Netzwerks einen IP-Adressbereich fest, den Sie speziell für dieses virtuelle Netzwerk verwenden können. Wenn ein Adressbereich auf beiden Seiten der VPN-Verbindung und somit doppelt vorhanden ist, wird Datenverkehr nicht wie erwartet weitergeleitet. Wenn Sie für dieses VNet eine Verbindung mit einem anderen VNet herstellen möchten, darf sich der Adressraum außerdem nicht mit dem anderen VNet überlappen. Planen Sie Ihre Netzwerkkonfiguration entsprechend.
>
>

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Geben Sie in **Nach Ressourcen, Diensten und Dokumenten suchen (G+/)** den Begriff *virtuelles Netzwerk* ein.

   ![Navigieren zur Seite mit den Ressourcen des virtuellen Netzwerks](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace.png "Navigieren zur Seite mit den Ressourcen des virtuellen Netzwerks")
1. Wählen Sie aus den **Marketplace**-Ergebnissen das Ergebnis **Virtual Network** aus.

   ![Virtuelles Netzwerk auswählen](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace-results.png "Navigieren zur Seite mit den Ressourcen des virtuellen Netzwerks")
1. Klicken Sie auf der Seite **Virtuelles Netzwerk** auf **Erstellen**.

   ![Seite „Virtuelles Netzwerk“](./media/vpn-gateway-basic-vnet-rm-portal-include/vnet-click-create.png "Klicken Sie auf „Erstellen“.")
1. Nachdem Sie auf „Erstellen“ geklickt haben, wird die Seite **Virtuelles Netzwerk erstellen** geöffnet.

   ![Seite „Virtuelles Netzwerk erstellen“](./media/vpn-gateway-basic-vnet-rm-portal-include/create-virtual-network-page.png "Seite „Virtuelles Netzwerk erstellen“")
1. Konfigurieren Sie auf der Seite **Virtuelles Netzwerk erstellen** die VNET-Einstellungen. Beim Ausfüllen der Felder ändert sich das rote Ausrufezeichen in ein grünes Häkchen, sobald die eingegebenen Zeichen überprüft wurden. Einige Werte werden automatisch eingetragen und können durch eigene Werte ersetzt werden:

   - **Name**: Geben Sie den Namen für Ihr virtuelles Netzwerk ein.
   - **Adressraum:** Geben Sie den Adressraum ein. Falls Sie mehrere Adressräume hinzufügen möchten, geben Sie hier den ersten Adressraum ein. Sie können später weitere Adressräume hinzufügen, nachdem Sie das VNET erstellt haben. Wenn für Ihre Konfiguration der IPv6-Adressraum erforderlich ist, aktivieren Sie das Kontrollkästchen, um die folgenden Informationen einzugeben.
   - **Abonnement**: Vergewissern Sie sich, dass das richtige Abonnement angegeben ist. Das Abonnement kann über die Dropdownliste geändert werden.
   - **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe aus, oder geben Sie einen Namen ein, um eine neue Ressourcengruppe zu erstellen. Verwenden Sie für eine neue Ressourcengruppe einen Namen, der zu Ihren geplanten Konfigurationswerten passt. Weitere Informationen zu Ressourcengruppen finden Sie unter [Azure Resource Manager – Übersicht](../articles/azure-resource-manager/management/overview.md#resource-groups).
   - **Standort**: Wählen Sie den Standort für Ihr VNET aus. Der Standort gibt an, wo sich die in diesem VNET bereitgestellten Ressourcen befinden.
   - **Subnetz**: Fügen Sie den **Namen** und **Adressbereich** des Subnetzes hinzu. Sie können später weitere Subnetze hinzufügen, nachdem Sie das VNET erstellt haben.
   - **DDoS Protection**: Wählen Sie **Basic** aus, sofern Sie nicht den Standard-Dienst verwenden möchten.
   - **Dienstendpunkte**: Sie können diese Einstellung als **Deaktiviert** beibehalten – außer wenn Ihre Konfiguration diese Einstellung erfordert.
   - **Firewall**: Sie können diese Einstellung als **Deaktiviert** beibehalten – außer wenn Ihre Konfiguration diese Einstellung erfordert.
1. Klicken Sie auf **Erstellen**, um mit der Bereitstellung des virtuellen Netzwerks zu beginnen.
