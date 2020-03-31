---
title: Installieren von Visual Studio 2019
description: Installieren von Visual Studio und SQL Server Data Tools (SSDT) für SQL Analytics
services: synapse-analytics
ms.custom: vs-azure, azure-synapse
ms.workload: azure-vs
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: d0a1772706fa838f51322d5f5d5bd1b46eb9144a
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351624"
---
# <a name="getting-started-with-visual-studio-2019"></a>Erste Schritte mit Visual Studio 2019
SQL Server Data Tools (SSDT) von Visual Studio **2019** ist ein einzelnes Tool, mit dem Sie folgende Aufgaben ausführen können:

- Verbinden, Abfragen und Entwickeln von Anwendungen
- Nutzen eines Objekt-Explorers, um alle Objekte im Datenmodell visuell zu untersuchen, einschließlich Tabellen, Ansichten, gespeicherten Prozeduren usw.
- Generieren von T-SQL-DDL-Skripts (Data Definition Language) für die Objekte
- Entwickeln Ihres Data Warehouse mithilfe eines zustandsbasierten Ansatzes für SSDT-Datenbankprojekte
- Integrieren Ihres Datenbankprojekts in Quellcodeverwaltungssysteme wie Git mit Azure Repos
- Einrichten von Pipelines für Continuous Integration und Continuous Deployment mit Automatisierungsservern wie Azure DevOps

## <a name="install-visual-studio-2019"></a>Installieren von Visual Studio 2019
Informationen zum Herunterladen und Installieren von Visual Studio [Visual Studio 2019 herunterladen][] finden Sie unter **Visual Studio 2019 herunterladen**. Wählen Sie während der Installation die Workload „Datenspeicherung und -verarbeitung“ aus. Eine eigenständige SSDT-Installation ist in Visual Studio 2019 nicht mehr erforderlich.

## <a name="unsupported-features-in-ssdt"></a>Nicht unterstützte Funktionen in SSDT

Gelegentlich bieten für SQL Analytics veröffentliche Funktionen keine Unterstützung für SSDT. Die folgenden Funktionen werden derzeit nicht unterstützt:

- [Materialisierte Sichten](https://docs.microsoft.com/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest) (in Bearbeitung)
- [Sortierte gruppierte Columnstore-Indizes](https://docs.microsoft.com/sql/t-sql/statements/create-columnstore-index-transact-sql?view=azure-sqldw-latest#examples--and-) (in Bearbeitung)
- [COPY-Anweisung](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (in Bearbeitung)
- [Workloadverwaltung](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-workload-management)– Workloadgruppen und -klassifizierungen (in Bearbeitung)
- [Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security?view=sql-server-ver15)
  - Übermitteln Sie ein Supportticket, oder geben Sie [hier](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040057-ssdt-row-level-security) Feedback, damit die Funktion unterstützt wird.
- [Dynamische Datenmaskierung](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?toc=%2Fazure%2Fsql-data-warehouse%2Ftoc.json&view=sql-server-2017#defining-a-dynamic-data-mask)
  - Übermitteln Sie ein Supportticket, oder geben Sie [hier](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/39040048-ssdt-support-dynamic-data-masking) Feedback, damit die Funktion unterstützt wird. 
- [PREDICT](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=sql-server-ver15&viewFallbackFrom=azure-sqldw-latest)-Funktion 
- [Tabelleneinschränkungen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-table-constraints#table-constraints) werden nicht unterstützt. Legen Sie die Buildaktion für diese Tabellenobjekte auf „Keine“ fest.

## <a name="next-steps"></a>Nächste Schritte

Da Sie jetzt die neueste Version von SSDT verwenden, sind Sie bereit für die [Verbindungsherstellung](sql-data-warehouse-query-visual-studio.md) mit Ihrem SQL-Pool.




<!--Other-->

[Visual Studio 2019 herunterladen]: https://visualstudio.microsoft.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
