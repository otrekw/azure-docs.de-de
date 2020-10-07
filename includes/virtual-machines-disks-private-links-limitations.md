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
ms.openlocfilehash: 293f0f459e1f1e464fdec16b76eaf08336c92e93
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91377461"
---
- Mit einem Datenträgerzugriffsobjekt kann nur ein virtuelles Netzwerk verknüpft werden.
- Ihr virtuelles Netzwerk muss sich im gleichen Abonnement befinden wie das Datenträgerzugriffsobjekt, damit die beiden verknüpft werden können.
- Mit demselben Datenträgerzugriffsobjekt können bis zu zehn Datenträger oder Momentaufnahmen gleichzeitig importiert oder exportiert werden.
- Sie können keine manuelle Genehmigung anfordern, um ein virtuelles Netzwerk mit einem Datenträgerzugriffsobjekt zu verknüpfen.
- Inkrementelle Momentaufnahmen können nicht exportiert werden, wenn sie einem Datenträgerzugriffsobjekt zugeordnet sind.
- Sie können AzCopy nicht zum Herunterladen der VHD eines Datenträgers oder einer Momentaufnahme verwenden, der bzw. die über private Links in einem Speicherkonto gesichert wird. Sie können mit AzCopy jedoch eine VHD auf Ihre virtuellen Computer herunterladen.
