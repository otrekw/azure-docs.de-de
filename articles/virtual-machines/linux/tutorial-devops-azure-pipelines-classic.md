---
title: 'Tutorial: DevOps-Integration für IaaS und PaaS in Azure'
description: In diesem Tutorial erfahren Sie, wie Sie mit Azure Pipelines für eine App CI- und CD-Vorgänge (Continuous Integration bzw. Continuous Deployment) für Azure-VMs einrichten.
author: ushan
manager: jpconnock
tags: azure-devops-pipelines
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: tutorial
ms.tgt_pltfrm: azure-pipelines
ms.workload: infrastructure
ms.date: 1/16/2020
ms.author: ushan
ms.custom: devops
ms.openlocfilehash: 5707a99b329915b35131fe793b0dfabd02348677
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "77912527"
---
# <a name="tutorial-integrated-devops-for-iaas-and-paas-on-azure"></a>Tutorial: DevOps-Integration für IaaS und PaaS in Azure

Mit End-to-End-Lösungen in Azure können Teams DevOps-Verfahren in allen Phasen des Anwendungslebenszyklus implementieren: Planung, Entwicklung, Bereitstellung und Betrieb. 

Unten sind einige Azure-Dienste aufgeführt, mit denen Cloudworkloads vereinfacht werden und die kombiniert werden können, um Szenarien mit beeindruckender Leistung zu ermöglichen.
Mit diesen Technologien sowie den zugehörigen Personen und Prozessen können Teams kontinuierlich Mehrwert für Kunden schaffen. 

- Azure: https://portal.azure.com – Portal zum Erstellen von Cloudworkloads. Verwalten und Überwachen aller Komponenten – von einfachen Web-Apps bis zu komplexen Cloudanwendungen 
- Azure DevOps: https://dev.azure.com – Intelligenteres Planen, besseres Zusammenarbeiten und schnelleres Bereitstellen mit modernen Entwicklungsdiensten 
- Azure Machine Learning Studio: https://ml.azure.com – Vorbereiten von Daten, Durchführen des Trainings und Bereitstellen von Machine Learning-Modellen 
 

Azure DevOps ist ein integrierter Azure-Dienst, bei dem alle Teile des DevOps-Prozesses für alle Azure-Ressourcen per Continuous Integration und Continuous Delivery automatisiert werden.
Unabhängig davon, ob für Ihre App virtuelle Computer, Web-Apps, Kubernetes oder andere Ressourcen genutzt werden, gilt Folgendes: Sie können Infrastructure-as-Code, Continuous Integration, fortlaufende Tests, Continuous Delivery und fortlaufende Überwachung mit Azure und Azure DevOps implementieren.  
![AzDevOps_portalView](media/tutorial-devops-azure-pipelines-classic/azdevops-view.png) 
 
 
## <a name="iaas---configure-cicd"></a>IaaS: Konfigurieren von CI/CD 
Azure Pipelines verfügt über einen umfassenden Satz an CI/CD-Automatisierungstools für Bereitstellungen auf virtuellen Computern. Sie können eine Continuous Delivery-Pipeline für eine Azure-VM direkt über das Azure-Portal konfigurieren. Das vorliegende Dokument enthält die Schritte, die zum Einrichten einer CI/CD-Pipeline für die Bereitstellung auf mehreren Computern über das Azure-Portal ausgeführt werden müssen. Konfigurieren Sie CI/CD auf virtuellen Computern.

Virtuelle Computer können einer [Bereitstellungsgruppe](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) als Ziele hinzugefügt werden und als Ziele für parallele Updates auf mehreren Computern angegeben werden. Ansichten des Bereitstellungsverlaufs in Bereitstellungsgruppen ermöglichen die Nachverfolgung von der VM über die Pipeline bis hin zum Commit. 
 
**Parallele Updates**: Bei einer parallelen Bereitstellung werden bei jeder Iteration auf einer festgelegten Gruppe von Computern (Gruppe für parallele Vorgänge) Instanzen der vorherigen Version einer Anwendung durch Instanzen der neuen Version ersetzt. Als Nächstes wird beschrieben, wie Sie ein paralleles Update auf virtuellen Computern konfigurieren können.  
Sie können parallele Updates für Ihre „**virtuellen Computer**“ konfigurieren, indem Sie im Azure-Portal die Option für Continuous Delivery verwenden. 

