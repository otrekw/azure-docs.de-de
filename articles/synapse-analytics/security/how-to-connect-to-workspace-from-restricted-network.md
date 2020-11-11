---
title: Herstellen einer Verbindung zur Synapse Studio-Arbeitsbereichsressource über ein eingeschränktes Netzwerk
description: In diesem Artikel erfahren Sie, wie Sie über ein eingeschränktes Netzwerk eine Verbindung mit Ihren Azure Synapse Studio-Arbeitsplatzressourcen herstellen.
author: xujxu
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: security
ms.date: 10/25/2020
ms.author: xujiang1
ms.reviewer: jrasnick
ms.openlocfilehash: 5d28b8f2ff3045c9fdf5e8a866419a22bfbc6504
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93328612"
---
# <a name="connect-to-synapse-studio-workspace-resources-from-a-restricted-network"></a>Herstellen einer Verbindung zu Synapse Studio-Arbeitsbereichsressourcen über ein eingeschränktes Netzwerk

Der Zielleser dieses Artikels ist der IT-Administrator im Unternehmen, der das eingeschränkte Netzwerk des Unternehmens verwaltet. Der IT-Administrator ist dabei, die Netzwerkverbindung zwischen Azure Synapse Studio und der Arbeitsstation innerhalb dieses eingeschränkten Netzwerks zu aktivieren.

In diesem Artikel erfahren Sie, wie Sie sich von einer eingeschränkten Netzwerkumgebung aus mit Ihrem Azure Synapse-Arbeitsplatz verbinden können. 

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Abonnement** : Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure Synapse-Arbeitsbereich** : Wenn Sie nicht über Synapse Studio verfügen, erstellen Sie einen Synapse-Arbeitsbereich über Azure Synapse Analytics. Der Name des Arbeitsbereichs wird im folgenden Schritt 4 benötigt.
* **Eingeschränktes Netzwerk** : Das eingeschränkte Netzwerk wird von der IT-Administration des Unternehmens verwaltet. Der IT-Administrator verfügt über die Berechtigung zur Konfiguration der Netzwerkrichtlinie. Der Name des virtuellen Netzwerks und seines Subnetzes werden im folgenden Schritt 3 benötigt.



## <a name="step-1-add-network-outbound-security-rules-to-the-restricted-network"></a>Schritt 1: Hinzufügen von Netzwerksicherheitsregeln für ausgehenden Datenverkehr zum eingeschränkten Netzwerk

Sie müssen vier Netzwerksicherheitsregeln für ausgehenden Datenverkehr mit vier Diensttags hinzufügen. Weitere Informationen finden Sie unter der [Übersicht über Diensttags](/azure/virtual-network/service-tags-overview.md). 
* AzureResourceManager
* AzureFrontDoor.Frontend
* AzureActiveDirectory
* AzureMonitor (Optional. Fügen Sie diese Art von Regel nur dann hinzu, wenn Sie die Daten für Microsoft freigeben möchten).

**Azure Resource Manager** -Details der Ausgangsregel wie nachfolgend aufgeführt. Wenn Sie die anderen drei Regeln erstellen, ersetzen Sie den Wert von „ **Zieldiensttag** “ durch die Auswahl des Diensttagnamens „ **AzureFrontDoor.Front-End** “, „ **AzureActiveDirectory** “, „ **AzureMonitor** “ aus der Dropdownliste.

![AzureResourceManager](./media/how-to-connect-to-workspace-from-restricted-network/arm-servicetag.png)


## <a name="step-2-create-azure-synapse-analytics-private-link-hubs"></a>Schritt 2: Erstellen von Azure Synapse Analytics (Private Link-Hubs)

Sie müssen eine Azure Synapse Analytics-Instanz (Private Link-Hubs) über das Azure-Portal erstellen. Suchen Sie über das Azure-Portal nach „ **Azure Synapse Analytics (Private Link-Hubs)** “, und füllen Sie dann das erforderliche Feld aus, um es zu erstellen. 

> [!Note]
> Die Region sollte dieselbe sein wie die, in der sich Ihr Synapse-Arbeitsbereich befindet.

![Erstellen von Private Link-Hubs für Synapse Analytics](./media/how-to-connect-to-workspace-from-restricted-network/private-links.png)

