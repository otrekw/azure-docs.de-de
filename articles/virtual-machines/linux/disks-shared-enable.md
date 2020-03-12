---
title: Aktivieren von freigegebenen Datenträgern für verwaltete Azure-Datenträger
description: Hier erfahren Sie, wie Sie einen verwalteten Azure-Datenträger mit freigegebenen Datenträgern (Vorschau) konfigurieren, um ihn für mehrere virtuelle Computer freigeben zu können.
author: roygara
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 0e925abf151abc1a229b57bb035775430fa5332f
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "78970376"
---
# <a name="enable-shared-disk"></a>Aktivieren freigegebener Datenträger

In diesem Artikel erfahren Sie, wie Sie freigegebene Datenträger (Vorschaufeature) für verwaltete Azure-Datenträger aktivieren. Freigegebene Azure-Datenträger (Vorschau) sind ein neues Feature für verwaltete Azure-Datenträger, mit dem Sie einen verwalteten Datenträger gleichzeitig an mehrere virtuelle Computer (Virtual Machines, VMs) anfügen können. Durch das Anfügen eines verwalteten Datenträgers an mehrere virtuelle Computer können Sie entweder neue gruppierte Anwendungen in Azure bereitstellen oder bereits vorhandene gruppierte Anwendungen zu Azure migrieren. 

Informationen zum Konzept verwalteter Datenträger mit aktivierten freigegebenen Datenträgern finden Sie unter [Freigegebene Azure-Datenträger](disks-shared.md).
[!INCLUDE [virtual-machines-enable-shared-disk](../../../includes/virtual-machines-enable-shared-disk.md)]