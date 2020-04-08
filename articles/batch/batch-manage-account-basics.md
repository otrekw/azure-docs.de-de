---
title: 'Verwalten Ihres Kontos: Azure Batch | Microsoft-Dokumentation'
description: Erfahren Sie, was ein Azure Batch-Konto umfasst.
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ecae47f6aa0ab3f179632467b7da7805f06162d6
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397262"
---
# <a name="manage-your-batch-account"></a>Verwalten Ihres Batch-Kontos

Ein Batch-Konto ist eine eindeutig identifizierte Entität innerhalb des Batch-Diensts. Die gesamte Verarbeitung ist einem Batch-Konto zugeordnet.

Ein Azure Batch-Konto können Sie über das [Azure-Portal](batch-account-create-portal.md) oder programmgesteuert (beispielsweise mit der [Batch Management .NET-Bibliothek ](batch-management-dotnet.md)) erstellen. Bei der Kontoerstellung können Sie ein Azure-Speicherkonto zur Speicherung auftragsbezogener Eingabe- und Ausgabedaten oder Anwendungen zuordnen.

Sie können mehrere Batch-Workloads in einem Batch-Konto ausführen oder Ihre Workloads auf Batch-Konten in demselben Abonnement, aber verschiedenen Azure-Regionen aufteilen.

## <a name="components-of-the-batch-account"></a>Komponenten des Batch-Kontos

Das Batch-Konto ermöglicht Ihnen die effiziente Ausführung umfangreicher paralleler und HPC-Batchaufträge (High Performance Computing) in Azure. Innerhalb des Kontos verwalten Sie Folgendes:

- Die Anwendungen, die Sie ausführen

- Die Zuteilung von Pools und Knoten innerhalb von Pools

- Die Anzahl und Typen von Aufgaben 

- Die Ein- und Ausgabe von Daten Sie müssen keine zusätzliche Software installieren, um Aufgaben zu verwalten.

- Wenn Sie das Batch-Konto erstellen, werden Sie aufgefordert, ihm einen Namen zuzuweisen. Dieser Name ist seine ID und kann nach der Vergabe nicht mehr geändert werden.

- Um den Namen eines Kontos zu ändern, müssen Sie es löschen und ein neues Batch-Konto erstellen.

- Das Konto wird innerhalb des zu verwendenden Abonnements erstellt.

- Verwenden Sie das Konto, um primäre und sekundäre Kontoschlüssel von jedem Batch-Konto innerhalb Ihres Abonnements zu identifizieren und abzurufen.

- Das Konto enthält Informationen über die Poolzuteilung und die Kernkontingente.  

- Das Konto enthält Standortinformationen.

- Das Konto identifiziert Ihr Speicherkonto.

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie im [Azure-Portal](batch-account-create-portal.md) ein Batch-Konto.
- Erstellen Sie programmgesteuert ein Batch-Konto, z. B. mit der [Batch Management .NET-Bibliothek](batch-management-dotnet.md).
- [Konfigurieren oder Deaktivieren Sie den Remotezugriff auf Computeknoten in einem Azure Batch-Pool](pool-endpoint-configuration.md).
- [Ausführen von Tasks zum Vorbereiten und Freigeben von Aufträgen auf Azure Batch-Computeknoten](batch-job-prep-release.md)

