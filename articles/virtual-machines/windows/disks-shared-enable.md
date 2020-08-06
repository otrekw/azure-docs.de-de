---
title: Aktivieren von freigegebenen Datenträgern für verwaltete Azure-Datenträger
description: Konfigurieren eines verwalteten Azure-Datenträgers mit freigegebenen Datenträgern, um ihn für mehrere virtuelle Computer freigeben zu können
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 07/30/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: cc365f6eef4da61ab7de5113b8f4b2b185360740
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460053"
---
# <a name="enable-shared-disk"></a>Aktivieren freigegebener Datenträger

In diesem Artikel erfahren Sie, wie Sie das Feature für freigegebene Datenträger für verwaltete Azure-Datenträger aktivieren. Freigegebene Azure-Datenträger sind ein neues Feature für verwaltete Azure-Datenträger, mit dem Sie einen verwalteten Datenträger gleichzeitig an mehrere virtuelle Computer (Virtual Machines, VMs) anfügen können. Durch das Anfügen eines verwalteten Datenträgers an mehrere virtuelle Computer können Sie entweder neue gruppierte Anwendungen in Azure bereitstellen oder bereits vorhandene gruppierte Anwendungen zu Azure migrieren. 

Informationen zum Konzept verwalteter Datenträger mit aktivierten freigegebenen Datenträgern finden Sie unter [Freigegebene Azure-Datenträger](disks-shared.md).
[!INCLUDE [virtual-machines-enable-shared-disk](../../../includes/virtual-machines-enable-shared-disk.md)]