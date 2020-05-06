---
title: 'Tutorial: Konfigurieren von Canary-Bereitstellungen für virtuelle Linux-Computer in Azure'
description: In diesem Tutorial erfahren Sie, wie Sie eine CD-Pipeline (Continuous Deployment) einrichten, um eine Gruppe virtueller Linux-Computer in Azure unter Verwendung der Canary-Bereitstellungsstrategie zu aktualisieren.
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
ms.openlocfilehash: b51b4aed85f737e436565ce8ba1ab4a295714734
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82120528"
---
# <a name="tutorial---configure-canary-deployment-strategy-for-azure-linux-virtual-machines"></a>Tutorial: Konfigurieren der Canary-Bereitstellungsstrategie für virtuelle Linux-Computer in Azure


## <a name="iaas---configure-cicd"></a>IaaS: Konfigurieren von CI/CD 
Azure Pipelines verfügt über einen umfassenden Satz an CI/CD-Automatisierungstools für Bereitstellungen auf virtuellen Computern. Sie können eine Continuous Delivery-Pipeline für eine Azure-VM direkt über das Azure-Portal konfigurieren. Dieses Dokument enthält die Schritte für die Einrichtung einer CI/CD-Pipeline mit der Canary-Strategie für Bereitstellungen mit mehreren Computern. Sie können sich auch andere Strategien wie die [parallele Bereitstellung](https://aka.ms/AA7jlh8) und die [Blau-Grün-Bereitstellung](https://aka.ms/AA83fwu) ansehen, die standardmäßig über das Azure-Portal unterstützt werden. 


**Konfigurieren von CI/CD auf virtuellen Computern**

Virtuelle Computer können einer [Bereitstellungsgruppe](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) als Ziele hinzugefügt und bei Updates für mehrere Computer als Ziele verwendet werden. Nach der Bereitstellung ermöglicht der **Bereitstellungsverlauf** innerhalb Ihrer Bereitstellungsgruppe die Nachverfolgung vom virtuellen Computer über die Pipeline bis hin zum Commit. 
 
  
**Canary-Bereitstellungen:** Eine Canary-Bereitstellung trägt zur Risikominimierung bei, indem die Änderung vorerst nur für einen Teil der Benutzer eingeführt wird. Wenn Sie später mehr Vertrauen in die neue Version haben, können Sie sie für weitere Server in Ihrer Infrastruktur freigeben und mehr Benutzer an diese Version weiterleiten. Canary-Bereitstellungen für Ihre virtuellen Computer können über die Continuous Delivery-Option im Azure-Portal konfiguriert werden. Hier ist eine ausführliche exemplarische Vorgehensweise angegeben. 
1. Melden Sie sich an Ihrem Azure-Portal an, und navigieren Sie zu einem virtuellen Computer. 
2. Navigieren Sie im linken VM-Bereich zum Menü  **Continuous Delivery**. Klicken Sie auf **Konfigurieren**. 

   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure.png) 
3. Klicken Sie im Konfigurationsbereich auf **Azure DevOps-Organisation**, um ein vorhandenes Konto auszuwählen oder ein neues Konto zu erstellen. Wählen Sie anschließend das Projekt aus, unter dem Sie die Pipeline konfigurieren möchten.  


   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling.png) 
4. Eine Bereitstellungsgruppe ist eine logische Gruppe mit Bereitstellungsziel-Computern, die die physischen Umgebungen darstellen, z. B. „Entwicklung“, „Test“, „UAT“ und „Produktion“. Sie können eine neue Bereitstellungsgruppe erstellen oder eine vorhandene auswählen. 
5. Wählen Sie die Buildpipeline aus, die das Paket veröffentlicht, das auf dem virtuellen Computer bereitgestellt werden soll. Beachten Sie, dass das veröffentlichte Paket im Paketstammverzeichnis ein Bereitstellungsskript vom Typ _deploy.ps1_ oder _deploy.sh_ im Ordner `deployscripts` enthalten muss. Dieses Bereitstellungsskript wird zur Laufzeit von der Azure DevOps-Pipeline ausgeführt.
6. Wählen Sie die gewünschte Bereitstellungsstrategie aus. Wählen Sie **Canary** aus.
7. Versehen Sie die virtuellen Computer, die Teil von Canary-Bereitstellungen sein sollen, mit dem Tag „canary“. Fügen Sie virtuellen Computern, die Teil von Bereitstellungen sind, die im Anschluss an eine erfolgreiche Canary-Bereitstellung durchgeführt werden, das Tag „prod“ hinzu. Durch Tags können Sie einfacher virtuelle Computer mit einer spezifischen Rolle als Ziel verwenden.
![AzDevOps_configure_canary](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure-canary.png)

