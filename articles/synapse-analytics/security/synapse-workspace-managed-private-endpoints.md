---
title: Verwaltete private Endpunkte
description: In diesem Artikel erfahren Sie mehr über verwaltete private Endpunkte in Azure Synapse Analytics.
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: security
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: ecca67cab486c8f3524c8c8d4c221d52689cf62a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87070107"
---
# <a name="synapse-managed-private-endpoints-preview"></a>Verwaltete private Endpunkte in Synapse (Vorschau)

In diesem Artikel erfahren Sie mehr über verwaltete private Endpunkte in Azure Synapse Analytics.

## <a name="managed-private-endpoints"></a>Verwaltete private Endpunkte

Verwaltete private Endpunkte sind private Endpunkte, die in der verwalteten Microsoft Azure Virtual Network-Instanz des Arbeitsbereichs erstellt werden und einen privaten Link zu Azure-Ressourcen herstellen. Azure Synapse verwaltet diese privaten Endpunkte für Sie.

Azure Synapse unterstützt private Links. Mit einem privaten Link können Sie über Ihr virtuelles Azure-Netzwerk sicher auf Azure-Dienste (z. B. Azure Storage, Azure Cosmos DB und Azure SQL Data Warehouse) und auf in Azure gehostete Kunden-/Partnerdienste zugreifen.

Bei der Verwendung eines privaten Links wird der Datenverkehr zwischen Ihrem virtuellen Netzwerk und einem Arbeitsbereich vollständig über das Microsoft-Backbone-Netzwerk übertragen. Private Link schützt vor einer möglichen Datenexfiltration. Ein privater Link zu einer Ressource wird durch die Erstellung eines privaten Endpunkts eingerichtet.

Der private Endpunkt verwendet eine private IP-Adresse aus Ihrem virtuellen Netzwerk, um den Dienst effektiv in Ihr virtuelles Netzwerk einzubinden. Private Endpunkte werden einer bestimmten Ressource in Azure und nicht dem gesamten Dienst zugeordnet. Kunden können die Konnektivität auf eine bestimmte Ressource beschränken, die von Ihrer Organisation genehmigt wurde. Weitere Informationen finden Sie in der [Dokumentation zu Private Link](https://docs.microsoft.com/azure/private-link/).

>[!IMPORTANT]
>Verwaltete private Endpunkte werden nur in Azure Synapse-Arbeitsbereichen mit einem verwalteten virtuellen Arbeitsbereichsnetzwerk unterstützt.

>[!NOTE]
>Der gesamte ausgehende Datenverkehr aus dem verwalteten virtuellen Arbeitsbereichsnetzwerk wird in Zukunft blockiert – mit Ausnahme von Datenverkehr über verwaltete private Endpunkte. Es wird empfohlen, verwaltete private Endpunkte zu erstellen, um eine Verbindung mit all Ihren Azure-Datenquellen herzustellen, die extern zu dem Arbeitsbereich sind. 

Wenn Sie einen verwalteten privaten Endpunkt in Azure Synapse erstellen, wird eine Verbindung mit einem privaten Endpunkt mit dem Status „Ausstehend“ erstellt. Ein Genehmigungsworkflow wird ausgelöst. Der Besitzer der Ressource für private Links ist für die Genehmigung oder Ablehnung der Verbindung verantwortlich.

Wenn der Besitzer die Verbindung genehmigt, wird der private Link eingerichtet. Andernfalls wird der private Link nicht eingerichtet. In beiden Fällen wird der verwaltete private Endpunkt mit dem Status der Verbindung aktualisiert.

Nur ein verwalteter privater Endpunkt im genehmigten Zustand kann Datenverkehr an eine angegebene Ressource für private Links senden.

## <a name="managed-private-endpoints-for-sql-pool-and-sql-on-demand"></a>Verwaltete private Endpunkte für einen SQL-Pool und SQL On-Demand

Ein SQL-Pool und SQL On-Demand sind Analysefunktionen in Ihrem Azure Synapse-Arbeitsbereich. Diese Funktionen nutzen eine mehrinstanzenfähige Infrastruktur, die nicht im [verwalteten virtuellen Arbeitsbereichsnetzwerk](./synapse-workspace-managed-vnet.md) bereitgestellt wird.

Bei der Erstellung eines Arbeitsbereichs erstellt Azure Synapse zwei verwaltete private Endpunkte für den SQL-Pool und SQL On-Demand in diesem Arbeitsbereich. 

Diese beiden verwalteten privaten Endpunkte werden in Azure Synapse Studio aufgeführt. Wählen Sie für die Anzeige in Studio im linken Navigationsbereich **Verwalten** und anschließend **Verwaltete virtuelle Netzwerke** aus.

Der verwaltete private Endpunkt für den SQL-Pool heißt *synapse-ws-sql--\<workspacename\>* und der für SQL On-Demand *synapse-ws-sqlOnDemand--\<workspacename\>* .
![Verwaltete private Endpunkte für einen SQL-Pool und SQL On-Demand](./media/synapse-workspace-managed-private-endpoints/managed-pe-for-sql-1.png)

Diese beiden verwalteten privaten Endpunkte werden automatisch für Sie erstellt, wenn Sie Ihren Azure Synapse-Arbeitsbereich erstellen. Diese Endpunkte werden Ihnen nicht in Rechnung gestellt.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen von verwalteten privaten Endpunkten für Ihre Datenquellen](./how-to-create-managed-private-endpoints.md)
