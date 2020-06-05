---
title: Verwenden dynamischer Gruppen mit Azure Automation-Updateverwaltung
description: In diesem Artikel wird beschrieben, wie Sie dynamische Gruppen mit der Azure Automation-Updateverwaltung verwenden.
services: automation
ms.subservice: update-management
ms.date: 11/20/2019
ms.topic: conceptual
ms.openlocfilehash: c440862f1379983d4644fe99f33207456f747d23
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83830938"
---
# <a name="use-dynamic-groups-with-update-management"></a>Verwenden dynamischer Gruppen mit der Updateverwaltung

Die Updateverwaltung ermöglicht Ihnen, eine dynamische Gruppe von Azure-VMs oder Nicht-Azure-VMs als Ziel für Updatebereitstellungen zu verwenden. Mit der Verwendung einer dynamischen Gruppe können Sie verhindern, dass Sie Ihre Bereitstellung zum Aktualisieren der Computer bearbeiten müssen.

> [!NOTE]
> Dynamische Gruppen funktionieren nicht mit klassischen VMs.

Sie können dynamische Gruppen für Azure- oder Nicht-Azure-Computer im Azure-Portal über die **Updateverwaltung** definieren. Weitere Informationen finden Sie unter [Verwalten von Updates für mehrere virtuelle Azure-Computer](manage-update-multi.md).

Eine dynamische Gruppe wird mit einer Abfrage definiert, die von Azure Automation zur Bereitstellungszeit ausgewertet wird. Auch wenn bei einer Abfrage für eine dynamische Gruppe eine große Zahl von Computern abgerufen wird, kann Azure Automation jeweils nur maximal 1.000 Computer verarbeiten. Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#update-management). 

> [!NOTE]
> Falls Sie damit rechnen, dass mehr als 1.000 Computer aktualisiert werden müssen, empfehlen wir Ihnen, die Updates auf mehrere Aktualisierungszeitpläne aufzuteilen. 

## <a name="define-dynamic-groups-for-azure-machines"></a>Definieren dynamischer Gruppen für Azure-Computer

Beim Definieren einer Abfrage für eine dynamische Gruppe, die für Azure-Computer bestimmt ist, können Sie die dynamische Gruppe mit den folgenden Elementen füllen:

* Subscription
* Ressourcengruppen
* Standorte
* `Tags`

![Auswählen von Gruppen](./media/automation-update-management/select-groups.png)

Klicken Sie auf **Vorschau**, um die Ergebnisse Ihrer Abfrage für die dynamische Gruppe anzuzeigen. Die Vorschau zeigt die Gruppenmitgliedschaft zum aktuellen Zeitpunkt. Im Beispiel suchen wir nach Computern mit dem Tag `Role` für die Gruppe **BackendServer**. Wenn dieses Tag weiteren Computern hinzugefügt wird, werden sie bei künftigen Bereitstellungen dieser Gruppe hinzugefügt.

![Anzeigen einer Vorschau von Gruppen](./media/automation-update-management/preview-groups.png)

## <a name="define-dynamic-groups-for-non-azure-machines"></a>Definieren dynamischer Gruppen für Nicht-Azure-Computer

Bei Verwendung einer dynamischen Gruppe für nicht zu Azure gehörende Computer werden gespeicherte Suchen genutzt, die auch als Computergruppen bezeichnet werden. Informationen zum Erstellen einer gespeicherten Suche finden Sie unter [Erstellen einer Computergruppe](../azure-monitor/platform/computer-groups.md#creating-a-computer-group). Nachdem Ihre gespeicherte Suche erstellt wurde, können Sie sie im Azure-Portal unter **Updateverwaltung** in der Liste mit den gespeicherten Suchen auswählen. Klicken Sie auf **Vorschau**, um die Vorschau für die Computer in der gespeicherten Suche anzuzeigen.

![Auswählen von Gruppen](./media/automation-update-management/select-groups-2.png)

## <a name="next-steps"></a>Nächste Schritte

Wie Sie die Updateverwaltung verwenden, erfahren Sie unter [Verwalten von Updates und Patches für Ihre Azure-VMs](automation-tutorial-update-management.md).
