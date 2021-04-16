---
title: Verwalten von virtuellen Netzwerken – Azure Database for PostgreSQL – Flexible Server
description: Erstellen und Verwalten von virtuellen Netzwerken für Azure Database for PostgreSQL Flexible Server über das Azure-Portal
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: d5b60cfeb3bfce1253ece3708e6b48ce6bcb0b4f
ms.sourcegitcommit: b28e9f4d34abcb6f5ccbf112206926d5434bd0da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107226904"
---
# <a name="create-and-manage-virtual-networks-for-azure-database-for-postgresql---flexible-server-using-the-azure-portal"></a>Erstellen und Verwalten von virtuellen Netzwerken für Azure Database for PostgreSQL Flexible Server über das Azure-Portal

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

Azure Database for PostgreSQL – Flexible Server unterstützt zwei Arten von sich gegenseitig ausschließenden Netzwerkverbindungsmethoden, mit denen eine Verbindung mit Ihrem flexiblen Server hergestellt werden kann. Die zwei Optionen sind:

* Öffentlicher Zugriff (zugelassene IP-Adressen)
* Privater Zugriff (VNET-Integration)

In diesem Artikel wird die Erstellung von PostgreSQL-Servern mit der Option **Privater Zugriff (VNET-Integration)** über das Azure-Portal behandelt. Mit der Option „Privater Zugriff (VNET-Integration)“ können Sie Ihren flexiblen Server in Ihrer eigenen [Azure Virtual Network](../../virtual-network/virtual-networks-overview.md)-Instanz bereitstellen. Virtuelle Azure-Netzwerke ermöglichen eine private und sichere Netzwerkkommunikation. Bei privatem Zugriff sind die Verbindungen mit dem PostgreSQL-Server auf Ihr virtuelles Netzwerk beschränkt. Weitere Informationen hierzu finden Sie unter [Privater Zugriff (VNET-Integration)](./concepts-networking.md#private-access-vnet-integration).

Sie können Ihren flexiblen Server bei der Servererstellung in einem virtuellen Netzwerk und Subnetz bereitstellen. Nachdem der flexible Server bereitgestellt wurde, können Sie ihn nicht in ein anderes virtuelles Netzwerk oder Subnetz verschieben oder auf *Öffentlicher Zugriff (zugelassene IP-Adressen)* umstellen.

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen Folgendes, um eine Flexible Server-Instanz in einem virtuellen Netzwerk zu erstellen:
- Ein [virtuelles Netzwerk](../../virtual-network/quick-create-portal.md#create-a-virtual-network)
    > [!Note]
    > Das virtuelle Netzwerk und das Subnetz sollten sich in derselben Region und demselben Abonnement befinden wie Ihre Flexible Server-Instanz.

-  [Delegierung eines Subnetzes](../../virtual-network/manage-subnet-delegation.md#delegate-a-subnet-to-an-azure-service) an **Microsoft.DBforPostgreSQL/flexibleServers**. Diese Delegierung bedeutet, dass dieses Subnetz nur von Azure Database for PostgreSQL Flexible Server-Instanzen genutzt werden kann. Im delegierten Subnetz können sich keine anderen Azure-Ressourcentypen befinden.
-  Fügen Sie dem Dienstendpunkt für das Subnetz, das an flexible Server delegiert ist, `Microsoft.Storage` hinzu. Dazu führen Sie die folgenden Schritte aus:
     1. Wechseln Sie zur Seite Ihres virtuellen Netzwerks.
     2. Wählen Sie das VNET aus, in dem Sie Ihren flexiblen Server bereitstellen möchten.
     3. Wählen Sie das Subnetz aus, das für den flexiblen Server delegiert wird.
     4. Wählen Sie auf dem Pulloutbildschirm unter **Dienstendpunkt** in der Dropdownliste `Microsoft.storage` aus.
     5. Speichern Sie die Änderungen.


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