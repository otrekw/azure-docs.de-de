---
title: Azure Lab Services – Administratorhandbuch | Microsoft-Dokumentation
description: Dieses Handbuch hilft Administratoren, die Lab-Konten mit Azure Lab Services erstellen und verwalten.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2019
ms.author: spelluru
ms.openlocfilehash: 638a90615d248b3c2829770432dd6a08eb4bb2fb
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75771733"
---
# <a name="azure-lab-services---administrator-guide"></a>Azure Lab Services – Administratorhandbuch
IT-Administratoren, die die Cloudressourcen einer Organisation verwalten, sind in der Regel auch dafür verantwortlich, das Lab-Konto für Ihre Organisation einzurichten. Administratoren oder Dozenten erstellen Classroom-Labs im Lab-Konto. Dieser Artikel bietet eine allgemeine Übersicht über die beteiligten Azure-Ressourcen und die Anleitungen zu deren Erstellung.

![Allgemeine Übersicht über Azure-Ressourcen in einem Lab-Konto](../media/administrator-guide/high-level-view.png)

- Classroom-Labs werden innerhalb eines Azure-Abonnements gehostet, das im Besitz von Azure Lab Services ist.
- Lab-Konten, Shared Image Gallery und Imageversionen werden innerhalb Ihres Abonnements gehostet.
- Ihr Lab-Konto und die Shared Image Gallery können in derselben Ressourcengruppe sein. In diesem Diagramm befinden Sie sich in verschiedenen Ressourcengruppen. 

## <a name="subscription"></a>Subscription
Ihre Organisation besitzt mindestens ein Azure-Abonnement. Ein Abonnement wird verwendet, um die Abrechnung und Sicherheit für alle darin verwendeten Azure Ressourcen/Dienste, einschließlich Lab-Konten, zu verwalten.

Die Beziehung zwischen einem Lab-Konto und seinem Abonnement ist aus folgenden Gründen wichtig:

- Die Abrechnung wird über das Abonnement gemeldet, das das Lab-Konto enthält.
- Sie können Benutzern im Azure Active Directory (AD)-Mandanten, der dem Abonnement zugeordnet ist, den Zugriff auf Azure Lab Services gewähren. Sie können den Benutzer entweder als Lab-Kontobesitzer/-mitwirkenden oder als Classroom-Lab-Ersteller hinzufügen.

Classroom-Labs und deren virtuelle Computer (Virtual Machines, VMs) werden vollständig für Sie verwaltet. Insbesondere werden sie innerhalb eines dedizierten Abonnements gehostet, das im Besitz von Azure Lab Services ist.

## <a name="resource-group"></a>Resource group
Ein Abonnement enthält mindestens eine Ressourcengruppe. Ressourcengruppen werden verwendet, um logische Gruppierungen von Azure-Ressourcen zu erstellen, die innerhalb der selben Lösung verwendet werden.  

Wenn Sie ein Lab-Konto erstellen, müssen Sie die Ressourcengruppe konfigurieren, die das Lab-Konto enthält. 

