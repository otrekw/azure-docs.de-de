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
ms.openlocfilehash: 4159bf27c39087926d982552c49c606f3484de77
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885885"
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
Azure Pipelines verfügt über einen umfassenden Satz an CI/CD-Automatisierungstools für Bereitstellungen auf virtuellen Computern. Sie können eine Continuous Delivery-Pipeline für eine Azure-VM direkt über das Azure-Portal konfigurieren. Das vorliegende Dokument enthält die Schritte, die zum Einrichten einer CI/CD-Pipeline für die Bereitstellung auf mehreren Computern über das Azure-Portal ausgeführt werden müssen. 


**Konfigurieren von CI/CD auf virtuellen Computern**

Virtuelle Computer können einer [Bereitstellungsgruppe](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) als Ziele hinzugefügt und bei Updates für mehrere Computer als Ziele verwendet werden. Abhängig von Ihren Anforderungen können Sie entweder eine der vorgefertigten Bereitstellungsstrategien (_Parallel_, _Canary_, _Blaugrün_) wählen oder diese weiter anpassen. Nach der Bereitstellung ermöglichen Ansichten des Bereitstellungsverlaufs innerhalb von Bereitstellungsgruppen die Nachverfolgung vom virtuellen Computer über die Pipeline bis hin zum Commit. 
 
**Parallele Bereitstellungen:** Bei einer parallelen Bereitstellung werden bei jeder Iteration auf einer festgelegten Gruppe von Computern (Gruppe für parallele Vorgänge) Instanzen der vorherigen Version einer Anwendung durch Instanzen der neuen Version ersetzt. Als Nächstes wird beschrieben, wie Sie ein paralleles Update auf virtuellen Computern konfigurieren können.  
Sie können parallele Updates für Ihre „**virtuellen Computer**“ konfigurieren, indem Sie im Azure-Portal die Option für Continuous Delivery verwenden. 

Hier ist eine ausführliche exemplarische Vorgehensweise angegeben. 
1. Melden Sie sich an Ihrem Azure-Portal an, und navigieren Sie zu einem virtuellen Computer. 
2. Navigieren Sie im linken Bereich des virtuellen Computers zum Menü  **Continuous Delivery**. Klicken Sie anschließend auf  **Konfigurieren**. 

   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure.png) 
3. Klicken Sie im Konfigurationsbereich auf „Azure DevOps-Organisation“, um ein vorhandenes Konto auszuwählen oder ein neues Konto zu erstellen. Wählen Sie anschließend das Projekt aus, unter dem Sie die Pipeline konfigurieren möchten.  


   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling.png) 
4. Eine Bereitstellungsgruppe ist eine logische Gruppe mit Bereitstellungsziel-Computern, die die physischen Umgebungen darstellen, z. B. „Entwicklung“, „Test“, „UAT“ und „Produktion“. Sie können eine neue Bereitstellungsgruppe erstellen oder eine vorhandene auswählen. 
5. Wählen Sie die Buildpipeline aus, die das Paket veröffentlicht, das auf dem virtuellen Computer bereitgestellt werden soll. Beachten Sie, dass das veröffentlichte Paket im Paketstammverzeichnis im Ordner _deployscripts_ ein Bereitstellungsskript vom Typ _deploy.ps1_ oder _deploy.sh_ enthalten muss. Dieses Bereitstellungsskript wird zur Laufzeit von der Azure DevOps-Pipeline ausgeführt.
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

15. Durch den Task „Execute Deploy Script“ (Bereitstellungsskript ausführen) wird standardmäßig das Bereitstellungsskript _deploy.ps1_ oder _deploy.sh_ ausgeführt, das sich im Ordner _deployscripts_ im Stammverzeichnis des veröffentlichten Pakets befindet.
  
**Canary-Bereitstellungen:** Eine Canary-Bereitstellung trägt zur Risikominimierung bei, indem die Änderung vorerst nur für einen Teil der Benutzer eingeführt wird. Wenn Sie später mehr Vertrauen in die neue Version haben, können Sie sie für weitere Server in Ihrer Infrastruktur freigeben und mehr Benutzer an diese Version weiterleiten. Sie können Canary-Bereitstellung für Ihre „**virtuellen Computer**“ konfigurieren, indem Sie im Azure-Portal die Option für Continuous Delivery verwenden. Hier ist eine ausführliche exemplarische Vorgehensweise angegeben. 
1. Melden Sie sich an Ihrem Azure-Portal an, und navigieren Sie zu einem virtuellen Computer. 
2. Führen Sie die Schritte 2 bis 7 aus dem Abschnitt **Parallele Bereitstellungen** aus, um der Bereitstellungsgruppe mehrere virtuelle Computer hinzuzufügen. Wählen Sie in der Dropdownliste für die Bereitstellungsstrategie die Option „Canary“ aus.
![AzDevOps_configure_canary](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure-canary.png)

3. Versehen Sie die virtuellen Computer, die Teil von Canary-Bereitstellungen sein sollen, mit dem Tag „canary“. Fügen Sie virtuellen Computern, die Teil von Bereitstellungen sind, die im Anschluss an eine erfolgreiche Canary-Bereitstellung durchgeführt werden, das Tag „prod“ hinzu.
4. Anschließend verfügen Sie über eine Continuous Delivery-Pipeline, die für die Bereitstellung auf dem virtuellen Computer konfiguriert ist.
![AzDevOps_canary_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-pipeline.png)


