---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 39a013f5f4b587137366147ade77f0be1b353c4c
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204625"
---
- Inkrementelle Momentaufnahmen können derzeit nicht zwischen Abonnements verschoben werden.
- Sie können zurzeit nur SAS-URIs mit jeweils bis zu fünf Momentaufnahmen einer bestimmten Momentaufnahmefamilie generieren.
- Sie können keine inkrementelle Momentaufnahme für einen bestimmten Datenträger außerhalb des Abonnements dieses Datenträgers erstellen.
- Bis zu sieben inkrementelle Momentaufnahmen pro Datenträger können alle fünf Minuten erstellt werden.
- Für einen einzelnen Datenträger können insgesamt 200 inkrementelle Momentaufnahmen erstellt werden.
- Sie können die Änderungen zwischen erstellten Momentaufnahmen nicht abrufen, wenn zwischen diesen eine Änderung der Größe des übergeordneten Datenträgers vorgenommen wurde, die die Grenze von 4 TB überschreitet. Sie müssen die vollständige Kopie der Momentaufnahme, die nach der Größenänderung erstellt wurde, wieder herunterladen. Anschließend können Sie die Änderungen abrufen, die zwischen den erstellten Momentaufnahmen vorgenommen wurden, nachdem die Grenze von 4 TB bei einer Größenänderung überschritten wurde. 
