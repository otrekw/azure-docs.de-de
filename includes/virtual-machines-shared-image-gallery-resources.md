---
title: include file
description: include file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/04/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: bf01df2307796831dee602dad30455a4922ef90b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98873184"
---
| Resource | BESCHREIBUNG|
|----------|------------|
| **Imagequelle** | Dies ist eine Ressource, die zum Erstellen einer **Imageversion** in einem Imagekatalog verwendet werden kann. Eine Imagequelle kann eine vorhandene Azure-VM, die entweder [generalisiert oder spezialisiert](../articles/virtual-machines/shared-image-galleries.md#generalized-and-specialized-images) ist, ein verwaltetes Image, eine Momentaufnahme oder eine Imageversion in einem anderen Imagekatalog sein. |
| **Imagekatalog** | Wie der Azure Marketplace ist ein **Imagekatalog** ein Repository zum Verwalten und Teilen von Images, aber Sie kontrollieren, wer Zugriff hat. |
| **Imagedefinition** | Imagedefinitionen werden in einem Katalog erstellt und enthalten Informationen zum Image und zu den Anforderungen für seine interne Verwendung. Dies schließt ein, ob das Image Windows oder Linux ist, Anmerkungen zu dieser Version und Anforderungen an den minimalen und maximalen Arbeitsspeicher. Es ist eine Definition eines Imagetyps. |
| **Imageversion** | Eine **Imageversion** ist, was Sie verwenden, um einen virtuellen Computer zu erstellen, wenn Sie einen Katalog verwenden. Sie können nach Bedarf mehrere Versionen eines Images für Ihre Umgebung haben. Wie bei einem verwalteten Image wird, wenn Sie eine **Imageversion** zum Erstellen einer VM verwenden, wird die Imageversion verwendet, um neue Datenträger für den virtuellen Computer zu erstellen. Imageversionen können mehrmals verwendet werden. |