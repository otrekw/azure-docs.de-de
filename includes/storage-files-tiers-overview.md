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
ms.openlocfilehash: 7fd91e898c12a13e35ae8b9055ebb5a57de2a051
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89272147"
---
Azure Files bietet vier verschiedene Speicherebenen (Premium, transaktionsoptimiert, heiße Ebene und kalte Ebene), sodass Sie Ihre Freigaben an die Leistungs- und Preisanforderungen Ihres jeweiligen Szenarios anpassen können:

- **Premium**: Premium-Dateifreigaben basieren auf Solid State Drives (SSDs) und werden in **Speicherkonten vom Typ „FileStorage“** bereitgestellt. Premium-Dateifreigaben bieten konsistent hohe Leistung und niedrige Latenz im einstelligen Millisekundenbereich für die meisten E/A-Vorgänge für Workloads mit besonders umfassenden E/A. Dadurch sind sie für eine Vielzahl von Workloads wie Datenbanken, Websitehosting und Entwicklungsumgebungen geeignet. 
- **Transaktionsoptimiert**: Transaktionsoptimierte Dateifreigaben ermöglichen die Verwendung transaktionsintensiver Workloads, die nicht auf die Wartezeit von Premium-Dateifreigaben angewiesen sind. Transaktionsoptimierte Dateifreigaben werden auf Standardspeicherhardware mit Festplattenlaufwerken (Hard Disk Drives, HDDs) angeboten und in **Speicherkonten vom Typ „Allgemein v2 (GPv2)“** bereitgestellt. Diese Speicherebene wurde in der Vergangenheit „Standard“ genannt. Dies bezieht sich jedoch auf den Speichermedientyp und nicht auf die Ebene selbst. (Die heiße und die kalte Ebene sind ebenfalls Standardebenen, da sie auf Standardspeicherhardware basieren.)
- **Hot**: Heiße Dateifreigaben bieten Speicher, der für universelle Dateifreigabeszenarien optimiert ist (also beispielsweise für Teamfreigaben und für die Azure-Dateisynchronisierung). Heiße Dateifreigaben werden auf Standardspeicherhardware mit HDDs angeboten und in **Speicherkonten vom Typ „Allgemein v2 (GPv2)“** bereitgestellt.
- **Cool**: Kalte Dateifreigaben bieten kostengünstigen Speicher, der für Speicherszenarien mit Onlinearchiv optimiert ist. Bei Workloads mit wenig Änderungen kann auch die Azure-Dateisynchronisierung gut geeignet sein. Kalte Dateifreigaben werden auf Standardspeicherhardware mit HDDs angeboten und in **Speicherkonten vom Typ „Allgemein v2 (GPv2)“** bereitgestellt.

Premium-Dateifreigaben sind nur in einem Abrechnungsmodell nach Bereitstellung verfügbar. Weitere Informationen zum bereitgestellten Abrechnungsmodell für Premium-Dateifreigaben finden Sie unter [Grundlegendes zur Bereitstellung für Premium-Dateifreigaben](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares). Standarddateifreigaben (einschließlich transaktionsoptimierter, heißer und kalter Dateifreigaben) werden in einem Modell mit nutzungsbasierter Bezahlung angeboten.

Heiße und kalte Dateifreigaben stehen aktuell in den folgenden öffentlichen Regionen zur Verfügung. (Transaktionsoptimierte Dateifreigaben sind in allen Azure-Regionen verfügbar.)

- Australien, Mitte
- Australien, Mitte 2
- Australien (Osten)
- Australien, Südosten
- Brasilien Süd
- Kanada, Osten
- Kanada, Mitte
- Frankreich, Mitte
- Frankreich, Süden
- Deutschland, Norden (öffentlich)
- Deutschland, Westen-Mitte (öffentlich)
- Indien, Mitte
- Indien, Süden
- Indien, Westen
- Japan, Osten
- Japan, Westen
- Korea, Mitte
- Korea, Süden
- Norwegen, Osten
- Norwegen, Westen
- Südafrika, Norden
- Südafrika, Westen
- Schweiz, Norden
- Schweiz, Westen
- VAE, Mitte
- Vereinigte Arabische Emirate, Norden
- UK, Süden
- UK, Westen
- USA Nord Mitte
- USA Süd Mitte

Informationen zum Bereitstellen einer heißen oder kalten Dateifreigabe finden Sie unter [Erstellen einer heißen oder kalten Dateifreigabe](../articles/storage/files/storage-how-to-create-file-share.md#create-a-hot-or-cool-file-share). 