Hier ist eine ausführliche exemplarische Vorgehensweise angegeben. 
1. Melden Sie sich an Ihrem Azure-Portal an, und navigieren Sie zu einem virtuellen Computer. 
2. Navigieren Sie im linken Bereich des virtuellen Computers zum Menü  **Continuous Delivery**. Klicken Sie anschließend auf  **Konfigurieren**. 
   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azdevops-configure.png) 
3. Klicken Sie im Konfigurationsbereich auf „Azure DevOps-Organisation“, um ein vorhandenes Konto auszuwählen oder ein neues Konto zu erstellen. Wählen Sie anschließend das Projekt aus, unter dem Sie die Pipeline konfigurieren möchten.  
   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azdevops-project.png) 
4. Eine Bereitstellungsgruppe ist eine logische Gruppe mit Bereitstellungsziel-Computern, die die physischen Umgebungen darstellen, z. B. „Entwicklung“, „Test“, „UAT“ und „Produktion“. Sie können eine neue Bereitstellungsgruppe erstellen oder eine vorhandene auswählen. Optional können Sie dem Computer die gewünschte Rolle zuordnen. Beispiel: „web“, „db“ usw.  
5. Klicken Sie im Dialogfeld auf **OK**, um die Continuous Delivery-Pipeline zu konfigurieren. 
6. Anschließend verfügen Sie über eine Continuous Delivery-Pipeline, die für die Bereitstellung auf dem virtuellen Computer konfiguriert ist.  
   ![AzDevOps_pipeline](media/tutorial-devops-azure-pipelines-classic/azdevops-pipeline.png)
7. Sie sehen, dass die Bereitstellung auf dem virtuellen Computer durchgeführt wird. Sie können auf den Link klicken, um zur Pipeline zu navigieren. Klicken Sie auf **Release-1**, um die Bereitstellung anzuzeigen. Alternativ können Sie auf **Bearbeiten** klicken, um die Definition der Releasepipeline zu ändern. 
8. Wenn Sie mehrere VMs konfigurieren möchten, müssen Sie die Schritte 2 bis 5 für die anderen VMs wiederholen, die der Bereitstellungsgruppe hinzugefügt werden sollen. 
9. Klicken Sie auf die Pipelinedefinition, navigieren Sie zur Azure DevOps-Organisation, und klicken Sie auf **Bearbeiten**, um die Releasepipeline zu bearbeiten. 
   ![AzDevOps_edit_pipeline](media/tutorial-devops-azure-pipelines-classic/azdevops-edit-pipeline.png)
10. Klicken Sie für die Phase **Entwicklung** auf den Link für **1 Auftrag, 1 Aufgabe**. Klicken Sie auf die Phase **Bereitstellen**.  
   ![AzDevOps_deploymentGroup](media/tutorial-devops-azure-pipelines-classic/azdevops-deployment-group.png)
11. Im Konfigurationsbereich auf der rechten Seite wird angezeigt, dass die Pipeline standardmäßig so konfiguriert ist, dass für alle Ziele gleichzeitig ein paralleles Update durchgeführt wird. Sie können die gewünschten Bereitstellungen entweder einzeln nacheinander oder – indem Sie den Schieberegler verwenden – nach Prozentanteil konfigurieren.  
  
  
**Canary** trägt zur Risikominimierung bei, indem die Änderung vorerst nur für einen Teil der Benutzer eingeführt wird. Wenn Sie später mehr Vertrauen in die neue Version haben, können Sie sie für weitere Server in Ihrer Infrastruktur freigeben und mehr Benutzer an diese Version weiterleiten. Sie können Canary-Bereitstellung für Ihre „**virtuellen Computer**“ konfigurieren, indem Sie im Azure-Portal die Option für Continuous Delivery verwenden. Hier ist eine ausführliche exemplarische Vorgehensweise angegeben. 
1. Melden Sie sich an Ihrem Azure-Portal an, und navigieren Sie zu einem virtuellen Computer. 
2. Führen Sie die Schritte 2 bis 5 aus dem vorherigen Abschnitt aus, um der Bereitstellungsgruppe mehrere VMs hinzuzufügen. 
3. Fügen Sie den VMs, die Teil der Canary-Bereitstellungen sein sollen, ein benutzerdefiniertes Tag hinzu. Beispiel: „Canary“.
4. Gehen Sie wie folgt vor, nachdem die Pipeline für die VMs konfiguriert wurde: Klicken Sie auf die Pipeline, starten Sie die Azure DevOps-Organisation, **bearbeiten** Sie die Pipeline, und navigieren Sie zur Phase **Entwicklung**. Fügen Sie dem Filter „Canary“ ein Tag hinzu. 
5. Fügen Sie eine weitere Bereitstellungsgruppenphase hinzu, und konfigurieren Sie die Phase mit den Tags, um die restlichen VMs der Bereitstellungsgruppe als Ziele festzulegen.  
6. Optional können Sie einen manuellen Überprüfungsschritt konfigurieren, in dem die Canary-Bereitstellungen höhergestuft bzw. abgelehnt werden können. 
   ![AzDevOps_Canary](media/tutorial-devops-azure-pipelines-classic/azdevops-canary-deploy.png)

