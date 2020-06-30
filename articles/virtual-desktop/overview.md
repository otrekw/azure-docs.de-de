---
title: Was ist Windows Virtual Desktop? - Azure
description: Eine Übersicht über Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 05/07/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 61162a0e2e95a08547848f286f2db489f4b9d963
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/12/2020
ms.locfileid: "84737018"
---
# <a name="what-is-windows-virtual-desktop"></a>Was ist Windows Virtual Desktop? 

Bei Windows Virtual Desktop handelt es sich um einen in der Cloud ausgeführten Dienst für die Desktop- und App-Virtualisierung.

Windows Virtual Desktop in Azure ermöglicht Folgendes:

* Einrichten einer Windows 10-Bereitstellung mit mehreren Sitzungen, die eine vollständige Windows 10-Umgebung mit Skalierbarkeit bietet
* Virtualisieren von Microsoft 365 Apps for Enterprise und Optimieren der Ausführung in virtuellen Szenarien mit mehreren Benutzern
* Bereitstellen virtueller Windows 7-Desktops mit kostenlosen erweiterten Sicherheitsupdates
* Verwenden bereits vorhandener Remotedesktopdienste (Remote Desktop Services, RDS) und Windows Server-Desktops/-Apps auf einem beliebigen Computer
* Virtualisieren von Desktops und Apps
* Verwalten von Windows 10-, Windows Server- und Windows 7-Desktops und -Apps über eine einheitliche Verwaltungsoberfläche

## <a name="introductory-video"></a>Einführungsvideo

In diesem Video lernen Sie Windows Virtual Desktop kennen und erfahren, warum dieser Dienst so einzigartig ist und was es Neues für den Dienst gibt:

<br></br><iframe src="https://www.youtube.com/embed/NQFtI3JLtaU" width="640" height="320" allowFullScreen="true" frameBorder="0"></iframe>

