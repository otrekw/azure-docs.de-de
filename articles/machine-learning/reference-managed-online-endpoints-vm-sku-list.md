---
title: 'Verwaltete Onlineendpunkte: Liste der VM-SKUs (Vorschau)'
titleSuffix: Azure Machine Learning
description: Hier sind die VM-SKUs aufgeführt, die für verwaltete Onlineendpunkte (Vorschau) in Azure Machine Learning verwendet werden können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.reviewer: laobri
ms.author: seramasu
author: rsethur
ms.custom: devplatv2
ms.date: 05/10/2021
ms.openlocfilehash: 79711c803decdb2397701de7fd064639799e11aa
ms.sourcegitcommit: 8000045c09d3b091314b4a73db20e99ddc825d91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2021
ms.locfileid: "122445376"
---
# <a name="managed-online-endpoints-sku-list-preview"></a>SKU-Liste für verwaltete Onlineendpunkte (Vorschau)

[!INCLUDE [preview disclaimer](../../includes/machine-learning-preview-generic-disclaimer.md)]

Diese Tabelle enthält die VM-SKUs, die für verwaltete Azure Machine Learning-Onlineendpunkte (Vorschau) unterstützt werden.

* Das für die Bereitstellung verwendete Attribut `instance_type` muss im Format „Standard_F4s_v2“ angegeben werden. In der folgenden Tabelle sind Instanznamen aufgeführt, etwa „F2s v2“. Diese Namen müssen für Anforderungen der Azure CLI oder ARM-Vorlagen (Azure Resource Manager) zum Erstellen und Aktualisieren von Bereitstellungen im vorgegebenen Format (`Standard_{name}`) angegeben werden. 

* Weitere Informationen zu Konfigurationsdetails wie CPU und RAM finden Sie unter [Azure Machine Learning-Preise](https://azure.microsoft.com/pricing/details/machine-learning/).

| Size | Universell | Berechnungsoptimiert |  GPU |
| --- | --- | --- | --- | --- | 
| V.Small | DS2 v2 | F2s v2 | NC4as_T4_v3 |
| Klein | DS3 v2 | F4s v2 | NC6s v2 <br/> NC6s v3 <br/> NC8as_T4_v3 |
| Medium | DS4 v2 | F8s v2 | NC12s v2 <br/> NC12s v3 <br/> NC16as_T4_v3 |
| Groß | DS5 v2 | F16s v2 | NC24s v2 <br/> NC24s v3 <br/> NC64as_T4_v3 |
| XL| - | F32s v2 <br/> F48s v2 <br/> F64s v2 <br/> F72s v2 | - |


