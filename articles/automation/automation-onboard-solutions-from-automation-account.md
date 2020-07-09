---
title: Aktivieren der Azure Automation-Updateverwaltung über ein Automation-Konto
description: In diesem Artikel erfahren Sie, wie Sie die Updateverwaltung über ein Automation-Konto aktivieren.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: d756cf9705cc1ce0041c26dc3ef022c150c514c0
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86186060"
---
# <a name="enable-update-management-from-an-automation-account"></a>Aktivieren der Updateverwaltung über ein Automation-Konto

In diesem Artikel wird beschrieben, wie Sie mit Ihrem Automation-Konto das Feature [Updateverwaltung](automation-update-management.md) für VMs in Ihrer Umgebung aktivieren können. Sie müssen eine vorhandene VM über die Updateverwaltung aktivieren, um Azure-VMs im großen Stil zu aktivieren. 

> [!NOTE]
> Wenn Sie die Updateverwaltung aktivieren, werden nur bestimmte Regionen zum Verknüpfen mit einem Log Analytics-Arbeitsbereich und einem Automation-Konto unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](./index.yml) zum Verwalten von Computern.
* Ein [virtueller Computer](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com bei Azure an.

## <a name="enable-update-management"></a>Aktivieren der Updateverwaltung

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Wählen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto aus, und klicken Sie auf **Aktivieren**, um die Updateverwaltung zu aktivieren. Die Einrichtung dauert bis zu 15 Minuten.

    ![Aktivieren der Updateverwaltung](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="enable-azure-vms"></a>Aktivieren von Azure-VMs

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Klicken Sie auf **+ Azure-VMs hinzufügen**, und wählen Sie mindestens einen virtuellen Computer aus der Liste aus. Virtuelle Computer, die nicht aktiviert werden können, sind abgeblendet und können nicht ausgewählt werden. Virtuelle Azure-Computer können sich unabhängig vom Standort Ihres Automation-Kontos in einer beliebigen Region befinden. 

3. Klicken Sie auf **Aktivieren**, um die ausgewählten virtuellen Computer der gespeicherten Computergruppensuche für das Feature hinzuzufügen.

    ![Aktivieren von Azure-VMs](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="enable-non-azure-vms"></a>Aktivieren Azure-fremder virtueller Computer

Computer, die nicht in Azure enthalten sind, müssen manuell hinzugefügt werden. 

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Klicken Sie auf **Nicht-Azure-Computer hinzufügen**. Diese Aktion öffnet ein neues Browserfenster mit [Anweisungen zum Installieren und Konfigurieren des Log Analytics-Agents für Windows](../azure-monitor/platform/log-analytics-agent.md), sodass der Computer mit dem Erstellen von Berichten über Updateverwaltungsvorgänge beginnen kann. Wenn Sie einen Computer aktivieren, der aktuell von Operations Manager verwaltet wird, ist kein neuer Agent erforderlich. Die Arbeitsbereichsinformationen werden in den vorhandenen Agent eingegeben.

## <a name="enable-machines-in-the-workspace"></a>Aktivieren von Computern im Arbeitsbereich

Manuell installierte Computer oder Computer, die bereits Berichte für Ihren Arbeitsbereich erstellen, müssen Azure Automation hinzugefügt werden, damit die Updateverwaltung aktiviert wird. 

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Wählen Sie **Computer verwalten** aus. Die Schaltfläche **Computer verwalten** ist möglicherweise abgeblendet, wenn Sie zuvor die Option **Auf allen verfügbaren und zukünftigen Computern aktivieren** ausgewählt haben.

    ![Gespeicherte Suchvorgänge](media/automation-onboard-solutions-from-automation-account/managemachines.png)

4. Wenn Sie die Updateverwaltung für alle verfügbaren Computer aktivieren möchten, wählen Sie auf der Seite „Computer verwalten“ die Option **Auf allen verfügbaren Computern aktivieren** aus. Diese Aktion deaktiviert das Steuerelement zum einzelnen Hinzufügen von Computern. Mit dieser Aufgabe werden alle Namen der Computer hinzugefügt, die Berichte für den Arbeitsbereich und die Computergruppe der gespeicherten Suchabfrage erstellen. Diese Aktion deaktiviert die Schaltfläche **Computer verwalten**.

5. Wenn Sie das Feature für alle verfügbaren und zukünftigen Computer aktivieren möchten, wählen Sie **Auf allen verfügbaren und zukünftigen Computern aktivieren** aus. Mit dieser Option werden die gespeicherten Suchen und die Bereichskonfigurationen aus dem Arbeitsbereich gelöscht, und das Feature wird für alle Azure-Computer und Azure-fremden Computer geöffnet, von denen Berichte an den Arbeitsbereich übermittelt werden. Bei Verwendung dieser Aktion wird die Schaltfläche **Computer verwalten** dauerhaft deaktiviert, da keine Bereichskonfiguration mehr vorhanden ist.

6. Die Bereichskonfigurationen können bei Bedarf durch Hinzufügen der anfänglichen gespeicherten Suchen wieder hinzugefügt werden. Weitere Informationen finden Sie unter [Einschränken des Bereitstellungsbereichs für die Updateverwaltung](automation-scope-configurations-update-management.md).

7. Wenn Sie das Feature für einen oder mehrere Computer aktivieren möchten, wählen Sie **Auf ausgewählten Computern aktivieren** aus, und klicken Sie neben jedem Computer, für den das Feature aktiviert werden soll, auf **Hinzufügen**. Dadurch werden die Namen der ausgewählten Computer der gespeicherten Computergruppensuchabfrage für das Feature hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

* Wie Sie die Updateverwaltung für VMs verwenden, erfahren Sie unter [Verwalten von Updates und Patches für Ihre Azure-VMs](automation-tutorial-update-management.md).
* Informationen zum Behandeln von Fehlern bei der Updateverwaltung finden Sie unter [Behandeln von Problemen mit der Updateverwaltung](troubleshoot/update-management.md).
* Informationen zum Behandeln von Problemen mit dem Windows Update-Agent finden Sie unter [Beheben von Problemen mit dem Windows Update-Agent](troubleshoot/update-agent-issues.md).
* Informationen zum Behandeln von Problemen mit dem Linux-Update-Agent finden Sie unter [Beheben von Problemen mit dem Linux-Update-Agent](troubleshoot/update-agent-issues-linux.md).
