---
title: Datenbankdienste von Azure Deutschland | Microsoft-Dokumentation
description: Dieser Artikel enthält einen Vergleich der SQL-Datenbank-Dienste für Azure Deutschland.
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2019
ms.author: ralfwi
ms.openlocfilehash: 16c4d835da65d7dbb3e8f3ba08d99602f952b6b1
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75436649"
---
# <a name="azure-germany-database-services"></a>Datenbankdienste von Azure Deutschland

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/europe/2018/08/31/microsoft-to-deliver-cloud-services-from-new-datacentres-in-germany-in-2019-to-meet-evolving-customer-needs/) haben wir keine neuen Kunden akzeptiert und keine neuen Features und Dienste an den ursprünglichen Microsoft Cloud Deutschland-Standorten bereitgestellt.
>
> Aufgrund der Weiterentwicklung der Kundenbedürfnisse haben wir vor Kurzem zwei neue Rechenzentrumsregionen in Deutschland [gestartet](https://azure.microsoft.com/blog/microsoft-azure-available-from-new-cloud-regions-in-germany/), die Datenresidenz für Kundendaten, umfassende Konnektivität mit dem globalen Cloudnetzwerk von Microsoft sowie wettbewerbsfähige Preise bieten. 
>
> Profitieren Sie von der Vielfalt der Funktionen, Sicherheit auf Unternehmensniveau und den umfangreichen Features, die in unseren neuen deutschen Rechenzentrumsregionen zur Verfügung stehen, und [migrieren](germany-migration-main.md) Sie noch heute.

## <a name="sql-database"></a>SQL-Datenbank
Azure SQL-Datenbank V12 ist allgemein in Azure Deutschland verfügbar. Anleitungen zur Konfiguration der Metadatensichtbarkeit und bewährte Methoden für den Schutz finden Sie unter [Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](/sql/relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database) und in der [SQL-Datenbank-Dokumentation](../sql-database/index.yml).

### <a name="variations"></a>Abweichungen
Die Adresse für SQL-Datenbank in Azure Deutschland unterscheidet sich von der Adresse in der globalen Azure-Umgebung:

| Dienstart | Globale Azure-Umgebung | Azure Deutschland |
| --- | --- | --- |
| SQL-Datenbank | *.database.windows.net | *.database.cloudapi.de |


## <a name="azure-cache-for-redis"></a>Azure Cache for Redis
Einzelheiten zu Azure Cache for Redis und seiner Verwendung finden Sie in der [Azure Cache for Redis Documentation](../azure-cache-for-redis/index.yml).

### <a name="variations"></a>Abweichungen
Bei Azure Deutschland werden für den Zugriff auf und die Verwaltung von Azure Cache for Redis andere URLs verwendet als in der globalen Azure-Umgebung:

| Dienstart | Globale Azure-Umgebung | Azure Deutschland |
| --- | --- | --- |
| Cache-Endpunkt | *.redis.cache.windows.net | *.redis.cache.cloudapi.de |
| Azure-Portal | https://portal.azure.com | https://portal.microsoftazure.de |

> [!NOTE]
> Ihre gesamten Skripts und Ihr Code müssen die richtigen Endpunkte und Umgebungen berücksichtigen. Weitere Informationen finden Sie in [Verwalten von Azure Cache for Redis mit Azure PowerShell](../azure-cache-for-redis/cache-how-to-manage-redis-cache-powershell.md) unter „So stellen Sie eine Verbindung mit Microsoft Azure Deutschland her“.
>
>


## <a name="next-steps"></a>Nächste Schritte
Abonnieren Sie den [Azure Deutschland-Blog](https://blogs.msdn.microsoft.com/azuregermany/), um weitere Informationen und Updates zu erhalten.
