---
title: Verwalten von Updates und Patches für Ihre VMs in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie die Updateverwaltung verwenden können, um Updates und Patches für Ihre Azure- und Nicht-Azure-VMs zu verwalten.
services: automation
ms.subservice: update-management
ms.topic: conceptual
ms.date: 07/28/2020
ms.custom: mvc
ms.openlocfilehash: e4f630931e3cd79a46a539955b45d72e5b573271
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87449594"
---
# <a name="manage-updates-and-patches-for-your-vms"></a>Verwalten von Updates und Patches für Ihre VMs

Softwareupdates in der Azure Automation-Updateverwaltung bieten eine Gruppe von Tools und Ressourcen, mit denen die komplexe Aufgabe der Nachverfolgung und Anwendung von Softwareupdates auf Computer in Azure und Hybrid Cloud leichter verwaltet werden kann. Ein effektiver Softwareupdate-Verwaltungsprozess ist erforderlich, um die Betriebseffektivität aufrechtzuerhalten, Sicherheitsprobleme zu beheben und die Risiken gesteigerter Cybersicherheitsbedrohungen zu reduzieren. Da die Technologie sich kontinuierlich wandelt und immer wieder neue Sicherheitsrisiken auftreten, gebührt der effektiven Verwaltung von Softwareupdates besonderes Augenmerk.

> [!NOTE]
> Die Updateverwaltung unterstützt die Bereitstellung von Erstanbieterupdates sowie deren Vorabdownload. Hierzu sind Änderungen an den Systemen erforderlich, die aktualisiert werden. Informationen zum Konfigurieren dieser Einstellungen auf Ihren Systemen finden Sie unter [Konfigurieren von Windows Update-Einstellungen für die Azure Automation-Updateverwaltung](update-mgmt-configure-wuagent.md).

Bevor Sie versuchen, Updates für Ihre virtuellen Computer zu verwalten, vergewissern Sie sich, dass Sie die Updateverwaltung mit einer der folgenden Methoden auf ihnen aktiviert haben:

* [Aktivieren der Updateverwaltung über ein Automation-Konto](update-mgmt-enable-automation-account.md)
* [Aktivieren der Updateverwaltung mittels Durchsuchen im Azure-Portal](update-mgmt-enable-portal.md)
* [Aktivieren der Updateverwaltung über ein Runbook](update-mgmt-enable-runbook.md)
* [Aktivieren der Updateverwaltung über einen virtuellen Azure-Computer](update-mgmt-enable-vm.md)

## <a name="limit-the-scope-for-the-deployment"></a><a name="scope-configuration"></a>Beschränken des Bereichs für die Bereitstellung

Die Updateverwaltung verwendet eine Bereichskonfiguration innerhalb des Arbeitsbereichs, um die Computer, die Updates erhalten sollen, zu erreichen. Weitere Informationen finden Sie unter [Einschränken des Bereitstellungsbereichs für die Updateverwaltung](update-mgmt-scope-configuration.md).

## <a name="compliance-assessment"></a>Kompatibilitätsbewertung

Bevor Sie Softwareupdates auf ihren Computern bereitstellen, überprüfen Sie die Ergebnisse der Updatekompatibilitätsbewertung für aktivierte Computer. Für jedes Softwareupdate wird der zugehörige Kompatibilitätszustand aufgezeichnet, nach Abschluss der Bewertung erfasst und per Massenvorgang an Azure Monitor-Protokolle weitergeleitet.

Auf einem Windows-Computer wird der Konformitätsscan standardmäßig alle 12 Stunden ausgeführt. Außer dem geplanten Scan wird der Update-Konformitätsscan innerhalb von 15 Minuten nach dem Neustart des Log Analytics-Agent für Windows sowie vor und nach der Updateinstallation initiiert. Lesen Sie auch unbedingt unsere Empfehlungen zum [Konfigurieren von Windows Update-Einstellungen für die Azure Automation-Updateverwaltung](update-mgmt-configure-wuagent.md), um Probleme zu vermeiden, die eine korrekte Verwaltung verhindern.

Für einen Linux-Computer wird der Konformitätsscan standardmäßig jede Stunde durchgeführt. Wenn der Log Analytics-Agent für Linux neu gestartet wird, wird ein Konformitätsscan innerhalb von 15 Minuten eingeleitet.

Die Kompatibilitätsergebnisse werden für jeden bewerteten Computer in der Updateverwaltung angezeigt. Bis für einen neu für die Verwaltung aktivierten Computer aktualisierte Daten im Dashboard angezeigt werden, können bis zu 30 Minuten vergehen.

Weitere Informationen zum Anzeigen von Kompatibilitätsergebnissen finden Sie unter [Anzeigen von Updatebewertungen](update-mgmt-view-update-assessments.md).

## <a name="deploy-updates"></a>Bereitstellen von Updates

Nach Überprüfung der Kompatibilitätsergebnisse ist die Softwareupdate-Bereitstellungsphase der Vorgang der Bereitstellung von Softwareupdates. Planen Sie zum Installieren von Updates eine Bereitstellung, die Ihrem Releasezeitplan und Wartungsfenster entspricht. Sie können auswählen, welche Updatetypen in die Bereitstellung eingeschlossen werden sollen. Beispielsweise können Sie kritische oder Sicherheitsupdates einschließen und Updaterollups ausschließen.

Lesen Sie [Bereitstellen von Updates und Überprüfen von Ergebnissen](update-mgmt-deploy-updates.md), um zu erfahren, wie Sie eine Updatebereitstellung planen.

## <a name="review-update-deployments"></a>Überprüfen von Updatebereitstellungen

Nachdem die Bereitstellung abgeschlossen ist, überprüfen Sie den Prozess, um den Erfolg der Updatebereitstellung nach Computer oder Zielgruppe zu ermitteln. Weitere Informationen zum Überwachen des Bereitstellungsstatus finden Sie unter [Überprüfen des Bereitstellungsstatus](update-mgmt-deploy-updates.md#check-deployment-status).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Erstellen von Warnungen, die Sie über die Ergebnisse der Updatebereitstellung informieren, finden Sie unter [Erstellen von Warnungen für die Updateverwaltung](update-mgmt-configure-alerts.md).

* Sie können [Azure Monitor-Protokolle abfragen](update-mgmt-query-logs.md), um Updatebewertungen, Bereitstellungen und andere verwandte Verwaltungsaufgaben zu analysieren. Es umfasst vordefinierte Abfragen, die Ihnen beim Einstieg helfen sollen.