5. Genau wie im Abschnitt **Parallele Bereitstellungen** können Sie auch hier in Azure DevOps auf die Option zum **Bearbeiten** der Releasepipeline klicken, um die Pipelinekonfiguration anzuzeigen. Die Pipeline umfasst drei Phasen: Die erste Phase ist eine DG-Phase mit Bereitstellung auf virtuellen Computern, die mit dem Tag _canary_ versehen wurden. In der zweiten Phase wird die Pipeline angehalten und auf einen manuellen Eingriff zur Fortsetzung der Ausführung gewartet. Nachdem sich ein Benutzer von der Stabilität der Canary-Bereitstellung überzeugt hat, kann er die Pipelineausführung fortsetzen. Daraufhin wird die dritte Phase zur Bereitstellung auf Computern mit dem Tag _prod_ durchgeführt. ![AzDevOps_canary_task](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-task.png)



**Blaugrün-Bereitstellungen:** Bei einer Blaugrün-Bereitstellung wird zur Verringerung der Downtime eine identische Standbyumgebung verwendet. Eine der Umgebungen befindet sich hierbei immer im Livezustand. Wenn Sie sich auf das neue Release vorbereiten, führen Sie die letzte Testphase in der grünen Umgebung durch. Wenn die Software in der grünen Umgebung funktioniert, können Sie den Datenverkehr umleiten, damit alle eingehenden Anforderungen an die grüne Umgebung geleitet werden. Die blaue Umgebung wird dann als im Leerlauf befindlich angezeigt.
Sie können Blaugrün-Bereitstellungen für Ihre „**virtuellen Computer**“ konfigurieren, indem Sie im Azure-Portal die Option für Continuous Delivery verwenden. 

Hier ist eine ausführliche exemplarische Vorgehensweise angegeben.

1. Melden Sie sich an Ihrem Azure-Portal an, und navigieren Sie zu einem virtuellen Computer. 
2. Führen Sie die Schritte 2 bis 7 aus dem Abschnitt **Parallele Bereitstellungen** aus, um der Bereitstellungsgruppe mehrere virtuelle Computer hinzuzufügen. Fügen Sie den virtuellen Computern, die Teil von Blaugrün-Bereitstellungen sein sollen, ein benutzerdefiniertes Tag vom Typ „blue“ (blau) oder „green“ (grün) hinzu. Für eine Standbyrolle vorgesehene virtuelle Computer müssen mit dem Tag „green“ (grün) versehen werden.
![AzDevOps_bluegreen_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-configure.png)

4. Anschließend verfügen Sie über eine Continuous Delivery-Pipeline, die für die Bereitstellung auf dem virtuellen Computer konfiguriert ist.
![AzDevOps_bluegreen_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-pipeline.png)

5. Wie bereits unter **Parallele Bereitstellungen** beschrieben, können Sie in Azure DevOps auf die Option zum **Bearbeiten** der Releasepipeline klicken, um die Pipelinekonfiguration anzuzeigen. Die Pipeline umfasst drei Phasen: Die erste Phase ist eine DG-Phase mit Bereitstellung auf virtuellen Computern, die als virtuelle Standbycomputer mit dem Tag _green_ (grün) versehen wurden. In der zweiten Phase wird die Pipeline angehalten und auf einen manuellen Eingriff zur Fortsetzung der Ausführung gewartet. Nachdem sich ein Benutzer von der Stabilität der Bereitstellung überzeugt hat, kann er den Datenverkehr an virtuelle Computer mit dem Tag _green_ (grün) umleiten und die Pipelineausführung fortsetzen, woraufhin die Tags _blue_ (blau) und _green_ (grün) der virtuellen Computer vertauscht werden. Dadurch wird sichergestellt, dass die virtuellen Computer, die über eine ältere Anwendungsversion verfügen, mit dem Tag _green_ (grün) versehen und bei der nächsten Pipelineausführung als Bereitstellungsziel verwendet werden.
![AzDevOps_bluegreen_task](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-tasks.png)

6. Beachten Sie, dass für diese Bereitstellungsstrategie mindestens ein virtueller Computer mit dem Tag „blue“ (blau) und mindestens ein virtueller Computer mit dem Tag „green“ (grün) vorhanden sein muss. Vergewissern Sie sich, dass mindestens ein virtueller Computer mit dem Tag _blue_ (blau) vorhanden ist, bevor Sie die Pipelineausführung im Schritt mit dem manuellen Eingriff fortsetzen.





 
## <a name="azure-devops-project"></a>Azure DevOps-Projekt 
Der Einstieg in Azure ist jetzt noch einfacher.
 
Starten Sie mit DevOps Projects die Ausführung Ihrer Anwendung auf einem beliebigen Azure-Dienst mit nur drei Schritten: Sie müssen lediglich eine Anwendungssprache, eine Runtime und einen Azure-Dienst auswählen.
 
[Weitere Informationen](https://azure.microsoft.com/features/devops-projects/ )
 
## <a name="additional-resources"></a>Zusätzliche Ressourcen 
- [Tutorial: Bereitstellen einer ASP.NET-App auf virtuellen Azure-Computern mithilfe von Azure DevOps Projects](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Implementieren von Continuous Deployment für Ihre App in einer VM-Skalierungsgruppe von Azure](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
