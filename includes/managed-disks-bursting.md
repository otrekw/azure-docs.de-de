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
ms.openlocfilehash: 2af8b429b6addf6da32b34773525c51a36624e78
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85242223"
---
In Azure bieten wir sowohl bei virtuellen Computern als auch bei Datenträgern die Möglichkeit, die IOPS- und MB/s-Leistung für virtuelle Computer und für Datenträger zu erhöhen, was als Bursting bezeichnet wird. Das Bursting ist in vielen Szenarien nützlich, z. B. bei der Behandlung von unerwartetem Datenverkehr auf Datenträgern oder bei der Verarbeitung von Batchaufträgen. Sie können das Bursting auf VM- und Festplattenebene effektiv nutzen, um sowohl auf Ihrem virtuellen Computer als auch auf dem Datenträger eine hervorragende Baseline- und Bursting-Leistung zu erzielen. Auf diese Weise können Sie sowohl auf Ihrem virtuellen Computer als auch auf dem Datenträger eine hervorragende Baseline- und Bursting-Leistung erzielen. 

Beachten Sie, dass Bursting auf Datenträgern und VMs unabhängig voneinander erfolgt. Wenn Sie über einen Burstingdatenträger verfügen, benötigen Sie keine Bursting-VM, um Bursting des Datenträgers zuzulassen. Wenn Sie über eine Bursting-VM verfügen, benötigen Sie keinen Burstingdatenträger, um Bursting der VM zuzulassen. 