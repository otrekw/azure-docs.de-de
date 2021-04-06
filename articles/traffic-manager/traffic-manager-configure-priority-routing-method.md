---
title: 'Tutorial: Konfigurieren der prioritätsbasierten Routingmethode für Datenverkehr in Azure Traffic Manager'
description: In diesem Tutorial wird die Konfiguration der prioritätsbasierten Routingmethode für Datenverkehr in Traffic Manager erläutert.
services: traffic-manager
documentationcenter: ''
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2020
ms.author: duau
ms.openlocfilehash: 1835377f4690097c8390957bf7d897242ba7aace
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92208055"
---
# <a name="tutorial-configure-priority-traffic-routing-method-in-traffic-manager"></a>Tutorial: Konfigurieren der prioritätsbasierten Routingmethode für Datenverkehr in Traffic Manager

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Traffic Manager und der prioritätsbasierten Routingmethode Benutzerdatenverkehr an bestimmte Endpunkte weiterleiten. Bei dieser Routingmethode definieren Sie die Reihenfolge jedes Endpunkts, der in die Traffic Manager-Profilkonfiguration aufgenommen wird. Datenverkehr von Benutzern wird in der aufgeführten Reihenfolge an den jeweiligen Endpunkt weitergeleitet. Diese Routingmethode ist nützlich, wenn Sie für ein Dienstfailover konfigurieren möchten. Der primäre Endpunkt erhält die Prioritätsnummer „1“ und verarbeitet alle eingehenden Anforderungen. Endpunkte mit niedrigerer Priorität fungieren hingegen als Sicherungen.

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> - Erstellen eines Traffic Manager-Profils mit prioritätsbasiertem Routing.
> - Hinzufügen von Endpunkten.
> - Konfigurieren der Priorität von Endpunkten.
> - Verwenden des Traffic Manager-Profils.
> - Löschen des Traffic Manager-Profils

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="to-configure-the-priority-traffic-routing-method"></a>So konfigurieren Sie die prioritätsbasierte Routingmethode für Datenverkehr
1. Melden Sie sich im Browser beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Bereich **+ Ressource erstellen** aus. Suchen Sie nach **Traffic Manager-Profil**, und wählen Sie **Erstellen** aus.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/create-traffic-manager-priority-profile.png" alt-text="Erstellen eines Traffic Manager-Prioritätsprofils":::

1. Definieren Sie auf der Seite *Traffic Manager-Profil erstellen* die folgenden Einstellungen:

    | Einstellung         | Wert                                              |
    | ---             | ---                                                |
    | Name            | Geben Sie einen Namen für Ihr Profil an. Dieser Name muss innerhalb der Zone trafficmanager.net eindeutig sein. Um auf das Traffic Manager-Profil zuzugreifen, verwenden Sie den DNS-Namen `<profilename>.trafficmanager.net`. |    
    | Routingmethode  | Wählen Sie **Priorität**. |
    | Subscription    | Wählen Sie Ihr Abonnement aus. |
    | Resource group   | Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe, unter der Sie das Profil platzieren möchten. Wenn Sie sich für die Erstellung einer neuen Ressourcengruppe entscheiden, verwenden Sie die Dropdownliste *Ressourcengruppenstandort*, um den Speicherort der Ressourcengruppe anzugeben. Diese Einstellung bezieht sich auf den Speicherort der Ressourcengruppe und hat keine Auswirkungen auf das global bereitgestellte Traffic Manager-Profil. |

1. Wählen Sie **Erstellen** aus, um Ihr Traffic Manager-Profil zu erstellen.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/create-traffic-manager-profile-priority.png" alt-text="Erstellen einer Traffic Manager-Profilpriorität":::

## <a name="add-endpoints"></a>Hinzufügen von Endpunkten

1. Wählen Sie das Traffic Manager-Profil in der Liste aus.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/traffic-manager-profile-list.png" alt-text="Traffic Manager-Profilliste":::

1. Wählen Sie **Endpunkte** unter *Einstellungen* aus, und wählen Sie **+ Hinzufügen** aus, um einen neuen Endpunkt hinzuzufügen.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/traffic-manager-add-endpoints.png" alt-text="Traffic Manager: „Endpunkte hinzufügen“":::

