---
title: 'Tutorial: Konfigurieren von parallelen Bereitstellungen für virtuelle Linux-Computer in Azure'
description: In diesem Tutorial erfahren Sie, wie Sie eine CD-Pipeline (Continuous Deployment) einrichten, die mithilfe der Strategie für die parallele Bereitstellung eine Gruppe virtueller Linux-Computer in Azure inkrementell aktualisiert.
author: moala
manager: jpconnock
tags: azure-devops-pipelines
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: tutorial
ms.tgt_pltfrm: azure-pipelines
ms.workload: infrastructure
ms.date: 4/10/2020
ms.author: moala
ms.custom: devops
ms.openlocfilehash: 75888b1ebbda33891296fe0b54c5d204955e32a3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82113481"
---
# <a name="tutorial---configure-rolling-deployment-strategy-for-azure-linux-virtual-machines"></a>Tutorial: Konfigurieren der Strategie für parallele Bereitstellungen für virtuelle Linux-Computer in Azure

Azure DevOps ist ein integrierter Azure-Dienst, bei dem alle Teile des DevOps-Prozesses für alle Azure-Ressourcen per Continuous Integration und Continuous Delivery automatisiert werden.
Unabhängig davon, ob für Ihre App virtuelle Computer, Web-Apps, Kubernetes oder andere Ressourcen genutzt werden, gilt Folgendes: Sie können Infrastructure-as-Code, Continuous Integration, fortlaufende Tests, Continuous Delivery und fortlaufende Überwachung mit Azure und Azure DevOps implementieren.  

![AzDevOps_portalView](media/tutorial-devops-azure-pipelines-classic/azdevops-view.png) 


## <a name="iaas---configure-cicd"></a>IaaS: Konfigurieren von CI/CD 
Azure Pipelines verfügt über einen umfassenden Satz an CI/CD-Automatisierungstools für Bereitstellungen auf virtuellen Computern. Sie können eine Continuous Delivery-Pipeline für einen virtuellen Azure-Computer direkt über das Azure-Portal konfigurieren. Das vorliegende Dokument enthält die Schritte, die zum Einrichten einer CI/CD-Pipeline für die parallele Bereitstellung auf mehreren Computern über das Azure-Portal ausgeführt werden müssen. Sie können sich auch andere Strategien wie die [Canary-](https://aka.ms/AA7jdrz) und die [Blau-Grün-Strategie](https://aka.ms/AA83fwu) ansehen, die standardmäßig über das Azure-Portal unterstützt werden. 


**Konfigurieren von CI/CD auf virtuellen Computern**

Virtuelle Computer können einer [Bereitstellungsgruppe](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) als Ziele hinzugefügt und bei Updates für mehrere Computer als Ziele verwendet werden. Nach der Bereitstellung ermöglicht der **Bereitstellungsverlauf** innerhalb einer Bereitstellungsgruppe die Nachverfolgung vom virtuellen Computer über die Pipeline bis hin zum Commit. 
 

**Parallele Bereitstellungen:** Bei einer parallelen Bereitstellung werden bei jeder Iteration auf einer festgelegten Gruppe von Computern (Gruppe für parallele Vorgänge) Instanzen der vorherigen Version einer Anwendung durch Instanzen der neuen Version ersetzt. Als Nächstes wird beschrieben, wie Sie ein paralleles Update auf virtuellen Computern konfigurieren können.  
Sie können parallele Updates für Ihre „**virtuellen Computer**“ konfigurieren, indem Sie im Azure-Portal die Option für Continuous Delivery verwenden. 

Hier ist eine ausführliche exemplarische Vorgehensweise angegeben. 
1. Melden Sie sich an Ihrem Azure-Portal an, und navigieren Sie zu einem virtuellen Computer. 
2. Navigieren Sie im linken Bereich des virtuellen Computers zum Menü  **Continuous Delivery**. Klicken Sie anschließend auf  **Konfigurieren**. 

   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure.png) 
3. Klicken Sie im Konfigurationsbereich auf „Azure DevOps-Organisation“, um ein vorhandenes Konto auszuwählen oder ein neues Konto zu erstellen. Wählen Sie anschließend das Projekt aus, unter dem Sie die Pipeline konfigurieren möchten.  


   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling.png) 
