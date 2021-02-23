---
title: 'Bekannte Probleme und Einschränkungen: Azure Database for PostgreSQL – Einzelserver und flexibler Server (Vorschau)'
description: Listet die bekannten Probleme auf, die Kunden bewusst sein sollten.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/05/2020
ms.openlocfilehash: 3c26368e6281ad74d617891ba15c980117b25a6e
ms.sourcegitcommit: 126ee1e8e8f2cb5dc35465b23d23a4e3f747949c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100105412"
---
# <a name="azure-database-for-postgresql---known-issues-and-limitations"></a>Azure Database for PostgreSQL: Bekannte Probleme und Einschränkungen

Auf dieser Seite finden Sie eine Liste bekannter Probleme in Azure Database for PostgreSQL, die sich auf Ihre Anwendung auswirken können. Außerdem werden alle Risikominderungsmaßnahmen und Empfehlungen aufgelistet, um das Problem zu umgehen.

## <a name="intelligent-performance---query-store"></a>Intelligente Leistung: Abfragespeicher

Gilt für Azure Database for PostgreSQL – Einzelserver.

| Zutreffend | Ursache | Wartung|
| ----- | ------ | ---- | 
| PostgreSQL 9.6, 10, 11 | Wenn Sie den Serverparameter `pg_qs.replace_parameter_placeholders` aktivieren, kann es vorkommen, dass ein Server in einigen seltenen Szenarien heruntergefahren wird. | Legen Sie im Azure-Portal im Abschnitt „Serverparameter“ den Wert des Parameters `pg_qs.replace_parameter_placeholders` auf `OFF` fest, und speichern sie ihn.   | 

## <a name="server-parameters"></a>Serverparameter

Gilt für Azure Database for PostgreSQL – Einzelserver und flexibler Server

| Zutreffend | Ursache | Wartung| 
| ----- | ------ | ---- | 
| PostgreSQL 9.6, 10, 11 | Wenn Sie den Serverparameter `max_locks_per_transaction` auf einen höheren Wert als den [empfohlenen](https://www.postgresql.org/docs/11/kernel-resources.html) Wert ändern, kann dies dazu führen, dass der Server nach einem Neustart nicht mehr gestartet wird. | Belassen Sie den Standardwert (32 oder 64), oder verwenden Sie einen angemessenen Wert gemäß der PostgreSQL-[Dokumentation](https://www.postgresql.org/docs/11/kernel-resources.html). <br> <br> Auf der Dienstseite wird daran gearbeitet, den hohen Wert basierend auf der SKU einzuschränken.  | 

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen finden Sie unter [bewährte Methoden](./concepts-query-store-best-practices.md) für den Abfragespeicher.
