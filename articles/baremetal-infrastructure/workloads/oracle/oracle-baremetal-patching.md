---
title: Überlegungen im Zusammenhang mit dem Patchen der Bare-Metal-Infrastruktur für Oracle
description: Hier finden Sie Überlegungen im Zusammenhang mit dem Patchen des Betriebssystems/Kernels für die Bare-Metal-Infrastruktur für Oracle.
ms.topic: reference
ms.subservice: workloads
ms.date: 04/14/2021
ms.openlocfilehash: 5af713c776def8af558531c84013b221a8d30087
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107558678"
---
# <a name="patching-considerations-for-baremetal-for-oracle"></a>Überlegungen im Zusammenhang mit dem Patchen der Bare-Metal-Infrastruktur für Oracle

In diesem Artikel werden wichtige Überlegungen im Zusammenhang mit dem Patchen des Betriebssystems/Kernels für die Bare-Metal-Infrastruktur für Oracle behandelt.

Installieren Sie die betriebssystemspezifische Version der eNIC- und fNIC-Treiber, wie in der folgenden Kompatibilitätstabelle angegeben, um eine ordnungsgemäße Netzwerkleistung und Systemstabilität zu gewährleisten. 

Server werden mit kompatiblen Versionen an Kunden ausgeliefert. Beim Patchen des Betriebssystems/Kernels kann es jedoch vorkommen, dass Treiber auf die Standardtreiberversionen zurückgesetzt werden. Vergewissern Sie sich daher im Anschluss an Patchvorgänge für das Betriebssystem/den Kernel, dass die korrekte Treiberversion ausgeführt wird.

| Betriebssystemhersteller | Betriebssystem-Paketversion | Firmware Version | eNIC-Treiber | fNIC-Treiber |
| --- | --- | --- | --- | --- |
| Red Hat | RHEL 7.6 | 3.2.3i | 2.3.0.53 | 1.6.0.34 |
| Red Hat | RHEL 7.6 | 4.1.1b | 2.3.0.53 | 1.6.0.34 |

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit der Ethernetkonfiguration von Bare-Metal für Oracle vertraut.

> [!div class="nextstepaction"]
> [Ethernetkonfiguration von Bare-Metal für Oracle](oracle-baremetal-ethernet.md)

