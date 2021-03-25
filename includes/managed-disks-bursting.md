---
title: include file
description: include file
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 04/27/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 0f85f2770b98507618d5cbfdec1b55f397c7f514
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101750579"
---
Azure bietet die Möglichkeit, die IOPS- und MB/s-Leistung für Datenträger zu steigern. Dies wird sowohl bei virtuellen Computern als auch bei Datenträgern als „Bursting“ bezeichnet. Das Bursting für virtuelle Computer und Datenträger lässt sich effektiv nutzen, um sowohl auf Ihren virtuellen Computern als auch auf dem Datenträger eine bessere Bursting-Leistung zu erzielen.

Das Bursting für virtuelle Azure-Computer ist unabhängig vom Bursting für Datenträgerressourcen. Sie benötigen keinen virtuellen Computer mit Burst-Unterstützung, um Bursting für einen geeigneten angefügten Datenträger nutzen zu können. Analog dazu muss an einen virtuellen Computer mit Burst-Unterstützung kein Datenträger mit Burst-Unterstützung angefügt sein, um Bursting für den virtuellen Computer nutzen zu können.