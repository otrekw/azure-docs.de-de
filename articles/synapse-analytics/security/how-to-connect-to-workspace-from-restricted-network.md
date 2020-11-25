---
title: Herstellen einer Verbindung mit Arbeitsbereichsressourcen in Azure Synapse Analytics Studio über ein eingeschränktes Netzwerk
description: In diesem Artikel erfahren Sie, wie Sie über ein eingeschränktes Netzwerk eine Verbindung mit Ihren Arbeitsplatzressourcen herstellen.
author: xujxu
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: security
ms.date: 10/25/2020
ms.author: xujiang1
ms.reviewer: jrasnick
ms.openlocfilehash: 55ec8be176dc7274a3b9a1feca53726d57eeb422
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2020
ms.locfileid: "95024464"
---
# <a name="connect-to-workspace-resources-from-a-restricted-network"></a>Herstellen einer Verbindung mit Arbeitsbereichsressourcen über ein eingeschränktes Netzwerk

Angenommen, Sie sind ein IT-Administrator, der das eingeschränkte Netzwerk Ihrer Organisation verwaltet. Sie möchten die Netzwerkverbindung zwischen Azure Synapse Analytics Studio und einer Arbeitsstation innerhalb dieses eingeschränkten Netzwerks aktivieren. In diesem Artikel erfahren Sie, welche Schritte erforderlich sind.

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Abonnement**: Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* **Azure Synapse Analytics-Arbeitsbereich**: Sie können einen Arbeitsbereich aus Azure Synapse Analytics erstellen. Sie benötigen den Namen des Arbeitsbereichs in Schritt 4.
* **Eingeschränktes Netzwerk**: Der IT-Administrator verwaltet das eingeschränkte Netzwerk für die Organisation und verfügt über die Berechtigung zum Konfigurieren der Netzwerkrichtlinie. In Schritt 3 benötigen Sie den Namen des virtuellen Netzwerks und das zugehörige Subnetz.


## <a name="step-1-add-network-outbound-security-rules-to-the-restricted-network"></a>Schritt 1: Hinzufügen von Netzwerksicherheitsregeln für ausgehenden Datenverkehr zum eingeschränkten Netzwerk

Sie müssen vier Netzwerksicherheitsregeln für ausgehenden Datenverkehr mit vier Diensttags hinzufügen. 
* AzureResourceManager
* AzureFrontDoor.Frontend
* AzureActiveDirectory
* AzureMonitor (Dieser Regeltyp ist optional. Fügen Sie ihn nur dann hinzu, wenn Sie die Daten für Microsoft freigeben möchten.)

Der folgende Screenshot zeigt Details zur Ausgangsregel von Azure Resource Manager.

![Screenshot der Azure Resource Manager-Diensttagdetails.](./media/how-to-connect-to-workspace-from-restricted-network/arm-servicetag.png)

Wenn Sie die anderen drei Regeln erstellen, ersetzen Sie den Wert von **Zieldiensttag** durch **AzureFrontDoor.Front-End**, **AzureActiveDirectory** oder **AzureMonitor** aus der Dropdownliste.

Weitere Informationen finden Sie unter [Diensttags in virtuellen Netzwerken](/azure/virtual-network/service-tags-overview).

## <a name="step-2-create-private-link-hubs"></a>Schritt 2: Erstellen eines Private Link-Hubs

Erstellen Sie als Nächstes Private Link-Hubs aus dem Azure-Portal. Um diese im Portal zu finden, suchen Sie nach *Azure Synapse Analytics (Private Link-Hubs)* , und geben Sie dann die erforderlichen Informationen ein, um sie zu erstellen. 

> [!Note]
> Stellen Sie sicher, dass der Wert **Region** der Region entspricht, wo sich Ihr Azure Synapse Analytics-Arbeitsbereich befindet.

![Screenshot vom Erstellen des Private Link-Hubs für Synapse.](./media/how-to-connect-to-workspace-from-restricted-network/private-links.png)

## <a name="step-3-create-a-private-endpoint-for-your-gateway"></a>Schritt 3: Erstellen eines privaten Endpunkts für Ihr Gateway

Um auf das Azure Synapse Analytics Studio-Gateway zuzugreifen, müssen Sie einen privaten Endpunkt im Azure-Portal erstellen. Um diesen im Portal zu finden, suchen Sie nach *Private Link*. Wählen Sie **Privaten Endpunkt erstellen** in **Private Link Center** aus, und geben Sie dann die erforderlichen Informationen ein, um ihn zu erstellen. 

> [!Note]
> Stellen Sie sicher, dass der Wert **Region** der Region entspricht, wo sich Ihr Azure Synapse Analytics-Arbeitsbereich befindet.

![Screenshot vom Erstellen eines privaten Endpunkts, Registerkarte „Grundlagen“.](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-1.png)

Wählen Sie auf der Registerkarte **Ressource** den Private Link-Hub aus, den Sie in Schritt 2 erstellt haben.

![Screenshot vom Erstellen eines privaten Endpunkts, Registerkarte „Ressource“.](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-2.png)

Auf der Registerkarte **Konfiguration**: 
* Wählen Sie für **Virtuelles Netzwerk** den Namen des eingeschränkten virtuellen Netzwerks aus.
* Wählen Sie für **Subnetz** das Subnetz des eingeschränkten virtuellen Netzwerks aus. 
* Wählen Sie für die **Integration in einer privaten DNS-Zone** **Ja** aus.

