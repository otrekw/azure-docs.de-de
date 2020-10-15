---
title: Leitfaden für beschleunigte Classroom-Lab-Einrichtung für Azure Lab Services
description: Dieser Leitfaden unterstützt Lab-Ersteller beim schnellen Einrichten eines Lab-Kontos für die Verwendung innerhalb ihrer Bildungseinrichtung.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 510aa97a0a47e62f627203495c601bb2538e19ad
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91652209"
---
# <a name="classroom-lab-setup-guide"></a>Einrichtungsleitfaden für Classroom-Labs

Der Vorgang zum Veröffentlichen eines Labs für Ihre Kursteilnehmer kann – je nach Anzahl der virtuellen Computer (VMs), die in Ihrem Lab erstellt werden, – mehrere Stunden dauern. Zum Einrichten eines Labs sollten Sie mindestens einen Tag einplanen, um sicherzustellen, dass es einwandfrei funktioniert und ausreichend Zeit für die Veröffentlichung der VMs von Kursteilnehmern zur Verfügung steht.

## <a name="understand-the-lab-requirements-of-your-class"></a>Informationen zu den Lab-Anforderungen für Ihren Kurs

Vor dem Einrichten eines neuen Labs sollten Sie sich die folgenden Fragen stellen:

### <a name="what-software-requirements-does-the-class-have"></a>Welche Softwareanforderungen gelten für den Kurs?

Entscheiden Sie aufgrund der Lernziele Ihres Kurses, welches Betriebssystem, welche Anwendungen und Tools auf den VMs des Labs installiert werden müssen. Zum Einrichten von Lab-VMs haben Sie drei Möglichkeiten:

- **Verwenden Sie ein Azure Marketplace-Image**: Azure Marketplace bietet Hunderte von Images, die Sie beim Erstellen eines Labs verwenden können. Für einige Kurse enthält eines dieser Images möglicherweise bereits alles, was Sie für Ihren Kurs benötigen.

- **Erstellen Sie ein neues benutzerdefiniertes Image**: Sie können Ihr eigenes benutzerdefiniertes Image erstellen, indem Sie ein Azure Marketplace-Image als Ausgangspunkt verwenden und es anpassen, indem Sie zusätzliche Software installieren und Konfigurationsänderungen vornehmen.

- **Verwenden Sie ein vorhandenes benutzerdefiniertes Image**: Sie können vorhandene benutzerdefinierte Images wiederverwenden, die Sie zuvor erstellt haben oder die von anderen Administratoren oder Lehrkräften an Ihrer Bildungseinrichtung erstellt wurden. Dafür müssen Ihre Administratoren einen Katalog der freigegebenen Images (Shared Image Gallery) konfiguriert haben, bei dem es sich um ein Repository zum Speichern von benutzerdefinierten Images handelt.

> [!NOTE]
> Ihre Administratoren sind für die Aktivierung von Azure Marketplace-Images und benutzerdefinierten Images zuständig, damit Sie sie verwenden können. Stimmen Sie dies mit Ihrer IT-Abteilung ab, um sicherzustellen, dass die benötigten Images aktiviert sind. Benutzerdefinierte Images, die Sie erstellen, werden automatisch für die Verwendung in Labs aktiviert, deren Besitzer Sie sind.

### <a name="what-hardware-requirements-does-the-class-have"></a>Welche Hardwareanforderungen gelten für den Kurs?

Es gibt eine Vielzahl von Computegrößen, aus denen Sie auswählen können:

- Die Größen der geschachtelten Virtualisierung, damit Sie den Kursteilnehmern Zugriff auf eine VM erteilen können, die mehrere, geschachtelte VMs hosten kann. So könnten Sie diese Computegröße beispielsweise für Netzwerkkurse verwenden.

- GPU-Größen, damit Ihre Kursteilnehmer rechenintensive Anwendungstypen verwenden können. Diese Auswahl kann z. B. für künstliche Intelligenz und maschinelles Lernen geeignet sein.