## <a name="step-3-create-private-link-endpoint-for-synapse-studio-gateway"></a>Schritt 3: Erstellen eines privaten Endpunkts für das Synapse Studio-Gateway

Für den Zugriff auf das Synapse Studio-Gateway müssen Sie einen Private Link-Endpunkt vom Azure-Portal aus erstellen. Suchen Sie „ **Private Link** “ über das Azure-Portal. Wählen Sie „ **Privaten Endpunkt erstellen** “ in „ **Private Link Center** “ aus, und füllen Sie dann das erforderliche Feld aus, um es zu erstellen. 

> [!Note]
> Die Region sollte dieselbe sein wie die, in der sich Ihr Synapse-Arbeitsbereich befindet.

![Erstellen eines privaten Endpunkts für Synapse Studio 1](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-1.png)

Wählen Sie auf der nächsten Registerkarte von „ **Ressource** “ den Private Link-Hub aus, der oben in Schritt 2 erstellt wurde.

![Erstellen eines privaten Endpunkts für Synapse Studio 2](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-2.png)

Auf der nächsten Registerkarte von „ **Konfiguration** “: 
* Wählen Sie den Namen des eingeschränkten virtuellen Netzwerks aus, den Sie für „ **Virtuelles Netzwerk** “ verwenden.
* Wählen Sie das Subnetz des eingeschränkten virtuellen Netzwerks für „ **Subnetz** “ aus. 
* Wählen Sie „ **Ja** “ für die **Integration in einer privaten DNS-Zone** aus.

![Erstellen eines privaten Endpunkts für Synapse Studio 3](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-3.png)

Nachdem der Private Link-Endpunkt erstellt wurde, können Sie auf die Anmeldeseite des Synapse Studio-Webtools zugreifen. Sie können jedoch noch nicht auf die Ressourcen in Ihrem Synapse-Arbeitsbereich zugreifen, bis Sie den nächsten Schritt abschließen müssen.

## <a name="step-4-create-private-link-endpoints-for-synapse-studio-workspace-resource"></a>Schritt 4: Erstellen von Private Link-Endpunkten für die Synapse Studio-Arbeitsbereichsressource

Für den Zugriff auf die Ressourcen innerhalb Ihrer Synapse Studio-Arbeitsbereichsressource müssen Sie mindestens einen Private Link-Endpunkt mit dem Typ „ **Dev** “ der „ **Zielunterressource** “ und zwei weitere optionale Private Link-Endpunkte mit den Typen „ **Sql** “ oder „ **SqlOnDemand** “ erstellen, je nachdem, auf welche Ressourcen im Synapse Studio-Arbeitsbereich Sie zugreifen möchten. Diese Private Link-Endpunkterstellung für den Synapse Studio-Arbeitsbereich ähnelt der obigen Endpunkterstellung.  

Achten Sie auf die nachfolgenden Bereiche auf der Registerkarte von „ **Ressource** “:
* Wählen Sie „ **Microsoft.Synapse/workspaces** “ bis „ **Ressourcentyp** “ aus.
* Wählen Sie „ **YourWorkSpaceName** “ bis „ **Ressource** “ aus, die Sie zuvor erstellt haben.
* Wählen Sie den Endpunkttyp in „ **Zielunterressource** “ aus:
  * **Sql** : Gilt für die Ausführung von SQL-Abfragen im SQL-Pool.
  * **SqlOnDemand** : Gilt für die in SQL integrierte Abfrageausführung.
  * **Dev** : Gilt für den Zugriff auf alles andere innerhalb der Synapse Studio-Arbeitsbereiche. Sie müssen mindestens den Private Link-Endpunkt mit diesem Typ erstellen.

![Erstellen eines privaten Endpunkts für den Synapse Studio-Arbeitsbereich](./media/how-to-connect-to-workspace-from-restricted-network/plinks-endpoint-ws-1.png)

Jetzt ist alles bereit. Sie können auf Ihre Synapse Studio-Arbeitsbereichsressource zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [verwalteten virtuellen Arbeitsbereichsnetzwerken](./synapse-workspace-managed-vnet.md)

Weitere Informationen zu [Verwalteten privaten Endpunkten](./synapse-workspace-managed-private-endpoints.md).
