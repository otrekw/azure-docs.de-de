---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/18/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7151c110fd50f7485aa0b130832aace4f3143ad9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101750761"
---
- Dieses Feature wird derzeit nur für SSD Premium unterstützt.
- Sie müssen entweder die Zuordnung des virtuellen Computers aufheben oder den Datenträger von einem ausgeführten virtuellen Computer trennen, damit Sie die Stufe des Datenträgers ändern können.
- Die Leistungsstufen P60, P70 und P80 können nur von Datenträgern verwendet werden, die größer als 4.096 GiB sind.
- Die Leistungsstufe eines Datenträgers kann nur ein Mal alle 12 Stunden herabgestuft werden.

## <a name="change-performance-tier-without-downtime-preview"></a>Ändern der Leistungsstufe ohne Downtime (Vorschau)

Normalerweise müssen Sie die Zuordnung Ihres virtuellen Computers aufheben oder Ihren Datenträger trennen, um die Leistungsstufe zu ändern. Wenn Sie jedoch diese Previewfunktion aktivieren, müssen Sie die Zuordnung Ihres virtuellen Computers nicht aufheben oder Ihren Datenträger trennen, um die Leistungsstufe zu ändern. [Hier](https://aka.ms/liveperftiersignup) können Sie sich für die Vorschauversion registrieren.

Für die Vorschauversion gelten folgende Einschränkungen:
- Nur in der Region „EastUS2EUAP“ verfügbar
- Aktuell nicht für freigegebene Datenträger verfügbar
- Erfordert die Verwendung von Azure Resource Manager-Vorlagen mit der API `2020-12-01`, um Leistungsstufen ohne Downtime ändern zu können