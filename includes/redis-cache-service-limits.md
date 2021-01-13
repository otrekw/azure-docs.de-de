---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: 1ab6243be39bf30bc060ed5745fbf600924743a9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "71839224"
---
| Resource | Begrenzung |
| --- | --- |
| Cachegröße |1,2 TB |
| Datenbanken |64 |
| Maximale Anzahl verbundener Clients |40.000 |
| Azure Cache for Redis-Replikate, für Hochverfügbarkeit |1 |
| Shards in einem Premium-Cache mit Clustering |10 |

Die erhältliche Größe und die Konfigurationsbeschränkungen von Azure Cache for Redis unterscheiden sich je nach Tarif. Die Tarife und die damit verbundenen Größen finden Sie unter [Azure Cache for Redis – Preise](https://azure.microsoft.com/pricing/details/cache/).

Weitere Informationen zu den Konfigurationsbeschränkungen von Azure Cache for Redis finden Sie unter [Standardmäßige Redis-Serverkonfiguration](../articles/azure-cache-for-redis/cache-configure.md#default-redis-server-configuration).

Da die Konfiguration und Verwaltung von Azure Cache for Redis-Instanzen von Microsoft übernommen wird, werden nicht alle Redis-Befehle in Azure Cache for Redis unterstützt. Weitere Informationen finden Sie unter [Redis-Befehle, die in Azure Cache for Redis nicht unterstützt werden](../articles/azure-cache-for-redis/cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis).

