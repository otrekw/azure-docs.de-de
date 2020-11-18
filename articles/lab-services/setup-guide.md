---
title: Leitfaden für beschleunigte Lab-Einrichtung für Azure Lab Services
description: Dieser Leitfaden unterstützt Lab-Ersteller beim schnellen Einrichten eines Lab-Kontos für die Verwendung innerhalb ihrer Bildungseinrichtung.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 8ef168aefb69df32f57b623bb488adbb97cbd411
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94659674"
---
# <a name="lab-setup-guide"></a>Leitfaden für die Lab-Einrichtung

Der Vorgang zum Veröffentlichen eines Labs für Ihre Kursteilnehmer kann einige Stunden in Anspruch nehmen.  Die Zeitspanne hängt von der Anzahl der virtuellen Computer (VMs) ab, die im Lab erstellt werden. Zum Einrichten eines Labs sollten Sie mindestens einen Tag einplanen, um sicherzustellen, dass es einwandfrei funktioniert und ausreichend Zeit für die Veröffentlichung der VMs von Kursteilnehmern zur Verfügung steht.

## <a name="understand-the-lab-requirements-of-your-class"></a>Informationen zu den Lab-Anforderungen für Ihren Kurs

Vor dem Einrichten eines neuen Labs sollten Sie sich die folgenden Fragen stellen:

### <a name="what-software-requirements-does-the-class-have"></a>Welche Softwareanforderungen gelten für den Kurs?

Entscheiden Sie aufgrund der Lernziele Ihres Kurses, welches Betriebssystem, welche Anwendungen und Tools auf den VMs des Labs installiert werden müssen. Zum Einrichten von Lab-VMs haben Sie drei Möglichkeiten:

- **Verwenden Sie ein Azure Marketplace-Image**: Azure Marketplace bietet Hunderte von Images, die Sie beim Erstellen eines Labs verwenden können. Für einige Kurse enthält eines dieser Images möglicherweise bereits alles, was Sie für Ihren Kurs benötigen.

- **Erstellen Sie ein neues benutzerdefiniertes Image**: Sie können Ihr eigenes benutzerdefiniertes Image erstellen, indem Sie ein Azure Marketplace-Image als Ausgangspunkt verwenden und es anpassen, indem Sie zusätzliche Software installieren und Konfigurationsänderungen vornehmen.

- **Verwenden Sie ein vorhandenes benutzerdefiniertes Image**: Sie können vorhandene benutzerdefinierte Images wiederverwenden, die Sie zuvor erstellt haben oder die von anderen Administratoren oder Lehrkräften an Ihrer Bildungseinrichtung erstellt wurden. Wenn Sie benutzerdefinierte Images verwenden möchten, müssen Ihre Administratoren einen Shared Image Gallery-Dienst einrichten.  Ein Shared Image Gallery-Dienst ist ein Repository, das zum Speichern benutzerdefinierter Images.

> [!NOTE]
> Ihre Administratoren sind für die Aktivierung von Azure Marketplace-Images und benutzerdefinierten Images zuständig, damit Sie sie verwenden können. Stimmen Sie dies mit Ihrer IT-Abteilung ab, um sicherzustellen, dass die benötigten Images aktiviert sind. Benutzerdefinierte Images, die Sie erstellen, werden automatisch für die Verwendung in Labs aktiviert, deren Besitzer Sie sind.

### <a name="what-hardware-requirements-does-the-class-have"></a>Welche Hardwareanforderungen gelten für den Kurs?

Es gibt eine Vielzahl von Computegrößen, aus denen Sie auswählen können:

- Die Größen der geschachtelten Virtualisierung, damit Sie den Kursteilnehmern Zugriff auf eine VM erteilen können, die mehrere, geschachtelte VMs hosten kann. So könnten Sie diese Computegröße beispielsweise für Netzwerk- oder Hackingkurse verwenden.

- GPU-Größen, damit Ihre Kursteilnehmer rechenintensive Anwendungstypen verwenden können. Diese Auswahl wird oft mit künstlicher Intelligenz und maschinellem Lernen verwendet.

