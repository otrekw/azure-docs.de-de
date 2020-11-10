---
title: 'Schnellstart: JBoss Enterprise Application (EAP) unter Red Hat Enterprise Linux (RHEL) für Azure-VM und VM-Skalierungsgruppe'
description: Es wird beschrieben, wie Sie Java-Unternehmensanwendungen per Red Hat JBoss EAP auf einer Azure-VM bzw. in einer VM-Skalierungsgruppe mit RHEL bereitstellen.
author: theresa-nguyen
ms.author: bicnguy
ms.topic: quickstart
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.assetid: 8a4df7bf-be49-4198-800e-db381cda98f5
ms.date: 10/30/2020
ms.openlocfilehash: d97148393d3158e38f9740d4a8f8e17dd04326d5
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93135114"
---
# <a name="deploy-enterprise-java-applications-to-azure-with-jboss-eap-on-red-hat-enterprise-linux"></a>Bereitstellen von Java-Unternehmensanwendungen in Azure mit JBoss EAP unter Red Hat Enterprise Linux

Mit diesen Schnellstartvorlagen wird veranschaulicht, wie Sie [JBoss EAP](https://www.redhat.com/en/technologies/jboss-middleware/application-platform) mit [RHEL](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux) für virtuelle Azure-Computer (VMs) und VM-Skalierungsgruppen bereitstellen. Sie verfügen in der Bereitstellung über eine Java-Beispiel-App zum Überprüfen der Bereitstellung. JBoss EAP ist eine Open-Source-Anwendungsserverplattform. Sie ermöglicht für Ihre Java-Anwendungen Sicherheit, Skalierbarkeit und hohe Leistung auf Unternehmensniveau. RHEL ist eine Open-Source-Betriebssystemplattform. Sie ermöglicht das Skalieren von vorhandenen Apps und den Rollout neuer Technologien für alle Umgebungen. In JBoss EAP und RHEL ist alles enthalten, was Sie zum Erstellen, Ausführen, Bereitstellen und Verwalten von Java-Unternehmensanwendungen in einer Umgebung benötigen. Es ist eine hervorragende Open-Source-Lösung für lokale, virtuelle Umgebungen und private, öffentliche oder hybride Clouds.

## <a name="prerequisite"></a>Voraussetzung 

* Ein Azure-Konto mit einem aktiven Abonnement. Aktivieren Sie Ihre [Azure-Gutschriften für Visual Studio-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details), oder [erstellen Sie ein kostenloses Konto](https://azure.microsoft.com/pricing/free-trial), damit Sie über ein Azure-Abonnement verfügen.

* JBoss EAP-Installation: Sie benötigen ein Red Hat-Konto mit RHSM-Berechtigung (Red Hat Subscription Management) für JBoss EAP. Mit dieser Berechtigung können Sie die für Red Hat getestete und zertifizierte JBoss EAP-Version herunterladen.  Wenn Sie nicht über eine EAP-Berechtigung verfügen, sollten sie sich zunächst ein [JBoss EAP-Evaluierungsabonnement](https://access.redhat.com/products/red-hat-jboss-enterprise-application-platform/evaluation) beschaffen. Greifen Sie zum Erstellen eines neuen Red Hat-Abonnements auf das [Red Hat-Kundenportal](https://access.redhat.com/) zu, und richten Sie ein Konto ein.
F
* [Azure-Befehlszeilenschnittstelle (Azure CLI)](https://docs.microsoft.com/cli/azure/overview)

* RHEL-Optionen: Wählen Sie zwischen „Nutzungsbasierte Zahlung“ und „Bring-Your-Own-Subscription (BYOS)“. Bei BYOS müssen Sie Ihr RHEL Gold Image für [Red Hat Cloud Access](https://access.redhat.com/) aktivieren, bevor Sie die Schnellstartvorlage bereitstellen.

## <a name="java-ee--jakarata-ee-application-migration"></a>Java EE/Jakarata EE: Anwendungsmigration

### <a name="migrate-to-jboss-eap"></a>Migrieren zu JBoss EAP
JBoss EAP 7.2 und 7.3 sind zertifizierte Implementierungen der Spezifikationen von Java Enterprise Edition (Java EE) 8 und Jakarta EE 8. JBoss EAP verfügt über vorkonfigurierte Optionen für Features, z. B. Hochverfügbarkeitsclustering, Messaging und verteiltes Caching. Hiermit können Benutzer Anwendungen schreiben, bereitstellen und ausführen, indem sie die verschiedenen APIs und Dienste von JBoss EAP verwenden.  Weitere Informationen zu JBoss EAP finden Sie in der [Einführung zu JBoss EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html-single/introduction_to_jboss_eap/index) bzw. der [Einführung zu JBoss EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/introduction_to_jboss_eap/index).

 #### <a name="applications-on-jboss-eap"></a>Anwendungen von JBoss EAP

* Webdienstanwendungen: Webdienste verfügen über eine Standardmethode zur Erzielung der Interoperabilität zwischen verschiedenen Softwareanwendungen. Jede Anwendung kann auf verschiedenen Plattformen und Frameworks ausgeführt werden. Mit diesen Webdiensten wird die Kommunikation für interne und heterogene Subsysteme ermöglicht. Weitere Informationen finden Sie unter [Entwickeln von Webdienstanwendungen in EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/developing_web_services_applications/index) bzw. [Entwickeln von Webdienstanwendungen in EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/developing_web_services_applications/index).

* EJB-Anwendungen (Enterprise Java Beans): EJB 3.2 ist eine API für die Entwicklung von verteilten, transaktionalen, sicheren und portablen Java EE- und Jakarta EE-Anwendungen. Bei EJB werden serverseitige Komponenten (als Enterprise Beans bezeichnet) verwendet, um die Geschäftslogik einer Anwendung entkoppelt zu implementieren und so deren Wiederverwendung zu fördern. Weitere Informationen finden Sie unter [Entwickeln von EJB-Anwendungen in EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/developing_ejb_applications/index) bzw. [Entwickeln von EJB-Anwendungen in EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/developing_ejb_applications/index).

* Hibernate-Anwendungen: Entwickler und Administratoren können mit JBoss EAP Anwendungen vom Typ „Java Persistence API (JPA)“/„Hibernate“ entwickeln und bereitstellen. Hibernate Core ist ein Framework für die objektrelationale Zuordnung für die Java-Sprache. Hiermit kann ein objektorientiertes Domänenmodell einer relationalen Datenbank zugeordnet werden, damit Anwendungen die direkte Interaktion mit der Datenbank vermeiden können. Mit Hibernate Entity Manager werden die Programmierschnittstellen und Lebenszyklusregeln implementiert, die in der [JPA 2.1-Spezifikation](https://www.jcp.org/en/jsr/overview) definiert sind. Zusammen mit Hibernate Annotations implementiert dieser Wrapper zusätzlich zum ausgereiften Hibernate Core-Framework eine vollständige (und eigenständige) JPA-Lösung. Weitere Informationen zu Hibernate finden Sie unter [JPA unter EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/development_guide/java_persistence_api) bzw. [Jakarta Persistence unter EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/development_guide/java_persistence_api).

#### <a name="red-hat-migration-toolkit-for-applications-mta"></a>Red Hat Migration Toolkit for Applications (MTA)
Das [Red Hat Migration Toolkit for Applications (MTA)](https://developers.redhat.com/products/mta/overview) ist ein Migrationstool für Java-Anwendungsserver. Verwenden Sie dieses Tool für die Migration von einem anderen App-Server zu JBoss EAP. Hierfür können Sie IDE-Plug-Ins für [Eclipse IDE](https://www.eclipse.org/ide/), [Red Hat CodeReady Workspaces](https://developers.redhat.com/products/codeready-workspaces/overview) und [Visual Studio Code (VS Code)](https://code.visualstudio.com/docs/languages/java) für Java verwenden.  MTA ist ein kostenloses Open-Source-Tool mit folgenden Features:
* Automatisierung der Anwendungsanalyse
* Schätzung des Supportaufwands
* Beschleunigung der Codemigration
* Unterstützung der Containerisierung
* Integration in Azure Workload Builder

### <a name="migrate-jboss-eap-from-on-premises-to-azure"></a>Migrieren von JBoss EAP aus der lokalen Umgebung zu Azure
Das Azure Marketplace-Angebot von JBoss EAP unter RHEL wird auf Azure-VMs in weniger als 20 Minuten installiert und bereitgestellt. Sie können auf diese Angebote im [Azure Marketplace](https://azuremarketplace.microsoft.com/) zugreifen.

Dieses Marketplace-Angebot umfasst verschiedene Kombinationen von EAP- und RHEL-Versionen, damit Ihre jeweiligen Anforderungen erfüllt werden. Für JBoss EAP gilt immer „Bring-Your-Own-Subscription (BYOS)“, aber für das RHEL-Betriebssystem können Sie zwischen BYOS und nutzungsbasierter Zahlung wählen. Das Azure Marketplace-Angebot umfasst Planoptionen für JBoss EAP unter RHEL in Form von eigenständigen oder gruppierten VMs:

* JBoss EAP 7.2 auf RHEL 7.7-VM (nutzungsbasiert)
* JBoss EAP 7.2 auf RHEL 8.0-VM (nutzungsbasiert)
* JBoss EAP 7.3 auf RHEL 8.0-VM (nutzungsbasiert)
* JBoss EAP 7.2 auf RHEL 7.7-VM (BYOS)
* JBoss EAP 7.2 auf RHEL 8.0-VM (BYOS)
* JBoss EAP 7.3 auf RHEL 8.0-VM (BYOS)

Zusammen mit Azure Marketplace-Angeboten werden für Sie Schnellstartvorlagen verfügbar gemacht, damit Sie mit Ihrer Azure-Migrationsjourney beginnen können. Diese Schnellstarts enthalten vordefinierte ARM-Vorlagen (Azure Resource Manager) und ein Skript zum Bereitstellen von JBoss EAP unter RHEL mit verschiedenen Konfigurationen und Versionskombinationen. Sie erhalten hierbei Folgendes:

* Lastenausgleich (Load Balancer, LB)
* Private IP-Adresse für Lastenausgleich und VMs
* Virtuelles Netzwerk (VNET) mit einem einzelnen Subnetz
* VM-Konfiguration (Cluster oder eigenständig)
* Java-Beispielanwendung

Die Lösungsarchitektur für diese Vorlagen umfasst Folgendes:

* JBoss EAP auf eigenständiger RHEL-VM
* Gruppierung von JBoss EAP auf mehreren RHEL-VMs
* Gruppierung von JBoss EAP per Azure-VM-Skalierungsgruppe

#### <a name="linux-workload-migration-for-jboss-eap"></a>Migration der Linux-Workload für JBoss EAP
Mit Azure Workload Builder wird der Proof-of-Concept- (POC), Evaluierungs- und Migrationsprozess für lokale Java-Apps zu Azure vereinfacht. Workload Builder ist in das Ermittlungstool von Azure Migrate integriert, um JBoss EAP-Server identifizieren zu können. Anschließend wird das Ansible-Playbook für die JBoss EAP-Serverbereitstellung dynamisch generiert. Hierbei wird das MTA-Tool von Red Hat genutzt, um Server von anderen App-Servern zu JBoss EAP zu migrieren. Die Schritte zur Vereinfachung der Migration umfassen Folgendes:
* Evaluierung: JBoss EAP-Cluster mit Azure-VM oder VM-Skalierungsgruppe
* Bewertung: Untersuchung von Anwendungen und Infrastruktur
* Infrastrukturkonfiguration: Erstellung des Workloadprofils
* Bereitstellung und Tests: Bereitstellung, Migration und Testen der Workload
* Konfiguration nach der Bereitstellung: Integration in Bezug auf Daten, Überwachung, Sicherheit, Sicherung und mehr

## <a name="server-configuration-choice"></a>Auswahl für Serverkonfiguration

Für die Bereitstellung der RHEL-VM können Sie zwischen „Nutzungsbasierte Zahlung“ und „BYOS“ wählen. Für Images aus dem [Azure Marketplace](https://azuremarketplace.microsoft.com) wird standardmäßig die nutzungsbasierte Zahlung verwendet. Stellen Sie eine RHEL-VM mit BYOS bereit, falls Sie über ein eigenes RHEL-Betriebssystemimage verfügen. Stellen Sie sicher, dass Ihr RHSM-Konto über eine BYOS-Berechtigung per Cloud Access verfügt, bevor Sie die VMs oder die VM-Skalierungsgruppe bereitstellen.

JBoss EAP enthält leistungsstarke Verwaltungsfunktionen sowie Funktionen und APIs für seine Anwendungen. Diese Verwaltungsfunktionen unterscheiden sich in Abhängigkeit davon, welcher Betriebsmodus zum Starten von JBoss EAP verwendet wird. Unterstützung ist für RHEL und Windows Server vorhanden. JBoss EAP verfügt über einen Betriebsmodus für eigenständige Server, um die Verwaltung diskreter Instanzen zu ermöglichen. Darüber hinaus ist auch ein Betriebsmodus vom Typ „Verwaltete Domäne“ verfügbar, in dem Gruppen von Instanzen von einem zentralen Kontrollpunkt aus verwaltet werden können. Hinweis: Mit JBoss EAP verwaltete Domänen werden unter Microsoft Azure nicht unterstützt, weil das Feature für Hochverfügbarkeit von den Diensten der Azure-Infrastruktur verwaltet wird. Die Umgebungsvariable mit dem Namen *EAP_HOME* wird zum Angeben des Pfads zur JBoss EAP-Installation verwendet.

**Starten von JBoss EAP als eigenständigen Server** : Mit dem folgenden Befehl starten Sie den EAP-Dienst im eigenständigen Modus.

```
$EAP_HOME/bin/standalone.sh
```
    
In diesem Startskript wird die Datei „EAP_HOME/bin/standalone.conf“ zum Festlegen einiger Standardeinstellungen verwendet, z. B. JVM-Optionen. Die Einstellungen können in dieser Datei angepasst werden. In JBoss EAP wird die Konfigurationsdatei „standalone.xml“ verwendet, um standardmäßig das Starten im eigenständigen Modus durchzuführen, aber es kann auch ein anderer Startmodus verwendet werden. Ausführliche Informationen zu den verfügbaren eigenständigen Konfigurationsdateien und deren Verwendung finden Sie im Abschnitt [Konfigurationsdateien für eigenständige Server für EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/configuration_guide/jboss_eap_management#standalone_server_configuration_files) bzw. [Konfigurationsdateien für eigenständige Server für EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/configuration_guide/jboss_eap_management#standalone_server_configuration_files). Verwenden Sie das Argument „--server-config“, um JBoss EAP mit einer anderen Konfiguration zu starten. Beispiel:
    
 ```
 $EAP_HOME/bin/standalone.sh --server-config=standalone-full.xml
 ```
    
Verwenden Sie für eine vollständige Liste mit allen verfügbaren Startskriptargumenten und deren Zweck das Argument „--help“, oder lesen Sie den Abschnitt [Serverlaufzeitargumente in EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/configuration_guide/reference_material#reference_of_switches_and_arguments_to_pass_at_server_runtime) bzw. [Serverlaufzeitargumente in EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/configuration_guide/reference_material#reference_of_switches_and_arguments_to_pass_at_server_runtime).
    
JBoss EAP kann auch im Clustermodus ausgeführt werden. Weitere Informationen finden Sie unter [Clusterübersicht in EAP 7.2](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.2/html/configuring_messaging/clusters_overview) bzw. [Clusterübersicht in EAP 7.3](https://access.redhat.com/documentation/en/red_hat_jboss_enterprise_application_platform/7.3/html/configuring_messaging/clusters_overview). Das JBoss EAP-Clustermessaging ermöglicht die Gruppierung von JBoss EAP-Messagingservern zur gemeinsamen Verarbeitung der Nachrichtenworkload. Bei jedem aktiven Knoten im Cluster handelt es sich um einen aktiven JBoss EAP-Messagingserver, der seine eigenen Nachrichten verwaltet und seine eigenen Verbindungen verarbeitet.

## <a name="support-and-subscription-notes"></a>Hinweise zum Support und zu Abonnements
Diese Schnellstartvorlagen werden wie folgt angeboten: 

- Das RHEL-Betriebssystem wird mit nutzungsbasierter Zahlung oder BYOS basierend auf dem Red Hat Gold Image-Modell angeboten.
- JBoss EAP wird nur mit BYOS angeboten.

#### <a name="using-rhel-os-with-payg-model"></a>Verwenden des RHEL-Betriebssystems mit nutzungsbasierter Zahlung

Standardmäßig wird für diese Schnellstartvorlagen das On-Demand-Image RHEL 7.7 oder 8.0 mit nutzungsbasierter Zahlung verwendet. Für Images mit nutzungsbasierter Zahlung fallen stündliche Gebühren für RHEL-Abonnements zusätzlich zu den üblichen Compute-, Netzwerk- und Speicherkosten an. Die Instanz wird gleichzeitig auch unter Ihrem Red Hat-Abonnement registriert. Dies bedeutet, dass Sie eine Ihrer Berechtigungen verwenden. Dieses Image mit nutzungsbasierter Zahlung führt zu einer „doppelten Abrechnung“. Sie können dieses Problem vermeiden, indem Sie Ihr eigenes RHEL-Image erstellen. Weitere Informationen finden Sie bei Red Hat im Knowledge Base-Artikel (KB) zum Thema [Bereitstellen einer RHEL-VM für Microsoft Azure](https://access.redhat.com/articles/uploading-rhel-image-to-azure). Sie können auch Ihr RHEL Gold Image für [Red Hat Cloud Access](https://access.redhat.com/) aktivieren.

Ausführliche Informationen zu den Preisen für VMs mit nutzungsbasierter Zahlung finden Sie unter [Virtuelle Red Hat-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/red-hat/). Für die Nutzung von RHEL mit dem Modell für nutzungsbasierte Zahlung benötigen Sie ein Azure-Abonnement mit der angegebenen Zahlungsmethode für [RHEL 7.7 Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/RedHat.RedHatEnterpriseLinux77-ARM) bzw. [RHEL 8.0 Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/RedHat.RedHatEnterpriseLinux80-ARM). Bei diesen Angeboten muss eine Zahlungsmethode im Azure-Abonnement angegeben werden.

#### <a name="using-rhel-os-with-byos-model"></a>Verwenden des RHEL-Betriebssystems mit BYOS

Für die Verwendung von BYOS für das RHEL-Betriebssystem müssen Sie über ein gültiges Red Hat-Abonnement mit Berechtigungen zur Nutzung des RHEL-Betriebssystems in Azure verfügen. Erfüllen Sie die folgenden Voraussetzungen, bevor Sie diese Schnellstartvorlage bereitstellen:

1. Stellen Sie sicher, dass Sie unter Ihrem Red Hat-Abonnement über Berechtigungen für das RHEL-Betriebssystem und für JBoss EAP verfügen.
2. Führen Sie für Ihre Azure-Abonnement-ID die Autorisierung zur Nutzung von RHEL-Images mit BYOS durch. Halten Sie sich an die [RHSM-Dokumentation (Red Hat Subscription Management)](https://access.redhat.com/documentation/en/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/con-enable-subs), um diesen Prozess durchzuführen. Dies umfasst die folgenden Schritte:

    2.1 Aktivieren Sie Microsoft Azure in Ihrem Red Hat Cloud Access-Dashboard als Anbieter.

    2.2 Fügen Sie Ihre Azure-Abonnement-IDs hinzu.

    2.3 Aktivieren Sie neue Produkte für Cloud Access unter Microsoft Azure.
    
    2.4 Aktivieren Sie Red Hat Gold Images für Ihr Azure-Abonnement. Ausführlichere Informationen finden Sie im Kapitel zum Thema [Aktivieren und Verwalten von Abonnements für Cloud Access](https://access.redhat.com/documentation/en/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/using_red_hat_gold_images#con-gold-image-azure).

    2.5 Warten Sie ab, bis die Red Hat Gold Images in Ihrem Azure-Abonnement verfügbar sind. Diese Gold Images sind normalerweise innerhalb von drei Stunden nach der Übermittlung der Daten verfügbar.
    
3. Akzeptieren Sie die Azure Marketplace-Geschäftsbedingungen für RHEL-Images mit BYOS. Sie können diesen Prozess durchführen, indem Sie wie unten angegeben die entsprechenden Azure CLI-Befehle ausführen. Ausführlichere Informationen finden Sie in der Dokumentation unter [Bring-Your-Own-Subscription-Gold-Images für Red Hat Enterprise Linux in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/byos). Es ist wichtig, dass Sie die neueste Azure CLI-Version ausführen.

    3.1 Starten Sie eine Azure CLI-Sitzung, und führen Sie die Authentifizierung für Ihr Azure-Konto durch. Hilfreiche Informationen finden Sie unter [Anmelden mit der Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli). Stellen Sie vor dem Fortfahren sicher, dass Sie die neueste Azure CLI-Version ausführen.

    3.2 Vergewissern Sie sich, dass die RHEL-Images mit BYOS unter Ihrem Abonnement verfügbar sind, indem Sie den folgenden CLI-Befehl ausführen. Falls Sie hier keine Ergebnisse erhalten, sollten Sie mit Punkt 2 fortfahren und sicherstellen, dass Ihr Azure-Abonnement für RHEL-Images mit BYOS aktiviert ist.
   
   ```
   az vm image list --offer rhel-byos --all
   ```

    3.3 Führen Sie den folgenden Befehl aus, um die Marketplace-Geschäftsbedingungen für RHEL 7.7 BYOS bzw. RHEL 8.0 BYOS zu akzeptieren.
   ```
   az vm image terms accept --publisher redhat --offer rhel-byos --plan rhel-lvm77
   ``` 

   ```
   az vm image terms accept --publisher redhat --offer rhel-byos --plan rhel-lvm8
   ``` 
   
4. Ihr Abonnement ist nun für die Bereitstellung von RHEL 7.7 oder 8.0 mit BYOS auf virtuellen Azure-Computern bereit.

#### <a name="using-jboss-eap-with-byos-model"></a>Verwenden von JBoss EAP mit BYOS-Modell

In Azure ist JBoss EAP nur mit dem BYOS-Modell verfügbar. Beim Bereitstellen dieser Vorlage müssen Sie Ihre RHSM-Anmeldeinformationen mit der RHSM-Pool-ID mit gültigen EAP-Berechtigungen angeben. Wenn Sie nicht über eine EAP-Berechtigung verfügen, sollten sie sich zunächst ein [JBoss EAP-Evaluierungsabonnement](https://access.redhat.com/products/red-hat-jboss-enterprise-application-platform/evaluation) beschaffen.

## <a name="how-to-consume"></a>Nutzen

Sie können die Vorlage auf die folgenden drei Arten bereitstellen:

- PowerShell verwenden – Stellen Sie die Vorlage mithilfe der folgenden Befehle bereit: (Informationen zur Installation und Konfiguration von Azure PowerShell finden Sie unter [Azure PowerShell](https://docs.microsoft.com/powershell/azure/)).

  ```
  New-AzResourceGroup -Name <resource-group-name> -Location <resource-group-location> #use this command when you need to create a new resource group for your deployment
  ```

  ```
  New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateUri <raw link to the template which can be obtained from github>
  ```
    
- Azure CLI verwenden – Stellen Sie die Vorlage mithilfe der folgenden Befehle bereit: (Ausführliche Informationen zur Installation und Konfiguration der Azure CLI finden Sie [hier](https://docs.microsoft.com/cli/azure/install-azure-cli).)

  ```
  az group create --name <resource-group-name> --location <resource-group-location> #use this command when you need to create a new resource group for your deployment
  ```

  ```
  az group deployment create --resource-group <my-resource-group> --template-uri <raw link to the template which can be obtained from github>
  ```

- Azure-Portal verwenden: Für die Bereitstellung über das Azure-Portal können Sie die *Azure-Schnellstartvorlagen* verwenden (wie im Abschnitt unten beschrieben). Klicken Sie in der Schnellstartanleitung dann auf die Schaltfläche **In Azure bereitstellen** oder **Auf GitHub suchen**.

## <a name="azure-quickstart-templates"></a>Azure-Schnellstartvorlagen

Sie können mit einer der Schnellstartvorlagen starten, die über die Kombination von JBoss EAP und RHEL verfügt, mit der Sie Ihr Bereitstellungsziel erreichen. Die Liste mit den verfügbaren Schnellstartvorlagen ist hier angegeben.

* <a href="https://azure.microsoft.com/resources/templates/jboss-eap-standalone-rhel/">JBoss EAP unter RHEL (eigenständige VM)</a>: Hiermit wird eine Webanwendung mit dem Namen „JBoss-EAP“, die auf einer VM mit RHEL 7.7 oder 8.0 ausgeführt wird, unter Azure mit JBoss EAP 7.2 oder 7.3 bereitgestellt.

* <a href="https://azure.microsoft.com/resources/templates/jboss-eap-clustered-multivm-rhel/">JBoss EAP unter RHEL (mehrere gruppierte VMs)</a>: Hiermit wird eine Webanwendung mit dem Namen „eap-session-replication“, die auf einer bestimmten Anzahl von VMs (Wert n) mit RHEL 7.7 oder 8.0 ausgeführt wird, in einem JBoss EAP 7.2- oder 7.3-Cluster bereitgestellt. Der n-Wert wird vom Benutzer festgelegt. Alle VMs werden dem Back-End-Pool eines Load Balancers hinzugefügt.

* <a href="https://azure.microsoft.com/en-us/resources/templates/jboss-eap-clustered-vmss-rhel/">JBoss EAP unter RHEL (gruppierte VM-Skalierungsgruppe)</a>: Hiermit wird eine Webanwendung mit dem Namen „eap-session-replication“, die auf VM-Skalierungsgruppeninstanzen vom Typ RHEL 7.7 oder 8.0 ausgeführt wird, in einem JBoss EAP 7.2- oder 7.3-Cluster bereitgestellt.

## <a name="resource-links"></a>Links zu Ressourcen:

* [Azure-Hybridvorteile](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing)
* [Konfigurieren einer Java-App für Azure App Service](https://docs.microsoft.com/azure/app-service/configure-language-java)
* [JBoss EAP unter Azure Red Hat OpenShift](https://azure.microsoft.com/services/openshift/)
* [Schnellstart: Erstellen einer Java-App in Azure App Service](https://docs.microsoft.com/azure/app-service/quickstart-java)
* [Bereitstellen von JBoss EAP in Azure App Service](https://github.com/JasonFreeberg/jboss-on-app-service): Tutorial

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen: [JBoss EAP 7.2](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.2/)
* Weitere Informationen: [JBoss EAP 7.3](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.3/)
* Weitere Informationen: [Red Hat Subscription Management](https://access.redhat.com/products/red-hat-subscription-management)
* Microsoft-Dokumentation zu [Red Hat in Azure](https://aka.ms/rhel-docs)
* Bereitstellung: [JBoss EAP für RHEL-VM/-VM-Skalierungsgruppe über Azure Marketplace](https://aka.ms/AMP-JBoss-EAP)
* Bereitstellung: [JBoss EAP für RHEL-VM/-VM-Skalierungsgruppe per Azure-Schnellstart](https://aka.ms/Quickstart-JBoss-EAP)
