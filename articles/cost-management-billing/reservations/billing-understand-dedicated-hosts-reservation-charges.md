---
title: Grundlegendes zum Rabatt für reservierte Azure Dedicated Host-Instanzen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie der Rabatt für reservierte Azure-VM-Instanzen auf Azure Dedicated Host-Instanzen angewendet wird.
author: yashesvi
manager: yashar
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2020
ms.author: banders
ms.openlocfilehash: 26b71952e3d24214331b314f723728b56e3c4254
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2020
ms.locfileid: "78207771"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-azure-dedicated-hosts"></a>Anwendung des Azure-Reservierungsrabatts auf Azure Dedicated Host-Instanzen

Nach dem Erwerb einer reservierten Azure Dedicated Host-Instanz wird der Reservierungsrabatt automatisch auf dedizierte Hosts angewendet, die den Attributen und der Menge der Reservierung entsprechen. Eine Reservierung deckt die Computekosten Ihrer dedizierten Hosts ab.

## <a name="how-reservation-discount-is-applied"></a>Wie der Reservierungsrabatt angewendet wird

Reservierungsrabatte funktionieren nach dem Prinzip „*use-it-or-lose-it*“. Das heißt, wenn Sie für eine Stunde nicht über die entsprechenden Ressourcen verfügen, verlieren Sie eine Reservierungsmenge für diese Stunde. Sie können ungenutzte reservierte Stunden nicht übertragen.

Wenn Sie einen dedizierten Host löschen, gilt der Reservierungsrabatt automatisch für eine andere entsprechende Ressource im angegebenen Reservierungsumfang. Werden im angegebenen Reservierungsumfang keine entsprechenden Ressourcen gefunden, gehen die reservierten Stunden  *verloren*.

## <a name="reservation-discount-for-dedicated-hosts"></a>Reservierungsrabatt für Dedicated Host-Instanzen

Durch eine reservierte Azure Dedicated Host-Instanz erhalten Sie einen Rabatt auf die Kosten der Computeinfrastruktur, die für Ihre dedizierten Hosts verwendet wird. Der Rabatt für Ihre dedizierten Hosts gilt unabhängig davon, ob diese von virtuellen Computern genutzt werden oder nicht. Zusätzliche Kosten, etwa für die Lizenzierung, die Netzwerknutzung oder den Speicherverbrauch durch virtuelle Computer, die auf dem dedizierten Host bereitgestellt werden, sind durch die Reservierung nicht abgedeckt.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Sollten Sie weitere Fragen haben oder Hilfe benötigen,  [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)

- [Dedizierte Azure-Hosts](https://docs.microsoft.com/azure/virtual-machines/windows/dedicated-hosts)

- [Azure Dedicated Host – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/dedicated-host/)

- [Verwalten von Azure-Reservierungen](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)

- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage)

- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)

- [Grundlegendes zur Verwendung von Azure-Reservierungen für CSP-Abonnements](https://docs.microsoft.com/partner-center/azure-reservations)

