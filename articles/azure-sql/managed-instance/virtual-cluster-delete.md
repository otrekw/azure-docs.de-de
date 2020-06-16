---
title: Löschen eines Subnetzes nach dem Löschen einer Instanz von Azure SQL Managed Instance
description: Hier erfahren Sie, wie Sie nach dem Löschen einer Instanz von Azure SQL Managed Instance auch ein virtuelles Azure-Netzwerk löschen.
services: sql-database
ms.service: sql-database
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
ms.date: 06/26/2019
ms.openlocfilehash: 73150ee893ca23ed7996b001bd02acaabbf89dce
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84116687"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-managed-instance"></a>Löschen eines Subnetzes nach dem Löschen einer Instanz von Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Dieser Artikel enthält Richtlinien zum manuellen Löschen eines Subnetzes nach dem Löschen der letzten darin enthaltenen Instanz von Azure SQL Managed Instance.

Verwaltete Instanzen werden in [virtuellen Clustern](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture) bereitgestellt. Jeder virtuelle Cluster ist einem Subnetz zugeordnet. Der virtuelle Cluster bleibt nach dem Löschen der letzten Instanz noch 12 Stunden erhalten, damit Sie im gleichen Subnetz schneller verwaltete Instanzen erstellen können. Das Beibehalten eines leeren virtuellen Clusters ist kostenlos. Während dieses Zeitraums kann der mit dem Subnetz verbundene virtuelle Cluster nicht gelöscht werden.

Wenn Sie keine 12 Stunden warten, sondern den virtuellen Cluster sowie das Subnetz früher löschen möchten, können Sie dies manuell tun. Löschen Sie den virtuellen Cluster manuell über das Azure-Portal oder die API für virtuelle Cluster.

> [!IMPORTANT]
> - Damit der Löschvorgang erfolgreich ist, sollte der virtuelle Cluster keine verwalteten Instanzen enthalten. 
> - Das Löschen eines virtuellen Clusters ist ein zeitintensiver Vorgang mit einer Dauer von etwa eineinhalb Stunden (aktuelle Angaben zur Löschdauer für virtuelle Cluster finden Sie unter [Azure SQL Managed Instance-Verwaltungsvorgänge](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations).) Während dieser Zeit ist der virtuelle Cluster im Portal weiterhin sichtbar, bis der Vorgang abgeschlossen wurde.

## <a name="delete-virtual-cluster-from-the-azure-portal"></a>Löschen virtueller Cluster im Azure-Portal

Suchen Sie nach den Ressourcen eines virtuellen Clusters, um diesen über das Azure-Portal zu löschen.

![Screenshot: Azure-Portal mit hervorgehobenem Suchfeld](./media/virtual-cluster-delete/virtual-clusters-search.png)

Nachdem Sie den virtuellen Cluster gefunden haben, den Sie löschen möchten, wählen Sie diese Ressource aus, und klicken Sie auf **Löschen**. Sie werden aufgefordert, das Löschen des virtuellen Clusters zu bestätigen.

![Screenshot: Dashboard „Virtuelle Cluster“ im Azure-Portal mit hervorgehobener Option „Löschen“](./media/virtual-cluster-delete/virtual-clusters-delete.png)

In den Azure-Portalbenachrichtigungen wird eine Bestätigung dafür angezeigt, dass die Anforderung zum Löschen des virtuellen Clusters erfolgreich übermittelt wurde. Der eigentliche Löschvorgang dauert etwa 1,5 Stunden. Während dieser Zeit ist der virtuelle Cluster im Portal weiterhin sichtbar. Nach Abschluss des Vorgangs ist der virtuelle Cluster nicht mehr sichtbar, und das zugehörige Subnetz wird zur erneuten Verwendung freigegeben.

> [!TIP]
> Wenn in Ihrem virtuellen Cluster keine verwalteten Instanzen angezeigt werden und Sie den virtuellen Cluster nicht löschen können, vergewissern Sie sich, dass kein Instanzenbereitstellungsvorgang mehr läuft. Dies umfasst gestartete und abgebrochene Bereitstellungen, die noch nicht abgeschlossen sind. Der Grund: Diese Vorgänge verwenden weiterhin den virtuellen Cluster, was dazu führt, dass er nicht gelöscht werden kann. Auf der Registerkarte „Deployments“ (Bereitstellungen) der Ressourcengruppe, in der die Instanz bereitgestellt wurde, werden Bereitstellungen angezeigt, die noch nicht abgeschlossen sind. Warten Sie in diesem Fall, bis die Bereitstellung abgeschlossen ist, und löschen Sie dann die verwaltete Instanz und anschließend den virtuellen Cluster.

## <a name="delete-virtual-cluster-by-using-the-api"></a>Löschen eines virtuellen Clusters mithilfe der API

Verwenden Sie die in der [Methode zum Löschen virtueller Cluster](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete) angegebenen URI-Parameter, um einen virtuellen Cluster mithilfe der API zu löschen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist eine verwaltete Instanz?](sql-managed-instance-paas-overview.md).
- Machen Sie sich mit der [Verbindungsarchitektur der verwalteten Azure SQL-Datenbank-Instanz](connectivity-architecture-overview.md) vertraut.
- Unter [Ändern eines vorhandenen virtuellen Netzwerks für die verwaltete Instanz](vnet-existing-add-subnet.md) erfahren Sie, wie Sie Änderungen vornehmen können.
- Ein Tutorial zum Erstellen eines virtuellen Netzwerks, dem Erstellen einer Instanz von Managed Instance und dem Wiederherstellen einer Datenbank aus einer Datenbanksicherung finden Sie unter [Erstellen einer Instanz von Azure SQL Managed Instance](instance-create-quickstart.md).
- Lesen Sie bei Problemen im Zusammenhang mit dem DNS den Artikel [Konfigurieren eines benutzerdefinierten DNS](custom-dns-configure.md).
