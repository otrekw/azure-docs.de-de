---
title: Machine Learning Services in Azure SQL Managed Instance (Vorschauversion)
description: Dieser Artikel bietet eine Übersicht über Machine Learning Services in Azure SQL Managed Instance.
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
ms.openlocfilehash: 94495144c64b3770995a5f67e9129b3ba86e741e
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104599560"
---
# <a name="machine-learning-services-in-azure-sql-managed-instance"></a>Machine Learning Services in Azure SQL Managed Instance (Vorschauversion)

Machine Learning Services ist ein Feature von Azure SQL Managed Instance, das in die Datenbank integriertes maschinelles Lernen bereitstellt und sowohl Python- als auch R-Skripts unterstützt. Das Feature umfasst Python- und R-Pakete von Microsoft für leistungsstarke Predictive Analytics und maschinelles Lernen. Die relationalen Daten können in Skripts über gespeicherte Prozeduren, Python- oder R-Anweisungen in T-SQL-Skripts oder Python- oder R-Code mit T-SQL verwendet werden.

## <a name="what-is-machine-learning-services"></a>Was ist Machine Learning Services?

Mit Machine Learning Services in Azure SQL Managed Instance können Sie Python- und R-Skripts in einer Datenbank ausführen. Sie können das Feature verwenden, um Daten vorzubereiten und zu bereinigen, Features zu entwickeln und Machine Learning-Modelle in einer Datenbank zu trainieren, auszuwerten und bereitzustellen. Mit dem Feature können Sie Skripts ausführen, in denen sich die Daten befinden. Die Übertragung der Daten über das Netzwerk auf einen anderen Server entfällt.

Verwenden Sie Machine Learning Services mit R-/Python-Unterstützung in Azure SQL Managed Instance für Folgendes:

- **Ausführen von R- und Python-Skripts für die Datenvorbereitung und allgemeine Datenverarbeitung:** Sie können jetzt Ihre R-/Python-Skripts direkt in Azure SQL Managed Instance übertragen, wo sich auch Ihre Daten befinden. Somit müssen Sie keine Daten mehr auf einen anderen Server verschieben, um R- oder Python-Skripts auszuführen. Sie können den Bedarf an Datenverschiebungen und die damit zusammenhängenden Probleme in Bezug auf Latenz, Sicherheit und Compliance vermeiden.

- **Trainieren von Machine Learning-Modellen in der Datenbank:** Sie können Modelle mit beliebigen Open-Source-Algorithmen trainieren. Sie können Ihr Training problemlos auf das gesamte Dataset skalieren, anstatt sich auf Datasets mit Stichproben zu verlassen, die aus der Datenbank gepullt wurden.

- **Bereitstellen Ihrer Modelle und Skripts in der Produktion in gespeicherten Prozeduren:** Die Skripts und trainierten Modelle können einfach durch Einbetten in gespeicherte T-SQL-Prozeduren operationalisiert werden. Apps, die eine Verbindung mit Azure SQL Managed Instance herstellen, können von Vorhersagen und der Datenanalyse in diesen Modellen profitieren, indem sie einfach eine gespeicherte Prozedur aufrufen. Sie können auch die native T-SQL-Funktion PREDICT verwenden, um Modelle für die schnelle Bewertung in Szenarien zu operationalisieren, in denen viele gleichzeitige Echtzeitbewertungen durchgeführt werden.

In Machine Learning Services sind Basisverteilungen von R und Python enthalten. Neben den Microsoft-Paketen [revoscalepy](/sql/machine-learning/python/ref-py-revoscalepy) und [microsoftml](/sql/machine-learning/python/ref-py-microsoftml) für Python und [RevoScaleR](/sql/machine-learning/r/ref-r-revoscaler), [MicrosoftML](/sql/machine-learning/r/ref-r-microsoftml), [olapR](/sql/machine-learning/r/ref-r-olapr) und [sqlrutils](/sql/machine-learning/r/ref-r-sqlrutils) für R können Sie Open-Source-Pakete und -Frameworks wie PyTorch, TensorFlow und scikit-learn installieren und verwenden.

## <a name="how-to-enable-machine-learning-services"></a>Aktivieren von Machine Learning Services

Sie können Machine Learning Services in Azure SQL Managed Instance aktivieren, indem Sie die Erweiterbarkeit mit den folgenden SQL-Befehlen aktivieren (SQL Managed Instance wird neu gestartet und ist einige Sekunden nicht verfügbar):

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;
```

Ausführliche Informationen zu den Auswirkungen dieses Befehls auf SQL Managed Instance-Ressourcen finden Sie unter [Ressourcengovernance](machine-learning-services-differences.md#resource-governance).

### <a name="enable-machine-learning-services-in-a-failover-group"></a>Aktivieren von Machine Learning Services in einer Failovergruppe

In einer [Failovergruppe](failover-group-add-instance-tutorial.md) werden Systemdatenbanken nicht in die sekundäre Instanz repliziert. (Weitere Informationen finden Sie unter [Einschränkungen von Failovergruppen](../database/auto-failover-group-overview.md#limitations-of-failover-groups).)

Wenn Ihre verwaltete Instanz Teil einer Failovergruppe ist, gehen Sie wie folgt vor:

- Führen Sie die Befehle `sp_configure` und `RECONFIGURE` für jede Instanz der Failovergruppe aus, um Machine Learning Services zu aktivieren.

- Installieren Sie die R-/Python-Bibliotheken nicht in der Masterdatenbank, sondern stattdessen in einer Benutzerdatenbank.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie [Key differences from SQL Server Machine Learning Services](machine-learning-services-differences.md) (Wichtige Unterschiede zwischen Machine Learning Services in Azure SQL Server).
- Informationen zur Verwendung von Python in Machine Learning Services finden Sie unter [Schnellstart: Ausführen einfacher Python-Skripts mit SQL Server Machine Learning Services](/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true).
- Informationen zur Verwendung von R in Machine Learning Services finden Sie unter [Schnellstart: Ausführen einfacher R-Skripts mit SQL Machine Learning](/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=azuresqldb-mi-current&preserve-view=true).
- Weitere Informationen zu Machine Learning auf anderen SQL-Plattformen finden Sie in der [SQL Machine Learning-Dokumentation](/sql/machine-learning/index).
