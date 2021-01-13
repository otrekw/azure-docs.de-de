---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/27/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e96d205ef1a8f94baa3a0cfe6c5127b6cf570e5a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "80420954"
---
In diesem Artikel wird erläutert, wie Sie mithilfe von AzCopy eine VHD von Ihrem lokalen Computer auf einen verwalteten Azure-Datenträger hochladen oder einen verwalteten Datenträger in eine andere Region kopieren. Im Rahmen dieses Vorgangs (des direkten Uploads) können Sie auch eine VHD mit einer Größe von bis zu 32 TiB direkt auf einen verwalteten Datenträger hochladen. Der direkte Upload wird derzeit für verwaltete Datenträger des Typs HDD Standard, SSD Standard und SSD Premium unterstützt. Für Ultra-Datenträger wird er noch nicht unterstützt.

Wenn Sie eine Sicherungslösung für IaaS-VMs in Azure bereitstellen, sollten Sie einen direkten Upload verwenden, um Kundensicherungen auf verwalteten Datenträgern wiederherzustellen. Beim Hochladen einer VHD von einem Computer, der sich außerhalb von Azure befindet, hängt die Geschwindigkeit von der lokalen Bandbreite ab. Beim Hochladen oder Kopieren von einer Azure-VM entspricht die Bandbreite der von Standard-HDDs.