Eine Ressourcengruppe ist auch erforderlich, wenn Sie eine [Shared Image Gallery](#shared-image-gallery) erstellen. Sie können Ihr Lab-Konto und die Shared Image Gallery in zwei separaten Ressourcengruppen platzieren, was typisch ist, wenn Sie den Imagekatalog über verschiedene Lösungen hinweg teilen möchten. Oder Sie können sie in derselben Ressourcengruppe platzieren.

Wenn Sie ein Lab-Konto erstellen und automatisch eine Shared Image Gallery erstellen und gleichzeitig anfügen, werden das Lab-Konto und die Shared Image Gallery standardmäßig in separaten Ressourcengruppen erstellt. Dieses Verhalten zeigt sich bei Verwendung der in diesem Tutorial beschriebenen Schritte: [Konfigurieren einer Shared Image Gallery zum Zeitpunkt der Lab-Kontoerstellung](how-to-attach-detach-shared-image-gallery.md#configure-at-the-time-of-lab-account-creation). Das Image zu Beginn dieses Artikels verwendet ebenfalls diese Konfiguration. 

Wir empfehlen, im Voraus Zeit in die Planung der Struktur Ihrer Ressourcengruppen zu investieren, da es nach dem Erstellen einer Ressourcengruppe nicht mehr möglich ist, die Ressourcengruppe eines Lab-Kontos oder der Shared Image Gallery zu ändern. Wenn Sie die Ressourcengruppe für diese Ressourcen ändern müssen, müssen Sie Ihr Lab-Konto und/oder die Shared Image Gallery löschen und neu erstellen.

## <a name="lab-account"></a>Lab-Konto
Ein Lab-Konto dient als Container für ein oder mehrere Classroom-Labs. Beim Einstieg in Azure Lab Services ist es üblich, nur ein einzelnes Lab-Konto zu haben. Wenn sich Ihre Lab-Nutzung steigert, können Sie später immer noch weitere Lab-Konten erstellen.

In der folgenden Liste werden Szenarien hervorgehoben, in denen mehr als ein Lab-Konto von Vorteil sein kann:

- **Verwalten verschiedener Richtlinienanforderungen über Classroom-Labs hinweg** 

    Wenn Sie ein Lab-Konto einrichten, legen Sie Richtlinien fest, die für alle Classroom-Labs unter dem Lab-Konto gelten, wie:
    - Das virtuelle Azure-Netzwerk mit freigegebenen Ressourcen, auf die das Classroom-Lab zugreifen kann. Beispielsweise können Sie über eine Reihe von Classroom-Labs verfügen, die Zugriff auf ein freigegebenes Dataset in einem virtuellen Netzwerk benötigen.
    - Die VM-Images, die von den Classroom-Labs zum Erstellen virtueller Computer verwendet werden können. Beispielsweise können Sie über eine Reihe von Classroom-Labs verfügen, die Zugriff auf das Marketplace-Image der [Data Science VM für Linux](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.linux-data-science-vm-ubuntu) benötigen.  

    Wenn Sie über Classroom-Labs verfügen, die eindeutige Richtlinienanforderungen besitzen, kann es vorteilhaft sein, separate Lab-Konten zu erstellen, um diese Classroom-Labs getrennt zu verwalten.
- **Einschränken des Zugriffs auf bestimmte Classroom-Labs durch Lab-Ersteller**  

    Wenn ein Benutzer als Lab-Ersteller hinzugefügt wird, erhält er Zugriff auf alle Classroom-Labs innerhalb des Lab-Kontos, einschließlich der Labs, die von anderen Lab-Erstellern erstellt werden. Um Lab-Ersteller auf die Verwaltung bestimmter Labs zu beschränken, können Sie separate Lab-Konten erstellen, um den Umfang Ihres Zugriffs einzuschränken. Beispielsweise können Sie ein separates Lab-Konto für jede Abteilung innerhalb einer Universität erstellen. Beispiel: Ein Lab-Konto für die Naturwissenschaftsabteilung und ein anderes für die Mathematikabteilung usw.   
- **Separates Budget nach Lab-Konto**

    Anstatt alle Classroom-Kosten für ein einzelnes Lab-Konto melden zu lassen, benötigen Sie möglicherweise ein eindeutiger aufgeschlüsseltes Budget. Wenn Sie mit dem Beispiel aus dem vorangehenden Aufzählungspunkt fortfahren, können Sie ein Lab-Konto für jede Universitätsabteilung erstellen, um das Budget entsprechend zu trennen. Mithilfe von Azure Cost Management können Sie dann die Kosten für jedes einzelne Lab-Konto anzeigen.
- **Isolieren von Pilot-Labs von aktiven Labs**

    Möglicherweise gibt es Fälle, in denen Sie Richtlinienänderungen als Pilot in ein Lab-Konto übertragen möchten, ohne dass sich diese potenziell auf aktive Labs auswirken. In dieser Art von Szenario ermöglicht Ihnen das Erstellen eines separaten Lab-Kontos für Pilotzwecke, Änderungen zu isolieren. 

## <a name="classroom-lab"></a>Classroom-Lab
Ein Classroom-Lab enthält mindestens einen virtuellen Computer, der jeweils einem bestimmten Schüler zugewiesen ist. Generell können Sie von Folgendem ausgehen:

- Vorhandensein eines Classroom-Labs für jeden Kurs.
- Erstellen einer neuen Gruppe von Classroom-Labs pro Semester (oder für jeden Zeitraum, über den Ihr Kurs angeboten wird). Typischerweise würden Sie für Kurse, die dieselben Anforderungen an das Image haben, eine [Shared Image Gallery](#shared-image-gallery) verwenden, um Images zwischen Labs und über Semester hinweg zu teilen.

Beachten Sie die folgenden Punkte, wenn Sie ermitteln, wie Sie Ihre Classroom-Labs strukturieren möchten:

- **Alle VMs innerhalb eines Classroom-Labs werden mit demselben veröffentlichten Image bereitgestellt**. Hieraus resultiert, dass Sie, wenn Sie einen Kurs haben, für den verschiedene Lab-Images gleichzeitig veröffentlicht werden müssen, für jeden Kurs gesonderte Classroom-Labs erstellen müssen.
- **Das Nutzungskontingent wird auf Lab-Ebene festgelegt und gilt für alle Benutzer innerhalb des Labs**. Beispielsweise können Sie über eine Gruppe von Dozenten verfügen, die Zugriff auf die VMs eines Kurses benötigen, um sich auf den Unterricht vorzubereiten, aber die Dozenten benötigen nur ein 10-Stunden-Kontingent, während bei dem Kurs eingeschriebene Schüler ein Kontingent von 40 Stunden benötigen. Um unterschiedliche Kontingente für Benutzer festzulegen, müssen Sie gesonderte Classroom-Labs erstellen. Es ist jedoch möglich, auch nach dem Festlegen des Kontingents einem bestimmten Benutzer noch weitere Stunden hinzuzufügen.
- **Der Zeitplan für das Starten oder Herunterfahren wird auf Lab-Ebene festgelegt und gilt für alle virtuellen Computer innerhalb des Labs**. Ähnlich wie bei dem vorherigen Punkt, müssen Sie, wenn Sie unterschiedliche Zeitpläne für Benutzer festlegen müssen, gesonderte Classroom-Labs erstellen. 

## <a name="shared-image-gallery"></a>Shared Image Gallery
Eine Shared Image Gallery einem Lab-Konto angefügt und dient als zentrales Repository zum Speichern von Images. Ein Image wird im Katalog gespeichert, wenn sich ein Dozent entschließt, vom virtuellen Vorlagencomputer eines Classroom-Labs aus zu speichern. Jedes Mal, wenn der Dozent Änderungen an der Vorlagen-VM vornimmt und speichert, werden neue Versionen des Images gespeichert, während die vorherigen Versionen erhalten bleiben.

Kursleiter können eine Imageversion aus der Shared Image Gallery veröffentlichen, wenn sie ein neues Classroom-Lab erstellen. Zwar kann der Katalog mehrere Versionen eines Images speichern, doch können Dozenten während der Erstellung eines Labs nur die letzte Version auswählen.

Shared Image Gallery ist eine optionale Ressource, die Sie möglicherweise nicht sofort benötigen, wenn Sie mit nur ein paar Classroom-Labs beginnen. Die Verwendung der Shared Image Gallery bietet jedoch viele Vorteile, die hilfreich sind, wenn die Anzahl Ihrer Classroom-Labs steigt:

- **Ermöglicht Ihnen das Speichern und Verwalten von Versionen eines Vorlagen-VM-Images**.

    Dies ist hilfreich, wenn Sie ein benutzerdefiniertes Image erstellen oder Änderungen (Software, Konfiguration usw.) an einem Image aus dem öffentlichen Marketplace-Katalog vornehmen.  Beispielsweise ist es für Dozenten üblich, dass für sie unterschiedliche Software/Tools installiert werden müssen. Statt von Schülern zu fordern, dass sie diese Voraussetzungen selbst installieren, können verschiedene Versionen des Vorlagen-VM-Images in einer Shared Image Gallery gespeichert werden. Diese Imageversionen können dann zum Erstellen neuer Classroom-Labs verwendet werden.
- **Aktiviert die Freigabe\Wiederverwendung von Vorlagen-VM-Images in Classroom-Labs**.

    Dies verhindert, dass Sie jedes Mal, wenn Sie ein neues Classroom-Lab erstellen, ein Image von Grund auf neu konfigurieren müssen. Wenn z. B. mehrere Kurse angeboten werden, die dasselbe Image benötigen, muss dieses Bild nur einmal erstellt und in der Shared Image Gallery gespeichert werden, damit es von Classroom-Labs gemeinsam verwendet werden kann.
- **Gewährleistet die Imageverfügbarkeit durch Replikation**.

    Wenn Sie aus einem Classroom-Lab heraus in der Shared Image Gallery speichern, wird Ihr Image automatisch in andere Regionen innerhalb desselben geografischen Raums repliziert. Sollte es in einer Region zu einem Ausfall kommen, bleibt das Veröffentlichen der Vorlagen-VM in Ihrem Classroom-Lab davon unbeeinträchtigt, indem ein Imagereplikat auf einer anderen Region verwendet wird. Darüber hinaus hilft es bei der Leistung in Szenarien mit mehreren VM-Veröffentlichungen, indem diese verteilt unter Verwendung unterschiedlicher Replikate erfolgt.

Um freigegebene Images logisch zu gruppieren, haben Sie mehrere Optionen:

- Erstellen mehrerer Shared Image Gallerys. Jedes Lab-Konto kann nur mit einer Shared Image Gallery eine Verbindung herstellen, sodass Sie für diese Option auch mehrere Lab-Konten erstellen müssen.
- Alternativ können Sie eine einzelne Shared Image Gallery verwenden, die von mehreren Lab-Konten gemeinsam genutzt wird. In diesem Fall kann jedes Lab-Konto nur die Images aktivieren, die für die darin enthaltenen Classroom-Labs anwendbar sind.

## <a name="naming"></a>Benennung
Beim Einstieg in Azure Lab Services wird empfohlen, dass Sie Benennungskonventionen für Ressourcengruppen, Lab-Konten, Classroom-Labs und die Shared Image Gallery einrichten. Die Benennungskonventionen, die Sie einrichten, gelten ausschließlich für die Anforderungen Ihrer Organisation, doch in der folgenden Tabelle werden einige allgemeinen Richtlinien erläutert.

| Ressourcentyp | Role | Vorgeschlagenes Muster | Beispiele |
| ------------- | ---- | ----------------- | -------- | 
| Resource group | Enthält mindestens ein Lab-Konto und mindestens eine Shared Image Gallery. | \<Kurzname der Organisation\>-\<Umgebung\>-rg<ul><li>**Kurzname der Organisation** identifiziert den Namen der Organisation, die von der Ressourcengruppe unterstützt wird.</li><li>**Umgebung** identifiziert die Umgebung für die Ressource, z. B. Test oder Produktion.</li><li>**rg** steht für den Ressourcentyp: Ressourcengruppe.</li></ul> | contosouniversitätlabs-rg<br/>contosouniversitätlabs-test-rg<br/>contosouniversitätlabs-prod-rg |
| Lab-Konto | Enthält mindestens ein Lab. | \<Kurzname der Organisation\>-\<Umgebung\>-lk<ul><li>**Kurzname der Organisation** identifiziert den Namen der Organisation, die von der Ressourcengruppe unterstützt wird.</li><li>**Umgebung** identifiziert die Umgebung für die Ressource, z. B. Test oder Produktion.</li><li>**lk** steht für den Ressourcentyp: Lab-Konto.</li></ul> | contosouniversitätlabs-lk<br/>matheabtlabs-lk<br/>naturwissenschaftabtlabs-test-lk<br/>naturwissenschaftabtlabs-prod-lk |
| Classroom-Lab | Enthält mindestens eine VM. |\<Kursname\>-\<Zeitrahmen\>-\<Dozentenbezeichner\><ul><li>**Kursname** identifiziert den Namen des Kurs, der von dem Lab unterstützt wird.</li><li>**Zeitrahmen** identifiziert den Zeitrahmen, in dem der Kurs angeboten wird.</li>**Dozentenbezeichner** identifiziert den Dozenten, der Besitzer des Labs ist.</li></ul> | CS1234-Herbst2019-KatrinKöhler<br/>CS1234-Frühling2019-KatrinKöhler | 
| Shared Image Gallery | Enthält mindestens eine VM-Imageversion. | \<Kurzname der Organisation\>Katalog | contosouniversitätlabskatalog |

Weitere Informationen zur Benennung anderer Azure-Ressourcen finden Sie unter [Namenskonventionen für Azure-Ressourcen](/azure/architecture/best-practices/naming-conventions).

## <a name="regions-or-locations"></a>Regionen oder Standorte
Beim Einrichten Ihrer Azure Lab Services-Ressourcen müssen Sie eine Region oder einen Standort des Rechenzentrums angeben, in dem die Ressource gehostet werden soll. Hier finden Sie weitere Details dazu, wie sich Region/Standort auf jede der folgenden Ressourcen auswirkt, die in Ihrer Lab Services-Bereitstellung verwendet werden:

- **Ressourcengruppe**

    Die Region gibt das Rechenzentrum an, in dem Informationen über die Ressourcengruppe gespeichert werden. In der Ressourcengruppe enthaltene Azure-Ressourcen können sich in anderen Regionen befinden als ihre übergeordneten Ressourcen.
- **Lab-Konto oder Classroom-Lab**

    Der Standort des Lab-Kontos zeigt die Region für diese Ressource an. In dem Lab-Konto erstellte Classroom-Labs können in jeder Region desselben geografischen Raums bereitgestellt werden. Die spezifische Region, in der die VMs des Labs bereitgestellt werden, wird automatisch auf Grundlage der zu diesem Zeitpunkt in der Region verfügbaren Kapazität ausgewählt.  
    Wenn ein Administrator den Erstellern von Labs gestattet, den Standort ihrer Classroom-Labs auszuwählen, basieren die zur Auswahl stehenden Standorte auf der zum Zeitpunkt der Erstellung des Labs verfügbaren regionalen Kapazität.

    Der Standort des Classroom-Labs bestimmt außerdem die VM-Computegrößen, die für die Auswahl zur Verfügung stehen. Bestimmte Computegrößen sind nur innerhalb bestimmter Regionen verfügbar.
- **Shared Image Gallery**

    Die Region zeigt die Quellregion an, in der die erste Imageversion gespeichert wird, bevor sie automatisch in Zielregionen repliziert wird.
    
Eine allgemeine Regel ist es, die Region/den Standort einer Ressource auf eine/n festzulegen, die/der ihren Benutzern am nächsten liegt. Bei Classroom-Labs bedeutet dies, das Classroom-Lab in maximaler Nähe zu Ihren Schülern zu erstellen. Für Onlinekurse, bei denen Schüler sich weltweit verteilt aufhalten, müssen Sie nach Ihrem Ermessen verfahren, um ein Classroom-Lab zu erstellen, das zentral angesiedelt ist. Alternativ können Sie einen Kurs auch in mehrere Classroom-Labs aufteilen, basierend auf der jeweiligen Region Ihrer Schüler.

## <a name="vm-sizing"></a>Festlegen der VM-Größe
Wenn Administratoren oder Ersteller von Labs ein Classroom-Lab erstellen, können Sie unter den folgenden VM-Größen auswählen, basierend auf den Anforderungen für Ihren Kurs. Denken Sie daran, dass die Computegrößen, die verfügbar sind, von der Region abhängen, in der sich Ihr Lab-Konto befindet:

| Size | Spezifikationen | Vorgeschlagene Verwendung |
| ---- | ----- | ------------- |
| Klein| <ul><li>2 Kerne</li><li>3,5 GB RAM</li></ul> | Diese Größe eignet sich am besten für die Befehlszeile, das Öffnen von Webbrowsern, Webserver mit geringem Datenverkehr und kleine bis mittelgroße Datenbanken. |
| Medium | <ul><li>4 Kerne</li><li>7 GB RAM</li></ul> | Diese Größe eignet sich am besten für relationale Datenbanken, speicherinternes Caching und Analysen. |
| Mittel (geschachtelte Virtualisierung) | <ul><li>4 Kerne</li><li>16 GB RAM</li></ul> | Diese Größe eignet sich am besten für relationale Datenbanken, speicherinternes Caching und Analysen.  Sie unterstützt auch die geschachtelte Virtualisierung. |
| Groß | <ul><li>8 Kerne</li><li>32 GB RAM</li></ul> | Diese Größe eignet sich am besten für Anwendungen, die schnellere CPUs, eine bessere lokale Datenträgerleistung, große Datenbanken und große Caches benötigen.  Sie unterstützt auch die geschachtelte Virtualisierung. |
| Kleine GPU (Visualisierung) | <ul><li>6 Kerne</li><li>56 GB RAM</li> | Diese Größe eignet sich am besten für Remotevisualisierung, Streaming, Spiele und Codierung mit Frameworks wie OpenGL und DirectX. |
| Kleine GPU (Compute) | <ul><li>6 Kerne</li><li>56 GB RAM</li></ul> |Diese Größe eignet sich am besten für rechenintensive Anwendungen wie künstliche Intelligenz und Deep Learning. |
| Mittlere GPU (Visualisierung) | <ul><li>12 Kerne</li><li>112 GB RAM</li></ul> | Diese Größe eignet sich am besten für Remotevisualisierung, Streaming, Spiele und Codierung mit Frameworks wie OpenGL und DirectX. |

## <a name="manage-identity"></a>Verwalten der Identität
Es gibt zwei Arten von Rollen, die ein Lab-Kontoadministrator haben kann:

- **Besitzer**

    Ein Administrator, dem die Rolle **Besitzer** zugewiesen ist, besitzt Vollzugriff auf das Lab-Konto, einschließlich dem Recht, anderen Benutzern den Zugriff auf das Lab-Konto zu gewähren, sowie Ersteller von Labs hinzuzufügen. Der Administrator, der das Lab-Konto erstellt, wird standardmäßig als Besitzer hinzugefügt.
- **Mitwirkender**

    Ein Administrator, dem die Rolle „Mitwirkender“ zugewiesen ist, kann Lab-Kontoeinstellungen ändern, kann aber anderen Benutzern keinen Zugriff gewähren; auch kann er keine Ersteller von Labs hinzufügen.

Wenn Sie einem Lab eine Shared Image Gallery anfügen, erhalten sowohl der Administrator als auch die Ersteller des Labs automatisch den Zugriff, sodass sie Images in dem Katalog anzeigen und speichern können. 

## <a name="pricing"></a>Preise

### <a name="azure-lab-services"></a>Azure Lab Services
Die Preise für Azure Lab Services werden in dem folgenden Artikel beschrieben: [Preise für Azure Lab Services](https://azure.microsoft.com/pricing/details/lab-services/).

Sie müssen außerdem die Preise für die Shared Image Gallery berücksichtigen, wenn Sie planen, sie zum Speichern und Verwalten von Imageversionen zu verwenden. 

### <a name="shared-image-gallery"></a>Shared Image Gallery
Das Erstellen einer Shared Image Gallery und das Anfügen des Katalogs an Ihr Lab sind kostenlos. Es fallen erst Kosten an, nachdem Sie eine Imageversion im Katalog gespeichert haben. Typischerweise sind die Preise für die Verwendung einer Shared Image Gallery praktisch vernachlässigbar, doch es ist wichtig zu verstehen, wie die Berechnung erfolgt, da die Gebühren nicht in den Preisen für Azure Lab Services enthalten sind.  

### <a name="storage-charges"></a>Speichergebühren
Um Imageversionen zu speichern, verwendet eine Shared Image Gallery standardmäßig verwaltete HDD-Datenträger. Die Größe des verwendeten, verwalteten HDD-Datenträgers hängt von der Größe der Imageversion ab, die gespeichert wird. Die Preise finden Sie im folgenden Artikel: [Preise für verwaltete Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/).


### <a name="replication-and-network-egress-charges"></a>Kosten für Replikation und ausgehende Netzwerkdaten
Wenn Sie eine Imageversion mittels einer Vorlagen-VM eines Classroom-Labs speichern, speichert Lab Services diese zuerst in einer Quellregion und repliziert die Quellimageversion dann automatisch in eine oder mehrere Zielregionen. Es ist wichtig zu beachten, dass Azure Lab Services die Quellimageversion automatisch in alle Zielregionen innerhalb des geografischen Raums repliziert, in dem sich das Classroom-Lab befindet. Wenn sich Ihr Classroom-Lab beispielsweise im geografischen Raum der USA befindet, wird eine Imageversion in jede der acht Regionen repliziert, die innerhalb der USA bestehen.

Eine Gebühr für ausgehenden Netzwerkdatenverkehr fällt an, wenn eine Imageversion aus der Quellregion in zusätzliche Zielregionen repliziert wird. Die Höhe der berechneten Gebühr basiert auf der Größe der Imageversion, wenn die Daten des Images anfänglich ausgehend aus der Quellregion übertragen werden.  Details zu den Preisen finden Sie im folgenden Artikel: [Bandbreite: Preisübersicht](https://azure.microsoft.com/pricing/details/bandwidth/).

Kunden von [Education-Lösungen](https://www.microsoft.com/licensing/licensing-programs/licensing-for-industries?rtc=1&activetab=licensing-for-industries-pivot:primaryr3) wird die Zahlung von Gebühren für ausgehenden Datenverkehr möglicherweise erlassen. Wenden Sie sich an Ihren Account Manager, um mehr zu erfahren.  Weitere Information finden Sie im Abschnitt **Häufig gestellte Fragen** in dem verlinkten Dokument, insbesondere unter der Frage „Welche Datenübertragungsprogramme sind für akademische Kunden vorhanden, und wie qualifiziere ich mich dafür?“.

### <a name="pricing-example"></a>Preisbeispiel
Um die zuvor beschriebenen Preise zu rekapitulieren, sehen wir uns ein Beispiel für das Speichern unseres Vorlagen-VM-Images in der Shared Image Gallery an. Vorausgesetzt werden die folgenden Szenarien:

- Sie verfügen über ein benutzerdefiniertes VM-Image.
- Sie speichern zwei Versionen des Images.
- Ihr Lab befindet sich in den USA, die aus insgesamt acht Regionen bestehen.
- Jede Imageversion ist 32 GB groß. Hieraus resultiert, dass der Preis für den verwalteten HDD-Datenträger $ 1,54 pro Monat beträgt.

Die Gesamtkosten werden wie folgt geschätzt:

Anzahl der Images × Anzahl der Versionen × Anzahl der Replikate × Preis für verwalteten Datenträger

In diesem Beispiel betragen die Kosten:

1 benutzerdefiniertes Image (32 GB) x 2 Versionen x 8 US-Regionen x $ 1,54 = $ 24,64 pro Monat

### <a name="cost-management"></a>Kostenverwaltung
Es ist wichtig, dass Lab-Kontoadministratoren die Kosten verwalten, indem Sie routinemäßig nicht mehr benötigte Imageversionen aus dem Katalog löschen. 

Sie sollten nicht die Replikation in bestimmte Regionen löschen, um auf diese Weise die Kosten zu senken (diese Option ist in der Shared Image Gallery vorhanden). Replikationsänderungen können nachteilige Effekte auf die Fähigkeit von Azure Lab Service haben, VMs aus Images zu veröffentlichen, die in einer Shared Image Gallery gespeichert sind.

## <a name="next-steps"></a>Nächste Schritte
Schrittweise Anleitungen zum Erstellen eines Lab-Kontos und eines Labs finden Sie in folgendem Tutorial: [Tutorial: Einrichten eines Lab-Kontos](tutorial-setup-lab-account.md)
