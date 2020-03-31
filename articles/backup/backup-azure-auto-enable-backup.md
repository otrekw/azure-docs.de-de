---
title: Automatisches Aktivieren der Sicherung bei der VM-Erstellung mithilfe von Azure Policy
description: In diesem Artikel wird beschrieben, wie Sie Azure Policy für die automatische Aktivierung der Sicherung für alle VMs verwenden, die in einem bestimmten Bereich erstellt wurden.
ms.topic: conceptual
ms.date: 11/08/2019
ms.openlocfilehash: 1d423371788f87155328e8f5c9334e47349a68e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77584267"
---
# <a name="auto-enable-backup-on-vm-creation-using-azure-policy"></a>Automatisches Aktivieren der Sicherung bei der VM-Erstellung mithilfe von Azure Policy

Eine der Hauptaufgaben eines Sicherungs- oder Kompatibilitätsadministrators in einer Organisation besteht darin sicherzustellen, dass alle geschäftskritischen Computer mit der entsprechenden Aufbewahrung gesichert werden.

Heute bietet Azure Backup eine integrierte Richtlinie (unter Verwendung von Azure Policy), die **allen Azure-VMS an einem bestimmten Standort innerhalb eines Abonnements oder einer Ressourcengruppe** zugewiesen werden kann. Wird diese Richtlinie einem bestimmten Bereich zugewiesen, werden alle in diesem Bereich erstellten neuen virtuellen Computer automatisch für die Sicherung in einem **vorhandenen Tresor am gleichen Speicherort und im gleichen Abonnement** konfiguriert. Der Benutzer kann den Tresor und die Aufbewahrungsrichtlinie angeben, die den gesicherten VMs zugeordnet werden sollen.

## <a name="supported-scenarios"></a>Unterstützte Szenarien

* Die integrierte Richtlinie wird derzeit nur für Azure-VMs unterstützt. Benutzer müssen darauf achten, dass die bei der Zuweisung angegebene Aufbewahrungsrichtlinie eine VM-Aufbewahrungsrichtlinie ist. In [diesem](https://docs.microsoft.com/azure/backup/backup-azure-policy-supported-skus) Dokument finden Sie alle von dieser Richtlinie unterstützten VM-SKUs.

* Die Richtlinie kann immer nur einem einzelnen Standort und Abonnement zugewiesen werden. Zum Aktivieren der Sicherung für standort- und abonnementübergreifende virtuelle Computer müssen mehrere Instanzen der Richtlinienzuweisung erstellt werden, eine für jede Kombination aus Standort und Abonnement.

* Der angegebene Tresor und die für die Sicherung konfigurierten virtuellen Computer können sich in verschiedenen Ressourcengruppen befinden.

* Der Bereich „Verwaltungsgruppe“ wird derzeit nicht unterstützt.

* Die integrierte Richtlinie ist in nationalen Clouds zurzeit nicht verfügbar.

## <a name="using-the-built-in-policy"></a>Verwenden der integrierten Richtlinie

Um die Richtlinie dem erforderlichen Bereich zuzuweisen, führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zum Dashboard **Richtlinie**.
2. Wählen Sie im Menü auf der linken Seite **Definitionen** aus, um eine Liste aller integrierten Richtlinien für Azure-Ressourcen abzurufen.
3. Filtern Sie die Liste nach **Category=Backup**. Angezeigt wird die bis auf eine einzige Richtlinie mit dem Namen „Sicherung von VMs eines Standorts in einem vorhandenen zentralen Tresor am selben Standort konfigurieren“ gefilterte Liste.
![Dashboard „Richtlinie“](./media/backup-azure-auto-enable-backup/policy-dashboard.png)
4. Klicken Sie auf den Namen der Richtlinie. Sie werden zur detaillierten Definition für diese Richtlinie umgeleitet.
![Blatt „Richtliniendefinition“](./media/backup-azure-auto-enable-backup/policy-definition-blade.png)
5. Klicken Sie oben auf dem Blatt auf die Schaltfläche **Zuweisen**. Dadurch werden Sie zum Blatt **Richtlinie zuweisen** umgeleitet.
6. Klicken Sie unter **Grundlagen** auf die drei Punkte neben dem Feld **Bereich**. Dadurch wird ein Blatt mit dem richtigen Kontextgeöffnet, auf dem Sie das Abonnement auswählen können, auf das die Richtlinie angewendet werden soll. Optional können Sie auch eine Ressourcengruppe auswählen, damit die Richtlinie nur für VMs in einer bestimmten Ressourcengruppe angewendet wird.
![Grundlegendes zur Richtlinienzuweisung](./media/backup-azure-auto-enable-backup/policy-assignment-basics.png)
7. Wählen Sie auf der Registerkarte **Parameter** einen Speicherort aus der Dropdownliste aus, und wählen Sie dann den Tresor und die Sicherungsrichtlinie aus, denen die VMs im Bereich zugeordnet werden müssen.
![Parameter der Richtlinienzuweisung](./media/backup-azure-auto-enable-backup/policy-assignment-parameters.png)
8. Stellen Sie sicher, dass **Effekt** auf „deployIfNotExists“ festgelegt ist.
9. Navigieren Sie zu **Überprüfen und erstellen**, und klicken Sie auf **Erstellen**.

> [!NOTE]
>
> Azure Policy kann mithilfe von [Wiederherstellung](https://docs.microsoft.com/azure/governance/policy/how-to/remediate-resources) auch auf vorhandenen virtuellen Computern verwendet werden.

> [!NOTE]
>
> Es wird empfohlen, dass diese Richtlinie nicht mehr als 200 virtuellen Computern gleichzeitig zugewiesen wird. Wenn die Richtlinie mehr als 200 virtuellen Computern zugewiesen wird, kann dies dazu führen, dass die Sicherung einige Stunden später ausgelöst wird, als durch den Zeitplan festgelegt.

## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen zu Azure Policy](https://docs.microsoft.com/azure/governance/policy/overview)
