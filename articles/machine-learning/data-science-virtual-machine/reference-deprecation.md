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
ms.openlocfilehash: d5f541dec14eebc944e4eac11dbe569b38cb277e
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "89001619"
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

