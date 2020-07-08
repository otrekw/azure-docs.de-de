---
title: Aktivieren von freigegebenen Datenträgern für verwaltete Azure-Datenträger
description: Hier erfahren Sie, wie Sie einen verwalteten Azure-Datenträger mit freigegebenen Datenträgern (Vorschau) konfigurieren, um ihn für mehrere virtuelle Computer freigeben zu können.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 06/03/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 3b949905f311b1a8878aa691e32abc3f568e1e6b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84659291"
---
# <a name="enable-shared-disk"></a>Aktivieren freigegebener Datenträger

In diesem Artikel erfahren Sie, wie Sie freigegebene Datenträger (Vorschaufeature) für verwaltete Azure-Datenträger aktivieren. Freigegebene Azure-Datenträger (Vorschau) sind ein neues Feature für verwaltete Azure-Datenträger, mit dem Sie einen verwalteten Datenträger gleichzeitig an mehrere virtuelle Computer (Virtual Machines, VMs) anfügen können. Durch das Anfügen eines verwalteten Datenträgers an mehrere virtuelle Computer können Sie entweder neue gruppierte Anwendungen in Azure bereitstellen oder bereits vorhandene gruppierte Anwendungen zu Azure migrieren. 

Informationen zum Konzept verwalteter Datenträger mit aktivierten freigegebenen Datenträgern finden Sie unter [Freigegebene Azure-Datenträger](disks-shared.md).
[!INCLUDE [virtual-machines-enable-shared-disk](../../../includes/virtual-machines-enable-shared-disk.md)]