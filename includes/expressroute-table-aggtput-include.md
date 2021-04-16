---
title: include file
description: include file
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: include
ms.date: 12/05/2019
ms.author: duau
ms.custom: include file
ms.openlocfilehash: b1465b9bf855d592d195277b616c860a23c179f6
ms.sourcegitcommit: c2a41648315a95aa6340e67e600a52801af69ec7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106504686"
---
| Gateway-SKU | Verbindungen pro Sekunde | Anzahl von Flows | Megabits pro Sekunde | Pakete pro Sekunde | Leitungsbandbreite | Anzahl der vom Gateway angekündigten Routen (zu MSEE) | Anzahl der vom Gateway gelernten Routen (von MSEE) | Anzahl der virtuellen Computer im virtuellen Netzwerk |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Basic-SKU (veraltet)** | – | – | 500 | – | – | – | – | – |
| **Standard-SKU/ErGw1AZ** | 7\.000 | 400.000 | >1.000 | >100.000 | 1 GBit/s |  500 | 4\.000 | 2\.000 (bei Wartung auf 1.000 reduzieren, anschließend wiederherstellen) | 
| **Hochleistungs-SKU/ErGw2AZ** | 14.000 | 840.000 | >2.000 | 250.000 | 1 GBit/s | 500 | ca. 9.500 (auf 4.000 reduzieren, wenn das virtuelle Netzwerk mehr als 6.500 virtuelle Computer enthält) | 4\.500 |
| **Höchstleistung-SKU/ErGw3AZ** | 16.000 | 950.000 | ca. 10.000 | 1\.000.000 | 1 GBit/s | 500 | ca. 9.500 | 11.000 |
