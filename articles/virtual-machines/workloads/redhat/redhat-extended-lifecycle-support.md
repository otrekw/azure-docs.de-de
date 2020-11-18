---
title: Erweiterte Lebenszyklusunterstützung für Red Hat Enterprise Linux
description: Informationen zum Hinzufügen des Add-Ons zur erweiterten Lebenszyklusunterstützung für Red Hat Enterprise
author: mathapli
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 04/16/2020
ms.author: mathapli
ms.reviewer: cynthn
ms.openlocfilehash: e7c9f9b158d01204536712f8024ed098cd7a5037
ms.sourcegitcommit: 65d518d1ccdbb7b7e1b1de1c387c382edf037850
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2020
ms.locfileid: "94372858"
---
# <a name="red-hat-enterprise-linux-rhel-extended-lifecycle-support"></a>Erweiterte Lebenszyklusunterstützung für Red Hat Enterprise Linux (RHEL)
Dieser Artikel bietet Informationen zur erweiterten Lebenszyklusunterstützung für die Red Hat Enterprise-Images:
* Red Hat Enterprise Linux 6  

## <a name="red-hat-enterprise-linux-6-lifecycle"></a>Red Hat Enterprise Linux 6 Lifecycle
Am 30. November 2020 ist die Wartungsphase für Red Hat Enterprise Linux 6 beendet. Danach folgt eine Phase des erweiterten Lebenszyklus. Da die Phasen der vollständigen Unterstützung und Wartung für Red Hat Enterprise Linux 6 enden, wird dringend empfohlen, ein Upgrade auf Red Hat Enterprise Linux 7 oder 8 durchzuführen. Wenn Kunden weiterhin Red Hat Enterprise Linux 6 verwenden müssen, empfiehlt es sich, das Add-On zur erweiterten Lebenszyklusunterstützung (Extended Life Cycle Support, ELS) für Red Hat Enterprise Linux hinzuzufügen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

#### <a name="im-running-red-hat-enterprise-linux-6-and-cant-migrate-to-a-later-version-at-this-time-what-options-do-i-have"></a>Ich arbeite mit Red Hat Enterprise Linux 6 und kann derzeit nicht zu einer höheren Version migrieren. Welche Optionen stehen mir zur Verfügung?
* Führen Sie Red Hat Enterprise Linux 6 weiter aus, und erwerben Sie die Add-On-Repositorys zur erweiterten Lebenszyklusunterstützung (Extended Life Cycle Support, ELS). So erhalten Sie weiterhin eingeschränkten Support für Softwarewartung und technische Probleme (mehr dazu erfahren Sie in den folgenden Abschnitten zu Upgrades und Kosten).
* Migrieren Sie zu Red Hat Enterprise Linux 7 oder 8, so schnell es Ihnen möglich ist.

#### <a name="what-is-the-additional-charge-for-using-red-hat-enterprise-linux-extended-life-cycle-support-els-add-on"></a>Welche zusätzlichen Gebühren fallen für das Add-On zur erweiterten Lebenszyklusunterstützung (Extended Life Cycle Support, ELS) für Red Hat Enterprise Linux an?

|Größe des virtuellen Computers|Zeitrahmen für zusätzliche Gebühren|Zusätzliche Kosten (in USD)| Notizen|
|---|---|---|---|
| Kleine virtuelle Gast-VM (<=4 Kerne) | Zusätzliche Gebühr/Stunde | TBC | |
|  | Zusätzliche Gebühr/Monat | TBC | Für reservierte Instanzen |
|  | Zusätzliche Gebühr/Jahr | TBC | Für reservierte Instanzen |
| Große virtuelle Gast-VM (<=4 Kerne) | Zusätzliche Gebühr/Stunde | TBC | |
|  | Zusätzliche Gebühr/Monat | TBC | Für reservierte Instanzen |
|  | Zusätzliche Gebühr/Jahr | TBC | Für reservierte Instanzen |

#### <a name="what-is-the-process-to-add-extended-life-cycle-support-els-repositories-to-continue-to-receive-software-maintenance-bug-and-security-fixes-and--support-for-red-hat-enterprise-linux-6"></a>Wie füge ich ELS-Repositorys (Extended Life Cycle Support, erweiterte Lebenszyklusunterstützung) hinzu, um weiterhin Softwarewartungsaktivitäten (Fehler- und Sicherheitskorrekturen) und Unterstützung für Red Hat Enterprise Linux 6 zu erhalten?

Der vollständige Prozess zum Registrieren für die erweiterte Lebenszyklusunterstützung wird in Kürze hier zur Verfügung stehen (spätestens am 15. November 2020).

## <a name="next-steps"></a>Nächste Schritte

* Die vollständige Liste der RHEL-Images in Azure finden Sie unter [Verfügbare RHEL-Images (Red Hat Enterprise Linux) in Azure](./redhat-imagelist.md).
* Weitere Informationen zur Azure Red Hat-Updateinfrastruktur finden Sie unter [Red Hat-Updateinfrastruktur für On-Demand-RHEL-VMs in Azure](./redhat-rhui.md).
* Weitere Informationen zum RHEL BYOS-Angebot finden Sie unter [Bring-Your-Own-Subscription-Gold-Images für Red Hat Enterprise Linux in Azure](./byos.md).
* Weitere Informationen zu Red Hat-Supportrichtlinien für alle RHEL-Versionen finden Sie unter [Red Hat Enterprise Linux Life Cycle (Red Hat Enterprise Linux-Lebenszyklus)](https://access.redhat.com/support/policy/updates/errata).