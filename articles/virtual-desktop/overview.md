---
title: Was ist Windows Virtual Desktop? - Azure
description: Eine Übersicht über Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 01/27/2020
ms.author: helohr
ms.openlocfilehash: 168a345427be47dc1c33f43be1af47daa8f638ef
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76772785"
---
# <a name="what-is-windows-virtual-desktop"></a>Was ist Windows Virtual Desktop? 

Bei Windows Virtual Desktop handelt es sich um einen in der Cloud ausgeführten Dienst für die Desktop- und App-Virtualisierung.

Windows Virtual Desktop in Azure ermöglicht Folgendes:

* Einrichten einer Windows 10-Bereitstellung mit mehreren Sitzungen, die eine vollständige Windows 10-Umgebung mit Skalierbarkeit bietet
* Virtualisieren von Office 365 ProPlus und Optimieren der Ausführung in virtuellen Szenarien mit mehreren Benutzern
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
|Windows 10 Enterprise (mehrere Sitzungen) oder Windows 10 Enterprise|Microsoft 365 E3, E5, A3, A5, F1, Business<br>Windows E3, E5, A3, A5|
|Windows 7 Enterprise |Microsoft 365 E3, E5, A3, A5, F1, Business<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|RDS-Clientzugriffslizenz (Client Access License, CAL) mit Software Assurance|

In Ihrer Infrastruktur muss Folgendes vorhanden sein, um Windows Virtual Desktop verwenden zu können:

* Eine [Azure Active Directory-Instanz](https://docs.microsoft.com/azure/active-directory/).
* Eine mit Azure Active Directory synchronisierte Windows Server Active Directory-Instanz. Sie können diese dann mit einer der folgenden Komponenten konfigurieren:
  * Azure AD Connect (für Hybridorganisationen)
  * Azure Active Directory Domain Services (für Hybrid- oder Cloudorganisationen)
* Ein Azure-Abonnement mit einem virtuellen Netzwerk, das die Windows Server Active Directory-Instanz entweder enthält oder mit ihr verbunden ist.
  
Die virtuellen Azure-Computer, die Sie für Windows Virtual Desktop erstellen, müssen folgende Anforderungen erfüllen:

* Sie müssen in eine [Standard-Domäne](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-comparison) oder in [Hybrid AD](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan) eingebunden sein. Virtuelle Computer dürfen nicht in Azure AD eingebunden sein.
* Auf ihnen muss eines der folgenden [unterstützten Betriebssystemimages](#supported-virtual-machine-os-images) ausgeführt werden:

>[!NOTE]
>Sollten Sie ein Azure-Abonnement benötigen, können Sie sich [für eine einmonatige kostenlose Testversion registrieren](https://azure.microsoft.com/free/). Bei Verwendung der kostenlosen Testversion von Azure müssen Sie Azure AD Domain Services verwenden, um Ihre Windows Server Active Directory-Instanz mit Azure Active Directory zu synchronisieren.

Die virtuellen Azure-Computer, die Sie für Windows Virtual Desktop erstellen, müssen über Zugriff auf die folgenden URLs verfügen:

|Adresse|Ausgehender Port|Zweck|
|---|---|---|
|*.wvd.microsoft.com|TCP-Port 443|Dienstdatenverkehr|
|*.blob.core.windows.net|TCP-Port 443|Agent, SXS-Stapelupdates und Agent-Datenverkehr|
|*.core.windows.net|TCP-Port 443|Agent-Datenverkehr|
|*.servicebus.windows.net|TCP-Port 443|Agent-Datenverkehr|
|prod.warmpath.msftcloudes.com|TCP-Port 443|Agent-Datenverkehr|
|catalogartifact.azureedge.net|TCP-Port 443|Azure Marketplace|
|kms.core.windows.net|TCP-Port 1688|Windows 10-Aktivierung|

>[!IMPORTANT]
>Das Aufrufen dieser URLs ist für die zuverlässige Bereitstellung von Windows Virtual Desktop von entscheidender Bedeutung. Der Zugriff auf diese URLs darf nicht blockiert werden; andernfalls wird die Dienstfunktionalität beeinträchtigt. Diese URLs entsprechen lediglich Windows Virtual Desktop-Websites und -Ressourcen und beinhalten keine URLs für andere Dienste wie etwa Azure Active Directory.

>[!NOTE]
>Windows Virtual Desktop verfügt derzeit über keine Liste mit IP-Adressbereichen, die Sie in die Whitelist aufnehmen können, um Netzwerkdatenverkehr zuzulassen. Momentan wird nur das Hinzufügen spezifischer URLs zur Whitelist unterstützt.
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

* [Windows](connect-windows-7-and-10.md)
* [Web](connect-web.md)
* [Mac](connect-macos.md)
* [iOS](connect-ios.md)
* [Android (Vorschau)](connect-android.md)

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

|Betriebssystem|Azure-Imagekatalog|Manuelle VM-Bereitstellung|Integration von Azure Resource Manager-Vorlagen|Bereitstellungshost-Pools auf Azure Marketplace|Updates für Windows Virtual Desktop-Agents|
|--------------------------------------|:------:|:------:|:------:|:------:|:------:|
|Windows 10, Multisession, Version 1903|Ja|Ja|Ja|Ja|Automatic|
|Windows 10, Multisession, Version 1809|Ja|Ja|Nein|Nein|Automatic|
|Windows 10 Enterprise, Version 1903|Ja|Ja|Ja|Ja|Automatic|
|Windows 10 Enterprise, Version 1809|Ja|Ja|Nein|Nein|Automatic|
|Windows 7 Enterprise|Ja|Ja|Nein|Nein|Manuell|
|Windows Server 2019|Ja|Ja|Nein|Nein|Automatic|
|Windows Server 2016|Ja|Ja|Ja|Ja|Automatic|
|Windows Server 2012 R2|Ja|Ja|Nein|Nein|Automatic|

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie zunächst einen Mandanten. Weitere Informationen zum Erstellen eines Mandanten finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Erstellen eines Mandanten in Windows Virtual Desktop](tenant-setup-azure-active-directory.md)
