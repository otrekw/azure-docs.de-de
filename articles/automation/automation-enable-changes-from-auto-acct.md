---
title: Aktivieren des Features „Änderungsnachverfolgung und Bestand“ von Azure Automation über ein Automation-Konto
description: In diesem Artikel erfahren Sie, wie Sie „Änderungsnachverfolgung und Bestand“ über ein Automation-Konto aktivieren.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 302e0cc9b77605afd4012e95fc7c0ab8a22fef3e
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86186315"
---
# <a name="enable-change-tracking-and-inventory-from-an-automation-account"></a>Aktivieren von Änderungsnachverfolgung und Bestand über ein Automation-Konto

In diesem Artikel erfahren Sie, wie Sie das Feature [Änderungsnachverfolgung und Bestand](change-tracking.md) mithilfe eines Automation-Kontos für virtuelle Computer in Ihrer Umgebung aktivieren. Wenn Sie virtuelle Azure-Computer im großen Stil aktivieren möchten, müssen Sie einen vorhandenen virtuellen Computer mithilfe von „Änderungsnachverfolgung und Bestand“ aktivieren. 

> [!NOTE]
> Wenn Sie „Änderungsnachverfolgung und Bestand“ aktivieren, werden nur bestimmte Regionen für die Verknüpfung eines Log Analytics-Arbeitsbereichs und eines Automation-Kontos unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](./index.yml) zum Verwalten von Computern.
* Ein [virtueller Computer](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com bei Azure an.

## <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Lösung für Änderungsnachverfolgung und Bestand

1. Navigieren Sie zu Ihrem Automation-Konto, und wählen Sie unter **Konfigurationsverwaltung** eine der Optionen **Bestand** oder **Änderungsnachverfolgung** aus.

2. Wählen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto aus, und klicken Sie auf **Aktivieren**, um „Änderungsnachverfolgung und Bestand“ zu aktivieren. Die Einrichtung dauert bis zu 15 Minuten.

    ![Aktivieren der Lösung für Änderungsnachverfolgung und Bestand](media/automation-enable-changes-from-auto-acct/onboardsolutions.png)

## <a name="enable-azure-vms"></a>Aktivieren von Azure-VMs

1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Bestand** oder **Änderungsnachverfolgung** aus.

2. Klicken Sie auf **+ Azure-VMs hinzufügen**, und wählen Sie mindestens einen virtuellen Computer aus der Liste aus. Virtuelle Computer, die nicht aktiviert werden können, sind abgeblendet und können nicht ausgewählt werden. Virtuelle Azure-Computer können sich unabhängig vom Standort Ihres Automation-Kontos in einer beliebigen Region befinden. 

3. Klicken Sie auf **Aktivieren**, um die ausgewählten virtuellen Computer der gespeicherten Computergruppensuche für das Feature hinzuzufügen. Weitere Informationen finden Sie unter [Einschränken des Bereitstellungsumfangs für „Änderungsnachverfolgung und Bestand“](automation-scope-configurations-change-tracking.md).

    ![Aktivieren von Azure-VMs](media/automation-enable-changes-from-auto-acct/enable-azure-vms.png)

## <a name="enable-non-azure-vms"></a>Aktivieren Azure-fremder virtueller Computer

Computer, die nicht in Azure enthalten sind, müssen manuell hinzugefügt werden. 

1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Bestand** oder **Änderungsnachverfolgung** aus.

2. Klicken Sie auf **Nicht-Azure-Computer hinzufügen**. Daraufhin wird ein neues Browserfenster mit [Anweisungen zum Installieren und Konfigurieren des Log Analytics-Agents für Windows](../azure-monitor/platform/log-analytics-agent.md) geöffnet, sodass der Computer damit beginnen kann, Vorgänge für „Änderungsnachverfolgung und Bestand“ zu melden. Wenn Sie einen Computer aktivieren, der aktuell von Operations Manager verwaltet wird, ist kein neuer Agent erforderlich. Die Arbeitsbereichsinformationen werden in den vorhandenen Agent eingegeben.

## <a name="enable-machines-in-the-workspace"></a>Aktivieren von Computern im Arbeitsbereich

Manuell installierte Computer oder Computer, von denen bereits Berichte für Ihren Arbeitsbereich erstellt werden, müssen Azure Automation hinzugefügt werden, damit „Änderungsnachverfolgung und Bestand“ aktiviert werden kann. 

1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Bestand** oder **Änderungsnachverfolgung** aus.

2. Wählen Sie **Computer verwalten** aus. Die Schaltfläche **Computer verwalten** ist möglicherweise abgeblendet, wenn Sie zuvor die Option **Auf allen verfügbaren und zukünftigen Computern aktivieren** ausgewählt haben.

    ![Gespeicherte Suchvorgänge](media/automation-enable-changes-from-auto-acct/managemachines.png)

3. Wenn Sie „Änderungsnachverfolgung und Bestand“ für alle verfügbaren Computer aktivieren möchten, wählen Sie auf der Seite „Computer verwalten“ die Option **Auf allen verfügbaren Computern aktivieren** aus. Diese Aktion deaktiviert das Steuerelement zum einzelnen Hinzufügen von Computern. Mit dieser Aufgabe werden alle Namen der Computer hinzugefügt, die Berichte für den Arbeitsbereich und die Computergruppe der gespeicherten Suchabfrage erstellen. Diese Aktion deaktiviert die Schaltfläche **Computer verwalten**.

4. Wenn Sie das Feature für alle verfügbaren und zukünftigen Computer aktivieren möchten, wählen Sie **Auf allen verfügbaren und zukünftigen Computern aktivieren** aus. Mit dieser Option werden die gespeicherten Suchen und die Bereichskonfigurationen aus dem Arbeitsbereich gelöscht, und das Feature wird für alle Azure-Computer und Azure-fremden Computer geöffnet, von denen Berichte an den Arbeitsbereich übermittelt werden. Bei Verwendung dieser Aktion wird die Schaltfläche **Computer verwalten** dauerhaft deaktiviert, da keine Bereichskonfiguration mehr vorhanden ist.

5. Die Bereichskonfigurationen können bei Bedarf durch Hinzufügen der anfänglichen gespeicherten Suchen wieder hinzugefügt werden. Weitere Informationen finden Sie unter [Einschränken des Bereitstellungsumfangs für „Änderungsnachverfolgung und Bestand“](automation-scope-configurations-change-tracking.md).

6. Wenn Sie das Feature für einen oder mehrere Computer aktivieren möchten, wählen Sie **Auf ausgewählten Computern aktivieren** aus, und klicken Sie neben jedem Computer, für den das Feature aktiviert werden soll, auf **Hinzufügen**. Dadurch werden die Namen der ausgewählten Computer der gespeicherten Computergruppensuchabfrage für das Feature hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Arbeiten mit dem Feature finden Sie unter [Verwalten der Änderungsnachverfolgung und des Bestands](change-tracking-file-contents.md).
* Informationen zur Behandlung allgemeiner Probleme mit dem Feature finden Sie unter [Behandeln von Problemen mit Änderungsnachverfolgung und Bestand](troubleshoot/change-tracking.md).