Die vollständige Liste der verfügbaren Computegrößen finden Sie im Leitfaden zu den [VM-Größen](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#vm-sizing).

> [!NOTE]
> Abhängig von der Region Ihres Labs sind möglicherweise weniger Computegrößen verfügbar, da dies je nach Region variiert. Generell sollten Sie die kleinste Computegröße auswählen, die Ihren Anforderungen am ehesten entspricht. Bei Bedarf können Sie mit Azure Lab Services zu einem späteren Zeitpunkt ein neues Lab mit einer anderen Computekapazität einrichten.

### <a name="what-dependencies-does-the-class-have-on-external-azure-or-network-resources"></a>Welche Abhängigkeiten von externen Azure- oder Netzwerkressourcen gibt es für den Kurs?

Wenn Ihre Lab-VMs externe Ressourcen wie eine Datenbank, eine Dateifreigabe oder einen Lizenzierungsserver verwenden müssen, stimmen Sie sich mit Ihren Administratoren ab, um sicherzustellen, dass Ihr Lab auf diese Ressourcen zugreifen kann.

Für den Zugriff auf Azure-Ressourcen, die *nicht* durch ein virtuelles Netzwerk gesichert werden, müssen Sie keine zusätzliche Konfiguration durch ihre Administratoren suchen. Sie können auf diese Ressourcen über das öffentliche Internet zugreifen.

> [!NOTE]
> Sie sollten berücksichtigen, ob Sie die Abhängigkeiten Ihres Labs von externen Ressourcen verringern können, indem Sie die Ressource direkt auf der VM bereitstellen. Wenn Sie beispielsweise das Lesen von Daten aus einer externen Datenbank vermeiden möchten, können Sie die Datenbank direkt auf der VM installieren.  

### <a name="how-will-costs-be-controlled"></a>Wie werden die Kosten kontrolliert?

Lab Services verwendet ein Preismodell mit nutzungsbasierter Bezahlung. Dies bedeutet: Sie müssen nur für die Zeit bezahlen, in der eine Lab-VM ausgeführt wird. Zur Kostenkontrolle haben Sie drei Optionen, die in der Regel in Verbindung miteinander verwendet werden:

- **Zeitplan:** Mit einem Zeitplan können Sie automatisch steuern, wann die VMs Ihrer Labs gestartet und heruntergefahren werden.
- **Kontingent**: Das Kontingent steuert die Anzahl der Stunden, die Kursteilnehmer außerhalb der geplanten Stunden auf eine VM können. Wenn das Kontingent erreicht wird, während ein Kursteilnehmer es verwendet, wird die VM automatisch heruntergefahren. Der Kursteilnehmer kann die VM nur dann neu starten, wenn das Kontingent verlängert wurde.
- **Automatisches Herunterfahren**: Wenn diese Einstellung aktiviert ist, bewirkt das automatische Herunterfahren, dass Windows-VMs nach einem bestimmten Zeitraum automatisch heruntergefahren werden, nachdem ein Kursteilnehmer die Verbindung mit einer Remotedesktopprotokoll-Sitzung (RDP) getrennt hat. Diese Einstellung ist standardmäßig deaktiviert.  

### <a name="how-will-students-save-their-work"></a>Wie speichern Kursteilnehmer ihre Arbeit?

Kursteilnehmern wird für die Lebensdauer des Labs jeweils eine eigene VM zugewiesen. Sie haben folgende Auswahlmöglichkeiten:

- Direktes Speichern auf der VM.
- Speichern an einem externen Speicherort, z. B. OneDrive oder GitHub.

Es ist möglich, OneDrive für Kursteilnehmer auf Ihren Lab-VMS automatisch konfigurieren zu lassen.

> [!NOTE]
> Wenn Sie sicherstellen möchten, dass Ihre Kursteilnehmer außerhalb des Labs und nach dem Ende des Kurses weiterhin auf ihre gespeicherte Arbeit zugreifen können, sollten sie ihre Arbeit in einem externen Repository speichern.

### <a name="how-will-students-connect-to-their-vm"></a>Wie stellen Kursteilnehmer eine Verbindung mit ihrer VM her?

Bei VMs mit RDP zu Windows wird empfohlen, dass die Kursteilnehmer den [Microsoft-Remotedesktopclient](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients) verwenden. Remotedesktopclient unterstützt Mac-, Chromebook- und Windows-Computer.

Bei Linux-VMs können die Kursteilnehmer SSH oder RDP verwenden. Damit die Kursteilnehmer eine Verbindung über RDP herstellen können, müssen Sie die erforderlichen RDP- und GUI-Pakete installieren und konfigurieren.

## <a name="set-up-your-lab"></a>Einrichten des Labs

Nachdem Sie sich mit den Anforderungen für das Lab Ihres Kurses vertraut gemacht haben, können Sie es jetzt einrichten. Folgen Sie den Links in diesem Abschnitt, um zu erfahren, wie Sie Ihr Lab einrichten.

1. **Erstellen Sie ein Lab.** Im Tutorial zum [Erstellen eines Classroom-Labs](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#create-a-classroom-lab) finden Sie weitere Anleitungen.

    > [!NOTE]
    > Wenn für Ihren Kurs eine geschachtelte Virtualisierung erforderlich ist, lesen Sie die Schritte in [Aktivieren von geschachtelter Virtualisierung](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-nested-virtualization-template-vm).

1. **Passen Sie Images an, und veröffentlichen Sie Lab-VMs.** Stellen Sie eine Verbindung mit einer speziellen VM her, die als „Vorlagen-VM“ (auch: „virtueller Vorlagencomputer“) bezeichnet wird. Lesen Sie dazu die Schritte in den folgenden Anleitungen:
    - [Erstellen und Verwalten einer Vorlagen-VM](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#publish-the-template-vm)
    - [Verwenden eines Katalogs mit freigegebenen Images](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-shared-image-gallery)

    > [!NOTE]
    > Wenn Sie Windows verwenden, sollten Sie auch die Anleitungen in [Vorbereiten eines virtuellen Windows-Vorlagencomputers](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-prepare-windows-template) lesen. Diese Anweisungen enthalten Schritte zum Einrichten von OneDrive und Office für Ihre Kursteilnehmer.

1. **Verwalten Sie den VM-Pool und die VM-Kapazität.** Sie können die von Ihrem Kurs benötigte VM-Kapazität nach Bedarf problemlos zentral hoch- oder herunterskalieren. Denken Sie aber daran, dass das Erhöhen der VM-Kapazität möglicherweise mehrere Stunden dauert, weil damit das Einrichten neuer VMs verbunden ist. Lesen Sie dazu die Schritte in [Einrichten und Verwalten eines VM-Pools](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-set-virtual-machine-passwords).

1. **Fügen Sie Lab-Benutzer hinzu, und verwalten Sie sie.** Weitere Informationen zum Hinzufügen von Benutzern zu Ihrem Lab finden Sie in den einzelnen Schritten in den folgenden Tutorials:
   - [Hinzufügen von Benutzern zum Lab](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#add-users-to-the-lab)
   - [Senden von Einladungen an Benutzer](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#send-invitation-emails-to-users)

    Informationen zu den Typen von Konten, die Kursteilnehmer verwenden können, finden Sie unter [Konten für Kursteilnehmer](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#student-accounts).
  
1. **Legen Sie Kostenkontrollen fest.** Wenn Sie die Kosten Ihres Labs kontrollieren möchten, legen Sie Zeitpläne, Kontingente und automatisches Herunterfahren fest. Arbeiten Sie die folgenden Tutorials durch:

   - [Zeitplan festlegen](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#set-a-schedule-for-the-lab)
        > [!NOTE]
        > Je nach installiertem Betriebssystem dauert es vielleicht mehrere Minuten, bis eine VM gestartet wird. Wenn Sie sicherstellen möchten, dass eine Lab-VM während Ihrer geplanten Stunden einsatzbereit ist, sollten Sie VMs 30 Minuten im Voraus starten.

   - [Festlegen von Kontingenten für Benutzer](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-quotas-for-users) und [Festlegen eines zusätzlichen Kontingents für einen bestimmten Benutzer](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-additional-quotas-for-specific-users)
  
   - [Aktivieren von automatischem Herunterfahren beim Trennen der Verbindung](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

        > [!NOTE]
        > Zeitpläne und Kontingente gelten nicht für die Vorlagen-VM, die Einstellungen für das automatische Herunterfahren allerdings schon. 
        > 
        > Wenn Sie ein Lab erstellen,wird die Vorlagen-VM zwar erstellt, aber nicht gestartet. Sie können diese starten, eine Verbindung herstellen, eine beliebige erforderliche Software für das Lab installieren und die VM dann veröffentlichen. Wenn Sie die Vorlagen-VM veröffentlichen, wird diese automatisch für Sie heruntergefahren, wenn Sie dies nicht bereits selbst getan haben. 
        > 
        > Bei Vorlagen-VMs entstehen **Kosten**, wenn sie ausgeführt werden. Stellen Sie daher sicher, dass die Vorlagen-VM heruntergefahren wird, wenn sie nicht ausgeführt werden muss. 


1. **Verwenden Sie das Dashboard.** Entsprechende Anleitungen finden Sie unter [Verwenden des Dashboards des Labs](https://docs.microsoft.com/azure/lab-services/classroom-labs/use-dashboard).

    > [!NOTE]
    > Die im Dashboard angezeigten geschätzten Kosten sind die maximalen Kosten, die Sie für die Nutzung des Labs durch Kursteilnehmer erwarten können. Beispielsweise werden Ihnen durch Kursteilnehmer nicht genutzte Kontingentstunden *nicht* in Rechnung gestellt. Die geschätzten Kosten spiegeln *nicht* wider, ob Gebühren für die Verwendung der Vorlagen-VM oder der Shared Image Gallery anfallen bzw. wenn der Lab-Ersteller einen Benutzercomputer startet.

## <a name="next-steps"></a>Nächste Schritte

- [Nachverfolgen der Nutzung eines Classroom-Labs](tutorial-track-usage.md)
  
- [Zugreifen auf ein Classroom-Lab](tutorial-connect-virtual-machine-classroom-lab.md)
