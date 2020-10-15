---
title: Verwalten von virtuellen Netzwerken – Azure Database for PostgreSQL – Flexible Server
description: Erstellen und Verwalten von virtuellen Netzwerken für Azure Database for PostgreSQL Flexible Server über das Azure-Portal
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 46d8fe6427b2a3e7811719792ac4bf67ddbcc3c5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90931731"
---
# <a name="create-and-manage-virtual-networks-for-azure-database-for-postgresql---flexible-server-using-the-azure-portal"></a>Erstellen und Verwalten von virtuellen Netzwerken für Azure Database for PostgreSQL Flexible Server über das Azure-Portal

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

Azure Database for PostgreSQL – Flexible Server unterstützt zwei Arten von sich gegenseitig ausschließenden Netzwerkverbindungsmethoden, mit denen eine Verbindung mit Ihrem flexiblen Server hergestellt werden kann. Die zwei Optionen sind:

* Öffentlicher Zugriff (zugelassene IP-Adressen)
* Privater Zugriff (VNET-Integration)

In diesem Artikel wird die Erstellung von PostgreSQL-Servern mit der Option **Privater Zugriff (VNET-Integration)** über das Azure-Portal behandelt. Mit „Privater Zugriff (VNET-Integration)“ können Sie Ihre Flexible Server-Instanz in Ihrer eigenen [Azure Virtual Network](../../virtual-network/virtual-networks-overview.md)-Instanz bereitstellen. Azure Virtual Network-Instanzen ermöglichen eine private und sichere Netzwerkkommunikation. Bei privatem Zugriff sind die Verbindungen mit dem PostgreSQL-Server auf Ihr virtuelles Netzwerk beschränkt. Weitere Informationen hierzu finden Sie unter [Privater Zugriff (VNET-Integration)](./concepts-networking.md#private-access-vnet-integration).

Sie können Ihren flexiblen Server bei der Servererstellung in einem virtuellen Netzwerk und Subnetz bereitstellen. Nachdem der flexible Server bereitgestellt wurde, können Sie ihn nicht in ein anderes virtuelles Netzwerk oder Subnetz verschieben oder auf *Öffentlicher Zugriff (zugelassene IP-Adressen)* umstellen.

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen Folgendes, um eine Flexible Server-Instanz in einem virtuellen Netzwerk zu erstellen:
- Ein [virtuelles Netzwerk](../../virtual-network/quick-create-portal.md#create-a-virtual-network)
    > [!Note]
    > Das virtuelle Netzwerk und das Subnetz sollten sich in derselben Region und demselben Abonnement befinden wie Ihre Flexible Server-Instanz.

-  [Delegierung eines Subnetzes](../../virtual-network/manage-subnet-delegation.md#delegate-a-subnet-to-an-azure-service) an **Microsoft.DBforPostgreSQL/flexibleServers**. Diese Delegierung bedeutet, dass dieses Subnetz nur von Azure Database for PostgreSQL Flexible Server-Instanzen genutzt werden kann. Im delegierten Subnetz können sich keine anderen Azure-Ressourcentypen befinden.

## <a name="create-azure-database-for-postgresql---flexible-server-in-an-already-existing-virtual-network"></a>Erstellen von Azure Database for PostgreSQL Flexible Server-Instanzen in einem bereits vorhandenen virtuellen Netzwerk

1. Wählen Sie links oben im Portal **Ressource erstellen** (+) aus.
2. Wählen Sie **Datenbanken** > **Azure-Datenbank für PostgreSQL** aus. Sie können auch **PostgreSQL** in das Suchfeld eingeben, um nach dem Dienst zu suchen.
3. Wählen Sie **Flexible Server** als Bereitstellungsoption aus.
4. Füllen Sie das **Basisformular** aus.
5. Wechseln Sie zur Registerkarte **Netzwerk**, um zu konfigurieren, wie Sie die Verbindung mit dem Server hergestellt werden soll.
6. Wählen Sie unter **Konnektivitätsmethode** die Option **Privater Zugriff (VNET-Integration)** aus. Wechseln Sie zu **Virtual Network**, und wählen Sie das bereits vorhandene *virtuelle Netzwerk* und *Subnetz* aus, die im Rahmen der oben aufgeführten Voraussetzungen erstellt wurden.
7. Wählen Sie **Überprüfen + erstellen** aus, um Ihre Flexible Server-Konfiguration zu überprüfen.
8. Wählen Sie **Erstellen** aus, um den Server bereitzustellen. Die Bereitstellung kann einige Minuten dauern.

>[!Note]
> Nachdem der flexible Server in einem virtuellen Netzwerk und Subnetz bereitgestellt wurde, können Sie ihn nicht mehr in „Öffentlicher Zugriff (zugelassene IP-Adressen)“ verschieben.
## <a name="next-steps"></a>Nächste Schritte
- [Erstellen und Verwalten von virtuellen Netzwerken für Azure Database for PostgreSQL Flexible Server mithilfe der Azure CLI](./how-to-manage-virtual-network-cli.md)
- Hier erfahren Sie mehr über [Netzwerke in Azure Database for PostgreSQL Flexible Server](./concepts-networking.md).
- Hier erfahren Sie mehr über [Virtuelle Netzwerke für Azure Database for PostgreSQL Flexible Server](./concepts-networking.md#private-access-vnet-integration).