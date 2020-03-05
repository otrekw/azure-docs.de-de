---
title: Leistungsoptionen – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Optionen für eine Hyperscale (Citus)-Servergruppe, einschließlich der Compute- und Speicheroptionen für Knoten und Regionen.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 2/18/2020
ms.openlocfilehash: 1c9b4b1099bda69764aa7a1a5a984a6316e1047d
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77462410"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-performance-options"></a>Azure Database for PostgreSQL – Leistungsoptionen für Hyperscale (Citus)

## <a name="compute-and-storage"></a>Compute und Speicher
 
Sie können die Compute- und Speichereinstellungen für Workerknoten und den Koordinatorknoten in einer Hyperscale (Citus)-Servergruppe unabhängig voneinander auswählen.  Computeressourcen werden in Form von virtuellen Kernen bereitgestellt und repräsentieren die logische CPU der zugrunde liegenden Hardware. Die Speichergröße für die Bereitstellung bezieht sich auf die für die Koordinator- und Workerknoten in Ihrer Hyperscale (Citus)-Servergruppe verfügbare Kapazität. Der Speicher umfasst Datenbankdateien, temporäre Dateien, Transaktionsprotokolle und die Postgres-Serverprotokolle. Außerdem wird durch die Gesamtmenge an bereitgestelltem Speicher die für jeden Worker- und Koordinatorknoten verfügbare E/A-Kapazität definiert.
 
|                       | Workerknoten           | Koordinatorknoten      |
|-----------------------|-----------------------|-----------------------|
| Compute, virtuelle Kerne       | 4, 8, 16, 32, 64      | 4, 8, 16, 32, 64      |
| Arbeitsspeicher pro V-Kern, GiB | 8                     | 4                     |
| Speichergröße, TiB     | 0,5, 1, 2             | 0,5, 1, 2             |
| Speichertyp          | Allgemein (SSD) | Allgemein (SSD) |
| IOPS                  | Bis zu 3 IOPS/GiB      | Bis zu 3 IOPS/GiB      |


## <a name="regions"></a>Regions
Hyperscale (Citus)-Servergruppen sind in den folgenden Azure-Regionen verfügbar:

* Amerika:
    * Kanada, Mitte
    * USA (Mitte)
    * East US
    * USA (Ost) 2
    * USA Nord Mitte
    * USA, Westen 2
* Asien-Pazifik:
    * Australien (Osten)
    * Japan, Osten
    * Korea, Mitte
    * Asien, Südosten
* Europa:
    * Nordeuropa
    * UK, Süden
    * Europa, Westen

Einige dieser Regionen sind anfangs möglicherweise nicht für alle Azure-Abonnements aktiviert. Öffnen Sie eine [Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest), wenn Sie eine Region aus der oben genannten Liste verwenden möchten, sie aber nicht in Ihrem Abonnement angezeigt wird, oder wenn Sie eine nicht in dieser Liste enthaltene Region verwenden möchten.

## <a name="pricing"></a>Preise
Aktuelle Preisinformationen finden Sie auf der Seite [Azure-Datenbank für MySQL – Preise](https://azure.microsoft.com/pricing/details/postgresql/).
Informationen zu den Kosten der gewünschten Konfiguration können Sie im [Azure-Portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) auf der Registerkarte **Konfigurieren** anzeigen. Dort werden die monatlichen Kosten basierend auf den von Ihnen ausgewählten Optionen angegeben. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie den Azure-Preisrechner verwenden, um einen geschätzten Preis zu erhalten. Wählen Sie auf der Website des [Azure-Preisrechners](https://azure.microsoft.com/pricing/calculator/) die Option **Elemente hinzufügen** aus, erweitern Sie die Kategorie **Datenbanken**, und wählen Sie **Azure Database for PostgreSQL – Hyperscale (Citus)** aus, um die Optionen anzupassen.
 
## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie [eine Hyperscale (Citus)-Servergruppe im Portal erstellen](quickstart-create-hyperscale-portal.md).
