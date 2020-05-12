---
title: Verwenden der Azure Automation-Updateverwaltung mit Configuration Manager-Clients
description: Dieser Artikel hilft Ihnen beim Konfigurieren von Microsoft Endpoint Configuration Manager mit dieser Lösung, um Softwareupdates auf ConfigMgr-Clients bereitzustellen.
services: automation
ms.subservice: update-management
author: mgoedtel
ms.author: magoedte
ms.date: 12/11/2019
ms.topic: conceptual
ms.openlocfilehash: 2dbc33aa56c7e930596ba6806ba1dd2e128e1c82
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780214"
---
# <a name="deploy-updates-to-microsoft-endpoint-configuration-manager-clients-with-update-management"></a>Bereitstellen von Updates in Microsoft Endpoint Configuration Manager mit der Updateverwaltung

Kunden, die in Microsoft Endpoint Configuration Manager investiert haben, um PCs, Server und Mobilgeräte zu verwalten, profitieren auch von der Leistungsstärke und dem Funktionsumfang dieser Lösung bei der Verwaltung von Softwareupdates im Rahmen des Softwareupdateverwaltungs-Zyklus.

Sie können Berichte für verwaltete Windows-Server erstellen und die Server aktualisieren, indem Sie Softwareupdatebereitstellungen in Configuration Manager erstellen und vorab bereitstellen. Außerdem können Sie den detaillierten Status abgeschlossener Updatebereitstellungen mithilfe der [Updateverwaltung](automation-update-management.md) abrufen. Wenn Sie Configuration Manager zum Aktualisieren von Complianceberichten, aber nicht zum Verwalten von Updatebereitstellungen für Ihre Windows-Server verwenden, können Sie für die Berichterstellung weiterhin Configuration Manager nutzen, während Sicherheitsupdates mit der Updateverwaltung verwaltet werden.

## <a name="prerequisites"></a>Voraussetzungen

* Sie müssen Ihrem Automation-Konto die [Updateverwaltung](automation-update-management.md) hinzugefügt haben.
* Windows-Server, die zurzeit von Ihrer Configuration Manager-Umgebung verwaltet werden, müssen auch Berichte an den Log Analytics-Arbeitsbereich übermitteln, in dem auch die Updateverwaltung aktiviert ist.
* Dieses Feature ist in Configuration Manager Current Branch (Version 1606 und höher) aktiviert. Informationen zum Integrieren der zentralen Configuration Manager-Verwaltungswebsite oder einer eigenständigen primären Website mit Azure Monitor-Protokollen und zum Importieren von Sammlungen finden Sie unter [Herstellen einer Verbindung zwischen Configuration Manager und Azure Monitor-Protokolle](../azure-monitor/platform/collect-sccm.md).  
* Windows-Agents müssen entweder für die Kommunikation mit einem WSUS-Server (Windows Server Update Services) konfiguriert sein oder über Zugriff auf Microsoft-Update verfügen, wenn sie keine Sicherheitsupdates von Configuration Manager erhalten.

