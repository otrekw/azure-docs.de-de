---
title: Informationen zu Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Lab Services das Erstellen, Verwalten und Schützen von Labs mit virtuellen Computern vereinfachen können, die von Entwicklern, Testern, Lehrkräften, Kursteilnehmern und anderen genutzt werden können.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 06/16/2020
ms.author: spelluru
ms.openlocfilehash: 1c4fb6660f8eaebe2a65e3dba3f9adbb07f1eb22
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2020
ms.locfileid: "84882718"
---
# <a name="an-introduction-to-azure-lab-services"></a>Einführung in Azure Lab Services
Mit **Azure Lab Services** können Sie Labs erstellen, deren Infrastruktur von Azure verwaltet wird. Aktuell sind Classroom-Labs die einzige Art von verwalteten Labs, die von Azure Lab Services unterstützt werden. Der Dienst selbst übernimmt die gesamte Infrastrukturverwaltung für einen verwalteten Lab-Typ – vom Hochfahren der VMs über die Fehlerbehandlung bis hin zur Skalierung der Infrastruktur. Nachdem ein IT-Administrator ein Labkonto in Azure Lab Services erstellt hat, kann ein Kursleiter schnell ein Lab für seinen Kurs einrichten, die Anzahl und den Typ der virtuellen Computer angeben, die für Übungen im Kurs erforderlich sind, und Benutzer zum Kurs hinzufügen. Wenn sich ein Benutzer für den Kurs registriert hat, kann er auf den virtuellen Computer zugreifen, um Übungen für den Kurs auszuführen.  

## <a name="key-capabilities"></a>Wichtige Funktionen
Azure Lab Services unterstützt Folgendes:

- **Schnelle und flexible Einrichtung eines Labs**. Mithilfe von Azure Lab Services können Lab-Besitzer schnell ein Lab ihren Anforderungen entsprechend einrichten. Der Dienst bietet die Möglichkeit, alle Aufgaben im Zusammenhang mit der Azure-Infrastruktur für verwaltete Labs zu übernehmen Der Dienst bietet eine integrierte Skalierung und Resilienz von Infrastruktur für Labs, die er für Sie verwaltet.
- **Vereinfachte Umgebung für Lab-Benutzer**. Labbenutzer können sich mit einem Registrierungscode registrieren und jederzeit auf das Lab zugreifen, um dessen Ressourcen zu nutzen. 
- **Kostenoptimierung und -analyse**. Ein Lab-Besitzer kann Lab-Zeitpläne festlegen, gemäß denen virtuelle Computer automatisch herunter- und hochgefahren werden. Der Labbesitzer kann in einem Zeitplan die Zeitfenster festlegen, in denen die virtuellen Computer des Labs für Benutzer zugänglich sind, und zur Kostenoptimierung lab- oder benutzerbezogen Nutzungsrichtlinien festlegen. 

Wenn Sie lediglich Ihre Lab-Anforderungen eingeben und die Einrichtung und Verwaltung der dafür erforderlichen Infrastruktur dem Dienst überlassen möchten, können Sie einen der **verwalteten Lab-Typen** wählen. Derzeit können mit Azure Lab Services nur verwaltete Lab-Typen der Art **Classroom-Lab** erstellt werden.

Ausführlichere Informationen zu diesen Labs finden Sie in den folgenden Abschnitten. 

## <a name="managed-lab-types"></a>Verwaltete Labtypen
Mit Azure Lab Services können Sie Labs erstellen, deren Infrastruktur von Azure verwaltet wird. In diesem Artikel werden sie als verwaltete Lab-Typen bezeichnet. Verwaltete Lab-Typen umfassen verschiedene Arten von Labs für Ihre individuellen Anforderungen. Derzeit wird das **Classroom-Lab** als einziger verwalteter Lab-Typ unterstützt. 

Verwaltete Lab-Typen zeichnen sich durch ihren geringen Einrichtungsaufwand aus und sind schnell einsatzbereit. Der Dienst übernimmt die gesamte Infrastrukturverwaltung für das Lab – vom Hochfahren der virtuellen Computer über die Fehlerbehandlung bis hin zur Skalierung der Infrastruktur. Wenn Sie einen verwalteten Lab-Typ, z. B. ein Classroom-Lab, erstellen möchten, müssen Sie zunächst ein Lab-Konto für Ihre Organisation erstellen. Das Lab-Konto fungiert als zentrales Konto, unter dem alle Labs der Organisation verwaltet werden. 

Wenn Sie Azure-Ressourcen unter diesen verwalteten Lab-Typen erstellen und verwenden, erstellt und verwaltet der Dienst Ressourcen in internen Microsoft-Abonnements. Sie werden nicht in Ihrem eigenen Azure-Abonnement erstellt. Der Dienst überwacht die Nutzung dieser Ressourcen in internen Microsoft-Abonnements. Diese Nutzung wird dann über Ihr Azure-Abonnement abgerechnet, dem das Lab-Konto angehört.   

Im Anschluss finden Sie einige **Anwendungsfälle für verwaltete Lab-Typen**: 

- Stellen Sie den Teilnehmern ein Lab mit virtuellen Computern zur Verfügung, die exakt für ihre Anforderungen konfiguriert sind. Weisen Sie jedem Teilnehmer eine begrenzte Anzahl von VM-Stunden für Hausaufgaben oder persönliche Projekte zu.
- Richten Sie einen Pool mit hochleistungsfähigen virtuellen Computern für rechen- oder grafikintensive Untersuchungen ein. Führen Sie die virtuellen Computer nach Bedarf aus, und bereinigen Sie sie anschließend. 
- Verlagern Sie das physische Computerlabor Ihrer Einrichtung in die Cloud. Skalieren Sie die VM-Anzahl automatisch bis zur maximalen Auslastung und Kostenschwelle, die Sie für das Lab festgelegt haben.  
- Stellen Sie schnell ein Lab mit virtuellen Computern für einen Hackathon bereit. Löschen Sie das Lab mit nur einem Mausklick, wenn Sie es nicht mehr benötigen. 

## <a name="next-steps"></a>Nächste Schritte
Schrittweise Anleitungen zum Erstellen eines Labkontos und eines Classroom-Labs finden Sie in folgendem Tutorial:

- [Tutorial: Einrichten eines Labkontos mit Azure Lab Services](tutorial-setup-lab-account.md)
- [Tutorial: Einrichten eines Classroom-Labs](tutorial-setup-classroom-lab.md)
