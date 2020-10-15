---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 08/28/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7d0286b63703c165dda6cd12bb625fc64272aac1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91377571"
---
Azure Files bietet vier verschiedene Speicherebenen (Premium, transaktionsoptimiert, heiße Ebene und kalte Ebene), sodass Sie Ihre Freigaben an die Leistungs- und Preisanforderungen Ihres jeweiligen Szenarios anpassen können:

- **Premium**: Premium-Dateifreigaben basieren auf Solid State Drives (SSDs) und werden in **Speicherkonten vom Typ „FileStorage“** bereitgestellt. Premium-Dateifreigaben bieten konsistent hohe Leistung und niedrige Latenz im einstelligen Millisekundenbereich für die meisten E/A-Vorgänge für Workloads mit besonders umfassenden E/A. Premium-Dateifreigaben sind für eine Vielzahl von Workloads wie Datenbanken, Websitehosting und Entwicklungsumgebungen geeignet. Sie können mit SMB- und NFS-Protokollen (Server Message Block bzw. Network File System) verwendet werden.
- **Transaktionsoptimiert**: Transaktionsoptimierte Dateifreigaben ermöglichen die Verwendung transaktionsintensiver Workloads, die nicht auf die Wartezeit von Premium-Dateifreigaben angewiesen sind. Transaktionsoptimierte Dateifreigaben werden auf Standardspeicherhardware mit Festplattenlaufwerken (Hard Disk Drives, HDDs) angeboten und in **Speicherkonten vom Typ „Allgemein v2 (GPv2)“** bereitgestellt. „Transaktionsoptimiert“ wurde in der Vergangenheit „Standard“ genannt. Dies bezieht sich jedoch auf den Speichermedientyp und nicht auf die Ebene selbst. (Die heiße und die kalte Ebene sind ebenfalls Standardebenen, da sie auf Standardspeicherhardware basieren.)
- **Hot**: Heiße Dateifreigaben bieten Speicher, der für universelle Dateifreigabeszenarien optimiert ist (also beispielsweise für Teamfreigaben und für die Azure-Dateisynchronisierung). Heiße Dateifreigaben werden auf Standardspeicherhardware mit HDDs angeboten und in **Speicherkonten vom Typ „Allgemein v2 (GPv2)“** bereitgestellt.
- **Cool**: Kalte Dateifreigaben bieten kostengünstigen Speicher, der für Speicherszenarien mit Onlinearchiv optimiert ist. Bei Workloads mit wenig Änderungen kann auch die Azure-Dateisynchronisierung gut geeignet sein. Kalte Dateifreigaben werden auf Standardspeicherhardware mit HDDs angeboten und in **Speicherkonten vom Typ „Allgemein v2 (GPv2)“** bereitgestellt.

Premium-Dateifreigaben sind nur in einem Abrechnungsmodell nach Bereitstellung verfügbar. Weitere Informationen zum bereitgestellten Abrechnungsmodell für Premium-Dateifreigaben finden Sie unter [Grundlegendes zur Bereitstellung für Premium-Dateifreigaben](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares). Standarddateifreigaben (einschließlich transaktionsoptimierter, heißer und kalter Dateifreigaben) werden mit nutzungsbasierter Bezahlung angeboten.

Heiße und kalte Dateifreigaben sind in allen öffentlichen Azure-Regionen und allen Azure Government-Regionen verfügbar. Transaktionsoptimierte Dateifreigaben sind in allen Azure-Regionen verfügbar, einschließlich Azure China und Azure Deutschland.

> [!Important]  
> Sie können Dateifreigaben zwischen Ebenen innerhalb von GPv2-Speicherkontotypen verschieben (transaktionsoptimiert, heiß und kalt). Freigabeverschiebungen zwischen Ebenen verursachen Transaktionen: Beim Verschieben von einer heißeren Ebene in eine kältere Ebene fällt für jede Datei in der Freigabe die Gebühr für Schreibtransaktionen der kälteren Ebene an. Beim Verschieben von einer kälteren Ebene in eine heißere Ebene hingegen fällt für jede Datei in der Freigabe die Gebühr für Lesetransaktionen der kalten Ebene an.