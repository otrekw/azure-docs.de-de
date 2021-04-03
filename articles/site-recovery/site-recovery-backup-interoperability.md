---
title: Unterstützung für die Verwendung von Azure Site Recovery mit Azure Backup
description: Bietet einen Überblick darüber, wie Azure Site Recovery und Azure Backup zusammen verwendet werden können.
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: sideeksh
ms.openlocfilehash: c334eee34eb878135d3d81ab15d03618c6604846
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "86135180"
---
# <a name="support-for-using-site-recovery-with-azure-backup"></a>Unterstützung für die Verwendung von Site Recovery mit Azure Backup

Dieser Artikel fasst die Unterstützung für die Verwendung von [Site Recovery](site-recovery-overview.md) mit [Azure Backup](../backup/backup-overview.md) zusammen.

**Aktion** | **Site Recovery-Unterstützung** | **Details**
--- | --- | ---
**Gemeinsame Bereitstellung der Dienste** | Unterstützt | Dienste sind interoperabel und können zusammen konfiguriert werden.
**Datensicherung/-wiederherstellung** | Unterstützt | Wenn Sicherung und Replikation für eine VM aktiviert sind und Sicherungen durchgeführt werden, gibt es kein Problem bei der Wiederherstellung von Dateien auf den quellseitigen VMs oder Gruppen von VMs. Die Replikation wird wie gewohnt fortgesetzt, ohne dass sich der Zustand der Replikation ändert.
**Datenträgerwiederherstellung** | Aktuell keine Unterstützung | Wenn Sie einen gesicherten Datenträger wiederherstellen, müssen Sie die Replikation für den virtuellen Computer deaktivieren und wieder aktivieren.
**VM-Wiederherstellung** | Aktuell keine Unterstützung | Wenn Sie einen virtuellen Computer oder eine Gruppe virtueller Computer wiederherstellen, müssen Sie die Replikation für den virtuellen Computer deaktivieren und wieder aktivieren.  


