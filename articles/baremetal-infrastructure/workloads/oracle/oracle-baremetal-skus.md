---
title: BareMetal-SKUs für Oracle-Workloads
description: Erfahren Sie mehr über die SKUs für die Oracle BareMetal Infrastructure-Workloads.
ms.topic: reference
ms.subservice: workloads
ms.date: 04/15/2021
ms.openlocfilehash: 42ff26b9ea9bcc022f1ddbf3dddcb041b2cea3a2
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107588852"
---
# <a name="baremetal-skus-for-oracle-workloads"></a>BareMetal-SKUs für Oracle-Workloads

In diesem Artikel werfen wir einen Blick auf spezialisierte BareMetal Infrastructure-SKUs für Oracle-Workloads.

Die BareMetal-Infrastruktur für Oracle-SKUs reicht von zwei Sockets bis zu vier Sockets. Sie können auch aus verschiedenen CPU-Kernen und Arbeitsspeichergrößen auswählen, um die Anforderungen Ihrer Workload zu erfüllen. Im Folgenden finden Sie eine Tabelle, in der die Features der verfügbaren SKUs zusammengefasst sind.
 
| **Oracle**  **Zertifizierte Hardware** | **Modell** | **Gesamter Speicher** | **Storage** | **Verfügbarkeit** |
| --- | --- | --- | --- | --- |
| YES | SAP HANA in Azure S32m- 2 x Intel® Xeon® I6234 Processor 16 CPU-Kerne und 32 CPU-Threads | 1,5 TB | --- | Verfügbar |
| YES | SAP HANA in Azure S64m- 4 x Intel® Xeon® I6234 Processor 32 CPU-Kerne und 64 CPU-Threads | 3,0 TB | --- | Verfügbar |
| YES | SAP HANA in Azure S96– 2 x Intel® Xeon® E7-8890 v4 Processor 48 CPU-Kerne und 96 CPU-Threads | 768 GB | 3,0 TB | Verfügbar |
| YES | SAP HANA in Azure S224 – 4 x Intel® Xeon® Platinum 8276 processor 112 CPU-Kerne und 224 CPU-Threads | 3,0 TB | 6,3 TB | Verfügbar |
| YES | SAP HANA in Azure S224m– 4 x Intel® Xeon® Platinum 8276 processor 112 CPU-Kerne und 224 CPU-Threads | 6,0 TB | 10,5 TB | Verfügbar |

- CPU-Kerne = Summe der CPU-Kerne ohne Hyperthreading (Gesamtanzahl physischer Prozessoren) der Servereinheit. 
- CPU-Threads = Summe der von den Hyperthread-CPU-Kernen bereitgestellten Rechenthreads (Gesamtzahl der logischen Prozessoren) der Servereinheit. Die meisten Einheiten sind standardmäßig für die Verwendung der Hyperthreading-Technologie konfiguriert.
- Server sind für Kunden dediziert.
- Der Kunde hat Stammzugriff (kein Hypervisor).
- Server befinden sich nicht direkt in Azure-VNETs.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den Speicher, der von BareMetal Infrastructure für Oracle angeboten wird.

> [!div class="nextstepaction"]
> [Speicher auf BareMetal für Oracle-Workloads](oracle-baremetal-storage.md)
