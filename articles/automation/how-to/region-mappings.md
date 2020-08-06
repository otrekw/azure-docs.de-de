---
title: Unterstützte Regionen für einen verknüpften Log Analytics-Arbeitsbereich
description: In diesem Artikel werden die unterstützten Regionszuordnungen zwischen einem Automation-Konto und einem Log Analytics-Arbeitsbereich beschrieben.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 06/12/2020
ms.topic: conceptual
manager: carmonm
ms.custom: references_regions
ms.openlocfilehash: 4e5cad25c80661f9e707f545929e6ffcb00a1e42
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87447857"
---
# <a name="supported-regions-for-linked-log-analytics-workspace"></a>Unterstützte Regionen für einen verknüpften Log Analytics-Arbeitsbereich

In Azure Automation können Sie die Features „Updateverwaltung“, „Änderungsnachverfolgung und Bestand“ und „VMs außerhalb der Geschäftszeiten starten/beenden“ für Ihre virtuellen Computer aktivieren. Für die Verknüpfung eines Log Analytics-Arbeitsbereichs und eines Automation-Kontos in Ihrem Abonnement werden allerdings nur bestimmte Regionen unterstützt. Diese Regionszuordnungen gelten nur für das Automation-Konto und den Log Analytics-Arbeitsbereich. Der Log Analytics-Arbeitsbereich und das Automation-Konto müssen sich im selben Abonnement befinden, können sich aber in unterschiedlichen Ressourcengruppen befinden, die in derselben Region bereitgestellt sind. Weitere Informationen finden Sie unter [Log Analytics-Arbeitsbereich und das Automation-Konto](../../azure-monitor/insights/solutions.md#log-analytics-workspace-and-automation-account).

## <a name="supported-mappings"></a>Unterstützte Zuordnungen

Die folgende Tabelle zeigt die unterstützten Zuordnungen:

|**Region des Log Analytics-Arbeitsbereichs**|**Azure Automation-Region**|
|---|---|
|**USA**||
|EastUS<sup>1</sup>|EastUS2|
|WestUS2|WestUS2|
|WestCentralUS|WestCentralUS|
|**Kanada**||
|CanadaCentral|CanadaCentral|
|**Asien-Pazifik**||
|AustraliaSoutheast|AustraliaSoutheast|
|SoutheastAsia|SoutheastAsia|
|CentralIndia|CentralIndia|
|ChinaEast2<sup>2</sup>|ChinaEast2|
|JapanEast|JapanEast|
|**Europa**||
|UKSouth|UKSouth|
|Europa, Westen|Europa, Westen|
|**US Gov**||
|USGovVirginia|USGovVirginia|
|USGovArizona<sup>2</sup>|USGovArizona|

<sup>1</sup> Die EastUS-Zuordnung für die Zuordnung von Log Analytics-Arbeitsbereichen zu Automation-Konten ist zwar keine exakte Zuordnung zwischen Regionen, sie ist aber korrekt.

<sup>2</sup> In dieser Region wird nur die Updateverwaltung unterstützt. Andere Features wie Änderungsnachverfolgung und Bestand sind derzeit nicht verfügbar.

## <a name="unlink-a-workspace"></a>Aufheben der Verknüpfung eines Arbeitsbereichs

Wenn Sie Ihr Automation-Konto nicht länger in einen Log Analytics-Arbeitsbereich integriert sein soll, können Sie die Verknüpfung direkt im Azure-Portal aufheben. Dazu müssen zunächst „Updateverwaltung“, „Änderungsnachverfolgung und Bestand“ und „VMs außerhalb der Geschäftszeiten starten/beenden“ [entfernt](move-account.md#remove-features) werden (sofern Sie diese Features nutzen). Andernfalls kann der Vorgang zum Aufheben der Verknüpfung nicht ausgeführt werden. 

Nachdem Sie die Features entfernt haben, können Sie die folgenden Schritte ausführen, um die Verknüpfung Ihres Automation-Kontos aufzuheben.

> [!NOTE]
> Von einigen Features (einschließlich früherer Versionen der Azure SQL-Überwachungslösung) wurden möglicherweise Automatisierungsressourcen erstellt, die vor dem Aufheben der Verknüpfung des Arbeitsbereichs entfernt werden müssen.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto. Wählen Sie auf der Automation-Kontoseite unter **Verwandte Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

2. Wählen Sie auf der Seite „Verknüpfung des Arbeitsbereichs aufheben“ die Option **Verknüpfung des Arbeitsbereichs aufheben** aus. Sie werden gefragt, ob Sie fortfahren möchten.

3. Während die Verknüpfung zwischen dem Konto und Ihrem Log Analytics-Arbeitsbereich durch Azure Automation aufgehoben wird, können Sie den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.

4. Wenn Sie „Updateverwaltung“ verwendet haben, empfiehlt es sich gegebenenfalls, die folgenden Elemente zu entfernen, da sie nicht mehr benötigt werden:

    * Zeitpläne für Updates: Diese sind jeweils nach einer von Ihnen erstellten Updatebereitstellung benannt.
    * Für das Feature erstellte Hybrid Worker-Gruppen: Diese haben jeweils einen Namen wie beispielsweise `machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8`.

5. Wenn Sie „VMs außerhalb der Geschäftszeiten starten/beenden“ verwendet haben, können Sie optional die folgenden Elemente entfernen, da sie nicht mehr benötigt werden:

    * Starten und beenden Sie Zeitpläne für VM-Runbooks.
    * Starten und beenden Sie VM-Runbooks.
    * Variables

Alternativ können Sie Ihren Arbeitsbereich von Ihrem Automation-Konto im Arbeitsbereich trennen.

1. Wählen Sie im Arbeitsbereich unter **Verwandte Ressourcen** die Option **Automation-Konto**.
2. Klicken Sie auf der Seite „Automation-Konto“ auf **Verknüpfung zu diesem Konto aufheben**.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu „Updateverwaltung“ finden Sie in der [Übersicht über die Updateverwaltung](../update-management/update-mgmt-overview.md).
* Informationen zu „Änderungsnachverfolgung und Bestand“ finden Sie in der [Übersicht über Änderungsnachverfolgung und Bestand](../change-tracking.md).
* Informationen zu „VMs außerhalb der Geschäftszeiten starten/beenden“ finden Sie unter [VMs außerhalb der Geschäftszeiten starten/beenden – Übersicht](../automation-solution-vm-management.md).
