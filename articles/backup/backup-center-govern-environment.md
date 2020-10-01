---
title: Steuern Ihres Sicherungsbestands mit Backup Center
description: Erfahren Sie, wie Sie Ihre Azure-Umgebung so steuern, dass sichergestellt ist, dass sämtliche Ihrer Ressourcen aus Sicherungssicht zu Backup Center konform sind.
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: 009ee461b0372a3fb73ffb3b0ee5151b77bd8ef8
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90993444"
---
# <a name="govern-your-backup-estate-using-backup-center"></a>Steuern Ihres Sicherungsbestands mit Backup Center

Backup Center unterstützt Sie dabei, Ihre Azure-Umgebung so zu steuern, dass sichergestellt ist, dass sämtliche Ihrer Ressourcen aus Sicherungssicht zu Backup Center konform sind. Nachstehend sind einige der Steuerungsfunktionen von Backup Center aufgeführt:

* Anzeigen und Zuweisen von Azure-Richtlinien für Sicherungen

* Anzeigen aller Datenquellen, die nicht für eine Sicherung konfiguriert wurden

## <a name="supported-scenarios"></a>Unterstützte Szenarien

* Eine ausführliche Liste der unterstützten und nicht unterstützten Szenarien finden Sie in der [Supportmatrix](backup-center-support-matrix.md).

## <a name="azure-policies-for-backup"></a>Azure-Richtlinien für Sicherung

Um alle [Azure-Richtlinien](https://docs.microsoft.com/azure/governance/policy/overview) anzuzeigen, die für Sicherungen verfügbar sind, wählen Sie das Menüelement **Azure-Richtlinien für Sicherung** aus. Danach werden alle integrierten und benutzerdefinierten [Azure-Richtliniendefinitionen für Sicherung](policy-reference.md) angezeigt, die Sie Ihren Abonnements und Ressourcengruppen zuweisen können.

Nachdem Sie eine der Definitionen ausgewählt haben, können Sie [die Richtlinie einem Bereich zuweisen](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage#assign-a-policy).

![Azure Policy-Definitionen auswählen](./media/backup-center-govern-environment/azure-policy-definitions.png)

## <a name="protectable-datasources"></a>Schützbare Datenquellen

Wenn Sie das Menüelement **Schützbare Datenquellen** auswählen, können Sie alle Datenquellen anzeigen, die noch nicht für die Sicherung konfiguriert wurden. Sie können die Liste nach Datenquellenabonnement, -ressourcengruppe, -speicherort, -typ und Tags filtern. Sobald Sie eine Datenquelle ermittelt haben, die gesichert werden muss, können Sie mit der rechten Maustaste auf das entsprechende Rasterelement klicken und **Sicherung** auswählen, um die Sicherung für die Ressource zu konfigurieren.

![Menüelement „Schützbare Datenquellen“](./media/backup-center-govern-environment/protectable-datasources.png)

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen und Betreiben von Sicherungen](backup-center-monitor-operate.md)
* [Ausführen von Aktionen mithilfe des Backup Centers](backup-center-actions.md)
* [Gewinnen von Einblicken in Ihre Sicherungen](backup-center-obtain-insights.md)