1. Wählen Sie die folgenden Einstellungen aus, oder geben Sie sie ein: 

    | Einstellung                | Wert                                              |
    | ---                    | ---                                                |
    | Typ                   | Wählen Sie den Endpunkttyp aus. |    
    | Name                   | Vergeben Sie einen Namen zum Identifizieren dieses Endpunkts. |
    | Zielressourcentyp   | Wählen Sie den Ressourcentyp für das Ziel aus. |
    | Zielressource        | Wählen Sie die Ressource aus der Liste aus. |
    | Priorität               | Vergeben Sie eine Prioritätszahl an diesen Endpunkt. 1 ist die höchste Priorität. |


1. Wählen Sie **Hinzufügen** aus, um den Endpunkt hinzuzufügen. Wiederholen Sie die Schritte 2 und 3, um zusätzliche Endpunkte hinzuzufügen. Denken Sie daran, die entsprechende Prioritätsnummer festzulegen.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/add-endpoint.png" alt-text="Endpunkt mit Priorität 1 hinzufügen":::

1. Überprüfen Sie auf der Seite **Endpunkte** die Prioritätsreihenfolge für Ihre Endpunkte. Wenn Sie die Routingmethode für Datenverkehr **Priorität** auswählen, ist die Reihenfolge der ausgewählten Endpunkte von Bedeutung. Überprüfen Sie die Prioritätsreihenfolge der Endpunkte.  Der primäre Endpunkt steht an erster Stelle. Überprüfen Sie die angezeigte Reihenfolge dieses Endpunkts erneut. Alle Anforderungen werden an den ersten Endpunkt weitergeleitet. Wenn dieser in Traffic Manager als fehlerhaft erkannt wird, wird für den Datenverkehr automatisch ein Failover auf den nächsten Endpunkt durchgeführt. 

    :::image type="content" source="./media/traffic-manager-priority-routing-method/endpoints-list.png" alt-text="Liste der Prioritätsendpunkte":::

1. Um die Prioritätsreihenfolge des Endpunkts zu ändern, wählen Sie den Endpunkt aus, ändern Sie den Prioritätswert, und wählen Sie dann **Speichern** aus, um die Endpunkteinstellungen zu speichern.

## <a name="use-the-traffic-manager-profile"></a>Verwenden des Traffic Manager-Profils

1.  Suchen Sie in der Suchleiste des Portals nach dem Namen des **Traffic Manager-Profils**, das Sie im vorhergehenden Abschnitt erstellt haben, und wählen Sie in den angezeigten Ergebnissen das Traffic Manager-Profil aus.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/search-traffic-manager-profile.png" alt-text="Suchen eines Traffic Manager-Profils":::

1.  Auf der Übersichtsseite **Traffic Manager-Profil** wird der DNS-Name Ihres neu erstellten Traffic Manager-Profils angezeigt. Dieser kann von beliebigen Clients (etwa durch Navigation in einem Webbrowser) für das Routing zum richtigen Endpunkt nach Maßgabe des Routingtyps verwendet werden. In diesem Fall werden alle Anforderungen an den ersten Endpunkt weitergeleitet. Wenn dieser in Traffic Manager als fehlerhaft erkannt wird, wird für den Datenverkehr automatisch ein Failover auf den nächsten Endpunkt durchgeführt.

    :::image type="content" source="./media/traffic-manager-priority-routing-method/traffic-manager-profile-dns-name.png" alt-text="Traffic Manager-DNS-Name":::

1. Sobald das Traffic Manager-Profil funktionsfähig ist, bearbeiten Sie den DNS-Eintrag auf dem autoritativen DNS-Server, damit Ihre Unternehmensdomäne auf den Namen der Traffic Manager-Domäne verweisen kann.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie das Traffic Manager-Profil nicht mehr benötigen, suchen Sie das Profil, und wählen Sie **Profil löschen** aus.

:::image type="content" source="./media/traffic-manager-priority-routing-method/traffic-manager-delete-priority-profile.png" alt-text="Löschen eines Traffic Manager-Prioritätsprofils":::

## <a name="next-steps"></a>Nächste Schritte

Hier erfahren Sie mehr zur prioritätsbasierten Routingmethode:

> [!div class="nextstepaction"]
> [Prioritätsbasierte Routingmethode](traffic-manager-routing-methods.md#priority-traffic-routing-method)
