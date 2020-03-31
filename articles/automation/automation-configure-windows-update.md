---
title: Konfigurieren von Windows Update-Einstellungen für die Zusammenarbeit mit Azure-Updateverwaltung
description: In diesem Artikel werden die Windows Update-Einstellungen beschrieben, die Sie für die Zusammenarbeit mit der Azure-Updateverwaltung konfigurieren.
services: automation
ms.subservice: update-management
ms.date: 03/02/2020
ms.topic: conceptual
ms.openlocfilehash: 7f226c4d297d25644b2650d085655f70d8326927
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235638"
---
# <a name="configure-windows-update-settings-for-update-management"></a>Konfigurieren von Windows Update-Einstellungen für Updateverwaltung

Die Azure-Updateverwaltung verwendet den [Windows Update-Client](https://docs.microsoft.com//windows/deployment/update/windows-update-overview), um Windows-Updates herunterzuladen und zu installieren. Beim Herstellen einer Verbindung mit Windows Server Update Services (WSUS) oder Windows Update werden vom Windows Update-Client bestimmte Einstellungen verwendet. Viele dieser Einstellungen können verwaltet werden mit:

- Editor für lokale Gruppenrichtlinien
- Gruppenrichtlinie
- PowerShell
- Direktem Bearbeiten der Registrierung

Die Updateverwaltung berücksichtigt viele der Einstellungen, die zum Steuern des Windows Update-Clients angegeben werden. Wenn Sie Einstellungen für die Aktivierung Windows-fremder Updates verwenden, verwaltet die Updateverwaltung diese Updates ebenfalls. Wenn Sie das Herunterladen von Updates vor der Updatebereitstellung ermöglichen, können Bereitstellungen schneller, effizienter und in der Regel innerhalb des Wartungsfensters durchgeführt werden.

## <a name="pre-download-updates"></a>Vorzeitiges Herunterladen von Updates

Um das automatische Herunterladen von Updates, ohne sie zu installieren, per Gruppenrichtlinie zu konfigurieren, können Sie die Einstellung [Automatische Updates konfigurieren](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) auf **3** festlegen. Diese Einstellung ermöglicht das Herunterladen der erforderlichen Updates im Hintergrund und benachrichtigt Sie, dass die Updates zur Installation bereit sind. Dadurch behält die Updateverwaltung die Kontrolle über die Zeitpläne, und die Updates können außerhalb des Wartungsfensters der Updateverwaltung heruntergeladen werden. Dieses Verhalten verhindert Fehler im Zusammenhang mit der **Überschreitung des Wartungsfensters** in der Updateverwaltung.

Sie können diese Einstellung mithilfe von PowerShell aktivieren, indem Sie den folgenden Befehl ausführen:

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="configure-reboot-settings"></a>Konfigurieren von Neustarteinstellungen

Die unter [Konfigurieren automatischer Updates durch Bearbeiten der Registrierung](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) und [Zum Verwalten des Neustarts verwendete Registrierungsschlüssel](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) aufgeführten Registrierungsschlüssel können den Neustart Ihrer Computer bewirken, auch wenn Sie in den Einstellungen unter **Updatebereitstellung** die Option **Nie neu starten** angegeben haben. Konfigurieren Sie diese Registrierungsschlüssel so, wie es für Ihre Umgebung am besten passt.

## <a name="enable-updates-for-other-microsoft-products"></a>Aktivieren von Updates für andere Microsoft-Produkte

Standardmäßig ist der Windows Update-Client so konfiguriert, dass er nur Updates für Windows zur Verfügung stellt. Wenn Sie die Einstellung **Updates für andere Microsoft-Produkte bereitstellen, wenn ein Windows-Update ausgeführt wird** aktivieren, erhalten Sie auch Updates für andere Produkte – einschließlich Sicherheitspatches für Microsoft SQL Server und andere Software von Microsoft. Diese Option kann konfiguriert werden, wenn Sie die neuesten [administrativen Vorlagendateien](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra), die ab Windows 2016 verfügbar sind, heruntergeladen und kopiert haben.

Wenn Sie mit Windows Server 2012 R2 arbeiten, kann diese Einstellung nicht über Gruppenrichtlinien konfiguriert werden. Führen Sie auf diesen Computern den folgenden PowerShell-Befehl aus. Die Updateverwaltung berücksichtigt diese Einstellung.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="wsus-configuration-settings"></a>WSUS-Konfigurationseinstellungen

Die Updateverwaltung unterstützt WSUS-Einstellungen. Im Anschluss finden Sie eine Liste mit den WSUS-Einstellungen, die Sie für die Updateverwaltung konfigurieren können.

### <a name="intranet-microsoft-update-service-location"></a>Interner Pfad für den Microsoft Updatedienst

Unter [Internen Pfad für den Microsoft Updatedienst angeben](/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location) können Sie Quellen für die Suche nach und das Herunterladen von Updates angeben. Standardmäßig ist der Windows Update-Client so konfiguriert, dass Updates von Windows Update heruntergeladen werden. Wenn Sie einen WSUS-Server als Quelle für Ihre Computer angeben, schlägt die Update-Bereitstellung fehl, sofern die Updates in WSUS nicht genehmigt werden. 

Um Computer auf nur diesen internen Update-Dienst zu beschränken, aktivieren Sie [Keine Verbindungen mit Windows Update-Internetadressen herstellen](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#do-not-connect-to-any-windows-update-internet-locations). 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Windows Update-Einstellungen konfiguriert haben, können Sie eine Updatebereitstellung planen. Eine entsprechende Anleitung finden Sie unter [Verwalten von Updates und Patches für Ihre virtuellen Azure-Computer](automation-tutorial-update-management.md).
