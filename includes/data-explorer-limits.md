---
author: ilayrn
ms.service: data-explorer
ms.topic: include
ms.date: 01/20/2020
ms.author: ilayr
ms.openlocfilehash: d9e23e74cd980ecf44cd04ad2b6f89dc6a4cf868
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2020
ms.locfileid: "76548203"
---
In der nachstehenden Tabelle werden die maximalen Grenzwerte für Azure Data Explorer-Cluster beschrieben.

| Resource | Begrenzung |
| --- | --- |
| Cluster pro Region und Abonnement | 20 |
| Instanzen pro Cluster | 1000 | 
| Anzahl der Datenbanken in einem Cluster | 10.000 |
| Anzahl der angefügten Datenbankkonfigurationen in einem Cluster | 70 |

In der nachstehenden Tabelle werden die Grenzwerte für Verwaltungsvorgänge beschrieben, die in Azure Data Explorer-Clustern ausgeführt werden.

| `Scope` | Vorgang | Begrenzung |
| --- | --- | --- |
| Cluster | Lesen (z. B. Abrufen eines Clusters) | 500 pro 5 Minuten |
| Cluster | Schreiben (z. B. Erstellen einer Datenbank) | 1\.000 pro Stunde |

