---
title: Neuheiten in Windows Virtual Desktop - Azure
description: Neue Features und Produktupdates für Windows Virtual Desktop.
author: Heidilohr
ms.topic: overview
ms.date: 08/07/2020
ms.author: helohr
ms.reviewer: thhickli; darank
manager: lizross
ms.openlocfilehash: 8663fc3f016fadcd4f4c99acd800cd0ccf8844f8
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "88003524"
---
# <a name="whats-new-in-windows-virtual-desktop"></a>Neuheiten in Windows Virtual Desktop

Windows Virtual Desktop wird regelmäßig aktualisiert. In diesem Artikel werden Sie über Folgendes informiert:

- Die neuesten Updates
- Neue Funktionen
- Verbesserungen an vorhandenen Features
- Behebung von Programmfehlern

Dieser Artikel wird monatlich aktualisiert. Kehren Sie hier oft zurück, um mit neuen Updates auf dem Laufenden zu bleiben.

## <a name="july-2020"></a>Juli 2020  

Ab Juli ist Windows Virtual Desktop mit Azure Resource Manager-Integration allgemein verfügbar.

Hier ist aufgeführt, was sich mit diesem neuen Release geändert hat: 

- Das „Release vom Herbst 2019“ wird jetzt als „Windows Virtual Desktop (klassisch)“ bezeichnet, und das „Release vom Frühjahr 2020“ einfach als „Windows Virtual Desktop“. Weitere Informationen finden Sie in [diesem Blogbeitrag](https://azure.microsoft.com/blog/new-windows-virtual-desktop-capabilities-now-generally-available/). 

Weitere Informationen zu neuen Features finden Sie in [diesem Blogbeitrag](https://techcommunity.microsoft.com/t5/itops-talk-blog/windows-virtual-desktop-spring-update-enters-public-preview/ba-p/1340245). 

### <a name="autoscaling-tool-update"></a>Aktualisierung des Tools für die automatische Skalierung

Die aktuelle Version des Tools für die automatische Skalierung, die sich in der Vorschauphase befunden hat, ist jetzt allgemein verfügbar. Für dieses Tool werden ein Azure Automation-Konto und die Azure-Logik-App verwendet, um virtuelle Sitzungshostcomputer (VMs) in einem Hostpool automatisch herunterzufahren und neu zu starten und so die Infrastrukturkosten zu reduzieren. Weitere Informationen finden Sie unter [Skalieren von Sitzungshosts mit Azure Automation](set-up-scaling-script.md).

### <a name="azure-portal"></a>Azure-Portal

Sie können im Azure-Portal in Windows Virtual Desktop nun Folgendes durchführen: 

- Direktes Zuweisen von Benutzern zu Sitzungshosts von persönlichen Desktops  
- Ändern der Einstellung für die Überprüfungsumgebung für Hostpools 

### <a name="diagnostics"></a>Diagnose

Wir haben einige neue vordefinierte Abfragen für den Log Analytics-Arbeitsbereich veröffentlicht. Navigieren Sie für den Zugriff auf die Abfragen zu **Protokolle**, und wählen Sie unter **Kategorie** die Option **Windows Virtual Desktop** aus. Weitere Informationen finden Sie unter [Verwenden von Log Analytics für die Diagnosefunktion](diagnostics-log-analytics.md).

### <a name="update-for-remote-desktop-client-for-android"></a>Update für Remotedesktopclient für Android

Der [Remotedesktopclient für Android](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) unterstützt jetzt Windows Virtual Desktop-Verbindungen. Ab Version 10.0.7 verfügt der Android-Client über eine neue Benutzeroberfläche, um die Benutzerfreundlichkeit zu erhöhen. Der Client ist auch in Microsoft Authenticator auf Android-Geräten integriert, um beim Abonnieren von Windows Virtual Desktop-Arbeitsbereichen den bedingten Zugriff zu ermöglichen.  

Die vorherige Version des Remotedesktopclients wird jetzt als „Remotedesktop 8“ bezeichnet. Alle vorhandenen Verbindungen, über die Sie in der früheren Version des Clients verfügen, werden für den neuen Client nahtlos übernommen. Der neue Client wurde für die gleiche zugrunde liegende RDP-Haupt-Engine wie für die iOS- und macOS-Clients umgeschrieben. Dies ermöglicht eine schnellere übergreifende Veröffentlichung neuer Features auf allen Plattformen. 

### <a name="teams-update"></a>Teams-Update

Wir haben an Microsoft Teams für Windows Virtual Desktop Verbesserungen vorgenommen. Was am wichtigsten ist: Windows Virtual Desktop unterstützt jetzt die Audio- und Videooptimierung für den Windows Desktop-Client. Die Umleitung verbessert die Latenz, indem direkte Pfade zwischen Benutzern erstellt werden, wenn sie Audio- oder Videofunktionen für Anrufe und Besprechungen nutzen. Eine geringere Entfernung bedeutet weniger Hops, wodurch Optik und Klang verbessert werden. Weitere Informationen finden Sie unter [Verwenden von Microsoft Teams in Windows Virtual Desktop](teams-on-wvd.md).

## <a name="june-2020"></a>Juni 2020

Im letzten Monat haben wir Windows Virtual Desktop mit Azure Resource Manager-Integration als Vorschauversion eingeführt. Dieses Update enthält viele interessante neue Features, über die wir Sie gerne informieren möchten. Hier sind die Neuerungen für diese Version von Windows Virtual Desktop beschrieben.

### <a name="windows-virtual-desktop-is-now-integrated-with-azure-resource-manager"></a>Windows Virtual Desktop ist jetzt in Azure Resource Manager integriert.

Windows Virtual Desktop ist jetzt in Azure Resource Manager integriert. Im neuesten Update sind jetzt alle Windows Virtual Desktop-Objekte in Azure Resource Manager-Ressourcen enthalten. Dieses Update ist auch in Azure RBAC (Role-Based Access Control, rollenbasierte Zugriffssteuerung) integriert. Lesen Sie [Was ist Azure Resource Manager?](../azure-resource-manager/management/overview.md), um mehr zu erfahren.

Diese Änderung umfasst Folgendes:

- Windows Virtual Desktop ist jetzt in das Azure-Portal integriert. Dies bedeutet, dass Sie alles direkt im Portal verwalten können, ohne dass PowerShell, Web-Apps oder Drittanbietertools erforderlich sind. Informationen zu den ersten Schritten finden Sie in unserem Tutorial unter [Erstellen eines Hostpools mit dem Azure-Portal](create-host-pools-azure-marketplace.md).

- Vor diesem Update konnten Sie RemoteApps und Desktops nur für einzelne Benutzer veröffentlichen. Mit Azure Resource Manager können Sie jetzt Ressourcen in Azure Active Directory-Gruppen veröffentlichen.

- Bei der früheren Version von Windows Virtual Desktop gab es vier integrierte Administratorrollen, die Sie einem Mandanten oder Hostpool zuweisen konnten. Diese Rollen sind jetzt in [Azure RBAC (Role-Based Access Control, rollenbasierte Zugriffssteuerung)](../role-based-access-control/overview.md) enthalten. Sie können diese Rollen auf jedes Azure Resource Manager-Objekt von Windows Virtual Desktop anwenden, mit dem Sie ein vollständiges, umfangreiches Delegierungsmodell haben können.

- Bei diesem Update müssen Sie Azure Marketplace oder die GitHub-Vorlage nicht mehr wiederholt ausführen, um einen Hostpool zu erweitern. Zum Erweitern eines Hostpools müssen Sie lediglich im Azure-Portal zu Ihrem Hostpool wechseln und **+ Hinzufügen** auswählen, um weitere Sitzungshosts bereitzustellen.

- Die Hostpoolbereitstellung ist jetzt in die [Azure Shared Image Gallery](../virtual-machines/windows/shared-image-galleries.md) (Katalog der freigegebenen Images) vollständig integriert. Shared Image Gallery ist ein separater Azure-Dienst, der VM-Imagedefinitionen (Virtual Machine) speichert, einschließlich der Versionsverwaltung für Images. Sie können Ihre Images auch mithilfe der globalen Replikation kopieren und für lokale Bereitstellung in andere Azure-Regionen senden.

- Überwachungsfunktionen, die über PowerShell oder die Diagnostics Service-Webapp erledigt wurden, wurden jetzt im Azure-Portal in Log Analytics verschoben. Außerdem stehen Ihnen jetzt zwei Optionen zum Visualisieren Ihrer Berichte zur Verfügung. Sie können Kusto-Abfragen ausführen und Arbeitsmappen zum Erstellen virtueller Berichte verwenden.

- Sie müssen nicht mehr die Einwilligung für Azure Active Directory (Azure AD) ausfüllen, um Windows Virtual Desktop verwenden zu können. Bei diesem Update authentifiziert der Azure AD-Mandant in Ihrem Azure-Abonnement Ihre Benutzer und stellt Azure RBAC-Steuerelemente für Ihre Administratoren bereit.


### <a name="powershell-support"></a>PowerShell-Unterstützung

Wir haben dem Az-Modul von Azure PowerShell im Rahmen dieses Updates neue AzWvd-Cmdlets hinzugefügt. Dieses neue Modul wird in PowerShell Core unterstützt, das unter .NET Core ausgeführt wird.

Folgen Sie zum Installieren des Modells den Anleitungen in [Einrichten des PowerShell-Moduls für Windows Virtual Desktop](powershell-module.md).

Eine Liste der verfügbaren Befehle finden Sie auch in der [AzWvd PowerShell-Referenz](/powershell/module/az.desktopvirtualization/?view=azps-4.2.0#desktopvirtualization).

Weitere Informationen zu den neuen Features finden Sie in [unserem Blogbeitrag](https://techcommunity.microsoft.com/t5/itops-talk-blog/windows-virtual-desktop-spring-update-enters-public-preview/ba-p/1340245).

### <a name="additional-gateways"></a>Zusätzliche Gateways

Wir haben in Südafrika einen neuen Gatewaycluster hinzugefügt, um die Verbindungslatenz zu reduzieren.

### <a name="microsoft-teams-on-windows-virtual-desktop-preview"></a>Microsoft Teams in Windows Virtual Desktop (Vorschau)

Wir haben an Microsoft Teams für Windows Virtual Desktop einige Verbesserungen vorgenommen. Am wichtigsten: Windows Virtual Desktop unterstützt jetzt bei Anrufen die akustische und visuelle Umleitung. Die Umleitung verbessert die Latenz, indem direkte Pfade zwischen Benutzern erstellt werden, wenn sie Audio- oder Videoanrufe durchführen. Eine geringere Entfernung bedeutet weniger Hops, wodurch Optik und Klang verbessert werden.

Weitere Informationen finden Sie in [unserem Blogbeitrag](https://azure.microsoft.com/updates/windows-virtual-desktop-media-optimization-for-microsoft-teams-is-now-available-in-public-preview/).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu zukünftigen Plänen finden Sie auf der [Roadmap für Microsoft 365 Windows Virtual Desktop](https://www.microsoft.com/microsoft-365/roadmap?filters=Windows%20Virtual%20Desktop).

Lesen Sie die folgenden Artikel, um sich über die Updates für unsere Clients für Windows Virtual Desktop und Remotedesktopdienste zu informieren:

- [Windows](/windows-server/remote/remote-desktop-services/clients/windowsdesktop-whatsnew)
- [macOS](/windows-server/remote/remote-desktop-services/clients/mac-whatsnew)
- [iOS](/windows-server/remote/remote-desktop-services/clients/ios-whatsnew)
- [Android](/windows-server/remote/remote-desktop-services/clients/android-whatsnew)
- [Web](/windows-server/remote/remote-desktop-services/clients/web-client-whatsnew)
