---
title: Wartungssteuerung für virtuelle Azure-Computer über das Azure-Portal
description: Erfahren Sie, wie Sie mithilfe der Wartungssteuerung und des Azure-Portals steuern, wann eine Wartung auf Ihre virtuellen Azure-Computer angewandt wird.
author: cynthn
ms.service: virtual-machines
ms.subservice: maintenance-control
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 04/22/2020
ms.author: cynthn
ms.openlocfilehash: 0dea30723a3ebe1598d6304ac3c98bfe1b55b466
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102563012"
---
# <a name="control-updates-with-maintenance-control-and-the-azure-portal"></a>Steuern von Updates mit der Wartungssteuerung und dem Azure-Portal

Mit der Wartungssteuerung können Sie entscheiden, wann Sie Updates auf Ihre isolierten VMs und Ihre dedizierten Azure-Hosts anwenden. In diesem Thema werden die Optionen mit dem Azure-Portal für die Wartungssteuerung behandelt. Weitere Informationen zu den Vorteilen der Verwendung der Wartungssteuerung, ihren Einschränkungen und anderen Verwaltungsoptionen finden Sie unter [Verwalten von Plattformupdates mit der Wartungssteuerung](maintenance-control.md).

## <a name="create-a-maintenance-configuration"></a>Erstellen einer Wartungskonfiguration

1. Melden Sie sich beim Azure-Portal an.

1. Suchen Sie nach **Wartungskonfigurationen**.

   ![Screenshot des Öffnens von Wartungskonfigurationen](media/virtual-machines-maintenance-control-portal/maintenance-configurations-search.png)

1. Klicken Sie auf **Hinzufügen**.

   ![Screenshot des Hinzufügens einer Wartungskonfiguration](media/virtual-machines-maintenance-control-portal/maintenance-configurations-add.png)

1. Wählen Sie ein Abonnement und eine Ressourcengruppe aus, geben Sie einen Namen für die Konfiguration an, und wählen Sie eine Region aus. Klicken Sie auf **Weiter**.

   ![Screenshot der Grundlagen der Wartungskonfiguration](media/virtual-machines-maintenance-control-portal/maintenance-configurations-basics.png)

1. Fügen Sie Tags und Werte hinzu. Klicken Sie auf **Weiter**.

   ![Screenshot des Hinzufügens von Tags zur Wartungskonfiguration](media/virtual-machines-maintenance-control-portal/maintenance-configurations-tags.png)

1. Überprüfen Sie die Zusammenfassung. Klicken Sie auf **Erstellen**.

   ![Screenshot des Erstellens einer Wartungskonfiguration](media/virtual-machines-maintenance-control-portal/maintenance-configurations-create.png)

1. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.

   ![Screenshot der abgeschlossenen Bereitstellung einer Wartungskonfiguration](media/virtual-machines-maintenance-control-portal/maintenance-configurations-deployment-complete.png)

## <a name="assign-the-configuration"></a>Zuweisen der Konfiguration

Klicken Sie auf der Seite „Details“ der Wartungskonfiguration auf „Zuweisungen“ und dann auf **Ressource zuweisen**. 

![Screenshot des Zuweisens einer Ressource](media/virtual-machines-maintenance-control-portal/maintenance-configurations-add-assignment.png)

Wählen Sie die Ressourcen aus, denen die Wartungskonfiguration zugewiesen werden soll, und klicken Sie auf **OK**. In der Spalte **Typ** wird angezeigt, ob es sich bei der Ressource um eine isolierte VM oder einen dedizierten Azure-Host handelt. Die VM muss sich im ausgeführten Zustand befinden, damit die Konfiguration zugewiesen werden kann. Wenn Sie versuchen, einer angehaltenen VM eine Konfiguration zuzuweisen, tritt ein Fehler auf. 

<!---Shantanu to add details about the error case--->

![Screenshot des Auswählens einer Ressource](media/virtual-machines-maintenance-control-portal/maintenance-configurations-select-resource.png)

## <a name="check-configuration"></a>Überprüfen der Konfiguration

Mithilfe von **Wartungskonfigurationen** können Sie sich vergewissern, dass die Konfiguration ordnungsgemäß angewandt wurde, oder Wartungskonfigurationen überprüfen, die derzeit zugewiesen sind. In der Spalte **Typ** wird angezeigt, ob die Konfiguration einer isolierten VM oder einem dedizierten Azure-Host zugewiesen ist. 

![Screenshot des Überprüfens einer Wartungskonfiguration](media/virtual-machines-maintenance-control-portal/maintenance-configurations-host-type.png)

Sie können auch die Konfiguration für einen bestimmten virtuellen Computer auf der Seite „Eigenschaften“ überprüfen. Klicken Sie auf **Wartung**, um die Konfiguration anzuzeigen, die diesem virtuellen Computer zugewiesen ist.

![Screenshot des Überprüfens der Wartung für einen Host](media/virtual-machines-maintenance-control-portal/maintenance-configurations-check-config.png)

## <a name="check-for-pending-updates"></a>Prüfen auf ausstehende Updates

Es gibt auch zwei Möglichkeiten, zu überprüfen, ob für eine Wartungskonfiguration Updates ausstehen. Klicken Sie unter **Wartungskonfigurationen** in den Details für die Konfiguration auf **Zuweisungen**, und überprüfen Sie den **Wartungsstatus**.

![Screenshot des Überprüfens ausstehender Updates](media/virtual-machines-maintenance-control-portal/maintenance-configurations-pending.png)

Sie können auch einen bestimmten Host mithilfe von **Virtual Machines** oder der Eigenschaften des dedizierten Hosts überprüfen. 

![Screenshot: Hervorgehobener Wartungsstatus](media/virtual-machines-maintenance-control-portal/maintenance-configurations-pending-vm.png)

## <a name="apply-updates"></a>Anwenden von Updates

Sie können ausstehende Updates bedarfsgesteuert mithilfe von **Virtual Machines** anwenden. Klicken Sie in den VM-Details auf **Wartung** und dann auf **Wartungsfunktion jetzt anwenden**.

![Screenshot des Anwendens ausstehender Updates](media/virtual-machines-maintenance-control-portal/maintenance-configurations-apply-updates-now.png)

## <a name="check-the-status-of-applying-updates"></a>Überprüfen Sie den Status für das Anwenden von Updates. 

Sie können den Status der Updates für eine Konfiguration unter **Wartungskonfigurationen** oder mithilfe von **Virtual Machines** überprüfen. Klicken Sie in den VM-Details auf **Wartung**. Im folgenden Beispiel ist unter **Wartungsstatus** zu erkennen, dass ein Update **aussteht**.

![Screenshot des Überprüfens des Status ausstehender Updates](media/virtual-machines-maintenance-control-portal/maintenance-configurations-status.png)

## <a name="delete-a-maintenance-configuration"></a>Löschen einer Wartungskonfiguration

Um eine Konfiguration zu löschen, öffnen Sie die Konfigurationsdetails, und klicken Sie auf **Löschen**.

![Screenshot: Löschen einer Konfiguration](media/virtual-machines-maintenance-control-portal/maintenance-configurations-delete.png)


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Wartung und Updates](maintenance-and-updates.md).