![Screenshot vom Erstellen eines privaten Endpunkts, Registerkarte „Konfiguration“.](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-3.png)

Nachdem der Private Link-Endpunkt erstellt wurde, können Sie auf die Anmeldeseite des Azure Synapse Analytics Studio-Webtools zugreifen. Allerdings können Sie noch nicht auf die Ressourcen in Ihrem Arbeitsbereich zuzugreifen. Dazu müssen Sie den nächsten Schritt ausführen.

## <a name="step-4-create-private-endpoints-for-your-workspace-resource"></a>Schritt 4: Erstellen privater Endpunkte für Ihre Arbeitsbereichsressource

Um auf die Ressourcen in Ihrer Azure Synapse Analytics Studio-Arbeitsbereichsressource zuzugreifen, müssen Sie Folgendes erstellen:

- Mindestens einen Private Link-Endpunkt mit einem **Dev**-Typ **Untergeordnete Zielressource**.
- Zwei weitere optionale Private Link-Endpunkte des Typs **Sql** oder **SqlOnDemand**, abhängig von den Ressourcen im Arbeitsbereich, auf die Sie zugreifen möchten.

Deren Erstellen ähnelt dem Erstellen des Endpunkts im vorherigen Schritt.  

Auf der Registerkarte **Ressource**:

* Wählen Sie für **Ressourcentyp** die Option **Microsoft.Synapse/workspaces** aus.
* Wählen Sie für **Ressource** den Namen des Arbeitsbereichs aus, den Sie zuvor erstellt haben.
* Wählen Sie den Endpunkttyp für **Untergeordnete Zielressource** aus:
  * **Sql** gilt für die Ausführung von SQL-Abfragen im SQL-Pool.
  * **SqlOnDemand** gilt für die in SQL integrierte Abfrageausführung.
  * **Dev** gilt für den Zugriff auf alles andere innerhalb der Azure Synapse Analytics Studio-Arbeitsbereiche. Sie müssen mindestens einen Private Link-Endpunkt dieses Typs erstellen.

![Screenshot vom Erstellen eines privaten Endpunkts, Registerkarte „Ressource“, Arbeitsbereich.](./media/how-to-connect-to-workspace-from-restricted-network/plinks-endpoint-ws-1.png)


## <a name="step-5-create-private-endpoints-for-workspace-linked-storage"></a>Schritt 5: Erstellen von privaten Endpunkten für mit dem Arbeitsbereich verknüpften Speicher

Um mit dem Speicher-Explorer im Azure Synapse Analytics Studio-Arbeitsbereich auf den verknüpften Speicher zuzugreifen, müssen Sie einen privaten Endpunkt erstellen. Die Schritte hierfür ähneln denen in Schritt 3. 

Auf der Registerkarte **Ressource**:
* Wählen Sie für **Ressourcentyp** die Option **Microsoft.Synapse/storageAccounts** aus.
* Wählen Sie für **Ressource** den Namen des Speicherkontos aus, das Sie zuvor erstellt haben.
* Wählen Sie den Endpunkttyp für **Untergeordnete Zielressource** aus:
  * **blob** ist für den Azure-Blobspeicher.
  * **dfs** ist für Azure Data Lake Storage Gen2.

![Screenshot vom Erstellen eines privaten Endpunkts, Registerkarte „Ressource“, Speicher.](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-storage.png)

Nun können Sie auf die verknüpfte Speicherressource zugreifen. In Ihrem virtuellen Netzwerk können Sie in Ihrem Azure Synapse Analytics Studio-Arbeitsbereich mit dem Speicher-Explorer auf die verknüpfte Speicherressource zugreifen.

Sie können ein verwaltetes virtuelles Netzwerk für Ihren Arbeitsbereich aktivieren, wie in diesem Screenshot gezeigt:

![Screenshot des Arbeitsbereichs „Synapse erstellen“ mit hervorgehobener Option „Verwaltetes VNET aktivieren“.](./media/how-to-connect-to-workspace-from-restricted-network/ws-network-config.png)

Wenn Ihr Notebook unter einem bestimmten Speicherkonto auf die verknüpften Speicherressourcen zugreifen soll, fügen Sie unter Ihrer Azure Synapse Analytics Studio-Instanz verwaltete private Endpunkte hinzu. Der Speicherkontoname sollte dem entsprechen, auf den Ihr Notebook zugreifen muss. Weitere Informationen finden Sie unter [Erstellen eines verwalteten privaten Endpunkts zu Ihrer Datenquelle (Vorschau)](./how-to-create-managed-private-endpoints.md).

Nachdem Sie diesen Endpunkt erstellt haben, wird als Genehmigungsstatus **Ausstehend** angezeigt. Fordern Sie die Genehmigung vom Besitzer dieses Speicherkontos auf der Registerkarte **Private Endpunktverbindungen** dieses Speicherkontos im Azure-Portal an. Nach der Genehmigung kann Ihr Notebook auf die verknüpften Speicherressourcen unter diesem Speicherkonto zugreifen.

Jetzt ist alles bereit. Sie können auf Ihre Azure Synapse Analytics Studio-Arbeitsbereichsressource zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Azure Synapse Analytics: verwaltetes virtuelles Netzwerk (Vorschauversion)](./synapse-workspace-managed-vnet.md).

Erfahren Sie mehr über [Verwaltete private Endpunkte in Synapse (Vorschau)](./synapse-workspace-managed-private-endpoints.md).
