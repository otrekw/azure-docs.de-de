---
title: Data Tiering und Extension Nodes für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Erfahren Sie mehr über Data Tiering und Extension Nodes für SAP HANA in Azure (große Instanzen).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/17/2021
ms.author: madhukan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c9ab5f48aac13153cfc510ce8b7ca5b7b8cc3f41
ms.sourcegitcommit: a9f131fb59ac8dc2f7b5774de7aae9279d960d74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110189817"
---
# <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>Verwenden von SAP HANA Data Tiering und Extension Nodes

SAP unterstützt ein Data Tiering-Modell für SAP Business Warehouse (BW) für verschiedene Releases von SAP NetWeaver und SAP BW/4HANA. Weitere Informationen zum Data Tiering-Modell finden Sie unter [SAP BW/4HANA and SAP BW on HANA with SAP HANA extension nodes](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#) (SAP BW/4HANA und SAP BW unter HANA mit SAP HANA Extension Nodes).

Mit HANA (große Instanz) können Sie die Option 1 der Konfiguration von SAP HANA Extension Nodes wie in den häufig gestellten Fragen und Blogdokumenten von SAP beschrieben verwenden. Konfigurationen der Option 2 können mit folgenden HANA-SKUs (große Instanzen) eingerichtet werden: S72m, S192, S192m, S384 und S384m.

## <a name="advantages-of-sap-hana-extension-nodes"></a>Vorteile von SAP HANA Extension Nodes

Die Verwendung SAP HANA Extension Nodes (Option 1 oder 2) ist eine einfache Möglichkeit, SAP HANA-Arbeitsspeicher besser zu nutzen. Die Vorteile von SAP HANA Extension Nodes werden deutlich, wenn Sie sich die Richtlinien zur Dimensionierung von SAP ansehen. Hier sind einige Beispiele:

- SAP HANA-Dimensionierungsrichtlinien erfordern im Vergleich zum Arbeitsspeicher in der Regel das Doppelte des Datenvolumens. Wenn Sie Ihre SAP HANA-Instanz mit heißen Daten ausführen, werden nur 50 Prozent oder weniger des Arbeitsspeichers zum Speichern von Daten verwendet. Im Idealfall wird der verbleibende Arbeitsspeicher für SAP HANA reserviert, damit es seine Arbeit erledigen kann.
- Das bedeutet, dass in einer HANA-S192-Einheit (große Instanz) mit 2 TB Arbeitsspeicher, in der eine SAP BW-Datenbank ausgeführt wird, nur 1 TB als Datenvolumen verfügbar ist.
- Wenn Sie einen zusätzlichen Option-1-SAP HANA Extension Node verwenden, würde eine S192-SKU von HANA (große Instanz) zusätzliche 2 TB Kapazität für das Datenvolumen bieten. Bei der Option-2-Konfiguration erhalten Sie ein weiteres Volumen von 4 TB für warme Daten. Gegenüber dem heißen Knoten kann die vollständige Speicherkapazität des „warmen“ Extension Node zur Datenspeicherung für die Option-1 verwendet werden. Bei der Option 2 der SAP HANA Extension Node-Konfiguration kann die doppelte Menge an Arbeitsspeicher für das Datenvolumen genutzt werden.
- Bei Option 1 haben Sie letztlich eine Kapazität von 3 TB für Ihre Daten und ein Heiß/Warm-Verhältnis von 1:2. Bei der Option 2 der Extension Node-Konfiguration verfügen Sie über 5 TB an Daten und ein Verhältnis von 1:4.

Je größer das Datenvolumen im Vergleich zum Arbeitsspeicher ist, desto größer ist die Wahrscheinlichkeit, dass die angefragten warmen Daten auf dem Datenträger gespeichert sind.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Funktionsprinzipien für SAP HANA in Azure (große Instanzen) und Ihre Zuständigkeiten.

> [!div class="nextstepaction"]
> [Funktionsprinzipien und Zuständigkeiten](hana-operations-model.md)
