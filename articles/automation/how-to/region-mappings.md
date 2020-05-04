---
title: Zuordnungen zwischen Azure Automation und einem Log Analytics-Arbeitsbereich
description: In diesem Artikel werden die Zuordnungen beschrieben, die zwischen einem Automation-Konto und einem Log Analytics-Arbeitsbereich zulässig sind, um die Lösung zu unterstützen.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/23/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 607cebca3e6e8ddd95900ecdbd7041e5f7bb50cc
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2020
ms.locfileid: "82165772"
---
# <a name="workspace-mappings"></a>Arbeitsbereichzuordnungen

Beim Aktivieren von Updateverwaltung, Änderungsnachverfolgung und Bestand oder „VMs außerhalb der Geschäftszeiten starten/beenden“ werden in Bezug auf das Verknüpfen eines Log Analytics-Arbeitsbereichs mit einem Automation-Konto nur bestimmte Regionen in Ihrem Abonnement unterstützt. Diese Zuordnung gilt nur für das Automation-Konto und den Log Analytics-Arbeitsbereich. Der Log Analytics-Arbeitsbereich und das Automation-Konto müssen sich im selben Abonnement befinden, können sich aber in unterschiedlichen Ressourcengruppen befinden, die in derselben Region bereitgestellt sind.

Weitere Informationen finden Sie unter [Log Analytics-Arbeitsbereich und das Automation-Konto](../../azure-monitor/insights/solutions.md#log-analytics-workspace-and-automation-account).

## <a name="supported-mappings"></a>Unterstützte Zuordnungen

Die folgende Tabelle zeigt die unterstützten Zuordnungen:

|**Log Analytics-Arbeitsbereichsregion**|**Azure Automation-Region**|
|---|---|
|**USA**||
|EastUS<sup>1</sup>|EastUS2|
|WestUS2|WestUS2|
|WestCentralUS<sup>2</sup>|WestCentralUS<sup>2</sup>|
|**Kanada**||
|CanadaCentral|CanadaCentral|
|**Asien-Pazifik**||
|AustraliaSoutheast|AustraliaSoutheast|
|SoutheastAsia|SoutheastAsia|
|CentralIndia|CentralIndia|
|JapanEast|JapanEast|
|**Europa**||
|UKSouth|UKSouth|
|Europa, Westen|Europa, Westen|
|**US Gov**||
|USGovVirginia|USGovVirginia|

<sup>1</sup> EastUS-Zuordnung für Log Analytics-Arbeitsbereiche zu Automation-Konten ist keine exakte Region-zu-Region-Zuordnung, aber es handelt sich um die richtige Zuordnung.

<sup>2</sup> Aufgrund von Kapazitätseinschränkungen ist die Region nicht verfügbar, wenn neue Ressourcen erstellt werden. Dies umfasst auch Automation-Konten und Log Analytics-Arbeitsbereiche. Allerdings sollten bereits in der Region vorhandene verknüpfte Ressourcen weiterhin funktionieren.

## <a name="unlink-workspace"></a>Aufheben der Verknüpfung mit dem Arbeitsbereich

Wenn Sie Ihr Automation-Konto nicht länger in einen Log Analytics-Arbeitsbereich integriert sein soll, können Sie die Verknüpfung direkt im Azure-Portal aufheben. Vor dem Fortfahren müssen Sie zuerst Updateverwaltung, Änderungsnachverfolgung und Bestand bzw. „VMs außerhalb der Geschäftszeiten starten/beenden“ entfernen, falls Sie sie nutzen. Wenn Sie sie nicht entfernen, können Sie den Vorgang zum Aufheben der Verknüpfung nicht durchführen. Lesen Sie den Artikel für jede Lösung, die Sie aktivieren, um zu erfahren, welche Schritte zu ihrer Entfernung erforderlich sind.

Nachdem Sie sie entfernt haben, können Sie die folgenden Schritte ausführen, um die Verknüpfung mit Ihrem Automation-Konto aufzuheben.

> [!NOTE]
> Einige Lösungen – einschließlich früherer Versionen der Azure SQL-Überwachungslösung – haben möglicherweise Automatisierungsressourcen erstellt und müssen möglicherweise vor dem Aufheben der Verknüpfung des Arbeitsbereichs entfernt werden.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto. Wählen Sie auf der Automation-Kontoseite unter **Verwandte Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

2. Klicken Sie auf der Seite „Verknüpfung des Arbeitsbereichs aufheben“ auf **Verknüpfung des Arbeitsbereichs aufheben**. Sie werden gefragt, ob Sie fortfahren möchten.

3. Während Azure Automation versucht, die Verknüpfung des Kontos mit Ihrem Log Analytics-Arbeitsbereich aufzuheben, können Sie den Fortschritt unter **Benachrichtigungen** im Menü nachverfolgen.

4. Wenn Sie die „Updateverwaltung“ verwendet haben, können Sie optional die folgenden Elemente entfernen, die nach dem Entfernen der Lösung nicht mehr benötigt werden.

    * Zeitpläne für Updates: Jeder weist einen Namen auf, der einer Updatebereitstellung entspricht, die Sie erstellt haben.
    * Für die Lösung erstellte Hybrid Worker-Gruppen (jeweils mit einem ähnlichen Namen wie `machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8`).

5. Wenn Sie „VMs außerhalb der Geschäftszeiten starten/beenden“ verwendet haben, können Sie optional die folgenden Elemente entfernen, die nach dem Entfernen der Lösung nicht mehr benötigt werden.

    * Starten und beenden Sie Zeitpläne für VM-Runbooks.
    * Starten und beenden Sie VM-Runbooks.
    * Variables

Alternativ können Sie Ihren Arbeitsbereich von Ihrem Automation-Konto im Arbeitsbereich trennen.

1. Wählen Sie im Arbeitsbereich unter **Verwandte Ressourcen** die Option **Automation-Konto**. 
2. Klicken Sie auf der Seite „Automation-Konto“ auf **Verknüpfung zu diesem Konto aufheben**.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie die Updateverwaltung, Änderungsnachverfolgung und den Bestand integrieren:

    * Über einen [virtuellen Computer](../automation-onboard-solutions-from-vm.md)
    * Über Ihr [Automation-Konto](../automation-onboard-solutions-from-automation-account.md)
    * Über das [Durchsuchen mehrerer Computer](../automation-onboard-solutions-from-browse.md)
    * Über ein [Runbook](../automation-onboard-solutions.md)

* Informationen, wie Sie das Onboarding von „VMs außerhalb der Geschäftszeiten starten/beenden“ durchführen, finden Sie unter

    * [VMs außerhalb der Geschäftszeiten starten/beenden – Übersicht](../automation-solution-vm-management.md).
