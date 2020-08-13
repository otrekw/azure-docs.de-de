---
title: Integrieren der Azure Automation-Updateverwaltung mit Windows Endpoint Configuration Manager
description: Dieser Artikel hilft Ihnen beim Konfigurieren von Microsoft Endpoint Configuration Manager mit der Updateverwaltung, um Softwareupdates auf Manager-Clients bereitzustellen.
services: automation
ms.subservice: update-management
ms.date: 07/28/2020
ms.topic: conceptual
ms.openlocfilehash: 4eccd53fbf3b883d6e3ba6d2c7c80ddd4ae188af
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87449586"
---
# <a name="integrate-update-management-with-windows-endpoint-configuration-manager"></a>Integrieren der Updateverwaltung mit Windows Endpoint Configuration Manager

Kunden, die in Microsoft Endpoint Configuration Manager investiert haben, um PCs, Server und Mobilgeräte zu verwalten, profitieren auch von der Leistungsstärke und dem Funktionsumfang dieser Lösung bei der Verwaltung von Softwareupdates im Rahmen des Softwareupdateverwaltungs-Zyklus.

Sie können Berichte für verwaltete Windows-Server erstellen und die Server aktualisieren, indem Sie Softwareupdatebereitstellungen in Windows Endpoint Configuration Manager erstellen und vorab bereitstellen. Außerdem können Sie den detaillierten Status abgeschlossener Updatebereitstellungen mithilfe der [Updateverwaltung](update-mgmt-overview.md) abrufen. Wenn Sie Windows Endpoint Configuration Manager zum Aktualisieren von Complianceberichten, aber nicht zum Verwalten von Updatebereitstellungen für Ihre Windows-Server verwenden, können Sie für die Berichterstellung weiterhin Endpoint Configuration Manager nutzen, während Sicherheitsupdates mit der Azure Automation-Updateverwaltung verwaltet werden.

>[!NOTE]
>Obwohl die Updateverwaltung die Updatebewertung und das Patchen von Windows Server 2008 R2 unterstützt, werden von Endpoint Configuration Manager verwaltete Clients mit diesem Betriebssystem nicht unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

* Sie müssen Ihrem Automation-Konto die [Azure Automation-Updateverwaltung](update-mgmt-overview.md) hinzugefügt haben.
* Windows-Server, die zurzeit von Ihrer Windows Endpoint Configuration Manager-Umgebung verwaltet werden, müssen auch Berichte an den Log Analytics-Arbeitsbereich übermitteln, in dem auch die Updateverwaltung aktiviert ist.
* Dieses Feature ist im aktuellen Branch von Windows Endpoint Configuration Manager ab Version 1606 aktiviert. Informationen zum Integrieren der zentralen Windows Endpoint Configuration Manager-Verwaltungswebsite oder einer eigenständigen primären Website mit Azure Monitor-Protokollen und zum Importieren von Sammlungen finden Sie unter [Herstellen einer Verbindung zwischen Configuration Manager und Azure Monitor-Protokollen](../../azure-monitor/platform/collect-sccm.md).  
* Windows-Agents müssen entweder für die Kommunikation mit einem WSUS-Server (Windows Server Update Services) konfiguriert sein oder über Zugriff auf Microsoft-Update verfügen, wenn sie keine Sicherheitsupdates von Windows Endpoint Configuration Manager erhalten.

