---
title: Erstellen eines virtuellen Netzwerks
titleSuffix: Azure SQL Managed Instance
description: In diesem Artikel wird beschrieben, wie Sie ein virtuelles Netzwerk erstellen, das für die Bereitstellung von Azure SQL Managed Instance konfiguriert ist.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 09/12/2019
ms.openlocfilehash: 9ec6b7747ad22df96fd8ab5f9b5b5bc1e27b0993
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87543740"
---
# <a name="create-a-virtual-network-for-azure-sql-managed-instance"></a>Erstellen eines virtuellen Netzwerks für Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In diesem Artikel erfahren Sie, wie Sie ein gültiges virtuelles Netzwerk und ein entsprechendes Subnetz erstellen, in dem Sie Azure SQL Managed Instance bereitstellen können.

Azure SQL Managed Instance muss in einem [virtuellen Azure-Netzwerk](../../virtual-network/virtual-networks-overview.md) bereitgestellt werden. Diese Bereitstellung ermöglicht die folgenden Szenarien:

- Schützen der privaten IP-Adresse
- Direktes Herstellen einer Verbindung mit Azure SQL Managed Instance über ein lokales Netzwerk
- Herstellen einer Verbindung zwischen SQL Managed Instance und einem Verbindungsserver oder einem anderen lokalen Datenspeicher
- Herstellen einer Verbindung zwischen SQL Managed Instance und Azure-Ressourcen  

> [!NOTE]
> Sie sollten [die Größe des Subnetzes für SQL Managed Instance bestimmen](vnet-subnet-determine-size.md), bevor Sie die erste Instanz bereitstellen. Die Größe des Subnetzes kann nach Einbinden der Ressourcen nicht mehr geändert werden.
>
> Wenn Sie die Verwendung eines vorhandenen virtuellen Netzwerks beabsichtigen, müssen Sie diese Netzwerkkonfiguration ändern, um SQL Managed Instance einzubeziehen. Weitere Informationen finden Sie unter [Ändern eines vorhandenen virtuellen Netzwerks für SQL Managed Instance](vnet-existing-add-subnet.md).
>
> Nachdem eine verwaltete Instanz erstellt wurde, wird das Verschieben dieser Instanz oder des virtuellen Netzwerks in eine andere Ressourcengruppe oder ein anderes Abonnement nicht unterstützt.  Das Verschieben der verwalteten Instanz in ein anderes Subnetz wird ebenfalls nicht unterstützt.
>

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Die einfachste Möglichkeit zum Erstellen und Konfigurieren eines virtuellen Netzwerks ist die Verwendung einer Azure Resource Manager-Bereitstellungsvorlage.

1. Melden Sie sich beim Azure-Portal an.

2. Wählen Sie die Schaltfläche **Deploy to Azure** (In Azure bereitstellen):

   [![Abbildung einer Schaltfläche mit der Bezeichnung „Bereitstellung in Azure“](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json)

   Mit dieser Schaltfläche wird ein Formular zum Konfigurieren der Netzwerkumgebung geöffnet, in der Sie SQL Managed Instance bereitstellen können.

   > [!Note]
   > Mit dieser Azure Resource Manager-Vorlage wird ein virtuelles Netzwerk mit zwei Subnetzen bereitgestellt. Ein Subnetz namens **ManagedInstances** ist für die Instanz von SQL Managed Instance reserviert und verfügt über eine vorkonfigurierte Routingtabelle. Das andere Subnetz namens **Default** wird für andere Ressourcen verwendet, die auf SQL Managed Instance zugreifen müssen (z. B. virtuelle Azure-Computer).

3. Konfigurieren Sie die Netzwerkumgebung. Im folgenden Formular können Sie Parameter für Ihre Netzwerkumgebung konfigurieren:

   ![Resource Manager-Vorlage zum Konfigurieren des Azure-Netzwerks](./media/virtual-network-subnet-create-arm-template/create-mi-network-arm.png)

   Sie können die Namen des virtuellen Netzwerks und der Subnetze ändern und die Ihren Netzwerkressourcen zugeordneten IP-Adressbereiche anpassen. Sobald Sie auf die Schaltfläche **Kaufen** klicken, wird mit diesem Formular Ihre Umgebung erstellt und konfiguriert. Wenn Sie keine zwei Subnetze benötigen, können Sie das Standardsubnetz löschen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist SQL Managed Instance?](sql-managed-instance-paas-overview.md).
- Machen Sie sich mit der [Verbindungsarchitektur in SQL Managed Instance](connectivity-architecture-overview.md) vertraut.
- Unter [Ändern eines vorhandenen virtuellen Netzwerks für SQL Managed Instance](vnet-existing-add-subnet.md) erfahren Sie, wie Sie Änderungen vornehmen können.
- Ein Tutorial zum Erstellen eines virtuellen Netzwerks und einer verwalteten Instanz sowie zum Wiederherstellen einer Datenbank aus einer Datenbanksicherung finden Sie unter [Erstellen einer verwalteten Instanz](instance-create-quickstart.md).
- Informationen zu Problemen im Zusammenhang mit dem DNS finden Sie unter [Konfigurieren eines benutzerdefinierten DNS](custom-dns-configure.md).
