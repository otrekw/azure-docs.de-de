---
title: 'Entfernen von „VMs starten/beenden v2 (Vorschau)“: Übersicht'
description: In diesem Artikel wird beschrieben, wie Sie das Feature „VMs starten/beenden v2 (Vorschau)“ entfernen.
services: azure-functions
ms.subservice: start-stop-vms
ms.date: 03/30/2021
ms.topic: conceptual
ms.openlocfilehash: fd24de679781cda37ba9489eb7acad6332896659
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110076850"
---
# <a name="how-to-remove-startstop-vms-v2-preview"></a>Vorgehensweise zum Entfernen von „VMs starten/beenden v2 (Vorschau)“

Nachdem das Feature „VMs starten/beenden v2 (Vorschau)“ aktiviert wurde, um den Betriebszustand Ihrer Azure-VMs zu verwalten, entscheiden Sie möglicherweise, es nicht mehr zu verwenden. Sie können diese Funktion entfernen, indem Sie die Ressourcengruppe löschen, die dediziert für die Speicherung der folgenden Ressourcen vorgesehen ist, um „VMs starten/beenden v2 (Vorschau)“ zu unterstützen:

- Die Azure Functions-Anwendungen
- Zeitpläne in Azure Logic Apps
- Die Application Insights-Instanz
- Azure Storage-Konto

## <a name="delete-the-dedicated-resource-group"></a>Löschen der dedizierten Ressourcengruppe

Um die Ressourcengruppe zu löschen, befolgen Sie die Schritte, die im Artikel [Azure Resource Manager-Ressourcengruppe und Löschen von Ressourcen](../../azure-resource-manager/management/delete-resource-group.md) beschrieben sind.

## <a name="next-steps"></a>Nächste Schritte

Informationen, wie Sie diese Funktion erneut bereitstellen, finden Sie unter [Starten/Beenden v2 bereitstellen](deploy.md) (Vorschau).