Anleitungen zum Auswählen der richtigen VM-Größe finden Sie in den folgenden Artikeln:
- [Festlegen der VM-Größe](./administrator-guide.md#vm-sizing)
- [Von einem physischen Lab zu Azure Lab Services](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931)

> [!NOTE]
> Abhängig von der Region Ihres Labs sind möglicherweise weniger Computegrößen verfügbar, da dies je nach Region variiert. Generell sollten Sie die kleinste Computegröße auswählen, die Ihren Anforderungen am ehesten entspricht. Bei Bedarf können Sie mit Azure Lab Services zu einem späteren Zeitpunkt ein neues Lab mit einer anderen Computekapazität einrichten.

### <a name="what-dependencies-does-the-class-have-on-external-azure-or-network-resources"></a>Welche Abhängigkeiten von externen Azure- oder Netzwerkressourcen gibt es für den Kurs?
Ihre Lab-VMs benötigen möglicherweise Zugriff auf externe Ressourcen, z. B. den Zugriff auf eine Datenbank, eine Dateifreigabe oder einen Lizenzierungsserver.  Damit Ihre Lab-VMs externe Ressourcen verwenden können, sollten Sie sich mit Ihren IT-Administratoren koordinieren.

> [!NOTE]
> Sie sollten berücksichtigen, ob Sie die Abhängigkeiten Ihres Labs von externen Ressourcen verringern können, indem Sie die Ressource direkt auf der VM bereitstellen. Wenn Sie beispielsweise das Lesen von Daten aus einer externen Datenbank vermeiden möchten, können Sie die Datenbank direkt auf der VM installieren.  

### <a name="how-will-costs-be-controlled"></a>Wie werden die Kosten kontrolliert?
Lab Services verwendet ein Preismodell mit nutzungsbasierter Bezahlung. Dies bedeutet: Sie müssen nur für die Zeit bezahlen, in der eine Lab-VM ausgeführt wird. Zur Kostenkontrolle haben Sie drei Optionen, die in der Regel zusammen verwendet werden:

- **Zeitplan:** Mit einem Zeitplan können Sie automatisch steuern, wann die VMs Ihrer Labs gestartet und heruntergefahren werden.
- **Kontingent**: Das Kontingent steuert die Anzahl der Stunden, die Kursteilnehmer außerhalb der geplanten Stunden auf eine VM können.  Wenn ein Kursteilnehmer seine VM verwendet und das Kontingent erreicht ist, wird der virtuelle Computer automatisch heruntergefahren.  Der Kursteilnehmer kann die VM nur dann neu starten, wenn das Kontingent verlängert wurde.
- **Automatisches Herunterfahren**: Wenn diese Einstellung aktiviert ist, bewirkt das automatische Herunterfahren, dass Windows-VMs automatisch heruntergefahren werden, nachdem ein Kursteilnehmer die Verbindung mit einer Remotedesktopprotokoll-Sitzung (RDP) getrennt hat. Diese Einstellung ist standardmäßig deaktiviert.

Weitere Informationen finden Sie in den folgenden Artikeln:
- [Schätzen der Kosten](./cost-management-guide.md#estimate-the-lab-costs)
- [Verwalten von Kosten](./cost-management-guide.md#manage-costs)

### <a name="how-will-students-save-their-work"></a>Wie speichern Kursteilnehmer ihre Arbeit?
Kursteilnehmern wird für die Lebensdauer des Labs jeweils eine eigene VM zugewiesen. Sie haben folgende Auswahlmöglichkeiten:

- Direktes Speichern auf der VM.
- Speichern an einem externen Speicherort, z. B. OneDrive oder GitHub.

Es ist möglich, OneDrive für Kursteilnehmer auf Ihren Lab-VMS automatisch konfigurieren zu lassen.

> [!NOTE]
> Wenn Sie sicherstellen möchten, dass Ihre Kursteilnehmer außerhalb des Labs und nach dem Ende des Kurses weiterhin auf ihre gespeicherte Arbeit zugreifen können, sollten sie ihre Arbeit in einem externen Repository speichern.

### <a name="how-will-students-connect-to-their-vm"></a>Wie stellen Kursteilnehmer eine Verbindung mit ihrer VM her?
Bei VMs mit RDP zu Windows wird empfohlen, dass die Kursteilnehmer den [Microsoft-Remotedesktopclient](/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients) verwenden. Remotedesktopclient unterstützt Mac-, Chromebook- und Windows-Computer.

Bei Linux-VMs können die Kursteilnehmer SSH oder RDP verwenden. Damit die Kursteilnehmer eine Verbindung über RDP herstellen können, müssen Sie die erforderlichen RDP- und GUI-Pakete installieren und konfigurieren.

### <a name="will-students-also-be-using-microsoft-teams"></a>Werden Kursteilnehmer auch Microsoft Teams verwenden?
Azure Lab Services ist in Microsoft Teams integriert, sodass Lehrkräfte ihre Labs innerhalb von Teams erstellen und verwalten können.  Ebenso können Kursteilnehmer innerhalb von Teams auf das Lab zugreifen.

Weitere Informationen finden Sie im folgenden Artikel:
- [Azure Lab Services in Microsoft Teams](./lab-services-within-teams-overview.md)

## <a name="set-up-your-lab"></a>Einrichten des Labs

Nachdem Sie sich mit den Anforderungen für das Lab Ihres Kurses vertraut gemacht haben, können Sie es jetzt einrichten. Folgen Sie den Links in diesem Abschnitt, um zu erfahren, wie Sie Ihr Lab einrichten.  Beachten Sie, dass verschiedene Schritte bereitgestellt werden, je nachdem, ob Sie Labs innerhalb von Teams verwenden.

1. **Erstellen Sie ein Lab.** Weitere Informationen finden Sie in den Tutorials zum Erstellen eines Labs:
    - Sehen Sie sich den Artikel [Erstellen eines Classroom-Labs](./tutorial-setup-classroom-lab.md#create-a-classroom-lab) an.
    - [Erstellen eines Labs über Teams](./how-to-get-started-create-lab-within-teams.md)

    > [!NOTE]
    > Wenn für Ihren Kurs eine geschachtelte Virtualisierung erforderlich ist, lesen Sie die Schritte in [Aktivieren von geschachtelter Virtualisierung](./how-to-enable-nested-virtualization-template-vm.md).

1. **Passen Sie Images an, und veröffentlichen Sie Lab-VMs.** Stellen Sie eine Verbindung mit einer speziellen VM her, die als „Vorlagen-VM“ (auch: „virtueller Vorlagencomputer“) bezeichnet wird. Lesen Sie dazu die Schritte in den folgenden Anleitungen:
    - [Erstellen und Verwalten einer Vorlagen-VM](./tutorial-setup-classroom-lab.md#publish-the-template-vm)
    - [Verwenden eines Katalogs mit freigegebenen Images](./how-to-use-shared-image-gallery.md)

    > [!NOTE]
    > Wenn Sie Windows verwenden, sollten Sie auch die Anleitungen in [Vorbereiten eines virtuellen Windows-Vorlagencomputers](./how-to-prepare-windows-template.md) lesen. Diese Anweisungen enthalten Schritte zum Einrichten von OneDrive und Office für Ihre Kursteilnehmer.

1. **Verwalten Sie den VM-Pool und die VM-Kapazität.** Sie können die von Ihrem Kurs benötigte VM-Kapazität nach Bedarf problemlos zentral hoch- oder herunterskalieren. Denken Sie aber daran, dass das Erhöhen der VM-Kapazität möglicherweise mehrere Stunden dauert, weil neue VMs eingerichtet werden. Führen Sie die Schritte in den folgenden Artikeln aus:
    - [Einrichten und Verwalten eines VM-Pools](./how-to-set-virtual-machine-passwords.md)
    - [Verwalten eines VM-Pools in Lab Services von Teams aus](./how-to-manage-vm-pool-within-teams.md)

1. **Fügen Sie Lab-Benutzer hinzu, und verwalten Sie sie.** Weitere Informationen zum Hinzufügen von Benutzern zu Ihrem Lab finden Sie in den einzelnen Schritten in den folgenden Tutorials:
   - [Hinzufügen von Benutzern zum Lab](./tutorial-setup-classroom-lab.md#add-users-to-the-lab)
   - [Senden von Einladungen an Benutzer](./tutorial-setup-classroom-lab.md#send-invitation-emails-to-users)
   - [Verwalten von Lab Services-Benutzerlisten aus Teams](./how-to-manage-user-lists-within-teams.md)

    Informationen zu den Typen von Konten, die Kursteilnehmer verwenden können, finden Sie unter [Konten für Kursteilnehmer](./how-to-configure-student-usage.md#student-accounts).
  
1. **Legen Sie Kostenkontrollen fest.** Wenn Sie die Kosten Ihres Labs kontrollieren möchten, legen Sie Zeitpläne, Kontingente und automatisches Herunterfahren fest. Arbeiten Sie die folgenden Tutorials durch:

   - [Zeitplan festlegen](./tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)

        > [!NOTE]
        > Je nach installiertem Betriebssystem dauert es vielleicht mehrere Minuten, bis eine VM gestartet wird. Wenn Sie sicherstellen möchten, dass eine Lab-VM während Ihrer geplanten Stunden einsatzbereit ist, sollten Sie VMs 30 Minuten im Voraus starten.

   - [Festlegen von Kontingenten für Benutzer](./how-to-configure-student-usage.md#set-quotas-for-users) und [Festlegen eines zusätzlichen Kontingents für einen bestimmten Benutzer](./how-to-configure-student-usage.md#set-additional-quotas-for-specific-users)
  
   - [Aktivieren von automatischem Herunterfahren beim Trennen der Verbindung](./how-to-enable-shutdown-disconnect.md)

        > [!NOTE]
        > Zeitpläne und Kontingente gelten nicht für die Vorlagen-VM, die Einstellungen für das automatische Herunterfahren allerdings schon. 
        > 
        > Wenn Sie ein Lab erstellen,wird die Vorlagen-VM zwar erstellt, aber nicht gestartet. Sie können diese starten, eine Verbindung herstellen, eine beliebige erforderliche Software für das Lab installieren und die VM dann veröffentlichen. Wenn Sie die Vorlagen-VM veröffentlichen, wird diese automatisch für Sie heruntergefahren, wenn Sie dies nicht bereits selbst getan haben. 
        > 
        > Bei Vorlagen-VMs entstehen **Kosten**, wenn sie ausgeführt werden. Stellen Sie daher sicher, dass die Vorlagen-VM heruntergefahren wird, wenn sie nicht ausgeführt werden muss.

    - [Erstellen und Verwalten von Lab Services-Zeitplänen in Teams](./how-to-create-schedules-within-teams.md) 

1. **Verwenden Sie das Dashboard.** Entsprechende Anleitungen finden Sie unter [Verwenden des Dashboards des Labs](./use-dashboard.md).

    > [!NOTE]
    > Die im Dashboard angezeigten geschätzten Kosten sind die maximalen Kosten, die Sie für die Nutzung des Labs durch Kursteilnehmer erwarten können. Beispielsweise werden Ihnen durch Kursteilnehmer nicht genutzte Kontingentstunden *nicht* in Rechnung gestellt. Die geschätzten Kosten spiegeln *nicht* wider, ob Gebühren für die Verwendung der Vorlagen-VM oder der Shared Image Gallery anfallen bzw. wenn der Lab-Ersteller einen Benutzercomputer startet.

## <a name="next-steps"></a>Nächste Schritte

- [Nachverfolgen der Nutzung eines Classroom-Labs](tutorial-track-usage.md)
  
- [Zugreifen auf ein Classroom-Lab](tutorial-connect-virtual-machine-classroom-lab.md)