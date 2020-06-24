---
title: 'Schnellstart: Erstellen eines Workloadklassifizierers – Portal'
description: Verwenden Sie das Azure-Portal, um einen Workloadklassifizierer mit hoher Wichtigkeit zu erstellen.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql-dw
ms.date: 05/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 691cdcb525f8e9e3d1fb914372b9f62366f4bfba
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213022"
---
# <a name="quickstart-create-a-synapse-sql-pool-workload-classifier-using-the-azure-portal"></a>Schnellstart: Erstellen eines Workloadklassifizierers für Synapse SQL-Pools mithilfe des Azure-Portals

In dieser Schnellstartanleitung erstellen Sie einen [Workloadklassifizierer](sql-data-warehouse-workload-classification.md) zum Zuweisen von Abfragen zu einer Workloadgruppe.  Der Klassifizierer weist der Workloadgruppe `DataLoads` Anforderungen vom `ELTLogin`-SQL-Benutzer zu.   Führen Sie die Schritte unter [Schnellstart: Konfigurieren der Workloadisolation für Synapse SQL-Pools mithilfe einer Workloadgruppe im Azure-Portal](quickstart-configure-workload-isolation-portal.md) aus, um die Workloadgruppe `DataLoads` zu erstellen.  In diesem Tutorial wird ein Workloadklassifizierer mit der Option „WLM_LABEL“ erstellt, um die korrekte weitere Klassifizierung von Anforderungen zu unterstützen.  Der Klassifizierer weist diesen Anforderungen ebenfalls die [Workloadprioriät](sql-data-warehouse-workload-importance.md) `HIGH` zu.


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.


## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

> [!NOTE]
> Das Erstellen einer SQL-Poolinstanz in Azure Synapse Analytics führt möglicherweise zu einem neuen abrechenbaren Dienst.  Weitere Informationen finden Sie unter [Azure Synapse Analytics – Preise](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung wird vorausgesetzt, dass Sie bereits über eine SQL-Poolinstanz in Synapse SQL und über CONTROL DATABASE-Berechtigungen verfügen. Wenn Sie ein Data Warehouse erstellen müssen, verwenden Sie die Anweisungen unter [Erstellen und Verbinden – Portal](create-data-warehouse-portal.md), um ein Data Warehouse namens **mySampleDataWarehouse** zu erstellen.
<br><br>
Die Workloadgruppe `DataLoads` ist vorhanden.  Informationen zum Erstellen der Workloadgruppe finden Sie unter [Schnellstart: Konfigurieren der Workloadisolation für Synapse SQL-Pools mithilfe einer Workloadgruppe im Azure-Portal](quickstart-configure-workload-isolation-portal.md).
<br><br>
>[!IMPORTANT] 
>Der SQL-Pool muss online sein, damit die Workloadverwaltung konfiguriert werden kann. 


## <a name="create-a-login-for-eltlogin"></a>Erstellen einer Anmeldung für „ELTLogin“

Erstellen Sie eine SQL Server-Authentifizierungsanmeldung in der `master`-Datenbank, indem Sie [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) für `ELTLogin` verwenden.

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'ELTLogin')
BEGIN
CREATE LOGIN [ELTLogin] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user-and-grant-permissions"></a>Erstellen eines Benutzers und Zuweisen von Berechtigungen

Nach der Erstellung der Anmeldung muss ein Benutzer in der Datenbank erstellt werden.  Verwenden Sie [CREATE USER](/sql/t-sql/statements/create-user-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), um das `ELTRole`-Objekt für den SQL-Benutzer in **mySampleDataWarehouse** zu erstellen.  Im Rahmen dieses Tutorials wird die Klassifizierung getestet. Gewähren Sie `ELTLogin` daher Berechtigungen für **mySampleDataWarehouse**. 

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'ELTLogin')
BEGIN
CREATE USER [ELTLogin] FOR LOGIN [ELTLogin]
GRANT CONTROL ON DATABASE::mySampleDataWarehouse TO ELTLogin 
END
;
```

## <a name="configure-workload-classification"></a>Konfigurieren der Workloadklassifizierung
Mithilfe der Klassifizierung können Sie Anforderungen, die auf verschiedenen Regeln basieren, an eine Workloadgruppe weiterleiten.  Im Tutorial [Schnellstart: Konfigurieren der Workloadisolation für Synapse SQL-Pools mithilfe einer Workloadgruppe im Azure-Portal](quickstart-configure-workload-isolation-portal.md) wurde die Workloadgruppe `DataLoads` erstellt.  Nun erstellen Sie einen Workloadklassifizierer, um Abfragen an die Workloadgruppe `DataLoads` weiterzuleiten.


1.  Klicken Sie auf der linken Seite des Azure-Portals auf **Azure Synapse Analytics (vormals SQL DW)** .
2.  Wählen Sie auf der Seite **Azure Synapse Analytics (vormals SQL DW)** die Option **mySampleDataWarehouse** aus. Der SQL-Pool wird geöffnet.
3.  Klicken Sie auf **Workloadverwaltung**.

    ![Klicken auf das Menü](./media/quickstart-create-a-workload-classifier-portal/menu.png)

4.  Klicken Sie rechts neben der Workloadgruppe `DataLoads` auf **Einstellungen und Klassifizierer**.

    ![Klicken Sie auf „Erstellen“.](./media/quickstart-create-a-workload-classifier-portal/settings-classifiers.png)

5. Klicken Sie auf **Klassifizierer**.
6. Klicken Sie auf **Klassifizierer hinzufügen**.

    ![Klicken Sie auf "Hinzufügen".](./media/quickstart-create-a-workload-classifier-portal/add-wc.png)

7.  Geben Sie unter **Name** den Namen `ELTLoginDataLoads` ein.
8.  Geben Sie unter **Mitglied** die Zeichenfolge `ELTLogin` ein.
9.  Wählen Sie unter **Anforderungsrelevanz** die Option `High` aus.  Dies ist *optional*. Standardmäßig ist normale Relevanz festgelegt.
10. Geben Sie unter **Bezeichnung** die Zeichenfolge `fact_loads` ein.
11. Klicken Sie auf **Hinzufügen**.
12. Klicken Sie auf **Speichern**.

    ![Klicken auf Konfiguration](./media/quickstart-create-a-workload-classifier-portal/config-wc.png)

## <a name="verify-and-test-classification"></a>Überprüfen und Testen der Klassifizierung
Überprüfen Sie in der Katalogsicht [sys.workload_management_workload_classifiers](/sql/relational-databases/system-catalog-views/sys-workload-management-workload-classifiers-transact-sql?view=azure-sqldw-latest), ob der Klassifizierer `ELTLoginDataLoads` vorhanden ist.

```sql
SELECT * FROM sys.workload_management_workload_classifiers WHERE name = 'ELTLoginDataLoads'
```

Überprüfen Sie in der Katalogansicht [sys.workload_management_workload_classifier_details](/sql/relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql?view=azure-sqldw-latest) die Details zum Klassifizierer.

```sql
SELECT c.[name], c.group_name, c.importance, cd.classifier_type, cd.classifier_value
  FROM sys.workload_management_workload_classifiers c
  JOIN sys.workload_management_workload_classifier_details cd
    ON cd.classifier_id = c.classifier_id
  WHERE c.name = 'ELTLoginDataLoads'