4. Eine Bereitstellungsgruppe ist eine logische Gruppe mit Bereitstellungsziel-Computern, die die physischen Umgebungen darstellen, z. B. „Entwicklung“, „Test“, „UAT“ und „Produktion“. Sie können eine neue Bereitstellungsgruppe erstellen oder eine vorhandene auswählen. 
5. Wählen Sie die Buildpipeline aus, die das Paket veröffentlicht, das auf dem virtuellen Computer bereitgestellt werden soll. Beachten Sie, dass das veröffentlichte Paket im Paketstammverzeichnis im Ordner `deployscripts` ein Bereitstellungsskript vom Typ _deploy.ps1_ oder _deploy.sh_ enthalten muss. Dieses Bereitstellungsskript wird zur Laufzeit von der Azure DevOps-Pipeline ausgeführt.
6. Wählen Sie die gewünschte Bereitstellungsstrategie aus. In diesem Fall wird „Parallel“ verwendet.
7. Optional können Sie den Computer mit der Rolle taggen. Beispiel: „web“, „db“ usw. Dadurch können Sie einfacher virtuelle Computer mit einer spezifischen Rolle als Ziel verwenden.
8. Klicken Sie im Dialogfeld auf **OK**, um die Continuous Delivery-Pipeline zu konfigurieren. 
9. Anschließend verfügen Sie über eine Continuous Delivery-Pipeline, die für die Bereitstellung auf dem virtuellen Computer konfiguriert ist.  


   ![AzDevOps_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-deployment-history.png)
10. Sie sehen, dass die Bereitstellung auf dem virtuellen Computer durchgeführt wird. Sie können auf den Link klicken, um zur Pipeline zu navigieren. Klicken Sie auf **Release-1**, um die Bereitstellung anzuzeigen. Alternativ können Sie auf **Bearbeiten** klicken, um die Definition der Releasepipeline zu ändern. 
11. Wenn Sie mehrere virtuelle Computer konfigurieren möchten, müssen Sie die Schritte 2 bis 4 für die anderen virtuellen Computer wiederholen, die der Bereitstellungsgruppe hinzugefügt werden sollen. Beachten Sie Folgendes: Wenn Sie eine Bereitstellungsgruppe auswählen, für die bereits eine Pipelineausführung vorhanden ist, wird der virtuelle Computer lediglich der Bereitstellungsgruppe hinzugefügt, und es werden keine neuen Pipelines erstellt. 
12. Klicken Sie auf die Pipelinedefinition, navigieren Sie zur Azure DevOps-Organisation, und klicken Sie auf **Bearbeiten**, um die Releasepipeline zu bearbeiten. 
   ![AzDevOps_edit_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling-pipeline.png)
13. Klicken Sie für die Phase **Entwicklung** auf den Link für **1 Auftrag, 1 Aufgabe**. Klicken Sie auf die Phase **Bereitstellen**.
   ![AzDevOps_deploymentGroup](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling-pipeline-tasks.png)
14. Im Konfigurationsbereich auf der rechten Seite können Sie die Anzahl von Computer angeben, die pro Iteration parallel bereitgestellt werden sollen. Falls die Bereitstellung gleichzeitig auf mehreren Computern erfolgen soll, können Sie den gewünschten Umfang mithilfe des Schiebereglers als Prozentsatz angeben.  

15. Durch den Task „Execute Deploy Script“ (Bereitstellungsskript ausführen) wird standardmäßig das Bereitstellungsskript _deploy.ps1_ oder _deploy.sh_ ausgeführt, das sich im Ordner „deployscripts“ im Stammverzeichnis des veröffentlichten Pakets befindet.  
![AzDevOps_publish_package](media/tutorial-deployment-strategy/package.png)

## <a name="other-deployment-strategies"></a>Weitere Bereitstellungsstrategien

- [Konfigurieren der Strategie für die Canary-Bereitstellung](https://aka.ms/AA7jdrz)
- [Konfigurieren der Strategie für eine Blaugrün-Bereitstellung](https://aka.ms/AA83fwu)

 
## <a name="azure-devops-project"></a>Azure DevOps-Projekt 
Der Einstieg in Azure ist jetzt noch einfacher.
 
Starten Sie mit DevOps Projects die Ausführung Ihrer Anwendung auf einem beliebigen Azure-Dienst mit nur drei Schritten: Sie müssen lediglich eine Anwendungssprache, eine Runtime und einen Azure-Dienst auswählen.
 
[Weitere Informationen](https://azure.microsoft.com/features/devops-projects/ )
 
## <a name="additional-resources"></a>Zusätzliche Ressourcen 
- [Tutorial: Bereitstellen einer ASP.NET-App auf virtuellen Azure-Computern mithilfe von Azure DevOps Projects](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Implementieren von Continuous Deployment für Ihre App in einer VM-Skalierungsgruppe von Azure](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
