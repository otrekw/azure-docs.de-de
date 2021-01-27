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
ms.openlocfilehash: 6d06a46d2eaaad362890f1e3e44dbc746fa10898
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "98633442"
---
Azure Files bietet vier verschiedene Speicherebenen (Premium, transaktionsoptimiert, heiße Ebene und kalte Ebene), sodass Sie Ihre Freigaben an die Leistungs- und Preisanforderungen Ihres jeweiligen Szenarios anpassen können:

- **Premium**: Premium-Dateifreigaben basieren auf SSDs (Solid State Drives), die konsistent hohe Leistung und niedrige Latenz (im einstelligen Millisekundenbereich für die meisten E/A-Vorgänge) für E/A-intensive Workloads bieten. Premium-Dateifreigaben sind für eine Vielzahl von Workloads wie Datenbanken, Websitehosting und Entwicklungsumgebungen geeignet. Sie können mit SMB- und NFS-Protokollen (Server Message Block bzw. Network File System) verwendet werden.
- **Transaktionsoptimiert**: Transaktionsoptimierte Dateifreigaben ermöglichen die Verwendung transaktionsintensiver Workloads, die nicht auf die Wartezeit von Premium-Dateifreigaben angewiesen sind. Transaktionsoptimierte Dateifreigaben werden auf Standardspeicherhardware angeboten, die auf Festplattenlaufwerken (Hard Disk Drives, HDDs) basiert. „Transaktionsoptimiert“ wurde in der Vergangenheit „Standard“ genannt. Dies bezieht sich jedoch auf den Speichermedientyp und nicht auf die Ebene selbst. (Die heiße und die kalte Ebene sind ebenfalls Standardebenen, da sie auf Standardspeicherhardware basieren.)
- **Hot**: Heiße Dateifreigaben verfügen über Speicher, der für universelle Dateifreigabeszenarien optimiert ist, z. B. Teamfreigaben. Heiße Dateifreigaben werden auf HDD-basierter Standardspeicherhardware angeboten.
- **Cool**: Kalte Dateifreigaben bieten kostengünstigen Speicher, der für Speicherszenarien mit Onlinearchiv optimiert ist. Kalte Dateifreigaben werden auf HDD-basierter Standardspeicherhardware angeboten.

Premium-Dateifreigaben werden im **Speicherkonto vom Typ „FileStorage“** bereitgestellt und sind nur in einem bereitgestellten Abrechnungsmodell verfügbar. Weitere Informationen zum bereitgestellten Abrechnungsmodell für Premium-Dateifreigaben finden Sie unter [Grundlegendes zur Bereitstellung für Premium-Dateifreigaben](../articles/storage/files/understanding-billing.md#provisioned-model). Standarddateifreigaben (einschließlich transaktionsoptimierter, heißer und kalter Dateifreigaben) werden im **Speicherkonto vom Typ „Allgemein v2“ (GPv2)** mit nutzungsbasierter Bezahlung bereitgestellt. Heiße und kalte Dateifreigaben sind in allen öffentlichen Azure-Regionen und allen Azure Government-Regionen verfügbar. Transaktionsoptimierte Dateifreigaben sind in allen Azure-Regionen verfügbar, einschließlich Azure China und Azure Deutschland.

Berücksichtigen Sie bei der Wahl einer Speicherebene für Ihre Workload Ihre Leistungs- und Nutzungsanforderungen. Wenn Ihre Workload Latenz im einstelligen Bereich erfordert oder Sie lokale SSD-Speichermedien verwenden, ist wahrscheinlich der Premium-Tarif die beste Wahl. Sollte eine geringe Latenz nicht so wichtig sein (etwa bei Teamfreigaben, die aus Azure lokal eingebunden oder unter Verwendung der Azure-Dateisynchronisierung lokal zwischengespeichert werden), bietet der Standardspeicher unter Umständen ein besseres Preis-Leistungs-Verhältnis.

Nachdem Sie eine Dateifreigabe in einem Speicherkonto erstellt haben, kann sie nicht mehr in für andere Speicherkontoarten exklusive Ebenen verschoben werden. Wenn Sie also beispielsweise eine transaktionsoptimierte Dateifreigabe in die Premium-Ebene verschieben möchten, müssen Sie eine neue Dateifreigabe in einem FileStorage-Speicherkonto erstellen und die Daten aus der ursprünglichen Freigabe in eine neue Dateifreigabe im FileStorage-Konto kopieren. Wir empfehlen die Verwendung von AzCopy, um Daten zwischen Azure-Dateifreigaben zu kopieren. Sie können aber auch Tools wie `robocopy` (Windows) oder `rsync` (macOS und Linux) verwenden. 

Im Rahmen von GPv2-Speicherkonten bereitgestellte Dateifreigaben können zwischen den Standardebenen (transaktionsoptimiert, heiß und kalt) verschoben werden, ohne ein neues Speicherkonto zu erstellen und Daten zu migrieren. Durch die Änderung der Ebene fallen jedoch Transaktionskosten an. Wenn Sie eine Freigabe von einer heißeren in eine kältere Ebene verschieben, werden Ihnen für jede Datei in der Freigabe die Schreibtransaktionskosten der kälteren Ebene berechnet. Wenn Sie eine Dateifreigabe von einer kälteren in eine heißere Ebene verschieben, werden Ihnen für jede Datei in der Freigabe die Lesetransaktionskosten der kalten Ebene berechnet.

Weitere Informationen finden Sie unter [Grundlegendes zur Azure Files-Abrechnung](../articles/storage/files/understanding-billing.md).