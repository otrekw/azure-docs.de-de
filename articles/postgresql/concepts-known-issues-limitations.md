---
title: 'Bekannte Probleme und Einschränkungen: Azure Database for PostgreSQL – Einzelserver und flexibler Server (Vorschau)'
description: Listet die bekannten Probleme auf, die Kunden bewusst sein sollten.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/02/2021
ms.openlocfilehash: 0806ef1a191759e769271a97fb1809298cde10db
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122345911"
---
# <a name="azure-database-for-postgresql---known-issues-and-limitations"></a>Azure Database for PostgreSQL: Bekannte Probleme und Einschränkungen

Auf dieser Seite finden Sie eine Liste bekannter Probleme in Azure Database for PostgreSQL, die sich auf Ihre Anwendung auswirken können. Außerdem werden alle Risikominderungsmaßnahmen und Empfehlungen aufgelistet, um das Problem zu umgehen.

## <a name="intelligent-performance---query-store"></a>Intelligente Leistung: Abfragespeicher

Gilt für Azure Database for PostgreSQL – Einzelserver.

| Zutreffend | Ursache | Wartung|
| ----- | ------ | ---- | 
| PostgreSQL 9.6, 10, 11 | Wenn Sie den Serverparameter `pg_qs.replace_parameter_placeholders` aktivieren, kann es vorkommen, dass ein Server in einigen seltenen Szenarien heruntergefahren wird. | Legen Sie im Azure-Portal im Abschnitt „Serverparameter“ den Wert des Parameters `pg_qs.replace_parameter_placeholders` auf `OFF` fest, und speichern sie ihn.   | 


## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen finden Sie unter [bewährte Methoden](./concepts-query-store-best-practices.md) für den Abfragespeicher.
