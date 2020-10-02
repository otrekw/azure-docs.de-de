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
ms.openlocfilehash: e75cb7d13fb74d32191ab7f076d73ad66976503d
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90606758"
---
Azure Files bietet vier verschiedene Speicherebenen (Premium, transaktionsoptimiert, heiße Ebene und kalte Ebene), sodass Sie Ihre Freigaben an die Leistungs- und Preisanforderungen Ihres jeweiligen Szenarios anpassen können:

- **Premium**: Premium-Dateifreigaben basieren auf Solid State Drives (SSDs) und werden in **Speicherkonten vom Typ „FileStorage“** bereitgestellt. Premium-Dateifreigaben bieten konsistent hohe Leistung und niedrige Latenz im einstelligen Millisekundenbereich für die meisten E/A-Vorgänge für Workloads mit besonders umfassenden E/A. Premium-Dateifreigaben sind für eine Vielzahl von Workloads wie Datenbanken, Websitehosting und Entwicklungsumgebungen geeignet. Sie können mit SMB- und NFS-Protokollen (Server Message Block bzw. Network File System) verwendet werden.
- **Transaktionsoptimiert**: Transaktionsoptimierte Dateifreigaben ermöglichen die Verwendung transaktionsintensiver Workloads, die nicht auf die Wartezeit von Premium-Dateifreigaben angewiesen sind. Transaktionsoptimierte Dateifreigaben werden auf Standardspeicherhardware mit Festplattenlaufwerken (Hard Disk Drives, HDDs) angeboten und in **Speicherkonten vom Typ „Allgemein v2 (GPv2)“** bereitgestellt. „Transaktionsoptimiert“ wurde in der Vergangenheit „Standard“ genannt. Dies bezieht sich jedoch auf den Speichermedientyp und nicht auf die Ebene selbst. (Die heiße und die kalte Ebene sind ebenfalls Standardebenen, da sie auf Standardspeicherhardware basieren.)
- **Hot**: Heiße Dateifreigaben bieten Speicher, der für universelle Dateifreigabeszenarien optimiert ist (also beispielsweise für Teamfreigaben und für die Azure-Dateisynchronisierung). Heiße Dateifreigaben werden auf Standardspeicherhardware mit HDDs angeboten und in **Speicherkonten vom Typ „Allgemein v2 (GPv2)“** bereitgestellt.
- **Cool**: Kalte Dateifreigaben bieten kostengünstigen Speicher, der für Speicherszenarien mit Onlinearchiv optimiert ist. Bei Workloads mit wenig Änderungen kann auch die Azure-Dateisynchronisierung gut geeignet sein. Kalte Dateifreigaben werden auf Standardspeicherhardware mit HDDs angeboten und in **Speicherkonten vom Typ „Allgemein v2 (GPv2)“** bereitgestellt.

Premium-Dateifreigaben sind nur in einem Abrechnungsmodell nach Bereitstellung verfügbar. Weitere Informationen zum bereitgestellten Abrechnungsmodell für Premium-Dateifreigaben finden Sie unter [Grundlegendes zur Bereitstellung für Premium-Dateifreigaben](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares). Standarddateifreigaben (einschließlich transaktionsoptimierter, heißer und kalter Dateifreigaben) werden mit nutzungsbasierter Bezahlung angeboten.

Heiße und kalte Dateifreigaben sind in allen öffentlichen Azure-Regionen und allen Azure Government-Regionen verfügbar. Transaktionsoptimierte Dateifreigaben sind in allen Azure-Regionen verfügbar, einschließlich Azure China und Azure Deutschland.

Informationen zum Bereitstellen einer heißen oder kalten Dateifreigabe finden Sie unter [Erstellen einer heißen oder kalten Dateifreigabe](../articles/storage/files/storage-how-to-create-file-share.md#create-a-hot-or-cool-file-share). 