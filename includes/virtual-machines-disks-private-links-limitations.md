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
ms.openlocfilehash: 138e7efd33ee36a77fd56deda155a29017432302
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87420283"
---
- Mit einem Datenträgerzugriffsobjekt kann nur ein virtuelles Netzwerk verknüpft werden.
- Ihr virtuelles Netzwerk muss sich im gleichen Abonnement befinden wie das Datenträgerzugriffsobjekt, damit die beiden verknüpft werden können.
- Mit demselben Datenträgerzugriffsobjekt können bis zu zehn Datenträger oder Momentaufnahmen gleichzeitig importiert oder exportiert werden.
- Sie können keine manuelle Genehmigung anfordern, um ein virtuelles Netzwerk mit einem Datenträgerzugriffsobjekt zu verknüpfen.
- Die differenzielle Funktion wird für inkrementelle Momentaufnahmen nicht unterstützt, die einem Datenträgerzugriffsobjekt zugeordnet sind.
- Sie können AzCopy nicht zum Herunterladen der VHD eines Datenträgers oder einer Momentaufnahme verwenden, der bzw. die über private Links in einem Speicherkonto gesichert wird. Sie können mit AzCopy jedoch eine VHD auf Ihre virtuellen Computer herunterladen.
