---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/26/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a67ad4c5010cf93ff55123013a35c697ce5971f8
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77597810"
---
Azure-Dateifreigaben werden in *Speicherkonten* bereitgestellt, bei denen es sich um Objekte der obersten Ebene handelt, die einen freigegebenen Speicherpool darstellen. Dieser Speicherpool kann verwendet werden, um mehrere Dateifreigaben sowie andere Speicherressourcen wie Blobcontainer, Warteschlangen oder Tabellen bereitzustellen. Für alle Speicherressourcen, die in einem Speicherkonto bereitgestellt werden, treffen die für dieses Speicherkonto geltenden Grenzwerte zu. Die aktuellen Grenzwerte für ein Speicherkonto finden Sie unter [Azure Files-Skalierbarkeits- und -Leistungsziele](../articles/storage/files/storage-files-scale-targets.md).

Es gibt zwei Haupttypen von Speicherkonten, die Sie für Azure Files-Bereitstellungen verwenden: 
- **Speicherkonten vom Typ Universell Version 2 (GPv2)** : GPv2-Speicherkonten ermöglichen Ihnen die Bereitstellung von Azure-Dateifreigaben auf Standard- bzw. festplattenbasierter Hardware (HDD-basiert). Neben Azure-Dateifreigaben können in GPv2-Speicherkonten auch andere Speicherressourcen wie Blobcontainer, Warteschlangen oder Tabellen gespeichert werden. 
- **FileStorage-Speicherkonten**: FileStorage-Speicherkonten ermöglichen Ihnen die Bereitstellung von Azure-Dateifreigaben auf SSD-basierter Hardware (Premium/ Solid State Drive). FileStorage-Konten können nur zum Speichern von Azure-Dateifreigaben verwendet werden. In einem FileStorage-Konto können keine anderen Speicherressourcen (Blobcontainer, Warteschlangen, Tabellen usw.) bereitgestellt werden.

Es gibt mehrere andere Speicherkontotypen, die Sie möglicherweise im Azure-Portal, in PowerShell oder in der CLI finden. Zwei Speicherkontotypen (BlockBlobStorage- und BlobStorage-Speicherkonten) dürfen keine Azure-Dateifreigaben enthalten. Die anderen beiden Speicherkontotypen, die Sie möglicherweise sehen, sind Universell Version 1 (GPv1) und klassische Speicherkonten. Beide können Azure-Dateifreigaben enthalten. Obwohl GPv1 und klassische Speicherkonten Azure-Dateifreigaben enthalten können, sind die meisten neuen Features von Azure Files nur in GPv2- und FileStorage-Speicherkonten verfügbar. Daher empfiehlt es sich, für neue Bereitstellungen nur GPv2- und FileStorage-Speicherkonten zu verwenden und GPv1- und klassische Speicherkonten zu aktualisieren, wenn diese bereits in Ihrer Umgebung vorhanden sind.  