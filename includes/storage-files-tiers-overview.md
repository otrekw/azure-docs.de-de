---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f4e5c59a5ce7c0d743a574309bb1b9276ce80bea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597806"
---
Azure Files bietet zwei verschiedene Speicherebenen (Premium und Standard), sodass Sie Ihre Freigaben an die Leistungs- und Preisanforderungen Ihres jeweiligen Szenarios anpassen können:

- **Premium-Dateifreigaben:** Premium-Dateifreigaben basieren auf Solid State Drives (SSDs) und werden in **Speicherkonten vom Typ „FileStorage“** bereitgestellt. Premium-Dateifreigaben bieten konsistent hohe Leistung und niedrige Latenz im einstelligen Millisekundenbereich für die meisten E/A-Vorgänge für Workloads mit besonders umfassenden E/A. Dadurch sind sie für eine Vielzahl von Workloads wie Datenbanken, Websitehosting und Entwicklungsumgebungen geeignet. Premium-Dateifreigaben sind nur in einem Abrechnungsmodell nach Bereitstellung verfügbar. Weitere Informationen zum bereitgestellten Abrechnungsmodell für Premium-Dateifreigaben finden Sie unter [Grundlegendes zur Bereitstellung für Premium-Dateifreigaben](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares).
- **Standarddateifreigaben:** Standarddateifreigaben basieren auf Festplattenlaufwerken (Hard Disk Drives, HDDs) und werden in **Speicherkonten vom Typ „Allgemein v2 (GPv2)“** bereitgestellt. Standard-Dateifreigaben bieten eine zuverlässige Leistung für E/A-Workloads, die weniger anfällig für Leistungsschwankungen sind, z. B. universelle Dateifreigaben und Dev/Test-Umgebungen. Standard-Dateifreigaben sind nur in einem nutzungsbasierten Abrechnungsmodell verfügbar.