---
title: Einrichten von Project Lead The Way-Labs mit Azure Lab Services
description: Erfahren Sie, wie Sie Labs einrichten, um Project Lead The Way-Kurse zu unterrichten.
ms.topic: article
ms.date: 10/28/2020
ms.openlocfilehash: e3783ae4fa07bf783841022903c4bcf3ab6fbe23
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94648003"
---
# <a name="set-up-labs-for-project-lead-the-way-classes"></a>Einrichten von Labs für Project Lead The Way-Kurse

[Project Lead The Way (PLTW)](https://www.pltw.org/) ist eine gemeinnützige Organisation, die in den USA einen PreK-12-Lehrplan für Informatik, Engineering und biomedizinische Wissenschaft bereitstellt.  In jedem PLTW-Kurs verwenden die Kursteilnehmer eine Reihe von Softwareanwendungen als Teil ihrer praktischen Lernerfahrung.  Für viele Softwareanwendungen ist entweder eine schnelle CPU oder in einigen Fällen eine GPU erforderlich.  In diesem Artikel erfahren Sie, wie Sie Labs für die folgenden PLTW-Kurse einrichten, die in der Regel für Kursteilnehmer der Klassen 9 bis 12 angeboten werden:

- **Introduction to Engineering Design (Einführung in technische Konstruktion)**

    Die Kursteilnehmer werden in den Prozess der technischen Konstruktion eingeführt, wozu auch die Verwendung der [CAD-Software (Computer-Aided Design) Inventor von Autodesk](https://www.autodesk.com/products/inventor/new-features) für 3D-Modellierung gehört.

- **Principles of Engineering (Grundlagen der Konstruktion)**
    
    Die Kursteilnehmer erfahren mehr über Konstruktionsmechanismen, strukturelle Aspekte/Materialstärke und Automatisierung.  Dieser Kurs verwendet Software wie [MD Solids](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/MD+Solids/MD+Solids+Software+Installation+Guide.pdf), [West Point Bridge Designer](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/West+Point+Bridge+Builder/Installation+Guide+for+West+Point+Bridge+Designer.pdf) und die [America‘s Army-Simulation](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/America's+Army/Installation+Guide+for+Americas+Army+Simulation+17-18.pdf).

- **Civil Engineering and Architecture(Bauwesen und Architektur)**

    Die Kursteilnehmer werden mit der Architekturdesignsoftware [Autodesk Revit](https://www.autodesk.com/products/revit/overview) für 3D-Gebäudeinformationsmodellierung (Building Information Modeling, BIM) im Bereich Bau- und Standortentwurf und -entwicklung unterrichtet.

- **Computer Integrated Manufacturing (CIM)**

    Die Kursteilnehmer untersuchen moderne Fertigungsprozesse, die Robotik und Automatisierung einbeziehen.   In diesem Kurs verwenden die Kursteilnehmer die [CAD-Software Inventor von Autodesk](https://www.autodesk.com/products/inventor/new-features) und die [CAM-Software Inventor von Autodesk](https://www.autodesk.com/products/inventor-cam/overview). 

- **Digital Electronics (Digitale Elektronik)**

    Die Kursteilnehmer untersuchen elektronische Logikschaltungen und -geräte mit der Simulations- und Schaltkreisentwurfssoftware [Multisim von National Instruments](https://www.ni.com/en-us/shop/electronic-test-instrumentation/application-software-for-electronic-test-and-instrumentation-category/what-is-multisim.html).

- **Engineering Design and Development (Konstruktionsdesign und -entwicklung)**

    Die Kursteilnehmer tragen zu einer End-to-End-Lösung bei, die Forschung, Entwurf und Tests kombiniert und die sie einem Gremium von Ingenieuren vorstellen.  In diesem Kurs verwenden die Kursteilnehmer die [CAD-Software Inventor von Autodesk](https://www.autodesk.com/products/inventor/new-features).

- **Computer Science Essentials (Grundlagen der Informatik)**

    Die Kursteilnehmer werden in Informatikkonzepte und -tools eingeführt.  Sie beginnen mit blockbasierter Programmierung und fahren dann mit textbasierter Codierung mithilfe von Codierungsumgebungen wie [VEXcode V5 Blocks](https://s3.amazonaws.com/support-downloads.pltw.org/2020-21/VEXcode+V5+Blocks/VexCode+V5+Blocks+Installation+Guide.pdf) fort.

- **Computer Science Principles (Prinzipien der Informatik)**
    
    Die Kursteilnehmer erweitern ihre Programmierkenntnisse mit [Python](https://www.python.org/) in der [Visual Studio Code-Entwicklungsumgebung von Microsoft](https://code.visualstudio.com/). 

- **Computer Science A (Informatik A)**

    Die Kursteilnehmer erweitern ihre Programmierkenntnisse in diesem Kurs, indem sie die Entwicklung mobiler Apps erlernen.  In diesem Kurs lernen sie [Java](https://www.java.com/) in der [Visual Studio Code-Entwicklungsumgebung von Microsoft](https://code.visualstudio.com/) kennen.  Außerdem verwenden die Kursteilnehmer einen Emulator, der Ihnen das Ausführen und Testen Ihres Codes für mobile Apps ermöglicht.  Um Informationen zum Einrichten eines Emulators in Azure Labs zu erhalten, kontaktieren Sie uns unter azlabspilot@microsoft.com.

Auf der Website von PLTW finden Sie die [vollständige Liste der Software](https://www.pltw.org/pltw-software) für jeden Kurs.

## <a name="lab-configuration"></a>Labkonfiguration
Zum Einrichten von Labs für PLTW benötigen Sie zunächst ein Azure-Abonnement und ein Lab-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Sobald Sie ein Azure-Abonnement erhalten, können Sie ein neues Lab-Konto in Azure Lab Services erstellen. Weitere Informationen zum Erstellen eines neuen Lab-Kontos finden Sie im Tutorial zum [Einrichten eines Lab-Kontos](./tutorial-setup-lab-account.md). Sie können auch ein vorhandenes Lab-Konto verwenden.

Sobald Sie über ein Lab-Konto verfügen, sollten Sie separate Labs für jede Sitzung eines PLTW-Kurses erstellen, die Ihre Bildungseinrichtung anbietet.  Außerdem wird empfohlen, separate Images für jeden Typ von PLTW-Kurs zu erstellen.  Weitere Informationen zum Strukturieren von Labs und Images finden Sie im folgenden Blogbeitrag: [Moving from a Physical Lab to Azure Lab Services](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931) (Von einem physischen Lab zu Azure Lab Services).

### <a name="lab-account-settings"></a>Lab-Kontoeinstellungen
Aktivieren Sie die Einstellungen für das Lab-Konto, die in der nachfolgenden Tabelle beschrieben werden. Weitere Informationen zum Aktivieren von Marketplace-Images finden Sie im Artikel zum [Angeben von für Lab-Ersteller verfügbaren Marketplace-Images](./specify-marketplace-images.md).

| Lab-Kontoeinstellungen | Instructions |
| -------------------- | ----- |
| Marketplace-Image | Aktivieren Sie das Windows 10 Pro-Image zur Verwendung in Ihrem Lab-Konto. |

### <a name="lab-settings"></a>Lab-Einstellungen
Die Größe der VM, die wir für PLTW-Kurse empfehlen, hängt von der Art der Workloads ab, die Ihre Kursteilnehmer im Kurs verwenden.  Für die oben genannten Kurse empfehlen wir die Verwendung von VM-Größen für große und kleine GPU (Visualisierung).  Informationen zum Einrichten von Labs für Ihre PLTW-Kurse finden Sie in der folgenden Tabelle.

| Lab-Einrichtung | Wert/Anweisungen |
| ------------ | ------------------ |
|VM-Größe| **Kleine GPU (Visualisierung)** .  Diese VM eignet sich am besten für Remotevisualisierung, Streaming, Gaming und Codierung mit Frameworks wie OpenGL und DirectX.  Es wird empfohlen, diese Größe für die folgenden PLTW-Kurse zu verwenden: Civil Engineering and Architecture (Bauwesen und Architektur), Digital Electronics (Digitale Elektronik), Computer Integrated Manufacturing (CIM) und Engineering Design and Development (Konstruktionsdesign und -entwicklung).
|VM-Größe| **Groß**.  Diese Größe eignet sich am besten für Anwendungen, die schnellere CPUs, eine bessere lokale Datenträgerleistung, große Datenbanken und große Caches benötigen.  Es wird empfohlen, diese Größe für die folgenden PLTW-Kurse zu verwenden: Introduction to Engineering Design (Einführung in technische Konstruktion), Principles of Engineering (Grundlagen der Konstruktion), Computer Science Essentials (Grundlagen der Informatik), Computer Science Principles (Prinzipien der Informatik) und Computer Science A (Informatik A).

### <a name="licensing-server"></a>Lizenzierungsserver
Der größte Teil der Software, die in den oben genannten PLTW-Kursen verwendet wird, benötigt **_keinen_* _ Zugriff auf einen Lizenzierungsserver.  Sie müssen jedoch auf einen Lizenzierungsserver zugreifen, wenn Sie beabsichtigen, das Netzwerklizenzierungsmodell von Autodesk für die folgende Software zu verwenden:
-   Revit
-   Inventor
-   Inventor CAM

Um Netzwerklizenzierung mit Software von Autodesk zu verwenden, [beschreibt PLTW detaillierte Schritte](https://www.pltw.org/pltw-software) zur Installation des Lizenz-Managers von Autodesk auf Ihrem Lizenzierungsserver.  Dieser Lizenzierungsserver befindet sich in der Regel in Ihrem lokalen Netzwerk, oder er wird auf einem virtuellen Azure-Computer (VM) in einem virtuellen Azure-Netzwerk (VNET) gehostet.

Nach dem Einrichten des Lizenzservers müssen Sie eine [Peerverbindung zwischen dem VNET](./how-to-connect-peer-virtual-network.md) und Ihrem [Lab-Konto](./tutorial-setup-lab-account.md) herstellen. Das Netzwerkpeering muss vor dem Erstellen des Labs erfolgen, damit die virtuellen Lab-Computer auf den Lizenzserver zugreifen können und umgekehrt.

Die von Autodesk generierten Lizenzdateien betten die MAC-Adresse des Lizenzierungsservers ein.  Wenn Sie Ihren Lizenzierungsserver über eine Azure-VM hosten möchten, müssen Sie sicherstellen, dass sich die MAC-Adresse Ihres Lizenzierungsservers nicht ändert.   Andernfalls müssen die Lizenzierungsdateien neu generiert werden, wenn sich die MAC-Adresse ändert.  Befolgen Sie diese Tipps, um zu verhindern, dass sich Ihre MAC-Adresse ändert:

- [Legen Sie eine statische private IP- und MAC-Adresse](./how-to-create-a-lab-with-shared-resource.md#static-private-ip-and-mac-address) für die Azure-VM fest, die Ihren Lizenzierungsserver hostet.
- Stellen Sie sicher, dass Sie sowohl Ihr Lab-Konto als auch das VNET des Lizenzierungsservers in einer Region bzw. an einem Standort mit ausreichender VM-Kapazität einrichten, damit Sie diese Ressourcen nicht zu einem späteren Zeitpunkt in eine neue Region oder an einen neuen Standort verschieben müssen.

Lesen Sie außerdem den Artikel zum [Einrichten eines Lizenzierungsservers als freigegebene Ressource](./how-to-create-a-lab-with-shared-resource.md), um weitere Informationen zu erhalten.

### <a name="template-machine"></a>Vorlagencomputer
Einige der Installationsdateien, die Sie für PLTW benötigen, sind groß und das Kopieren dauert lange, wenn Sie sie auf den Vorlagencomputer eines Labs herunterladen.

Anstatt Installationsdateien auf den Vorlagencomputer herunterzuladen und dort alles zu installieren, empfiehlt es sich, Ihre PLTW-Images in Ihrer physischen Umgebung zu erstellen.  Anschließend können Sie die Images in Shared Image Gallery importieren, damit Sie diese benutzerdefinierten Images zum Erstellen Ihrer Labs verwenden können.  Lesen Sie den folgenden Artikel, um weitere Informationen zu erhalten: [Hochladen eines benutzerdefinierten Images in Shared Image Gallery](./upload-custom-image-shared-image-gallery.md).

Abgesehen von dieser Empfehlung finden Sie hier die wichtigsten Aufgaben beim Einrichten eines Labs:

1. Erstellen Sie in ihrer physischen Umgebung das Image für den Kurs.

    a.  Befolgen Sie die detaillierten Schritte von PLTW zum Herunterladen von Installationsdateien und zum Installieren der erforderlichen Software.

    > [!NOTE]    
    > Wenn Sie Anwendungen von Autodesk installieren, muss der Computer, auf dem Sie Autodesk installieren, mit Ihrem Lizenzierungsserver kommunizieren können (der Installations-Assistent von Autodesk fordert Sie auf, den Computernamen des Computers anzugeben, auf dem der Lizenzierungsserver gehostet wird).  Wenn Sie Ihren Lizenzierungsserver auf einer Azure-VM hosten, müssen Sie möglicherweise mit der Installation von Autodesk auf dem Vorlagencomputer des Labs warten, damit der Installations-Assistent von Autodesk auf Ihren Lizenzierungsserver zugreifen kann.

    b.  [Installieren und konfigurieren Sie OneDrive](./how-to-prepare-windows-template.md#install-and-configure-onedrive) (oder andere Sicherungsoptionen, die Ihre Bildungseinrichtung ggf. verwendet).
    
    c.  [Installieren und konfigurieren Sie Windows-Updates](./how-to-prepare-windows-template.md#install-and-configure-updates).

1.  Laden Sie das benutzerdefinierte Image in die [Shared Image Gallery hoch, die an Ihr Lab-Konto angefügt ist](./how-to-attach-detach-shared-image-gallery.md).

1.  Erstellen Sie ein Lab, und wählen Sie das benutzerdefinierte Image aus, das Sie im vorherigen Schritt hochgeladen haben.

1.  Starten Sie nach dem Erstellen des Labs den Vorlagencomputer, und stellen Sie eine Verbindung mit diesem her, um zu überprüfen, ob das Image erwartungsgemäß funktioniert.

1.  Veröffentlichen Sie schließlich den Vorlagencomputer, um die VMs der Kursteilnehmer zu erstellen.

## <a name="student-devices"></a>Kursteilnehmergeräte
Ihre Kursteilnehmer können mit Windows-\Mac-Computern und Chromebooks eine Verbindung mit ihren Lab-VMs herstellen.  Hier finden Sie Links zu Anleitungen für jede dieser Optionen:

- [Herstellen einer Verbindung aus Windows](./how-to-use-classroom-lab.md#connect-to-the-vm)
- [Verbindungsherstellung über Mac](./connect-virtual-machine-mac-remote-desktop.md)
- [Verbindungsherstellung über Chromebook](./connect-virtual-machine-chromebook-remote-desktop.md)

## <a name="cost"></a>Kosten
Betrachten wir eine mögliche Kostenschätzung für die oben genannten PLTW-Kurse.  Diese Schätzung umfasst nicht die Kosten für die Ausführung eines Lizenzierungsservers oder die Verwendung der Shared Image Gallery.  Wir nehmen einen Kurs mit 25 Kursteilnehmern an.  Es ist eine Kursdauer von 20 Stunden geplant.  Zudem erhält jeder Kursteilnehmer ein Kontingent von 10 Stunden für Hausaufgaben und Aufgaben außerhalb der regulären Kurszeiten.  Weitere Informationen finden Sie in den folgenden Kostenschätzungen für die GPU-Größen **Groß** und **Klein (Visualisierung)** .

- **Große VM**

    25 Kursteilnehmer x (20 geplante Stunden + 10 Kontingentstunden) x 70 Lab-Einheiten x 0,01 USD pro Stunde = 525,00 USD

- **Kleine GPU (Visualisierung)**

    25 Kursteilnehmer x (20 geplante Stunden + 10 Kontingentstunden) x 160 Lab-Einheiten x 0,01 USD pro Stunde = 1.200,00 USD

> [!IMPORTANT] 
> Diese Kostenschätzung dient ausschließlich zu Beispielzwecken.  Weitere Informationen zu den aktuellen Preisen finden Sie unter [Azure Lab Services – Preise](https://azure.microsoft.com/pricing/details/lab-services/).

> [!NOTE] 
> Viele der PLTW-Kurse verwenden Anwendungen, auf die über einen Browser zugegriffen wird, z. B. MIT App Inventor.  Diese browserbasierten Anwendungen erfordern keine schnelle CPU oder GPU und können von jedem Gerät aus aufgerufen werden, das über eine Internetverbindung verfügt.  Wenn Kursteilnehmer diese Arten von Anwendungen verwenden, empfiehlt es sich, den Browser auf dem physischen Gerät zu verwenden, anstatt den Browser auf den Lab-VMs zu nutzen.  Dies trägt dazu bei, die Kosten zu senken, indem die Lab-VMs nur für Anwendungen verwendet werden, die eine schnelle CPU- oder GPU-Leistung erfordern.

## <a name="next-steps"></a>Nächste Schritte
Die nächsten Schritte sind die gleichen für sämtliche Labs:

- [Hinzufügen von Benutzern](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kontingent festlegen](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zeitplan festlegen](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [Registrierungslinks an Kursteilnehmer senden](how-to-configure-student-usage.md#send-invitations-to-users).