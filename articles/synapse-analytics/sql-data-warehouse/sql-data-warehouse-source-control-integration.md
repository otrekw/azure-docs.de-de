---
title: Integration der Quellcodeverwaltung
description: Professionelle DevOps-Datenbankumgebung für den SQL-Pool mit nativer Integration der Quellcodeverwaltung mithilfe von Azure Repos (Git und GitHub)
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 08/23/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 0ffa33811957af95fc10396ffecef0b3de7959fe
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633019"
---
# <a name="source-control-integration-for-sql-pool"></a>Integration der Quellcodeverwaltung für den SQL-Pool

In diesem Tutorial wird erläutert, wie Sie Ihr SSDT-Datenbankprojekt (SQL Server Data Tools) mit Quellcodeverwaltung integrieren.  Die Integration der Quellcodeverwaltung ist der erste Schritt bei der Erstellung Ihrer Continuous Integration- und Continuous Deployment-Pipeline mit der SQL-Pool-Ressource in Azure Synapse Analytics.

## <a name="before-you-begin"></a>Voraussetzungen

- Registrieren für eine [Azure DevOps-Organisation](https://azure.microsoft.com/services/devops/)
- Absolvieren des Tutorials [Schnellstart: Erstellen und Abfragen einer Azure SQL Data Warehouse-Instanz im Azure-Portal](create-data-warehouse-portal.md)
- [Installieren von Visual Studio 2019](https://visualstudio.microsoft.com/vs/older-downloads/)

## <a name="set-up-and-connect-to-azure-devops"></a>Einrichten von Azure DevOps und Herstellen einer Verbindung

1. Erstellen Sie in Ihrer Azure DevOps-Organisation ein Projekt, das Ihr SSDT-Datenbankprojekt über ein Azure-Repository hostet.

   ![Erstellen des Projekts](./media/sql-data-warehouse-source-control-integration/1-create-project-azure-devops.png "Projekt erstellen")

2. Öffnen Sie Visual Studio, und stellen Sie mit Ihrer Azure DevOps-Organisation und Ihrem Projekt aus Schritt 1 eine Verbindung her, indem Sie „Verbindungen verwalten“ auswählen

   ![Verwalten von Verbindungen](./media/sql-data-warehouse-source-control-integration/2-manage-connections.png "Verwalten von Verbindungen")

   ![Herstellen einer Verbindung](./media/sql-data-warehouse-source-control-integration/3-connect.png "Verbinden")

3. Klonen Sie das Azure-Repository aus Ihrem Projekt auf dem lokalen Computer.

   ![Klonen des Repositorys](./media/sql-data-warehouse-source-control-integration/4-clone-repo.png "Repository klonen")

## <a name="create-and-connect-your-project"></a>Erstellen und Verbinden Ihres Projekts

1. Erstellen Sie in Visual Studio ein neues SQL Server-Datenbankprojekt mit einem Verzeichnis und einem lokalen Git-Repository in Ihrem **lokalen geklonten Repository**.

   ![Erstellen eines neuen Projekts](./media/sql-data-warehouse-source-control-integration/5-create-new-project.png "Erstellen eines neuen Projekts")  

2. Klicken Sie mit der rechten Maustaste auf das leere SQL-Projekt, und importieren Sie Ihr Data Warehouse in das Datenbankprojekt.

   ![Importieren des Projekts](./media/sql-data-warehouse-source-control-integration/6-import-new-project.png "Importieren des Projekts")  

3. Committen Sie im Team Explorer in Visual Studio alle Änderungen im lokalen Git-Repository.

   ![Commit](./media/sql-data-warehouse-source-control-integration/6.5-commit-push-changes.png "Commit")  

4. Nachdem Sie die Änderungen lokal im geklonten Repository committet haben, synchronisieren Sie die Änderungen, und pushen Sie sie in das Azure-Repository in Ihrem Azure DevOps-Projekt.

   ![Synchronisieren und Pushen: Staging](./media/sql-data-warehouse-source-control-integration/7-commit-push-changes.png "Synchronisieren und Pushen: Staging")

   ![Synchronisieren und Pushen](./media/sql-data-warehouse-source-control-integration/7.5-commit-push-changes.png "Synchronisieren und Pushen")  

## <a name="validation"></a>Überprüfen

1. Überprüfen Sie, ob die Änderungen in das Azure-Repository gepusht wurden, indem Sie eine Tabellenspalte in Ihrem Datenbankprojekt über Visual Studio SQL Server Data Tools (SSDT) aktualisieren.

   ![Überprüfen der Aktualisierungsspalte](./media/sql-data-warehouse-source-control-integration/8-validation-update-column.png "Überprüfen der Aktualisierungsspalte")

2. Committen und Pushen der Änderung von Ihrem lokalen Repository in das Azure-Repository

   ![Pushen der Änderungen](./media/sql-data-warehouse-source-control-integration/9-push-column-change.png "Änderungen pushen")

3. Überprüfen, ob die Änderung in Ihr Azure-Repository gepusht wurde

   ![Überprüfen](./media/sql-data-warehouse-source-control-integration/10-verify-column-change-pushed.png "Überprüfen der Änderungen")

4. (**Optional**) Verwenden Sie den Schemavergleich, und übertragen Sie die Änderungen mithilfe von SSDT in Ihre Data Warehouse-Zielinstanz, um sicherzustellen, dass die Objektdefinitionen in Ihrem Azure-Repository und im lokalen Repository Ihr Data Warehouse widerspiegeln.

## <a name="next-steps"></a>Nächste Schritte

- [Entwurfsentscheidungen und Programmiertechniken für Data Warehouses in Azure Synapse Analytics](sql-data-warehouse-overview-develop.md)