Weitere Videos zu Windows Virtual Desktop finden Sie in [unserer Playlist](https://www.youtube.com/watch?v=NQFtI3JLtaU&list=PLXtHYVsvn_b8KAKw44YUpghpD6lg-EHev).

## <a name="key-capabilities"></a>Wichtige Funktionen

Mit Windows Virtual Desktop können Sie eine skalierbare und flexible Umgebung einrichten:

* Erstellen Sie in Ihrem Azure-Abonnement eine vollständige Desktopvirtualisierungsumgebung ohne zusätzliche Gatewayserver.
* Veröffentlichen Sie so viele Hostpools, wie Sie für Ihre verschiedenen Workloads benötigen.
* Verwenden Sie Ihr eigenes Image für Produktionsworkloads, oder testen Sie ein Image aus dem Azure-Katalog.
* Senken Sie Kosten mit in einem Pool zusammengefassten Ressourcen für mehrere Sitzungen. Mit der neuen exklusiven Windows Virtual Desktop-Funktion für mehrere Windows 10 Enterprise-Sitzungen und der Rolle „Remotedesktop-Sitzungshost“(Remote Desktop Session Host, RDSH) unter Windows Server können Sie die Anzahl virtueller Computer sowie den Betriebssystemmehraufwand erheblich verringern und Ihren Benutzern weiterhin die gleichen Ressourcen bereitstellen.
* Ermöglichen Sie individuelle Besitzer über persönliche (dauerhafte) Desktops.

Sie können virtuelle Desktops bereitstellen und verwalten:

* Verwenden Sie die PowerShell- und die REST-Oberfläche von Windows Virtual Desktop, um die Hostpools zu konfigurieren, App-Gruppen zu erstellen, Benutzer zuzuweisen und Ressourcen zu veröffentlichen.
* Sie können vollständige Desktops oder einzelne Remote-Apps über einen einzelnen Hostpool veröffentlichen, individuelle App-Gruppen für unterschiedliche Benutzergruppen erstellen und sogar Benutzer mehreren App-Gruppen zuweisen, um die Anzahl von Images zu verringern.
* Bei der Umgebungsverwaltung können Sie dank des integrierten delegierten Zugriffs Rollen zuweisen und Diagnoseinformationen erfassen, um verschiedene Konfigurations- und Benutzerfehler nachzuvollziehen.
* Verwenden Sie den Diagnosedienst, um Fehler zu behandeln.
* Verwalten Sie nur das Image und die virtuellen Computer, nicht die Infrastruktur. Sie müssen die Remotedesktop-Rollen nicht wie bei den Remotedesktopdiensten selbst verwalten, sondern nur die virtuellen Computer in Ihrem Azure-Abonnement.

Darüber hinaus können Sie Benutzer zuweisen und mit Ihren virtuellen Desktops verbinden:

* Zugewiesene Benutzer können einen beliebigen Windows Virtual Desktop-Client starten, um Benutzer mit ihren veröffentlichten Windows-Desktops und -Anwendungen zu verbinden. Stellen Sie eine Verbindung über ein beliebiges Gerät her – entweder über eine native Anwendung auf Ihrem Gerät oder über den HTML5-Webclient für Windows Virtual Desktop.
* Richten Sie Benutzer auf sichere Weise über umgekehrte Verbindungen mit dem Dienst ein, um keine eingehenden Ports mehr geöffnet lassen zu müssen.

## <a name="requirements"></a>Requirements (Anforderungen)

Für die Einrichtung von Windows Virtual Desktop und die erfolgreiche Verknüpfung Ihrer Benutzer mit ihren Windows-Desktops und -Anwendungen benötigen Sie ein paar Dinge.

Da wir planen, die Unterstützung auf die folgenden Betriebssysteme zu erweitern, vergewissern Sie sich, dass Sie über die [erforderlichen Lizenzen](https://azure.microsoft.com/pricing/details/virtual-desktop/) für die Desktops und Apps verfügen, die Sie für Ihre Benutzer bereitstellen möchten:

|OS|Erforderliche Lizenz|
|---|---|
|Windows 10 Enterprise (mehrere Sitzungen) oder Windows 10 Enterprise|Microsoft 365 E3, E5, A3, A5, F3, Business Premium<br>Windows E3, E5, A3, A5|
|Windows 7 Enterprise |Microsoft 365 E3, E5, A3, A5, F3, Business Premium<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|RDS-Clientzugriffslizenz (Client Access License, CAL) mit Software Assurance|

In Ihrer Infrastruktur muss Folgendes vorhanden sein, um Windows Virtual Desktop verwenden zu können:

* Eine [Azure Active Directory-Instanz](/azure/active-directory/).
* Eine mit Azure Active Directory synchronisierte Windows Server Active Directory-Instanz. Sie können diese dann mit einer der folgenden Komponenten konfigurieren:
  * Azure AD Connect (für Hybridorganisationen)
  * Azure Active Directory Domain Services (für Hybrid- oder Cloudorganisationen)
* Ein Azure-Abonnement mit einem virtuellen Netzwerk, das die Windows Server Active Directory-Instanz entweder enthält oder mit ihr verbunden ist.
  
Die virtuellen Azure-Computer, die Sie für Windows Virtual Desktop erstellen, müssen folgende Anforderungen erfüllen:

* Sie müssen in eine [Standard-Domäne](../active-directory-domain-services/active-directory-ds-comparison.md) oder in [Hybrid AD](../active-directory/devices/hybrid-azuread-join-plan.md) eingebunden sein. Virtuelle Computer dürfen nicht in Azure AD eingebunden sein.
* Auf ihnen muss eines der folgenden [unterstützten Betriebssystemimages](#supported-virtual-machine-os-images) ausgeführt werden:

>[!NOTE]
>Sollten Sie ein Azure-Abonnement benötigen, können Sie sich [für eine einmonatige kostenlose Testversion registrieren](https://azure.microsoft.com/free/). Bei Verwendung der kostenlosen Testversion von Azure müssen Sie Azure AD Domain Services verwenden, um Ihre Windows Server Active Directory-Instanz mit Azure Active Directory zu synchronisieren.

Die virtuellen Azure-Computer, die Sie für Windows Virtual Desktop erstellen, müssen über Zugriff auf die folgenden URLs verfügen:

|Adresse|Ausgehender TCP-Port|Zweck|Diensttag|
|---|---|---|---|
|*.wvd.microsoft.com|443|Dienstdatenverkehr|WindowsVirtualDesktop|
|mrsglobalsteus2prod.blob.core.windows.net|443|Agent- und SXS-Stapelupdates|AzureCloud|
|*.core.windows.net|443|Agent-Datenverkehr|AzureCloud|
|*.servicebus.windows.net|443|Agent-Datenverkehr|AzureCloud|
|prod.warmpath.msftcloudes.com|443|Agent-Datenverkehr|AzureCloud|
|catalogartifact.azureedge.net|443|Azure Marketplace|AzureCloud|
|kms.core.windows.net|1688|Aktivierung von Windows|Internet|
|wvdportalstorageblob.blob.core.windows.net|443|Unterstützung des Azure-Portals|AzureCloud|

>[!IMPORTANT]
>Windows Virtual Desktop unterstützt jetzt das FQDN-Tag. Weitere Informationen finden Sie unter [Verwenden von Azure Firewall zum Schutz von Windows Virtual Desktop-Bereitstellungen](../firewall/protect-windows-virtual-desktop.md).
>
>Wir empfehlen Ihnen, FQDN- oder Diensttags anstelle von URLs zu verwenden, um Dienstprobleme zu verhindern. Die aufgeführten URLs und Tags beziehen sich nur auf Windows Virtual Desktop-Websites und -Ressourcen. Sie enthalten keine URLs für andere Dienste, z. B. Azure Active Directory.

In der folgenden Tabelle sind optionale URLs aufgeführt, auf die Ihre virtuellen Azure-Computer Zugriff haben können:

|Adresse|Ausgehender TCP-Port|Zweck|Diensttag|
|---|---|---|---|
|*.microsoftonline.com|443|Authentifizierung bei Microsoft Online Services|Keine|
|*.events.data.microsoft.com|443|Telemetriedienst|Keine|
|www.msftconnecttest.com|443|Ermittelt, ob das Betriebssystem mit dem Internet verbunden ist.|Keine|
|*.prod.do.dsp.mp.microsoft.com|443|Windows-Update|Keine|
|login.windows.net|443|Anmelden von Benutzern bei Microsoft Online Services, Office 365|Keine|
|*.sfx.ms|443|Updates für die OneDrive-Clientsoftware|Keine|
|*.digicert.com|443|Überprüfung der Zertifikatsperre|Keine|


>[!NOTE]
>Windows Virtual Desktop verfügt derzeit über keine Liste mit IP-Adressbereichen, die Sie in die Whitelist aufnehmen können, um Netzwerkdatenverkehr zuzulassen. Momentan wird nur das Hinzufügen spezifischer URLs zur Whitelist unterstützt.
>
>Eine Liste der URLs für Office, einschließlich der erforderlichen URLs für Azure Active Directory, finden Sie unter [URLs und IP-Adressbereiche für Office 365](/office365/enterprise/urls-and-ip-address-ranges).
>
>Sie müssen das Platzhalterzeichen (*) für URLs für Dienstdatenverkehr verwenden. Wenn Sie kein Platzhalterzeichen (*) für Agent-Datenverkehr verwenden möchten, ermitteln Sie wie folgt die URLs ohne Platzhalter:
>
>1. Registrieren Sie Ihre virtuellen Computer für den Windows Virtual Desktop-Hostpool.
>2. Öffnen Sie die **Ereignisanzeige**, navigieren Sie zu **Windows-Protokolle** > **Anwendung** > **WVD-Agent**, und suchen Sie nach der Ereignis-ID 3702.
>3. Nehmen Sie die URLs, die Sie unter der Ereignis-ID 3702 finden, in eine Whitelist auf. Die URLs unter der Ereignis-ID 3702 sind regionsspezifisch. Sie müssen den Whitelistprozess mit den relevanten URLs für jede Region wiederholen, in der Sie Ihre virtuellen Computer bereitstellen möchten.

Windows Virtual Desktop umfasst die Windows-Desktops und -Apps, die Sie für Benutzer bereitstellen, sowie die Verwaltungslösung. Letztere wird von Microsoft in Azure gehostet. Desktops und Apps können auf virtuellen Computern (VMs) in einer beliebigen Azure-Region bereitgestellt werden. Die Verwaltungslösung und Daten für diese virtuellen Computer befinden sich dagegen in den Vereinigten Staaten. Dies kann zu Datenübertragungen in die USA führen.

Ihr Netzwerk muss folgende Anforderungen erfüllen, um eine optimale Leistung zu erzielen:

* Die Roundtriplatenz zwischen dem Netzwerk des Clients und der Azure-Region, in der die Hostpools bereitgestellt wurden, muss unter 150 ms liegen.
* Netzwerkdatenverkehr wird ggf. außerhalb der Grenzen des Landes bzw. der Region übertragen, wenn virtuelle Computer, die Desktops und Apps hosten, eine Verbindung mit dem Verwaltungsdienst herstellen.
* Zur Optimierung der Netzwerkleistung empfiehlt es sich, die virtuellen Computer des Sitzungshosts in der Azure-Region zu platzieren, in der sich auch der Verwaltungsdienst befindet.

## <a name="supported-remote-desktop-clients"></a>Unterstützte Remotedesktopclients

Die folgenden Remotedesktopclients unterstützen Windows Virtual Desktop:

* [Windows Desktop](connect-windows-7-and-10.md)
* [Web](connect-web.md)
* [macOS](connect-macos.md)
* [iOS](connect-ios.md)
* [Android (Vorschau)](connect-android.md)

> [!IMPORTANT]
> Windows Virtual Desktop unterstützt nicht den RemoteApp-Client und den Client für Desktopverbindungen (RADC). Der Client für Remotedesktopverbindung (MSTSC) wird ebenfalls nicht unterstützt.

> [!IMPORTANT]
> Windows Virtual Desktop verfügt derzeit nicht über Unterstützung für den Remotedesktopclient aus dem Windows Store. Die Unterstützung für diesen Client wird in einer zukünftigen Version enthalten sein.

Für die Remotedesktopclients muss Zugriff auf die folgenden URLs bestehen:

|Adresse|Ausgehender TCP-Port|Zweck|Client(s)|
|---|---|---|---|
|*.wvd.microsoft.com|443|Dienstdatenverkehr|All|
|*.servicebus.windows.net|443|Problembehandlung für Daten|All|
|go.microsoft.com|443|Microsoft FWLinks|All|
|aka.ms|443|Microsoft-URL-Verkürzung|All|
|docs.microsoft.com|443|Dokumentation|All|
|privacy.microsoft.com|443|Datenschutzbestimmungen|All|
|query.prod.cms.rt.microsoft.com|443|Clientupdates|Windows Desktop|

>[!IMPORTANT]
>Das Öffnen dieser URLs ist für einen zuverlässigen Clientbetrieb von entscheidender Bedeutung. Der Zugriff auf diese URLs darf nicht blockiert werden; andernfalls wird die Dienstfunktionalität beeinträchtigt. Diese URLs gelten lediglich für die Clientstandorte und -ressourcen und beinhalten keine URLs für andere Dienste wie Azure Active Directory.

## <a name="supported-virtual-machine-os-images"></a>Unterstützte Betriebssystemimages virtueller Computer

Windows Virtual Desktop unterstützt die folgenden Images von x64-Betriebssystemen:

* Windows 10 Enterprise mit mehreren Sitzungen, Version 1809 oder höher
* Windows 10 Enterprise, Version 1809 oder höher
* Windows 7 Enterprise
* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

Windows Virtual Desktop unterstützt keine Betriebssystemimages mit x86 (32 Bit), Windows 10 Enterprise N oder Windows 10 Enterprise KN. Aufgrund einer Sektorgrößenbeschränkung unterstützt Windows 7 auch keine VHD- oder VHDX-basierten Profillösungen, die in einer verwalteten Azure Storage-Instanz gehostet werden.

Die verfügbaren Automatisierungs- und Bereitstellungsoptionen hängen davon ab, welches Betriebssystem und welche Version gewählt werden; siehe dazu die folgende Tabelle: 

|Betriebssystem|Azure-Imagekatalog|Manuelle VM-Bereitstellung|Integration von Azure Resource Manager-Vorlagen|Bereitstellungshost-Pools auf Azure Marketplace|
|--------------------------------------|:------:|:------:|:------:|:------:|
|Windows 10, Multisession, Version 1903|Ja|Ja|Ja|Ja|
|Windows 10, Multisession, Version 1809|Ja|Ja|Nein|Nein|
|Windows 10 Enterprise, Version 1903|Ja|Ja|Ja|Ja|
|Windows 10 Enterprise, Version 1809|Ja|Ja|Nein|Nein|
|Windows 7 Enterprise|Ja|Ja|Nein|Nein|
|Windows Server 2019|Ja|Ja|Nein|Nein|
|Windows Server 2016|Ja|Ja|Ja|Ja|
|Windows Server 2012 R2|Ja|Ja|Nein|Nein|

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 verwenden, können Sie mit dem Tutorial [Erstellen eines Mandanten in Windows Virtual Desktop](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md) beginnen.

Wenn Sie das Windows Virtual Desktop-Release vom Frühjahr 2020 verwenden, müssen Sie stattdessen einen Hostpool erstellen. Im folgenden Tutorial finden Sie Informationen zu den ersten Schritten:

> [!div class="nextstepaction"]
> [Erstellen eines Hostpools mit dem Azure-Portal](create-host-pools-azure-marketplace.md)
