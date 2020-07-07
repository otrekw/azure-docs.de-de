---
title: Übersicht über Azure DevOps Starter | Microsoft-Dokumentation
description: Grundlegendes zum Nutzen von Azure DevOps Starter
services: devops-project
documentationcenter: ''
author: mlearned
manager: gwallace
editor: mlearned
ms.assetid: ''
ms.service: az-devops-project
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 03/24/2020
ms.author: mlearned
ms.openlocfilehash: 99a1fdb8caff9953041c996d0f5581318ce11c66
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82233685"
---
# <a name="overview-of-azure-devops-starter"></a>Übersicht über Azure DevOps Starter

 Azure DevOps Starter erleichtert die ersten Schritte mit Azure. Mit Azure DevOps Projects können Sie Ihre bevorzugte App für den Azure-Dienst Ihrer Wahl in wenigen kurzen Schritten über das Azure-Portal starten. 

 Azure DevOps Starter richtet alles ein, was Sie zum Entwickeln, Bereitstellen und Überwachen Ihrer Anwendung benötigen. Über das Dashboard von Azure DevOps Starter können Sie Codecommits, Buildvorgänge und Bereitstellungen in einer zentralen Ansicht im Azure-Portal überwachen.

## <a name="advantages-of-using-devops-starter"></a>Vorteile der Verwendung von DevOps Starter

  DevOps Starter automatisiert die Einrichtung einer gesamten CI- und CD-Pipeline (Continuous Integration und Continuous Delivery) für Azure.  Sie können bereits vorhandenen Code nutzen oder eine der bereitgestellten Beispielanwendungen verwenden. Anschließend können Sie die Anwendung schnell für verschiedene Azure-Dienste wie Virtual Machines, App Service, Azure Kubernetes Service (AKS), Azure SQL-Datenbank oder Azure Service Fabric bereitstellen.  

  DevOps Starter nimmt Ihnen die gesamte Erstkonfiguration einer DevOps-Pipeline ab – von der Einrichtung des anfänglichen Git-Repositorys und der Konfiguration der CI/CD-Pipeline über die Erstellung einer Application Insights-Ressource für die Überwachung bis hin zur Bereitstellung einer zentralen Ansicht der gesamten Lösung durch Erstellung eines DevOps Projects-Dashboards im Azure-Portal.

DevOps Starter ermöglicht Folgendes:

* Schnelles Bereitstellen Ihrer Anwendung in Azure
* Automatisieren der Einrichtung einer CI/CD-Pipeline
* Anzeigen und Nachvollziehen der ordnungsgemäßen Einrichtung einer CI/CD-Pipeline
* Weiteres Anpassen der Releasepipelines auf der Grundlage Ihres spezifischen Szenarios

## <a name="how-to-use-devops-starter"></a>Wie wird DevOps Starter verwendet?

  DevOps Starter steht über das Azure-Portal zur Verfügung. Eine DevOps Starter-Ressource wird im Portal auf die gleiche Weise erstellt wie jede andere Azure-Ressource. DevOps Projects bietet einen Assistenten zum Durchlaufen der verschiedenen Konfigurationsoptionen.  

Bei der Ersteinrichtung müssen mehrere Konfigurationsoptionen ausgewählt werden. Die Optionen umfassen:

* Verwenden der bereitgestellten Beispiel-App oder Ihres eigenen Codes
* Auswählen einer App-Sprache
* Auswählen eines App-Frameworks auf der Grundlage der Sprache
* Auswählen eines Azure-Diensts (Bereitstellungsziel)
* Erstellen einer neuen Azure DevOps-Organisation oder Verwenden einer bereits vorhandenen Organisation 
* Auswählen Ihres Azure-Abonnements
* Auswählen des Standorts von Azure-Diensten
* Auswählen eines Tarifs für Azure-Dienste

Nach Verwendung von DevOps Starter können die Ressourcen auch alle von einem zentralen Ort aus über das DevOps Starter-Dashboard im Azure-Portal gelöscht werden.

## <a name="devops-starter-and-azure-devops-integration"></a>DevOps Starter- und Azure DevOps-Integration

DevOps Starter basiert auf Azure DevOps. DevOps Starter automatisiert sämtliche Aufgaben, die in Azure Pipelines zum Einrichten einer CI/CD-Pipeline ausgeführt werden müssen. Es erstellt ein Git-Repository in einer neuen oder vorhandenen Azure DevOps-Organisation und committet dann eine Beispielanwendung oder Ihren vorhandenen Code in einem neuen Git-Repository.  

Die Automatisierung richtet auch einen CI-Trigger für den Build ein, sodass nach jedem neuen Codecommit ein Buildvorgang initiiert wird. DevOps Starter erstellt einen CD-Trigger und stellt jeden neuen erfolgreichen Build für den Azure-Dienst Ihrer Wahl bereit.  

Build- und Releasepipelines können für zusätzliche Szenarien angepasst werden. Darüber hinaus können Sie die Build- und die Releasepipelines für die Verwendung in anderen Projekten klonen.

Nach der Erstellung von DevOps Starter haben Sie folgende Möglichkeiten:

* Anpassen der Build- und Releasepipeline
* Verwenden von Pullanforderungen, um den Codefluss zu verwalten und eine hohe Qualität zu gewährleisten
* Testen und Erstellen jedes Commits vor dem Zusammenführen Ihres Codes zur Erhöhung der Qualität
* Direktes Nachverfolgen von Backlog und Problemen zusammen mit Ihrer Anwendung

## <a name="getting-started-with-devops-starter"></a>Erste Schritten mit DevOps Starter

* [Erste Schritten mit DevOps Starter](https://docs.microsoft.com/azure/devops-project/azure-devops-project-github)

##  <a name="devops-starter-videos"></a>DevOps Starter-Videos

* [Erstellen Ihrer CI/CD-Pipeline mit Azure DevOps Starter](https://www.youtube.com/watch?v=NuYDAs3kNV8)
