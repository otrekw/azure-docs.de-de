---
title: 'Referenz: Deaktivierungen von Data Science Virtual Machine-Images'
titleSuffix: Azure Data Science Virtual Machine
description: Details zu Deaktivierungen, die sich auf Azure Data Science Virtual Machine auswirken
author: lobrien
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: laobri
ms.date: 07/17/2020
ms.topic: reference
ms.openlocfilehash: ac56e2e69cf62c8afae73e0a7616e7e7f80c0493
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88816319"
---
# <a name="reference-retirements-of-dsvm-images"></a>Referenz: Deaktivierungen von DSVM-Images

Im Folgenden erläutern wir das Deaktivieren (als veraltet markieren) von Images und Vorschläge für den Umgang mit bevorstehenden Deaktivierungen für Azure Data Science Virtual Machine.

## <a name="why-we-retire-dsvm-images"></a>Gründe für das Deaktivieren von DSVM-Images

Data Science Virtual Machine liegt zurzeit in zwei Editionen vor:

* Ubuntu
* Windows Server

Das DSVM-Image für diese Editionen basiert auf einer bestimmten Betriebssystemversion, z. B. auf Ubuntu 18.04 LTS. Im Laufe der Zeit endet das Wartungsfenster für die Betriebssystem_version_ und/oder die Data Science-Tools unterstützen ältere Betriebssystemversionen nicht mehr. Um das DSVM-Image mit aktuellen Betriebssystemen und Data Science-Tools auf dem neuesten Stand zu halten, wird ein neues DSVM-Image basierend auf neueren Betriebssystemversionen veröffentlicht.

## <a name="how-we-retire-dsvm-images"></a>Vorgehensweise beim Deaktivieren von DSVM-Images

Wir stellen eine _Deaktivierungsankündigung_ aus, mit der vorhandene DSVM-Benutzer (über E-Mail) benachrichtigt werden, dass ein DSVM-Image bald deaktiviert wird. In der Deaktivierungsankündigung werden das _Datum der Deaktivierung_ (nach diesem Datum ist das Image nicht mehr verfügbar) sowie Empfehlungen für Abhilfemaßnahmen (z. B. ein Upgrade auf ein neueres DSVM-Image) aufgeführt.

Am Datum der _Ankündigung_ wird das Image in Marketplace ausgeblendet, damit Folgendes gilt:

1. Neue Benutzer werden daran gehindert, versehentlich ein zurückgezogenes DSVM-Image bereitzustellen.
2. Vorhandene Benutzer können ARM-Bereitstellungen bis zum Deaktivierungsdatum verwenden.

Das ausgeblendete Image erhält Betriebssystempatches bis zum _Deaktivierungsdatum_, jedoch __keine__ weiteren Updates für die Data Science-Tools und Frameworks. Ab dem _Deaktivierungsdatum_ ist das Image für ARM-Bereitstellungen nicht mehr verfügbar.

Alle bereitgestellten DSVM-Images in Ihrem Abonnement funktionieren nach dem Deaktivierungsdatum weiterhin. Es wird jedoch empfohlen, ein Upgrade auf das neueste DSVM-Image durchführen, damit Sie über die neuesten Betriebssysteme und Data Science-Tools verfügen.

## <a name="impact"></a>Auswirkung

Vorhandene von DSVM bereitgestellte Images in Ihrem Abonnement funktionieren nach dem Deaktivierungsdatum weiterhin. Es wird jedoch empfohlen, dass Benutzer ein Upgrade ihres DSVM-Images auf die neuere Version durchführen, indem sie das Azure-Portal oder eine ARM-Vorlage verwenden.

> [!WARNING]
> Veraltete DSVM-Images, die mit Virtual Machine Scale Sets bereitgestellt werden, können nach dem Deaktivierungsdatum nicht mehr zentral hochskaliert werden.
>
> ARM-Vorlagen, die nicht mit den neuen Details zum DSVM-Image aktualisiert wurden, können nach dem Deaktivierungsdatum nicht mehr bereitgestellt werden.

## <a name="mitigating-upcoming-retirements"></a>Abmildern bevorstehender Deaktivierungen

In diesem Abschnitt werden Maßnahmen für bevorstehende Deaktivierungen erörtert.

### <a name="upgrade-windows-2016-dsvm"></a>Upgrade der Windows 2016 DSVM

Führen Sie die folgenden Schritte aus, um einen Datenträger von Ihrer vorhandenen Windows 2016 DSVM zu einer Windows 2019 DSVM zu migrieren:

1. Erstellen Sie eine neue Windows 2019 DSVM, indem Sie die [hier](./provision-vm.md#create-your-dsvm) beschriebenen Anweisungen befolgen.
1. Trennen Sie unter Befolgen [dieser Anweisungen](../../virtual-machines/windows/detach-disk.md) vorhandene Datenträger von Ihrem Windows 2016-Image.
1. Fügen Sie den Datenträger aus dem vorherigen Schritt an Ihr Windows 2019-Image an, indem Sie [diese Anweisungen](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm) befolgen.

### <a name="upgrade-ubuntu-1604-dsvm"></a>Upgrade einer Ubuntu 1604-DSVM

Es wird empfohlen, vorhandene Ubuntu 16.04-DSVMs auf die [Ubuntu 18.04-DSVM-Edition](./dsvm-ubuntu-intro.md) zu aktualisieren.
