---
title: Wichtige Unterschiede bei Machine Learning Services
description: In diesem Artikel werden die wichtigsten Unterschiede zwischen Machine Learning Services in SQL Managed Instance und SQL Server Machine Learning Services beschrieben.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: sstein, davidph
manager: cgronlun
ms.date: 03/17/2021
ms.openlocfilehash: a322efdf5ef21912a8e29e9b3dea4bc041c61cca
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108131889"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-managed-instance-and-sql-server"></a>Wichtige Unterschiede zwischen Machine Learning Services in Azure SQL Managed Instance und SQL Server

In diesem Artikel werden einige wichtige Unterschiede der Funktion zwischen [Machine Learning Services in Azure SQL Managed Instance](machine-learning-services-overview.md) und [SQL Server Machine Learning Services](/sql/advanced-analytics/what-is-sql-server-machine-learning) beschrieben.

## <a name="language-support"></a>Sprachunterstützung

Das [Erweiterbarkeitsframework](/sql/machine-learning/concepts/extensibility-framework) von Python und R wird in Machine Learning Services in SQL Managed Instance und SQL Server unterstützt. Die wichtigsten Unterschiede zu SQL Managed Instance sind:

- Nur Python und R werden unterstützt. Externe Sprachen wie Java können nicht hinzugefügt werden.

- Die anfänglichen Versionen von Python und R unterscheiden sich:

  | Plattform                   | Version der Python-Runtime           | Versionen der R-Runtime                   |
  |----------------------------|----------------------------------|--------------------------------------|
  | Verwaltete Azure SQL-Instanz | 3.7.2                            | 3.5.2                                |
  | SQL Server 2019            | 3.7.1                            | 3.5.2                                |
  | SQL Server 2017            | 3.5.2 und 3.7.2 (CU22 und höher) | 3.3.3 und 3.5.2 (CU22 und höher)     |
  | SQL Server 2016            | Nicht verfügbar                    | 3.2.2 und 3.5.2 (SP2 CU14 und höher) |

## <a name="python-and-r-packages"></a>Python- und R-Pakete

Es gibt in SQL Managed Instance keine Unterstützung für Pakete, die von externen Runtimes (z. B. Java) abhängig sind oder Zugriff auf Betriebssystem-APIs für die Installation oder Verwendung benötigen.

Weitere Informationen zum Verwalten von Python- und R-Paketen finden Sie unter:

- [Abrufen von Paketinformationen für Python](/sql/machine-learning/package-management/python-package-information?context=%2fazure%2fazure-sql%2fmanaged-instance%2fcontext%2fml-context&preserve-view=true&view=azuresqldb-mi-current)
- [Abrufen von R-Paketinformationen](/sql/machine-learning/package-management/r-package-information?context=%2fazure%2fazure-sql%2fmanaged-instance%2fcontext%2fml-context&preserve-view=true&view=azuresqldb-mi-current)

## <a name="resource-governance"></a>Ressourcengovernance

Es ist in SQL Managed Instance nicht möglich, R-Ressourcen durch [Resource Governor](/sql/relational-databases/resource-governor/resource-governor?view=azuresqldb-mi-current&preserve-view=true) einzuschränken, und externe Ressourcenpools werden nicht unterstützt.

Standardmäßig werden R-Ressourcen auf maximal 20 % der verfügbaren SQL Managed Instance-Ressourcen festgelegt, wenn die Erweiterbarkeit aktiviert ist. Um diesen Standardprozentsatz zu ändern, erstellen Sie unter [https://azure.microsoft.com/support/create-ticket/](https://azure.microsoft.com/support/create-ticket/) ein Azure-Supportticket.

Die Erweiterbarkeit wird mit den folgenden SQL-Befehlen aktiviert (SQL Managed Instance wird neu gestartet und ist für einige Sekunden nicht verfügbar):

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;
```

Verwenden Sie die folgenden Befehle, um die Erweiterbarkeit und die Wiederherstellung von 100 % des Arbeitsspeichers und der CPU-Ressourcen in SQL Server zu deaktivieren:

```sql
sp_configure 'external scripts enabled', 0;
RECONFIGURE WITH OVERRIDE;
```

Die insgesamt für SQL Managed Instance verfügbaren Ressourcen sind davon abhängig, welche Dienstebene Sie auswählen. Weitere Informationen finden Sie unter [Kaufmodelle für Azure SQL-Datenbank](../database/purchasing-models.md).

### <a name="insufficient-memory-error"></a>Fehler bei nicht ausreichendem Arbeitsspeicher

Die Arbeitsspeichernutzung hängt von der in R-Skripts verwendeten Anzahl und von der Anzahl parallel ausgeführter Abfragen ab. Wenn für R nicht genügend Arbeitsspeicher verfügbar ist, erhalten Sie eine Fehlermeldung. Häufige Fehlermeldungen:

- `Unable to communicate with the runtime for 'R' script for request id: *******. Please check the requirements of 'R' runtime`
- `'R' script error occurred during execution of 'sp_execute_external_script' with HRESULT 0x80004004. ...an external script error occurred: "..could not allocate memory (0 Mb) in C function 'R_AllocStringBuffer'"`
- `An external script error occurred: Error: cannot allocate vector of size.`

Wenn Sie einen dieser Fehler erhalten, können Sie ihn beheben, indem Sie die Datenbank auf eine höhere Dienstebene skalieren.

## <a name="sql-managed-instance-pools"></a>SQL Managed Instance-Pools

Machine Learning Services wird derzeit in [Azure SQL Managed Instance-Pools (Vorschau)](instance-pools-overview.md) nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Machine Learning Services in Azure SQL Managed Instance](machine-learning-services-overview.md).
- Weitere Informationen zur Verwendung von Python in Machine Learning Services finden Sie unter [Ausführen von Python-Skripts](/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true).
- Informationen zur Verwendung von R in Machine Learning Services finden Sie unter [Schnellstart: Ausführen einfacher R-Skripts mit SQL Machine Learning](/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true).