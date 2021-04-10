---
title: Größen von Azure-VMs – für Compute optimiert | Microsoft-Dokumentation
description: Auflistung der verschiedenen für Compute optimierten Größen, die für virtuelle Computer in Azure verfügbar sind Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
author: mimckitt
ms.service: virtual-machines
ms.subservice: vm-sizes-compute
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: mimckitt
ms.openlocfilehash: 01eba7bff571a8db25591b8bfa5c5e712511588f
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102557691"
---
# <a name="compute-optimized-virtual-machine-sizes"></a>Compute-optimierte VM-Größen

Für Compute optimierte VM-Größen verfügen über ein hohes Verhältnis zwischen CPU und Arbeitsspeicher. Diese Größen sind ideal für Webserver, Netzwerkappliances, Batchvorgänge und Anwendungsserver mit mittlerer Auslastung. Dieser Artikel enthält Informationen über die Anzahl von vCPUs, Datenträgern und NICs. Er umfasst zudem Informationen zum Speicherdurchsatz und zur Netzwerkbandbreite für die jeweiligen Größen in dieser Gruppe.

Die [Fsv2-Serie](fsv2-series.md) wird auf dem Intel® Xeon® Platin 8272CL-Prozessor (Cascade Lake) der zweiten Generation und auf dem Intel® Xeon® Platinum 8168-Prozessor (Skylake) ausgeführt. Dieser verfügt über eine kontinuierliche Turbo-Taktfrequenz von 3,4 GHz für alle Kerne und eine maximale Turbofrequenz von 3,7 GHz für Einzelkerne. Intel® AVX-512-Anweisungen sind neu auf von Intel skalierbaren Prozessoren. Diese Anweisungen können bei Gleitkommaoperationen mit einfacher und doppelter Genauigkeit mit Vektorverarbeitungsworkloads die Leistung verdoppeln. Sie bewältigen demnach alle Rechenworkloads wirklich schnell.

Die Fsv2-Serie hat einen niedrigeren Listenpreis pro Stunde und bietet auf Basis der Azure-Compute-Einheit (Azure Compute Unit, ACU) das beste Preis-Leistungs-Verhältnis pro vCPU im Azure-Portfolio.

## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.

Weitere Informationen zur Benennung von VMs in Azure finden Sie unter [Namenskonventionen für Azure-VM-Größen](./vm-naming-conventions.md).