```

Führen Sie die folgenden Anweisungen aus, um die Klassifizierung zu testen.  Stellen Sie sicher, dass Sie als ``ELTLogin`` verbunden sind und ``Label`` in der Abfrage verwendet wird.
```sql
CREATE TABLE factstaging (ColA int)
INSERT INTO factstaging VALUES(0)
INSERT INTO factstaging VALUES(1)
INSERT INTO factstaging VALUES(2)
GO

CREATE TABLE testclassifierfact WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT * FROM factstaging
OPTION (LABEL='fact_loads')
```

Überprüfen Sie die `CREATE TABLE`-Anweisung, die mithilfe des Workloadklassifizierers `ELTLoginDataLoads` für die Workloadgruppe `DataLoads` klassifiziert wurde.
```sql 
SELECT TOP 1 request_id, classifier_name, group_name, resource_allocation_percentage, submit_time, [status], [label], command 
FROM sys.dm_pdw_exec_requests 
WHERE [label] = 'fact_loads'
ORDER BY submit_time DESC
```



## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

So löschen Sie den in diesem Tutorial erstellten Workloadklassifizierer `ELTLoginDataLoads`:

1. Klicken Sie rechts neben der Workloadgruppe `DataLoads` auf **1 Klassifizierer**.

    ![Klicken auf „Löschen“](./media/quickstart-create-a-workload-classifier-portal/delete-wc.png)

2. Klicken Sie auf **Klassifizierer**.
3. Klicken Sie rechts neben dem Workloadklassifizierer `ELTLoginDataLoads` auf die Schaltfläche **`...`** .
4. Klicken Sie auf **Löschen**.
5. Klicken Sie auf **Speichern**.

    ![Klicken Sie auf Speichern.](./media/quickstart-create-a-workload-classifier-portal/delete-save-wc.png)

Ihnen werden Gebühren für Ihre Data Warehouse-Einheiten und die in Ihrem Data Warehouse gespeicherten Daten berechnet. Diese Compute- und Speicherressourcen werden separat in Rechnung gestellt.

- Falls Sie die Daten im Speicher belassen möchten, können Sie Computeressourcen anhalten, wenn Sie das Data Warehouse nicht verwenden. Wenn Sie Computeressourcen anhalten, werden Ihnen nur die Datenspeichergebühren in Rechnung gestellt. Sie können die Computeressourcen fortsetzen, wenn Sie mit den Daten arbeiten möchten.
- Wenn zukünftig keine Gebühren anfallen sollen, können Sie das Data Warehouse löschen.

Führen Sie die folgenden Schritte aus, um Ressourcen zu bereinigen.

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und wählen Sie Ihre Data Warehouse-Instanz aus.

    ![Bereinigen von Ressourcen](./media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Wählen Sie zum Anhalten von Computeressourcen die Schaltfläche **Anhalten**. Wenn das Data Warehouse angehalten ist, wird die Schaltfläche **Starten** angezeigt.  Wählen Sie zum Fortsetzen der Computeressourcen die Option **Starten**.

3. Wählen Sie **Löschen**, wenn Sie die Data Warehouse-Instanz entfernen möchten, damit keine Gebühren für Compute- oder Speicherressourcen anfallen.

4. Wählen Sie zum Entfernen des von Ihnen erstellten SQL-Servers die Option **sqlpoolservername.database.windows.net** (siehe Abbildung oben) und dann **Löschen** aus.  Seien Sie bei diesem Löschvorgang vorsichtig, da beim Löschen des Servers auch alle Datenbanken gelöscht werden, die dem Server zugewiesen sind.

5. Wählen Sie zum Entfernen der Ressourcengruppe die Option **myResourceGroup** und dann **Ressourcengruppe löschen**.

## <a name="next-steps"></a>Nächste Schritte

Überwachen Sie Ihre Arbeitsauslastung anhand der Überwachungsmetriken im Azure-Portal.  Ausführliche Informationen finden Sie unter [Verwalten und Überwachen der Workloadpriorität in Azure Synapse Analytics](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md).
