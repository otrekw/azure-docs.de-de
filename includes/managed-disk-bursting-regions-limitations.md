---
title: include file
description: include file
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 01/04/2021
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 5658b68081fae8dab528030f7fc1cd6fe4935730
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102178358"
---
- Das bedarfsgesteuerte Bursting kann nicht auf einem SSD Premium-Datenträger mit weniger als 512 GiB aktiviert werden. Bei SSD Premium-Datenträgern, die kleiner als 512 GiB sind, wird immer das guthabenbasierte Bursting angewandt.
- Das bedarfsgesteuerte Bursting wird nur bei SSD Premium unterstützt. Wenn ein SSD Premium-Datenträger mit aktiviertem bedarfsgesteuertem Bursting auf einen anderen Datenträgertyp umgestellt wird, wird das Datenträgerbursting deaktiviert.
- Das bedarfsgesteuerte Bursting wird jedoch nicht automatisch deaktiviert, wenn die Leistungsstufe geändert wird. Wenn Sie die Leistungsstufe ändern und auch das Datenträgerbursting deaktivieren möchten, müssen Sie es selbst deaktivieren.
- Das bedarfsgesteuerte Bursting kann nur aktiviert werden, wenn der Datenträger von einer VM getrennt wird oder wenn die VM angehalten wurde. Sie können das bedarfsgesteuerte Bursting 12 Stunden nach der Aktivierung deaktivieren.