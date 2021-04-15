---
title: include file
description: file
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: include
author: emuench
ms.author: mingshen
ms.date: 10/09/2020
ms.openlocfilehash: f4e34e850391696506beed9f6f386f85528dff24
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92283202"
---
Wenn Sie eine der VMs ausgewählt haben, die mit einem Betriebssystem (und optional weiteren Diensten) vorkonfiguriert sind, verfügen Sie bereits über eine Azure-VM mit einer Standardgröße. Ein Starten Ihrer Lösung mit einem vorkonfigurierten Betriebssystem ist die empfohlene Vorgehensweise. Wenn Sie ein Betriebssystem aber manuell installieren, müssen Sie die Größe Ihrer primären VHD in Ihrem VM-Image festlegen. Stellen Sie sicher, dass die Größe des Betriebssystemdatenträgers innerhalb der Grenzwerte für Linux bzw. Windows liegt.

| Betriebssystem | VHD-Größe |
| --- | --- |
| Linux | 30 bis 1023 GB |
| Windows | 30 bis 250 GB |
|

Die als genehmigte Basis bereitgestellten Windows- und SQL Server-Basisimages erfüllen diese Anforderungen. Ändern Sie daher weder das Format noch die Größe der VHD.

Datenträger können bis zu 1 TB groß sein. Bedenken Sie bei der Auswahl der Größe, dass Kunden die Größe der VHDs in einem Image bei der Bereitstellung nicht ändern können. Erstellen Sie Datenträger-VHDs als VHDs mit festem Format. Sie sollten außerdem erweiterbar sein (geringe Dichte/dynamisch). Datenträger können anfänglich leer sein oder Daten enthalten.