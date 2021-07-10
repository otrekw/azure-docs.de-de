---
title: Datei einfügen
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/27/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c5741ab0207d5631aab4abd594f5ff557ebfe23b
ms.sourcegitcommit: 1b698fb8ceb46e75c2ef9ef8fece697852c0356c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110658245"
---
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Geben Sie in **Nach Ressourcen, Diensten und Dokumenten suchen (G+/)** den Begriff *virtuelles Netzwerk* ein.

   :::image type="content" source="./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace.png" alt-text="Screenshot der Suchleiste im Azure-Portal" border="false":::
1. Wählen Sie aus den **Marketplace**-Ergebnissen das Ergebnis **Virtual Network** aus.

   :::image type="content" source="./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace-results.png" alt-text="Screenshot: Azure-Portal mit Suchleistenergebnissen und Auswahl von „Virtuelles Netzwerk“ im Marketplace." border="false":::
1. Wählen Sie auf der Seite **Virtuelles Netzwerk** die Option **Erstellen** aus.

   :::image type="content" source="./media/vpn-gateway-basic-vnet-rm-portal-include/vnet-click-create.png" alt-text="Screenshot: Seite „Virtuelles Netzwerk“ mit Auswahl der Schaltfläche „Erstellen“." border="false":::
1. Nachdem Sie **Erstellen** ausgewählt haben, wird die Seite **Virtuelles Netzwerk erstellen** geöffnet.
1. Konfigurieren Sie auf der Registerkarte **Grundlagen** die VNET-Einstellungen **Projektdetails** und **Instanzendetails**.

   :::image type="content" source="./media/vpn-gateway-basic-vnet-rm-portal-include/basics.png" alt-text="Screenshot der Registerkarte „Grundlagen“." border="false":::

   Beim Ausfüllen der Felder wird ein grünes Häkchen angezeigt, nachdem die eingegebenen Zeichen überprüft wurden. Einige Werte werden automatisch eingetragen und können durch eigene Werte ersetzt werden:

   - **Abonnement**: Vergewissern Sie sich, dass das richtige Abonnement angegeben ist. Das Abonnement kann über die Dropdownliste geändert werden.
   - **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe aus, oder klicken Sie auf **Neu erstellen**, um eine zu erstellen. Weitere Informationen zu Ressourcengruppen finden Sie unter [Azure Resource Manager – Übersicht](../articles/azure-resource-manager/management/overview.md#resource-groups).
   - **Name**: Geben Sie den Namen für Ihr virtuelles Netzwerk ein.
   - **Region**: Wählen Sie den Standort für Ihr VNET aus. Der Standort gibt an, wo sich die in diesem VNET bereitgestellten Ressourcen befinden.

1. Konfigurieren Sie auf der Registerkarte **IP-Adressen** die Werte. Die in den nachstehenden Beispielen dargestellten Werte dienen zu Demonstrationszwecken. Passen Sie diese Werte entsprechend Ihren erforderlichen Einstellungen an.

   :::image type="content" source="./media/vpn-gateway-basic-vnet-rm-portal-include/addresses.png" alt-text="Screenshot: Registerkarte „IP-Adressen“." border="false"::: 
   - **IPv4-Adressraum:** Standardmäßig wird automatisch ein Adressraum erstellt. Sie können auf den Adressraum klicken, um ihn an Ihre eigenen Werte anzupassen. Darüber hinaus können Sie zusätzliche Adressräume hinzufügen.
   - **Subnetz**: Wenn Sie den Standardadressraum verwenden, wird automatisch ein Standardsubnetz erstellt. Wenn Sie den Adressraum ändern, müssen Sie ein Subnetz hinzufügen. Wählen Sie **+ Subnetz hinzufügen** aus, um das Fenster **Subnetz hinzufügen** zu öffnen. Konfigurieren Sie die folgenden Einstellungen, und wählen Sie dann **Hinzufügen** aus, um die Werte hinzuzufügen:
      - **Subnetzname**: In diesem Beispiel haben wir dem Subnetz den Namen „FrontEnd“ gegeben.
      - **Subnetzadressbereich:** Der Adressbereich für dieses Subnetz.

1. Behalten Sie auf der Registerkarte **Sicherheit** vorläufig die Standardwerte bei:

   - **DDoS Protection**: Basic
   - **Firewall**: Disabled
1. Wählen Sie **Bewerten + erstellen** aus, um die Einstellungen für das virtuelle Netzwerk zu überprüfen.
1. Nachdem die Einstellungen überprüft wurden, wählen Sie **Erstellen** aus.
