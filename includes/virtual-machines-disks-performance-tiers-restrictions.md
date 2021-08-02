---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/13/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: d5915d8628254f24343571c1adc254c548558415
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110077702"
---
- Dieses Feature wird derzeit nur für SSD Premium unterstützt.
- Sie müssen entweder die Zuordnung des virtuellen Computers aufheben oder den Datenträger von einem ausgeführten virtuellen Computer trennen, damit Sie die Stufe des Datenträgers ändern können.
- Die Leistungsstufen P60, P70 und P80 können nur von Datenträgern verwendet werden, die größer als 4.096 GiB sind.
- Die Leistungsstufe eines Datenträgers kann nur ein Mal alle 12 Stunden herabgestuft werden.

## <a name="change-performance-tier-without-downtime-preview"></a>Ändern der Leistungsstufe ohne Downtime (Vorschau)

Normalerweise müssen Sie die Zuordnung Ihres virtuellen Computers aufheben oder Ihren Datenträger trennen, um die Leistungsstufe zu ändern. Wenn Sie jedoch diese Previewfunktion aktivieren, müssen Sie die Zuordnung Ihres virtuellen Computers nicht aufheben oder Ihren Datenträger trennen, um die Leistungsstufe zu ändern.

Für die Vorschauversion gelten folgende Einschränkungen:
- Sie ist nur in der Region „USA, Westen-Mitte“ verfügbar.
- Sie ist derzeit nicht für freigegebene Datenträger verfügbar.
- Sie muss eine der folgenden Methoden verwenden, um die Stufe ohne Downtime zu wechseln:
    - Azure Resource Manager-Vorlagen mit der `2020-12-01`-API, um Leistungsstufen ohne Downtime zu ändern
    - Aufrufen des Azure-Portals über den folgenden Link: [https://aka.ms/diskPerfTiersPreview](https://aka.ms/diskPerfTiersPreview)
    - Die aktuelle Version der Azure CLI
- Sie ist derzeit mit dem Azure PowerShell-Modul nicht verfügbar.