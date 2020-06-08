---
title: Konfigurieren von Windows Update-Einstellungen für die Azure Automation-Updateverwaltung
description: In diesem Artikel wird beschrieben, wie Sie die Windows Update-Einstellungen für die Azure Automation-Updateverwaltung konfigurieren.
services: automation
ms.subservice: update-management
ms.date: 05/04/2020
ms.topic: conceptual
ms.openlocfilehash: ea05e75c0d1db1ef27ae2e8e9364327528a7c8ed
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83837160"
---
# <a name="configure-windows-update-settings-for-azure-automation-update-management"></a>Konfigurieren von Windows Update-Einstellungen für die Azure Automation-Updateverwaltung

Die Azure Automation-Updateverwaltung verwendet den [Windows Update-Client](https://docs.microsoft.com//windows/deployment/update/windows-update-overview), um Windows-Updates herunterzuladen und zu installieren. Beim Herstellen einer Verbindung mit Windows Server Update Services (WSUS) oder Windows Update werden vom Windows Update-Client bestimmte Einstellungen verwendet. Viele dieser Einstellungen können verwaltet werden mit:

- Editor für lokale Gruppenrichtlinien
- Gruppenrichtlinie
- PowerShell
- Direktem Bearbeiten der Registrierung

Die Updateverwaltung berücksichtigt viele der Einstellungen, die zum Steuern des Windows Update-Clients angegeben werden. Wenn Sie Einstellungen für die Aktivierung Windows-fremder Updates verwenden, verwaltet die Updateverwaltung diese Updates ebenfalls. Wenn Sie das Herunterladen von Updates vor der Updatebereitstellung ermöglichen, können Bereitstellungen schneller, effizienter und in der Regel innerhalb des Wartungsfensters durchgeführt werden.

Weitere Empfehlungen zum Einrichten von WSUS in Ihrem Azure-Abonnement und wie Sie Ihre virtuellen Windows-Computer auf sichere Weise auf dem neuesten Stand halten, finden Sie unter [Planen Ihrer Bereitstellung für die Aktualisierung von virtuellen Windows-Computern in Azure mithilfe von WSUS](https://docs.microsoft.com/azure/architecture/example-scenario/wsus/).

## <a name="pre-download-updates"></a>Vorzeitiges Herunterladen von Updates

Um zu konfigurieren, dass Updates automatisch heruntergeladen werden, ohne sie automatisch zu installieren, können Sie mithilfe der Gruppenrichtlinie [die Einstellung „Automatische Updates“](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) auf „3“ festlegen. Diese Einstellung ermöglicht das Herunterladen der erforderlichen Updates im Hintergrund und benachrichtigt Sie, dass die Updates zur Installation bereit sind. Dadurch behält die Updateverwaltung die Kontrolle über die Zeitpläne, und die Updates können außerhalb des Wartungsfensters der Updateverwaltung heruntergeladen werden. Dieses Verhalten verhindert `Maintenance window exceeded`-Fehler in der Updateverwaltung.

Sie können diese Einstellung in PowerShell aktivieren:

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="configure-reboot-settings"></a>Konfigurieren von Neustarteinstellungen

Die unter [Konfigurieren automatischer Updates durch Bearbeiten der Registrierung](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) und [Zum Verwalten des Neustarts verwendete Registrierungsschlüssel](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) aufgeführten Registrierungsschlüssel können den Neustart Ihrer Computer bewirken, auch wenn Sie in den Einstellungen unter **Updatebereitstellung** die Option **Nie neu starten** angegeben haben. Konfigurieren Sie diese Registrierungsschlüssel so, wie es für Ihre Umgebung am besten passt.

## <a name="enable-updates-for-other-microsoft-products"></a>Aktivieren von Updates für andere Microsoft-Produkte

Standardmäßig ist der Windows Update-Client so konfiguriert, dass er nur Updates für Windows zur Verfügung stellt. Wenn Sie die Einstellung **Updates für andere Microsoft-Produkte bereitstellen, wenn ein Windows-Update ausgeführt wird** aktivieren, erhalten Sie auch Updates für andere Produkte – einschließlich Sicherheitspatches für Microsoft SQL Server und andere Software von Microsoft. Sie können diese Option konfigurieren, wenn Sie die neuesten [administrativen Vorlagendateien](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra), die ab Windows 2016 verfügbar sind, heruntergeladen und kopiert haben.

Wenn Sie über Computer verfügen, auf denen Windows Server 2012 R2 ausgeführt wird, können Sie diese Einstellung nicht über die Gruppenrichtlinie konfigurieren. Führen Sie auf diesen Computern den folgenden PowerShell-Befehl aus:

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="make-wsus-configuration-settings"></a>Festlegen von WSUS-Konfigurationseinstellungen

Die Updateverwaltung unterstützt WSUS-Einstellungen. Sie können entsprechend den Anweisungen in [Internen Pfad für den Microsoft Updatedienst angeben](/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location) Quellen für die Suche nach und das Herunterladen von Updates angeben. Standardmäßig ist der Windows Update-Client so konfiguriert, dass Updates von Windows Update heruntergeladen werden. Wenn Sie einen WSUS-Server als Quelle für Ihre Computer angeben, schlägt die Update-Bereitstellung fehl, sofern die Updates in WSUS nicht genehmigt werden. 

Um Computer auf den internen Updatedienst zu beschränken, aktivieren Sie [Keine Verbindungen mit Windows Update-Internetadressen herstellen](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#do-not-connect-to-any-windows-update-internet-locations). 

## <a name="next-steps"></a>Nächste Schritte

Planen Sie eine Updatebereitstellung entsprechend der Anleitung unter [Verwalten von Updates und Patches für Ihre virtuellen Azure-Computer](automation-tutorial-update-management.md).
