---
title: 'Tutorial: Konfigurieren von Routenfiltern für Microsoft-Peering: Azure-Portal'
description: In diesem Tutorial wird beschrieben, wie mithilfe des Azure-Portals Routenfilter für das Microsoft-Peering konfiguriert werden.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: tutorial
ms.date: 10/15/2020
ms.author: duau
ms.custom: seodec18
ms.openlocfilehash: d142028d197f9e279b5f1e05757946dc40d2c153
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92109135"
---
# <a name="tutorial-configure-route-filters-for-microsoft-peering-using-the-azure-portal"></a>Tutorial: Konfigurieren von Routenfiltern für Microsoft-Peering mithilfe des Azure-Portals

> [!div class="op_single_selector"]
> * [Azure-Portal](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure-Befehlszeilenschnittstelle](how-to-routefilter-cli.md)
> 

Routenfilter stellen eine Möglichkeit dar, um eine Teilmenge von unterstützten Diensten durch das Microsoft-Peering zu nutzen. Die in diesem Artikel erläuterten Schritte unterstützen Sie bei der Konfiguration und Verwaltung von Routenfiltern für ExpressRoute-Verbindungen.

Das Microsoft-Peering ermöglicht den Zugriff auf Microsoft 365-Dienste wie Exchange Online, SharePoint Online und Skype for Business. Wenn das Microsoft-Peering in einer ExpressRoute-Verbindung konfiguriert wird, werden alle Präfixe im Zusammenhang mit diesen Diensten über die eingerichteten BGP-Sitzungen angekündigt. Jedem Präfix wird zur Identifizierung des Diensts, der über das Präfix angeboten wird, ein BGP-Communitywert angefügt. Eine Liste der BGP-Communitywerte und der Dienste, denen sie zugeordnet sind, finden Sie unter [BGP-Communitys](expressroute-routing.md#bgp).

Die Konnektivität mit allen Azure- und Microsoft 365-Diensten bewirkt, dass eine große Anzahl von Präfixen über BGP angekündigt wird. Dadurch werden die Routingtabellen, die von Routern innerhalb Ihres Netzwerks verwaltet werden, erheblich größer. Wenn Sie nur eine Teilmenge der Dienste nutzen möchten, die über das Microsoft-Peering angeboten werden, können Sie die Routentabellen auf zwei Arten verringern. Ihre Möglichkeiten:

* Sie filtern unerwünschte Präfixe heraus, indem Sie Routenfilter auf BGP-Communitys anwenden. Routenfilterung ist eine standardmäßige Vorgehensweise bei Netzwerken, die bei zahlreichen Netzwerken zum Einsatz kommt.

* Sie definieren Routenfilter und wenden sie auf Ihre ExpressRoute-Verbindung an. Ein Routenfilter ist eine neue Ressource, mit der Sie die Liste der Dienste, die Sie über das Microsoft-Peering nutzen möchten, auswählen können. ExpressRoute-Router senden lediglich die Liste der Präfixe, die den im Routenfilter identifizierten Diensten zugehörig sind.

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:
> [!div class="checklist"]
> - Abrufen von BGP-Communitywerten
> - Erstellen eines Routenfilters und einer Filterregel
> - Verknüpfen des Routenfilters mit einer ExpressRoute-Leitung

### <a name="about-route-filters"></a><a name="about"></a>Informationen zu Routenfiltern

Wenn Microsoft-Peering für Ihre ExpressRoute-Leitung konfiguriert wird, richten die Microsoft Edge-Router über Ihren Konnektivitätsanbieter ein BGP-Sitzungspaar mit den Edge-Routern ein. Ihrem Netzwerk werden keine Routen angekündigt. Um Routenankündigungen für Ihr Netzwerk zu aktivieren, müssen Sie einen Routenfilter zuordnen.

Durch einen Routenfilter können Sie die Dienste identifizieren, die Sie über das Microsoft-Peering Ihrer ExpressRoute-Verbindung nutzen möchten. Im Wesentlichen handelt es sich um eine Zulassungsliste für alle BGP-Communitywerte. Sobald eine Routenfilterressource definiert und an eine ExpressRoute-Leitung angefügt ist, werden Ihrem Netzwerk alle Präfixe angekündigt, die den BGP-Communitywerten zugeordnet sind.

Um Routenfilter mit Microsoft 365-Diensten anfügen zu können, müssen Sie zur Nutzung von Microsoft 365-Diensten über ExpressRoute autorisiert sein. Wenn Sie nicht zur Nutzung von Microsoft 365-Diensten über ExpressRoute autorisiert sind, tritt beim Vorgang zum Anfügen von Routenfiltern ein Fehler auf. Weitere Informationen zum Autorisierungsvorgang finden Sie unter [Azure ExpressRoute für Microsoft 365](/microsoft-365/enterprise/azure-expressroute).

> [!IMPORTANT]
> Beim Microsoft-Peering von ExpressRoute-Verbindungen, die vor dem 1. August 2017 konfiguriert wurden, werden alle Dienstpräfixe über das Microsoft-Peering angekündigt, auch wenn keine Routenfilter definiert sind. Beim Microsoft-Peering von ExpressRoute-Verbindungen, die am oder nach dem 1. August 2017 konfiguriert wurden, werden Präfixe erst angekündigt, wenn der Verbindung ein Routenfilter hinzugefügt wurde.
> 

## <a name="prerequisites"></a>Voraussetzungen

- Lesen Sie vor Beginn der Konfiguration die Seiten zu den [Voraussetzungen](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md).

- Sie benötigen eine aktive ExpressRoute-Verbindung, für die das Microsoft-Peering bereitgestellt ist. Zur Durchführung dieser Aufgaben können Sie folgende Anweisungen befolgen:
  - [Erstellen Sie eine ExpressRoute-Leitung](expressroute-howto-circuit-portal-resource-manager.md), und lassen Sie die Leitung von Ihrem Konnektivitätsanbieter aktivieren, bevor Sie fortfahren. Die ExpressRoute-Verbindung muss den Zustand „Provisioned“ und „Enabled“ aufweisen.
  - [Erstellen Sie das Microsoft-Peering](expressroute-howto-routing-portal-resource-manager.md), wenn Sie die BGP-Sitzung direkt verwalten. Überlassen Sie die Bereitstellung des Microsoft-Peerings für Ihre Verbindung wahlweise Ihrem Konnektivitätsanbieter.

## <a name="get-a-list-of-prefixes-and-bgp-community-values"></a><a name="prefixes"></a>Abrufen einer Liste von Präfixen und BGP-Communitywerten

### <a name="get-a-list-of-bgp-community-values"></a>Abrufen einer Liste von BGP-Communitywerten

BGP-Communitywerte, die Diensten zugeordnet sind, die über Microsoft-Peering verfügbar sind, finden Sie auf der Seite [ExpressRoute-Routinganforderungen](expressroute-routing.md).

### <a name="make-a-list-of-the-values-that-you-want-to-use"></a>Erstellen einer Liste der zu verwendenden Werte

Erstellen Sie eine Liste mit [BGP-Communitywerten](expressroute-routing.md#bgp), die Sie im Routenfilter verwenden möchten. 

## <a name="create-a-route-filter-and-a-filter-rule"></a><a name="filter"></a>Erstellen eines Routenfilters und einer Filterregel

Ein Routenfilter kann nur eine Regel aufweisen, die zudem vom Typ „Zulassen“ sein muss. Diese Regel kann eine Liste von BGP-Communitywerten enthalten, die ihr zugeordnet sind.

1. Wählen Sie **Ressource erstellen** aus, und suchen Sie dann nach *Routenfilter* wie in der folgenden Abbildung gezeigt:

    :::image type="content" source="./media/how-to-routefilter-portal/create-route-filter.png" alt-text="Screenshot: Seite „Routenfilter“":::

1. Platzieren Sie den Routenfilter in einer Ressourcengruppe. Stellen Sie sicher, dass sein Ort mit dem der ExpressRoute-Leitung übereinstimmt. Wählen Sie **Überprüfen + erstellen** und danach **Erstellen** aus.

    :::image type="content" source="./media/how-to-routefilter-portal/create-route-filter-basic.png" alt-text="Screenshot: Seite zum Erstellen eines Routenfilters mit eingegebenen Beispielwerten":::

### <a name="create-a-filter-rule"></a>Erstellen einer Filterregel

1. Wählen Sie zum Hinzufügen und Aktualisieren von Regeln die Registerkarte „Regel verwalten“ für Ihren Routenfilter aus.

    :::image type="content" source="./media/how-to-routefilter-portal/manage-route-filter.png" alt-text="Screenshot: Seite „Übersicht“ mit hervorgehobener Aktion „Regel verwalten“":::

1. Wählen Sie die Dienste, mit denen Sie eine Verbindung herstellen möchten, in der Dropdownliste aus, und speichern Sie die Regel, wenn Sie fertig sind.

    :::image type="content" source="./media/how-to-routefilter-portal/add-route-filter-rule.png" alt-text="Screenshot: Fenster „Regel verwalten“ mit ausgewählten Diensten in der Dropdownliste „Zulässige Dienstcommunitys“":::

## <a name="attach-the-route-filter-to-an-expressroute-circuit"></a><a name="attach"></a>Anfügen des Routenfilters zu einer ExpressRoute-Verbindung

Fügen Sie den Routenfilter an eine Leitung an, indem Sie die Schaltfläche **+ Leitung hinzufügen** und dann die ExpressRoute-Leitung aus der Dropdownliste auswählen.

:::image type="content" source="./media/how-to-routefilter-portal/add-circuit-to-route-filter.png" alt-text="Screenshot der Seite „Übersicht“ mit ausgewählter Aktion „Leitung hinzufügen“":::

Wenn der Konnektivitätsanbieter Peering für Ihre ExpressRoute-Leitung konfiguriert, aktualisieren Sie die Leitung auf der Seite „ExpressRoute-Leitung“, bevor Sie die Schaltfläche **+ Leitung hinzufügen** auswählen.

:::image type="content" source="./media/how-to-routefilter-portal/refresh-express-route-circuit.png" alt-text="Screenshot: Seite „Übersicht“ mit ausgewählter Aktion „Aktualisieren“":::

## <a name="common-tasks"></a><a name="tasks"></a>Häufige Aufgaben

### <a name="to-get-the-properties-of-a-route-filter"></a><a name="getproperties"></a>Abrufen der Eigenschaften eines Routenfilters

Sie können Eigenschaften eines Routenfilters anzeigen, wenn Sie die Ressource im Portal öffnen.

:::image type="content" source="./media/how-to-routefilter-portal/view-route-filter.png" alt-text="Screenshot mit der Seite „Übersicht“":::

### <a name="to-update-the-properties-of-a-route-filter"></a><a name="updateproperties"></a>Aktualisieren der Eigenschaften eines Routenfilters

1. Sie können die Liste der BGP-Communitywerte, die einer Leitung angefügt sind, durch Auswählen der Schaltfläche **Regel verwalten** aktualisieren.

    :::image type="content" source="./media/how-to-routefilter-portal/update-route-filter.png" alt-text="Aktualisieren von Routenfiltern mit der Aktion „Regel verwalten“":::

1. Wählen Sie die gewünschten Dienstcommunities und dann **Speichern** aus.

    :::image type="content" source="./media/how-to-routefilter-portal/add-route-filter-rule.png" alt-text="Screenshot: Fenster „Regel verwalten“ mit ausgewählten Diensten":::

### <a name="to-detach-a-route-filter-from-an-expressroute-circuit"></a><a name="detach"></a>Trennen eines Routenfilters von einer ExpressRoute-Verbindung

Um eine Leitung vom Routenfilter zu trennen, klicken Sie mit der rechten Maustaste auf die Leitung, und wählen Sie dann **Zuordnung aufheben** aus.

:::image type="content" source="./media/how-to-routefilter-portal/detach-route-filter.png" alt-text="Screenshot: Seite „Übersicht“ mit hervorgehobener Aktion „Zuordnung aufheben“":::


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können einen Routenfilter durch Auswahl der Schaltfläche **Löschen** löschen. Stellen Sie vorab sicher, dass der Routenfilter keiner Leitung zugeordnet ist.

:::image type="content" source="./media/how-to-routefilter-portal/delete-route-filter.png" alt-text="Löschen eines Routenfilters":::

## <a name="next-steps"></a>Nächste Schritte

Beispiele für Routerkonfigurationen finden Sie hier:

> [!div class="nextstepaction"]
> [Beispiele für die Routerkonfiguration zum Einrichten und Verwalten des Routings](expressroute-config-samples-routing.md)
