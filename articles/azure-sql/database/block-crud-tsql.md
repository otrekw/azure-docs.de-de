---
title: Blockieren von T-SQL-Befehlen zum Erstellen oder Ändern von Azure SQL-Ressourcen
titleSuffix: Block T-SQL commands to create or modify Azure SQL resources
description: In diesem Artikel wird ein Feature erläutert, mit dem Azure-Administratoren T-SQL-Befehle zum Erstellen oder Ändern von Azure SQL-Ressourcen blockieren können.
ms.service: sql-database
ms.subservice: security
ms.custom: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.topic: article
ms.date: 03/31/2021
ms.reviewer: ''
ROBOTS: NOINDEX
ms.openlocfilehash: 65ea6d78eb807671df1732392e112d368877a2b5
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108132051"
---
# <a name="what-is-block-t-sql-crud-feature"></a>Was ist das Feature „Block T-SQL CRUD“?
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb-sqlmi.md)]


Mit diesem Feature können Azure-Administratoren die Erstellung oder Änderung von Azure SQL-Ressourcen über T-SQL blockieren. Dies wird auf Abonnementebene erzwungen, um T-SQL-Befehle daran zu hindern, SQL-Ressourcen in einer beliebigen Azure SQL-Datenbank-Instanz oder einer verwalteten Instanz zu beeinträchtigen.

## <a name="overview"></a>Übersicht

Zum Blockieren der Erstellung oder Änderung von Ressourcen über T-SQL und Erzwingen der Ressourcenverwaltung über eine Azure Resource Manager-Vorlage (ARM-Vorlage) für ein bestimmtes Abonnement können die Vorschaufeatures auf Abonnementebene im Azure-Portal verwendet werden. Dies ist besonders nützlich, wenn Sie [Azure-Richtlinien](../../governance/policy/overview.md) verwenden, um Organisationsstandards über ARM-Vorlagen zu erzwingen. Da T-SQL nicht den Azure-Richtlinien entspricht, können Erstellungs- oder Änderungsvorgänge von T-SQL blockiert werden. Die blockierte Syntax enthält CRUD-Anweisungen (Create, Update, Delete) für Datenbanken in Azure SQL, insbesondere `CREATE DATABASE`-, `ALTER DATABASE`- und `DROP DATABASE`-Anweisungen. 

T-SQL-CRUD-Vorgänge können über Azure-Portal, [PowerShell](/powershell/module/az.resources/register-azproviderfeature) oder [Azure CLI](/cli/azure/feature#az_feature_register) blockiert werden.

## <a name="permissions"></a>Berechtigungen

Der Azure-Benutzer muss ein Mitglied der Rolle „Besitzer“ oder „Mitwirkender“ des Abonnements sein, um dieses Feature zu registrieren oder zu entfernen.

## <a name="examples"></a>Beispiele

Im folgenden Abschnitt wird beschrieben, wie Sie ein Vorschaufeature beim Microsoft.Sql-Ressourcenanbieter im Azure-Portal registrieren oder die Registrierung aufheben können: 

### <a name="register-block-t-sql-crud"></a>Registrieren von „Block T-SQL CRUD“

1. Wechseln Sie zu Ihrem Abonnement im Azure-Portal.
2. Wählen Sie die Registerkarte **Vorschaufeatures** aus. 
3. Wählen Sie **Block T-SQL CRUD** aus.
4. Nachdem Sie **Block T-SQL CRUD** ausgewählt haben, wird ein neues Fenster geöffnet. Wählen Sie nun **Registrieren** aus, um diese Blockierung beim Microsoft.Sql-Ressourcenanbieter zu registrieren.

![Wählen Sie „Block T-SQL CRUD“ in der Liste der Vorschaufeatures aus.](./media/block-tsql-crud/block-tsql-crud.png)

![Wählen Sie mit aktivierter Option „Block T-SQL CRUD“ die Option „Registrieren“ aus.](./media/block-tsql-crud/block-tsql-crud-register.png)

  
### <a name="re-register-microsoftsql-resource-provider"></a>Erneutes Registrieren des Microsoft.Sql-Ressourcenanbieters 
Nachdem Sie „Block T-SQL CRUD“ beim Microsoft.Sql-Ressourcenanbieter registriert haben, müssen Sie den Microsoft.Sql-Ressourcenanbieter erneut registrieren, damit die Änderungen wirksam werden. So registrieren Sie den Microsoft.Sql-Ressourcenanbieter erneut:

1. Wechseln Sie zu Ihrem Abonnement im Azure-Portal.
2. Wählen Sie die Registerkarte **Ressourcenanbieter** aus.
3. Suchen Sie den Ressourcenanbieter **Microsoft.Sql**, und wählen Sie ihn aus.
4. Wählen Sie **Erneut registrieren** aus. 

> [!NOTE]
> Der Schritt der erneuten Registrierung ist obligatorisch, wenn „Block T-SQL CRUD“ auf Ihr Abonnement angewendet werden soll. 

![Erneutes Registrieren des Microsoft.Sql-Ressourcenanbieters](./media/block-tsql-crud/block-tsql-crud-re-register.png)

### <a name="removing-block-t-sql-crud"></a>Entfernen von „Block T-SQL CRUD“
Wenn Sie das Blockieren von T-SQL-Vorgängen zum Erstellen oder Ändern aus Ihrem Abonnement entfernen möchten, heben Sie zunächst die Registrierung des zuvor registrierten „Block T-SQL CRUD“ auf. Registrieren Sie anschließend den Microsoft.Sql-Ressourcenanbieter wie oben gezeigt erneut, damit das Entfernen von „Block T-SQL CRUD“ wirksam wird. 


## <a name="next-steps"></a>Nächste Schritte

- [Überblick über die Sicherheitsfunktionen der Azure SQL-Datenbank](security-overview.md)
- [Bewährte Sicherheitsmethoden für Azure SQL-Datenbank](security-best-practice.md)