Mit der **Blaugrün**-Bereitstellung wird die Ausfallzeit für die Bereitstellung reduziert, indem eine identische Standbyumgebung verwendet wird. Eine der Umgebungen befindet sich hierbei immer im Livezustand. Wenn Sie sich auf das neue Release vorbereiten, führen Sie die letzte Testphase in der grünen Umgebung durch. Wenn die Software in der grünen Umgebung funktioniert, können Sie den Datenverkehr umleiten, damit alle eingehenden Anforderungen an die grüne Umgebung geleitet werden. Die blaue Umgebung wird dann als im Leerlauf befindlich angezeigt.
Sie können Blaugrün-Bereitstellungen für Ihre „**virtuellen Computer**“ konfigurieren, indem Sie im Azure-Portal die Option für Continuous Delivery verwenden. 

Hier ist eine ausführliche exemplarische Vorgehensweise angegeben. 

1. Melden Sie sich an Ihrem Azure-Portal an, und navigieren Sie zu einem virtuellen Computer. 
2. Führen Sie die Schritte 2 bis 5 aus dem Abschnitt **Parallele Updates** aus, um der Bereitstellungsgruppe mehrere VMs hinzuzufügen. Fügen Sie den VMs, die Teil der Blaugrün-Bereitstellungen sein sollen, ein benutzerdefiniertes Tag hinzu. Beispiel: „Blau“ oder „Grün“ für die VMs, die für die Standbyrolle vorgesehen sind. 
3. Gehen Sie wie folgt vor, nachdem die Pipeline für die VMs konfiguriert wurde: Klicken Sie auf die Pipeline, starten Sie die Azure DevOps-Organisation, **bearbeiten** Sie die Pipeline, und navigieren Sie zur Phase **Entwicklung**. Fügen Sie dem Filter „Grün“ ein Tag hinzu. 
4. Fügen Sie eine Phase ohne Agent hinzu, und konfigurieren Sie die Phase mit einem manuellen Überprüfungsschritt und einem Schritt zum Aufrufen der REST-API, um die Tags auszutauschen. 
   ![AzDevOps_BlueGreen](media/tutorial-devops-azure-pipelines-classic/azdevops-blue-green-deploy.png)
 
 
## <a name="azure-devops-project"></a>Azure DevOps-Projekt 
Der Einstieg in Azure ist jetzt noch einfacher.
 
Starten Sie mit DevOps Projects die Ausführung Ihrer Anwendung auf einem beliebigen Azure-Dienst mit nur drei Schritten: Sie müssen lediglich eine Anwendungssprache, eine Runtime und einen Azure-Dienst auswählen.
 
[Weitere Informationen](https://azure.microsoft.com/features/devops-projects/ )
 
## <a name="additional-resources"></a>Zusätzliche Ressourcen 
- [Tutorial: Bereitstellen einer ASP.NET-App auf virtuellen Azure-Computern mithilfe von Azure DevOps Projects](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Implementieren von Continuous Deployment für Ihre App in einer VM-Skalierungsgruppe von Azure](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