8. Klicken Sie im Dialogfeld auf **OK**, um die Continuous Delivery-Pipeline zu konfigurieren. Damit verfügen Sie über eine Continuous Delivery-Pipeline, die für die Bereitstellung auf dem virtuellen Computer konfiguriert ist.
![AzDevOps_canary_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-pipeline.png)


9. Sie können in Azure DevOps auf die Option zum **Bearbeiten** der Releasepipeline klicken, um die Pipelinekonfiguration anzuzeigen. Die Pipeline umfasst drei Phasen. Die erste Phase ist eine Bereitstellungsgruppenphase mit Bereitstellung auf virtuellen Standbycomputern, die mit dem Tag _canary_ versehen wurden. In der zweiten Phase wird die Pipeline angehalten und auf einen manuellen Eingriff zur Fortsetzung der Ausführung gewartet. Nachdem sich ein Benutzer von der Stabilität der Canary-Bereitstellung überzeugt hat, kann er die Pipelineausführung fortsetzen. Daraufhin wird die dritte Phase zur Bereitstellung auf Computern mit dem Tag _prod_ durchgeführt. ![AzDevOps_canary_task](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-task.png)

10. Vergewissern Sie sich, dass mindestens ein virtueller Computer mit dem Tag _prod_ vorhanden ist, bevor Sie die Pipelineausführung fortsetzen. In der dritten Phase der Pipeline wird die Anwendung nur auf virtuellen Computern bereitgestellt, die über das Tag _prod_ verfügen.

11. Durch den Task „Execute Deploy Script“ (Bereitstellungsskript ausführen) wird standardmäßig das Bereitstellungsskript _deploy.ps1_ oder _deploy.sh_ ausgeführt, das sich im Ordner „deployscripts“ im Stammverzeichnis des veröffentlichten Pakets befindet. Vergewissern Sie sich, dass die Veröffentlichung durch die ausgewählte Buildpipeline im Stammordner des Pakets erfolgt. 
![AzDevOps_publish_package](media/tutorial-deployment-strategy/package.png)




## <a name="other-deployment-strategies"></a>Weitere Bereitstellungsstrategien
- [Tutorial: Konfigurieren der Strategie für parallele Bereitstellungen für virtuelle Linux-Computer in Azure](https://aka.ms/AA7jlh8)
- [Tutorial: Konfigurieren der Blau-Grün-Bereitstellungsstrategie für virtuelle Linux-Computer in Azure](https://aka.ms/AA83fwu)

## <a name="azure-devops-project"></a>Azure DevOps-Projekt 
Der Einstieg in Azure ist jetzt noch einfacher.
 
Starten Sie mit DevOps Projects die Ausführung Ihrer Anwendung auf einem beliebigen Azure-Dienst mit nur drei Schritten: Sie müssen lediglich eine Anwendungssprache, eine Runtime und einen Azure-Dienst auswählen.
 
[Weitere Informationen](https://azure.microsoft.com/features/devops-projects/ )
 
## <a name="additional-resources"></a>Zusätzliche Ressourcen 
- [Tutorial: Bereitstellen einer ASP.NET-App auf virtuellen Azure-Computern mithilfe von Azure DevOps Projects](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Implementieren von Continuous Deployment für Ihre App in einer VM-Skalierungsgruppe von Azure](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