Wie Sie die in Azure IaaS gehosteten Clients mit Ihrer vorhandenen Configuration Manager-Umgebung verwalten, richtet sich primär nach der Verbindung zwischen Azure-Rechenzentren und Ihrer Infrastruktur. Diese Verbindung wirkt sich auf alle Designänderungen aus, die Sie an Ihrer Configuration Manager-Infrastruktur vornehmen müssen, und auf die Kosten, die anfallen, um diese Änderungen zu unterstützen. Informationen zu den Planungsüberlegungen, die Sie berücksichtigen müssen, finden Sie unter [Configuration Manager in Azure – häufig gestellte Fragen](https://docs.microsoft.com/configmgr/core/understand/configuration-manager-on-azure#networking).

## <a name="configuration"></a>Konfiguration

### <a name="manage-software-updates-from-configuration-manager"></a>Verwalten von Softwareupdates von Configuration Manager

Führen Sie folgende Schritte aus, wenn Sie Updatebereitstellungen weiterhin über Configuration Manager verwalten möchten. Azure Automation stellt eine Verbindung mit Configuration Manager her, um Updates auf Clientcomputer anzuwenden, die mit Ihrem Log Analytics-Arbeitsbereich verbunden sind. Die Updateinhalte sind im Cache der Clientcomputer verfügbar, so als würde die Bereitstellung von Configuration Manager verwaltet.

1. Erstellen Sie mithilfe des unter [Bereitstellen von Softwareupdates](https://docs.microsoft.com/configmgr/sum/deploy-use/deploy-software-updates) beschriebenen Prozesses eine Softwareupdatebereitstellung von der Top-Level-Website in Ihrer Configuration Manager-Hierarchie. Die einzige Einstellung, die anders konfiguriert werden muss als bei einer Standardbereitstellung, ist die Option **Softwareupdates nicht installieren**, um das Downloadverhalten des Bereitstellungspakets zu steuern. Dieses Verhalten wird von der Updateverwaltung durch Erstellen einer geplanten Updatebereitstellung im nächsten Schritt gesteuert.

1. Wählen Sie in Azure Automation die Option **Updateverwaltung** aus. Erstellen Sie anhand der unter [Erstellen einer Updatebereitstellung](automation-tutorial-update-management.md#schedule-an-update-deployment) beschriebenen Schritte eine neue Bereitstellung, und wählen Sie in der Dropdownliste **Typ** den Eintrag **Importierte Gruppen** aus, um die entsprechende Configuration Manager-Sammlung auszuwählen. Berücksichtigen Sie dabei die folgenden wichtigen Punkte: a. Wenn in der ausgewählten Configuration Manager-Gerätesammlung ein Wartungsfenster definiert ist, verwenden die Elemente der Sammlung dieses Fenster anstelle der Einstellung **Dauer**, die in der geplanten Bereitstellung definiert ist.
    b. Elemente der Zielsammlung müssen über eine Verbindung mit dem Internet verfügen (entweder direkt, über einen Proxyserver oder über das Log Analytics-Gateway).

Nach Abschluss der Updatebereitstellung über Azure Automation installieren die Zielcomputer, die Mitglieder der ausgewählten Computergruppe sind, die Updates zum geplanten Zeitpunkt aus ihrem lokalen Clientcache. Sie können den [Status der Updatebereitstellung anzeigen](automation-tutorial-update-management.md#view-results-of-an-update-deployment), um die Ergebnisse Ihrer Bereitstellung zu überwachen.

### <a name="manage-software-updates-from-azure-automation"></a>Verwalten von Softwareupdates über Azure Automation

Um Updates für virtuelle Windows Server-Computer zu verwalten, die Configuration Manager-Clients sind, müssen Sie die Clientrichtlinie so konfigurieren, dass die Funktion der Softwareupdateverwaltung für alle von der Updateverwaltung verwalteten Clients deaktiviert wird. Standardmäßig gelten Clienteinstellungen für alle Geräte in der Hierarchie. Weitere Informationen zu dieser Richtlinieneinstellung und ihrer Konfiguration finden Sie unter [Konfigurieren von Clienteinstellungen in Configuration Manager](https://docs.microsoft.com/configmgr/core/clients/deploy/configure-client-settings).

Nachdem Sie diese Konfigurationsänderung vorgenommen haben, erstellen Sie anhand der unter [Erstellen einer Updatebereitstellung](automation-tutorial-update-management.md#schedule-an-update-deployment) beschriebenen Schritte eine neue Bereitstellung, und wählen Sie in der Dropdownliste **Typ** den Eintrag **Importierte Gruppen** aus, um die entsprechende Configuration Manager-Sammlung auszuwählen.

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine neue Bereitstellung, indem Sie die unter [Erstellen einer Updatebereitstellung](automation-tutorial-update-management.md#schedule-an-update-deployment) beschriebenen Schritte ausführen.