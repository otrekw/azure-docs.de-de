---
title: Verschieben von Azure-Ressourcen zwischen Ressourcengruppen, Abonnements oder Regionen
description: Übersicht über Azure-Ressourcentypen, die zwischen Ressourcengruppen, Abonnements oder Regionen verschoben werden können.
ms.topic: conceptual
ms.date: 04/16/2021
ms.openlocfilehash: 646c0892ac3609fc531e91f4bfb9c1de6a61bc38
ms.sourcegitcommit: 6f1aa680588f5db41ed7fc78c934452d468ddb84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107730340"
---
# <a name="move-azure-resources-across-resource-groups-subscriptions-or-regions"></a>Verschieben von Azure-Ressourcen zwischen Ressourcengruppen, Abonnements oder Regionen

Azure-Ressourcen können in eine neue Ressourcengruppe, in ein neues Abonnement oder zwischen Regionen verschoben werden.

## <a name="move-resources-across-resource-groups-or-subscriptions"></a>Verschieben von Ressourcen zwischen Ressourcengruppen oder Abonnements

Azure-Ressourcen können entweder in ein anderes Azure-Abonnement oder in eine andere Ressourcengruppe im gleichen Abonnement verschoben werden. Sie können das Azure-Portal, Azure PowerShell, die Azure-Befehlszeilenschnittstelle oder die REST-API verwenden, um Ressourcen zu verschieben. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](move-resource-group-and-subscription.md).

### <a name="upgrade-a-subscription"></a>Aktualisieren eines Abonnements

Wenn Sie ein Upgrade für Ihr Azure-Abonnement durchführen möchten (z.B. Wechsel vom kostenlosen Abonnement zur nutzungsbasierten Bezahlung), müssen Sie Ihr Abonnement konvertieren.

- Falls Sie für eine kostenlose Testversion ein Upgrade durchführen möchten, helfen Ihnen die Informationen unter [Aktualisieren Ihrer kostenlosen Testversion oder Ihres Microsoft Imagine Azure-Abonnements auf nutzungsbasierte Bezahlung](../../cost-management-billing/manage/upgrade-azure-subscription.md) weiter.
- Informationen zum Ändern eines Kontos mit nutzungsbasierter Bezahlung finden Sie unter [Ändern Ihres Azure-Abonnements mit nutzungsbasierter Bezahlung in ein anderes Angebot](../../cost-management-billing/manage/switch-azure-offer.md).

Wenn Sie das Abonnement nicht konvertieren können, können Sie [eine Azure-Supportanfrage erstellen](../../azure-portal/supportability/how-to-create-azure-support-request.md). Wählen Sie **Abonnementverwaltung** als Problemtyp aus.

## <a name="move-resources-across-regions"></a>Verschieben von Ressourcen zwischen Regionen

Geografische Azure-Regionen, Regionen und Verfügbarkeitszonen bilden die Grundlage der globalen Azure-Infrastruktur. [Geografische Azure-Regionen](https://azure.microsoft.com/global-infrastructure/geographies/) enthalten in der Regel mindestens zwei [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/). Eine Region ist ein Bereich in einer geografischen Region, die Verfügbarkeitszonen und mehrere Rechenzentren enthält.

Nachdem Sie Ressourcen in einer bestimmten Azure-Region bereitgestellt haben, kann es verschiedenste Gründe geben, warum Sie Ressourcen in eine andere Region verschieben möchten.

- **Ausrichtung an einer neuen Region**: Verschieben von Ressourcen in eine neu eingeführte Azure-Region, die zuvor noch nicht verfügbar war.
- **Ausrichtung für Dienste/Features**: Verschieben von Ressourcen, um die Vorteile der Dienste oder Features zu nutzen, die in einer bestimmten Region verfügbar sind.
- **Reagieren auf Geschäftsentwicklungen**: Verschieben von Ressourcen in eine Region als Reaktion auf Geschäftsänderungen, z.B. Fusionen oder Akquisitionen.
- **Ausrichtung an einer nahegelegenen Region**: Verschieben von Ressourcen in eine Region, die für Ihr Unternehmen lokal ist.
- **Erfüllen von Datenanforderungen**: Verschieben von Ressourcen, um Anforderungen an Datenresidenz oder an Datenklassifizierung zu erfüllen [Weitere Informationen](https://azure.microsoft.com/mediahandler/files/resourcefiles/achieving-compliant-data-residency-and-security-with-azure/Achieving_Compliant_Data_Residency_and_Security_with_Azure.pdf)
- **Reagieren auf Bereitstellungsanforderungen**: Verschieben von Ressourcen, die fehlerhaft bereitgestellt wurden, oder als Reaktion auf Kapazitätsanforderungen.
- **Reagieren auf Außerbetriebnahmen**: Verschieben von Ressourcen aufgrund der Außerbetriebnahme von Regionen

### <a name="move-resources-with-resource-mover"></a>Verschieben von Ressourcen mit Resource Mover

Ressourcen können mit [Azure Resource Mover](../../resource-mover/overview.md) in eine andere Region verschoben werden. Resource Mover bietet Folgendes:

- Eine einzelne Anlaufstelle für das regionsübergreifende Verschieben von Ressourcen.
- Kürzere Verschiebungszeit und verringerte Komplexität Alle erforderlichen Komponenten an einem einzelnen Ort
- Ein einfaches und konsistentes Verfahren zum Verschieben verschiedener Arten von Azure-Ressourcen
- Eine einfache Möglichkeit zum Erkennen von Abhängigkeiten zwischen den zu verschiebenden Ressourcen. Dies hilft Ihnen dabei, zusammenhängende Ressourcen gemeinsam zu verschieben, damit nach der Verschiebung in der Zielregion alles wie erwartet funktioniert.
- Automatische Bereinigung der Ressourcen in der Quellregion, falls Sie sie nach dem Verschieben löschen möchten
- Tests. Sie können eine Verschiebung testen und ggf. verwerfen, wenn Sie keine vollständige Verschiebung durchführen möchten.

Für die Verschiebung von Ressourcen in eine andere Region stehen verschiedene Methoden zur Verfügung:

- **Starten der Ressourcenverschiebung über eine Ressourcengruppe:** Bei dieser Methode wird die Regionsverschiebung von einer Ressourcengruppe aus initiiert. Nach dem Auswählen der zu verschiebenden Ressourcen wird der Prozess im Resource Mover-Hub fortgesetzt, um die Ressourcenabhängigkeiten zu überprüfen und den Verschiebungsprozess zu orchestrieren. [Weitere Informationen](../../resource-mover/move-region-within-resource-group.md)
- **Starten der Ressourcenverschiebung direkt über den Resource Mover-Hub:** Bei dieser Methode wird der Regionsverschiebungsprozess direkt im Hub gestartet. [Weitere Informationen](../../resource-mover/tutorial-move-region-virtual-machines.md)

## <a name="next-steps"></a>Nächste Schritte

- Informationen dazu, ob die Verschiebung eines bestimmten Ressourcentyps unterstützt wird, finden Sie unter [Unterstützung des Verschiebevorgangs für Ressourcen](move-support-resources.md).
- Weitere Informationen zum Regionsverschiebungsprozess finden Sie unter [Informationen zum Verschiebungsprozess](../../resource-mover/about-move-process.md).
