---
title: Verteilte gemeinsame Entwicklung von Azure DevTest Labs-Ressourcen
description: Bietet bewährte Methoden zum Einrichten einer verteilten und gemeinsamen Entwicklungsumgebung für die Entwicklung von DevTest Labs-Ressourcen.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 9469591b1945adaffca973828d619d5d06655262
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76170125"
---
# <a name="best-practices-for-distributed-and-collaborative-development-of-azure-devtest-labs-resources"></a>Bewährte Methoden für die verteilte und gemeinsame Entwicklung von Azure DevTest Labs-Ressourcen
Mithilfe der verteilten gemeinsamen Entwicklung können unterschiedliche Teams oder Personen eine Codebasis entwickeln und verwalten. Der Erfolg des Entwicklungsprozesses hängt von der Fähigkeit ab, Informationen zu erstellen, freizugeben und zu integrieren. Dieses wichtige Entwicklungsprinzip kann in Azure DevTest Labs verwendet werden. Es gibt mehrere Arten von Ressourcen innerhalb eines Labs, die häufig zwischen verschiedenen Labs innerhalb eines Unternehmens verteilt werden. Die verschiedenen Arten von Ressourcen sind in zwei Bereiche unterteilt:

- Intern im Lab gespeicherte Ressourcen (lab-basiert).
- Ressourcen, die in [externen Repositorys gespeichert werden, die mit dem Lab verbunden sind](devtest-lab-add-artifact-repo.md) (coderepositorybasiert). 

In diesem Dokument werden einige bewährte Methoden beschrieben, die die Zusammenarbeit von und die Verteilung auf mehrerer Teams ermöglichen und gleichzeitig Anpassung und Qualität auf allen Ebenen gewährleisten.

## <a name="lab-based-resources"></a>Lab-basierte Ressourcen

### <a name="custom-virtual-machine-images"></a>Benutzerdefinierte VM-Images
Sie können eine allgemeine Quelle für benutzerdefinierte Images verwenden, die für Labs jede Nacht bereitgestellt werden. Ausführliche Informationen finden Sie unter [Imagefactory](image-factory-create.md).    

### <a name="formulas"></a>Formeln
[Formeln](devtest-lab-manage-formulas.md) sind lab-spezifisch und verfügen nicht über einen Verteilungsmechanismus. Die Lab-Mitglieder führen die gesamte Entwicklung von Formeln durch. 

## <a name="code-repository-based-resources"></a>Coderepositorybasierte Ressourcen
Es gibt zwei verschiedene Features, die auf Coderepositorys basieren: Artefakte und Umgebungen. In diesem Artikel werden die Features und die effektivste Einrichtung von Repositorys und Workflows erläutert, damit die verfügbaren Artefakte und Umgebungen auf Organisations- oder Teamebene angepasst werden können.  Dieser Workflow basiert auf der Standardstrategie für [Quellcodeverwaltungs-Branching](/azure/devops/repos/tfvc/branching-strategies-with-tfvc?view=azure-devops). 

### <a name="key-concepts"></a>Wichtige Begriffe
Die Quellinformationen für Artefakte enthalten Metadaten und Skripts. Zu den Quellinformationen für Umgebungen zählen Metadaten und Resource Manager-Vorlagen mit allen unterstützenden Dateien wie PowerShell-Skripts, DSC-Skripts, ZIP-Dateien usw.  

### <a name="repository-structure"></a>Repositorystruktur  
Die häufigste Konfiguration für Quellcodeverwaltung (Source Code Control, SCC) ist das Einrichten einer mehrschichtigen Struktur zum Speichern von Codedateien (Resource Manager-Vorlagen, Metadaten und Skripts), die in den Labs verwendet werden. Erstellen Sie insbesondere verschiedene Repositorys zum Speichern von Ressourcen, die von den verschiedenen Geschäftsebenen verwaltet werden:   

- Unternehmensweite Ressourcen.
- Ressourcen für die Geschäftseinheit/-division
- Teamspezifische Ressourcen.

Jede dieser Ebenen ist mit einem anderen Repository verknüpft, in dem der Masterbranch die Produktionsqualität aufweisen muss. Die [Branches](/azure/devops/repos/git/git-branching-guidance?view=azure-devops) in jedem Repository sind für die Entwicklung dieser spezifischen Ressourcen (Artefakte oder Vorlagen) vorgesehen. Diese Struktur ist gut für DevTest Labs geeignet, da Sie problemlos mehrere Repositorys und mehrere Branches gleichzeitig mit den Labs der Organisation verbinden können. Der Repositoryname ist in der Benutzeroberfläche (UI) enthalten, um Verwechslungen zu vermeiden, wenn identische Namen, Beschreibungen und Herausgeber vorhanden sind.
     
Die folgende Abbildung zeigt zwei Repositorys: ein Unternehmensrepository, das von der IT-Abteilung verwaltet wird, und ein Divisionsrepository, das von der Division R & D (Research & Development, Forschung und Entwicklung) verwaltet wird.

![Ein Beispiel für eine verteilende und kollaborative Entwicklungsumgebung](./media/best-practices-distributive-collaborative-dev-env/distributive-collaborative-dev-env.png)
   
Diese mehrschichtige Struktur ermöglicht die Entwicklung. Dabei wird ein höheres Maß an Qualität im Masterbranch gewährleistet, während mehrere mit einem Lab verbundene Repositorys größere Flexibilität ermöglichen.

## <a name="next-steps"></a>Nächste Schritte    
Weitere Informationen finden Sie in folgenden Artikeln:

- Hinzufügen eines Repositorys zu einem Lab mithilfe des [Azure-Portals](devtest-lab-add-artifact-repo.md) oder einer [Azure Resource Manager-Vorlage](add-artifact-repository.md)
- [DevTest Labs-Artefakte](devtest-lab-artifact-author.md)
- [DevTest Labs-Umgebungen](devtest-lab-create-environment-from-arm.md).
