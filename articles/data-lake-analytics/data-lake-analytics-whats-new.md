---
title: Aktuelle Änderungen in Data Lake Analytics
description: Dieser Artikel enthält eine fortlaufende Liste der jüngsten Änderungen, die an Data Lake Analytics vorgenommen wurden.
author: xujiang1
ms.service: data-lake-analytics
ms.topic: overview
ms.author: xujiang1
ms.date: 07/31/2020
ms.openlocfilehash: 6f4885594e4f67d91c595d8afdddc15e8ed4ae75
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88263179"
---
# <a name="whats-new-in-data-lake-analytics"></a>Neuerungen in Data Lake Analytics

Azure Data Lake Analytics wird für bestimmte Komponenten in unregelmäßigen Abständen aktualisiert. Damit Sie bezüglich der aktuellen Änderungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Benachrichtigung über wichtige Komponenten (Betavorschau)
- Versionsinformationen zu wichtigen Komponenten, z. B. die Liste der verfügbaren Komponentenversionen, aktuelle Standardversion usw.


## <a name="notification-of-key-component-beta-preview"></a>Benachrichtigung über wichtige Komponenten (Betavorschau)

Es ist keine Betaversion wichtiger Komponenten als Vorschau verfügbar. 

## <a name="u-sql-runtime"></a>U-SQL-Runtime

Mit der Azure Data Lake-U-SQL-Laufzeit, einschließlich Compiler, Optimierer und Auftrags-Manager, wird Ihr U-SQL-Code verarbeitet.

Wenn Sie den Azure Data Lake Analytics-Auftrag über ein beliebiges Tool übermitteln, verwendet Ihr Auftrag die aktuell verfügbare Standardruntime in der Produktionsumgebung. 

Die Runtimeversion wird in unregelmäßigen Abständen aktualisiert. Die vorherige Runtime bleibt für einige Zeit verfügbar. Wenn eine neue Betaversion als Vorschau bereit ist, ist sie ebenfalls dort verfügbar.

> [!CAUTION]
> - Wenn Sie eine andere Laufzeit als die Standardversion auswählen, können Ihre U-SQL-Aufträge möglicherweise unterbrochen werden. Es wird dringend empfohlen, diese nicht standardmäßigen Versionen nicht für die Produktion, sondern nur für Tests zu verwenden.
> - Für die nicht standardmäßige Runtimeversion gilt ein fester Lebenszyklus. Er läuft automatisch ab.

Die folgende Version ist die aktuelle Runtimestandardversion.

- release-20200124live_adl_16283022_2

Informationen zur Behandlung von U-SQL-Runtimefehlern finden Sie unter [Problembehandlung für U-SQL-Laufzeitfehler aufgrund von Laufzeitänderungen](runtime-troubleshoot.md).

## <a name="net-framework"></a>.NET Framework

Azure Data Lake Analytics verwendet nun **.NET Framework 4.7.2**. 

Wenn der Azure Data Lake Analytics-U-SQL-Skriptcode benutzerdefinierte Assemblys verwendet und diese wiederum .NET-Bibliotheken verwenden, überprüfen Sie Ihren Code auf Probleme.

Informationen zur Problembehandlung bei einem .NET-Upgrade finden Sie unter [Problembehandlung für U-SQL-Laufzeitfehler aufgrund von Laufzeitänderungen](runtime-troubleshoot.md).

## <a name="release-note"></a>Versionshinweis

Einzelheiten zu aktuellen Änderungen finden Sie in den [Versionshinweisen zu Azure Data Lake Analytics](https://github.com/Azure/AzureDataLake/tree/master/docs/Release_Notes).


## <a name="next-steps"></a>Nächste Schritte

* Erste Schritte mit Data Lake Analytics mithilfe [des Azure-Portals](data-lake-analytics-get-started-portal.md) | [von Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [der CLI](data-lake-analytics-get-started-cli.md)

