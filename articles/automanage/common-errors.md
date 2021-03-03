---
title: Behandeln von häufigen Fehlern mit dem Onboarding von Azure Automanage
description: Häufige Fehler beim Onboarding von Automanage und deren Behandlung
author: asinn826
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: alsin
ms.openlocfilehash: df5133ad4bb3155afdc9d43e595591d9cfda4ea0
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101644441"
---
# <a name="troubleshoot-common-automanage-onboarding-errors"></a>Behandeln von häufigen Fehlern mit dem Onboarding von Automanage
Die Integration eines Computers in den Dienst durch Automanage kann fehlschlagen. In diesem Dokument wird erläutert, wie Sie Bereitstellungsfehler behandeln können, Sie erfahren einige häufige Gründe für das Fehlschlagen von Bereitstellungen, und es werden mögliche nächste Schritte bei der Entschärfung beschrieben.

## <a name="troubleshooting-deployment-failures"></a>Beheben von Problemen bei der Bereitstellung
Das Onboarding eines Computers in Automanage führt dazu, dass eine Azure Resource Manager-Bereitstellung erstellt wird. Schlägt das Onboarding fehl, kann es hilfreich sein, die Bereitstellung auf weitere Details zur Ursache für das Fehlschlagen zu untersuchen. In dem unten dargestellten Fehlerdetails-Flyout finden Sie Links zu den Bereitstellungen.

:::image type="content" source="media\automanage-common-errors\failure-flyout.png" alt-text="Fehlerdetail-Flyout von Automanage.":::

### <a name="check-the-deployments-for-the-resource-group-containing-the-failed-vm"></a>Überprüfen der Bereitstellungen auf die Ressourcengruppe, die die fehlgeschlagene VM enthält
Das Fehlerflyout enthält einen Link zu den Bereitstellungen innerhalb der Ressourcengruppe, die den Computer enthält, dessen Onboarding fehlgeschlagen ist, sowie einen Präfixnamen, nach dem Sie Bereitstellungen filtern können. Wenn Sie auf den Link klicken, gelangen Sie zum Blatt der Bereitstellungen, auf dem Sie die Bereitstellungen filtern können, um die Automanage-Bereitstellungen auf Ihrem Computer anzuzeigen. Wenn Sie in mehreren Regionen bereitstellen, sollten Sie sicherstellen, dass Sie auf die Bereitstellung in der richtigen Region klicken.

### <a name="check-the-deployments-for-the-subscription-containing-the-failed-vm"></a>Überprüfen der Bereitstellungen auf das Abonnement, das die fehlgeschlagene VM enthält
Wenn Sie in der Ressourcengruppenbereitstellung keine Fehler finden können, besteht Ihr nächster Schritt darin, die Bereitstellungen in Ihrem Abonnement zu untersuchen, das den virtuellen Computer enthält, dessen Onboarding fehlgeschlagen ist. Klicken Sie auf den Link **Bereitstellungen für Abonnement** im Fehlerflyout, und filtern Sie Bereitstellungen mithilfe des Filters **Automanage-DefaultResourceGroup**. Verwenden Sie den Ressourcengruppennamen vom Blatt „Fehler“, um Bereitstellungen zu filtern. Dem Bereitstellungsnamen wird ein Regionsname als Suffix angefügt. Wenn Sie in mehreren Regionen bereitstellen, sollten Sie sicherstellen, dass Sie auf die Bereitstellung in der richtigen Region klicken.

### <a name="check-deployments-in-a-subscription-linked-to-a-log-analytics-workspace"></a>Überprüfen von Bereitstellungen in einem Abonnement, das mit einem Log Analytics-Arbeitsbereich verknüpft ist
Wenn keine fehlerhaften Bereitstellungen in der Ressourcengruppe oder dem Abonnement, die bzw. das Ihren fehlgeschlagenen virtuellen Computer enthält, angezeigt werden, und wenn Ihr fehlgeschlagener virtueller Computer mit einem Log Analytics-Arbeitsbereich in einem anderen Abonnement verbunden ist, wechseln Sie zu dem Abonnement, das mit Ihrem Log Analytics-Arbeitsbereich verknüpft ist, und suchen Sie nach fehlgeschlagenen Bereitstellungen.

## <a name="common-deployment-errors"></a>Häufige Bereitstellungsfehler

Fehler |  Minderung
:-----|:-------------|
Fehler wegen unzureichender Berechtigungen des Automanage-Kontos | Hierzu kann es kommen, wenn Sie vor Kurzem ein Abonnement, das ein neues Automanage-Konto enthält, in einen neuen Mandanten verschoben haben. Schritte zum Lösen des Problems finden Sie [hier](./repair-automanage-account.md).
Die Region des Arbeitsbereichs entspricht nicht den Anforderungen an die Regionszuordnung | Automanage konnte kein Onboarding Ihres Computers durchführen, aber der Log Analytics-Arbeitsbereich, mit dem der Computer zurzeit verknüpft ist, ist keiner unterstützten Automation-Region zugeordnet. Stellen Sie sicher, dass sich Ihr vorhandener Log Analytics-Arbeitsbereich und das Automation-Konto in einer [unterstützten Zuordnungsregion](../automation/how-to/region-mappings.md) befinden.
„Fehler bei der Zuweisung. Es gibt keine zusätzlichen Informationen.“ | Öffnen Sie einen Fall beim Microsoft Azure-Support.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Azure Automanage finden Sie [hier](./automanage-virtual-machines.md).

> [!div class="nextstepaction"]
> [Aktivieren der automatischen Verwaltung für virtuelle Computer im Azure-Portal](quick-create-virtual-machines-portal.md)