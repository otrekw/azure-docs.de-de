---
title: Aktivieren von freigegebenen Datenträgern für verwaltete Azure-Datenträger
description: Hier erfahren Sie, wie Sie einen verwalteten Azure-Datenträger mit freigegebenen Datenträgern (Vorschau) konfigurieren, um ihn für mehrere virtuelle Computer freigeben zu können.
author: roygara
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 0dc7cf6e3f72ff9fa7ef5d7cbd85c1fb93f76f9a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146770"
---
# <a name="enable-shared-disk"></a>Aktivieren freigegebener Datenträger

In diesem Artikel erfahren Sie, wie Sie freigegebene Datenträger (Vorschaufeature) für verwaltete Azure-Datenträger aktivieren. Freigegebene Azure-Datenträger (Vorschau) sind ein neues Feature für verwaltete Azure-Datenträger, mit dem Sie einen verwalteten Datenträger gleichzeitig an mehrere virtuelle Computer (Virtual Machines, VMs) anfügen können. Durch das Anfügen eines verwalteten Datenträgers an mehrere virtuelle Computer können Sie entweder neue gruppierte Anwendungen in Azure bereitstellen oder bereits vorhandene gruppierte Anwendungen zu Azure migrieren. 

Informationen zum Konzept verwalteter Datenträger mit aktivierten freigegebenen Datenträgern finden Sie unter [Freigegebene Azure-Datenträger](disks-shared.md).
[!INCLUDE [virtual-machines-enable-shared-disk](../../../includes/virtual-machines-enable-shared-disk.md)]