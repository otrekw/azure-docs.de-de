---
title: Bedrohungserkennung für Datendienste in Azure Security Center | Microsoft-Dokumentation
description: In diesem Artikel werden die Warnungen der Datendienste vorgestellt, die in Azure Security Center verfügbar sind.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: da960861-0b6c-4d80-932d-898cdebb4f83
ms.service: security-center
ms.topic: conceptual
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: 6d6e48c84f558840b3266193c4cbb9c3576f1a58
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75665733"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Bedrohungserkennung für Datendienste in Azure Security Center

 Azure Security Center analysiert die Protokolle der Datenspeicherdienste und löst Warnungen aus, wenn eine Bedrohung für Ihre Datenressourcen erkannt wird. In diesem Artikel werden die Warnungen aufgelistet, die von Security Center für die folgenden Dienste generiert werden:

* [Azure SQL-Datenbank und Azure SQL Data Warehouse](#data-sql)
* [Azure Storage (in englischer Sprache)](#azure-storage)
* [Azure Cosmos DB](#cosmos-db)

## SQL-Datenbank und SQL Data Warehouse <a name="data-sql"></a>

Advanced Threat Protection für Azure SQL-Datenbank erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbanken zuzugreifen oder diese zu nutzen.

Es werden Warnungen angezeigt, wenn verdächtige Datenbankaktivitäten, potenzielle Sicherheitsrisiken oder Angriffe durch Einschleusung von SQL-Befehlen sowie ungewöhnliche Datenbankzugriffs- und -abfragemuster vorliegen.

Advanced Threat Protection für Azure SQL-Datenbank und SQL ist Teil des einheitlichen Pakets [Advanced Data Security (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) für erweiterte SQL-Sicherheitsfunktionen, das Azure SQL-Datenbanken, verwaltete Azure SQL-Datenbank-Instanzen, Azure SQL Data Warehouse-Datenbanken und SQL Server-Instanzen in Azure Virtual Machines abdeckt.

Weitere Informationen finden Sie unter

* [Aktivieren von Advanced Threat Protection für Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)
* [Aktivieren von Advanced Threat Protection für SQL Server-Instanzen auf Azure Virtual Machines](security-center-iaas-advanced-data.md)
* [Liste der Bedrohungsschutzwarnungen für SQL-Datenbank und SQL Data Warehouse](alerts-reference.md#alerts-sql-db-and-warehouse)

## Azure Storage <a name="azure-storage"></a>

Advanced Threat Protection für Storage (derzeit nur für Blobspeicher verfügbar) erkennt ungewöhnliche und möglicherweise schädliche Versuchen, auf Speicherkonten zuzugreifen oder diese unbefugt zu nutzen. Dank dieser Schutzebene können Sie auch ohne fundierte Sicherheitskenntnisse etwas gegen Bedrohungen unternehmen und Ihre Systeme für die Sicherheitsüberwachung verwalten.

>[!NOTE]
>Advanced Threat Protection für Storage ist derzeit in Azure Government- und Sovereign Cloud-Regionen nicht verfügbar.

Weitere Informationen finden Sie unter

* [Aktivieren von Advanced Threat Protection für Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection)
* [Liste der Bedrohungsschutzwarnungen für Azure Storage](alerts-reference.md#alerts-azurestorage)

## Azure Cosmos DB<a name="cosmos-db"></a>

Die Azure Cosmos DB-Warnungen werden bei ungewöhnlichen und potenziell schädlichen Zugriffsversuchen oder Exploit-Vorgängen für Azure Cosmos DB-Konten generiert.

Weitere Informationen finden Sie unter

* [Advanced Threat Protection für Azure Cosmos DB (Vorschau)](../cosmos-db/cosmos-db-advanced-threat-protection.md)
* [Liste der Bedrohungsschutzwarnungen für Azure Cosmos DB (Vorschau)](alerts-reference.md#alerts-azurecosmos)