Wie Sie die in Azure IaaS gehosteten Clients mit Ihrer vorhandenen Windows Endpoint Configuration Manager-Umgebung verwalten, richtet sich primär nach der Verbindung zwischen Azure-Rechenzentren und Ihrer Infrastruktur. Diese Verbindung wirkt sich auf alle Designänderungen aus, die Sie an Ihrer Windows Endpoint Configuration Manager-Infrastruktur vornehmen müssen, und auf die Kosten, die aufgrund dieser Änderungen anfallen. Informationen zu den Planungsüberlegungen, die Sie berücksichtigen müssen, finden Sie unter [Configuration Manager in Azure – häufig gestellte Fragen](/configmgr/core/understand/configuration-manager-on-azure#networking).

## <a name="manage-software-updates-from-windows-endpoint-configuration-manager"></a>Verwalten von Softwareupdates mit Windows Endpoint Configuration Manager

Führen Sie folgende Schritte aus, wenn Sie Updatebereitstellungen weiterhin über Windows Endpoint Configuration Manager verwalten möchten. Azure Automation stellt eine Verbindung mit Windows Endpoint Configuration Manager her, um Updates auf Clientcomputer anzuwenden, die mit Ihrem Log Analytics-Arbeitsbereich verbunden sind. Die Updateinhalte sind im Cache der Clientcomputer verfügbar, so als würde die Bereitstellung von Windows Endpoint Configuration Manager verwaltet.

1. Erstellen Sie mithilfe des unter [Bereitstellen von Softwareupdates](/configmgr/sum/deploy-use/deploy-software-updates) beschriebenen Prozesses eine Softwareupdatebereitstellung von der Top-Level-Website in Ihrer Windows Endpoint Configuration Manager-Hierarchie. Die einzige Einstellung, die anders konfiguriert werden muss als bei einer Standardbereitstellung, ist die Option **Softwareupdates nicht installieren**, um das Downloadverhalten des Bereitstellungspakets zu steuern. Dieses Verhalten wird von der Updateverwaltung durch Erstellen einer geplanten Updatebereitstellung im nächsten Schritt gesteuert.

2. Wählen Sie in Azure Automation die Option **Updateverwaltung** aus. Erstellen Sie anhand der unter [Erstellen einer Updatebereitstellung](update-mgmt-deploy-updates.md#schedule-an-update-deployment) beschriebenen Schritte eine neue Bereitstellung, und wählen Sie in der Dropdownliste **Typ** den Eintrag **Importierte Gruppen** aus, um die entsprechende Windows Endpoint Configuration Manager-Sammlung auszuwählen. Berücksichtigen Sie dabei die folgenden wichtigen Punkte:

    a. Wenn in der ausgewählten Windows Endpoint Configuration Manager-Gerätesammlung ein Wartungsfenster definiert ist, verwenden die Elemente der Sammlung dieses Fenster anstelle der Einstellung **Dauer**, die in der geplanten Bereitstellung definiert ist.

    b. Elemente der Zielsammlung müssen über eine Verbindung mit dem Internet verfügen (entweder direkt, über einen Proxyserver oder über das Log Analytics-Gateway).

Nach Abschluss der Updatebereitstellung über Azure Automation installieren die Zielcomputer, die Mitglieder der ausgewählten Computergruppe sind, die Updates zum geplanten Zeitpunkt aus ihrem lokalen Clientcache. Sie können den [Status der Updatebereitstellung anzeigen](update-mgmt-deploy-updates.md#check-deployment-status), um die Ergebnisse Ihrer Bereitstellung zu überwachen.

## <a name="manage-software-updates-from-azure-automation"></a>Verwalten von Softwareupdates über Azure Automation

Um Updates für Windows Server-VMs zu verwalten, die Windows Endpoint Configuration Manager-Clients sind, müssen Sie die Clientrichtlinie so konfigurieren, dass die Funktion der Softwareupdateverwaltung für alle von der Updateverwaltung verwalteten Clients deaktiviert wird. Standardmäßig gelten Clienteinstellungen für alle Geräte in der Hierarchie. Weitere Informationen zu dieser Richtlinieneinstellung und ihrer Konfiguration finden Sie unter [Konfigurieren von Clienteinstellungen in Configuration Manager](/configmgr/core/clients/deploy/configure-client-settings).

Nachdem Sie diese Konfigurationsänderung vorgenommen haben, erstellen Sie anhand der unter [Erstellen einer Updatebereitstellung](update-mgmt-deploy-updates.md#schedule-an-update-deployment) beschriebenen Schritte eine neue Bereitstellung, und wählen Sie in der Dropdownliste **Typ** den Eintrag **Importierte Gruppen** aus, um die entsprechende Windows Endpoint Configuration Manager-Sammlung auszuwählen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Einrichten eines Integrationszeitplans finden Sie unter [Planen einer Updatebereitstellung](update-mgmt-deploy-updates.md#schedule-an-update-deployment).
