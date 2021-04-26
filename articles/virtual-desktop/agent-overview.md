---
title: Erste Schritte mit dem Windows Virtual Desktop-Agent
description: Übersicht über den Windows Virtual Desktop-Agent und Updateprozesse.
author: Sefriend
ms.topic: conceptual
ms.date: 12/16/2020
ms.author: sefriend
manager: clarkn
ms.openlocfilehash: 371cc78f3ebad638008f4195f164b66a64948c65
ms.sourcegitcommit: c2a41648315a95aa6340e67e600a52801af69ec7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106504548"
---
# <a name="get-started-with-the-windows-virtual-desktop-agent"></a>Erste Schritte mit dem Windows Virtual Desktop-Agent

Das Windows Virtual Desktop-Dienstframework enthält drei Hauptkomponenten: den Remotedesktopclient, den Dienst und die VMs. Diese VMs befinden sich in dem Kundenabonnement, in dem der Windows Virtual Desktop-Agent und der Bootloader des Agents installiert sind. Der Agent dient zur Kommunikation zwischen dem Dienst und den VMs und ermöglicht so die Konnektivität. Wenn Probleme mit der Installation, dem Update oder der Konfiguration des Agents auftreten, können Ihre VMs daher keine Verbindung mit dem Dienst herstellen. Der Bootloader des Agents ist die ausführbare Datei, die den Agent lädt. 

In diesem Artikel erhalten Sie eine kurze Übersicht über die Installations- und Updateprozesse für den Agent.

>[!NOTE]
>Diese Dokumentation gilt nicht für den FSLogix-Agent oder den Remotedesktopclient-Agent.


## <a name="initial-installation-process"></a>Erstinstallation

Für die Erstinstallation des Windows Virtual Desktop-Agents gibt es zwei Möglichkeiten. Wenn Sie VMs (virtual Machines, virtuelle Computer) über das Azure-Portal oder den Azure Marketplace bereitstellen, werden der Agent und der zugehörige Bootloader automatisch installiert. Wenn Sie VMs mithilfe von PowerShell bereitstellen, müssen Sie die MSI-Dateien für den Agent und den Bootloader manuell herunterladen, wenn Sie [mithilfe von PowerShell einen Hostpool für Windows Virtual Desktop erstellen](create-host-pools-powershell.md#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool). Wenn der Agent installiert ist, werden der parallele Stapel für Windows Virtual Desktop und der Überwachungsagent von Geneva ebenfalls gleichzeitig installiert. Die Komponente für den parallelen Stapel ist erforderlich, damit Benutzer umgekehrte Verbindungen vom Server zum Client sicher einrichten können. Der Überwachungs-Agent von Geneva überwacht die Integrität des Agents. Alle drei Komponenten sind für die ordnungsgemäße Funktion der End-to-End-Benutzerkonnektivität von entscheidender Bedeutung.

>[!IMPORTANT]
>Für eine erfolgreiche Installation von Windows Virtual Desktop-Agent, parallelem Stapel und dem Überwachungs-Agent von Geneva müssen Sie die Blockierung aller URLs in der [Liste erforderlicher URLs](safe-url-list.md#virtual-machines) deaktivieren. Das Aufheben der Blockierung dieser URLs ist für die Verwendung von Windows Virtual Desktop erforderlich.

## <a name="agent-update-process"></a>Updateprozess für den Agent

Der Windows Virtual Desktop-Dienst aktualisiert den Agent, sobald ein Update verfügbar ist. Agent-Updates können neue Funktionen enthalten oder bestehende Probleme beheben. Nachdem die anfängliche Version des Windows Virtual Desktop-Agents installiert wurde, fragt der Agent den Windows Virtual Desktop-Dienst regelmäßig auf neuere Versionen der Agent-Komponente, der Stapel- oder der Überwachungskomponente ab. Wenn bereits eine neuere Version einer der Komponenten bereitgestellt wurde, wird die aktualisierte Komponente automatisch installiert.

Neue Agent-Versionen werden in regelmäßigen Abständen in einwöchigen Zeiträumen für alle Azure-Abonnements bereitgestellt. Diese Aktualisierungszeiträume werden als „Flights“ bezeichnet. Wenn ein Flight eintritt, erhalten die VMs in Ihrem Hostpool das Agent-Update zu unterschiedlichen Zeitpunkten. Alle VM-Agents in allen Abonnements werden bis zum Ende des Bereitstellungszeitraums aktualisiert. Das Flight-System von Windows Virtual Desktop erhöht die Zuverlässigkeit des Diensts, indem die Stabilität und Qualität des Agent-Updates sichergestellt wird.


>[!NOTE]
>Da virtuelle Computer in Ihrem Hostpool ihre Agent-Updates zu unterschiedlichen Zeiten erhalten können, müssen Sie in der Lage sein, den Unterschied zwischen den Flightproblemen und den fehlgeschlagenen Agent-Updates zu erkennen. Wenn Sie die Ereignisprotokolle für Ihren virtuellen Computer unter **Ereignisanzeige** > **Windows-Protokoll** >  **-Anwendung** aufrufen und ein Ereignis mit der Bezeichnung „ID 3277“ anzeigen, funktioniert das Agent-Update nicht. Wenn dieses Ereignis nicht angezeigt wird, befindet sich der virtuelle Computer in einem anderen Flight und wird später aktualisiert.
>- Nachdem der Überwachungs-Agent von Geneva auf die neueste Version aktualisiert wurde, wird zunächst der alte Task „GenevaTask“ ermittelt und deaktiviert und danach ein neuer Task für den neuen Überwachungs-Agent erstellt. Die frühere Version des Überwachungs-Agents wird nicht gelöscht, damit bei einem Fehler in der aktuellsten Version des Überwachungs-Agents eine Wiederherstellung der früheren Version durchgeführt werden kann. Wenn bei der neuesten Version ein Problem auftritt, wird der alte Überwachungs-Agent erneut aktiviert, damit weiterhin Überwachungsdaten bereitgestellt werden. Alle Versionen des Überwachungs-Agent, die älter sind als die letzte Version vor dem Update, werden von Ihrer VM gelöscht.
>- Ihre VM behält jeweils drei Versionen des parallelen Stapels bei. Dies ermöglicht eine schnelle Wiederherstellung, wenn beim Update Fehler auftreten. Bei einem Update wird dann die jeweils älteste Version des Stapels von der VM entfernt.

Die Installation von Updates dauert auf einer neueren VM normalerweise 2–3 Minuten und sollte nicht dazu führen, dass die Verbindung der VM getrennt oder die VM selbst heruntergefahren wird. Dieser Updateprozess gilt sowohl für Windows Virtual Desktop (klassisch) als auch für die neueste Version von Windows Virtual Desktop mit Azure Resource Manager.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun den Windows Virtual Desktop-Agent besser verstehen, finden Sie im Folgenden einige Ressourcen, die Ihnen weiterhelfen können:

- Wenn Probleme mit dem Agent oder der Konnektivität auftreten, sehen Sie sich den [Leitfaden zur Problembehandlung beim Windows Virtual Desktop-Agent](troubleshoot-agent.md) an.
