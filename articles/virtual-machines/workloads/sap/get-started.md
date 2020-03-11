---
title: Erste Schritte mit SAP in Azure-VMs | Microsoft-Dokumentation
description: Erfahren Sie etwas über SAP-Lösungen auf virtuellen Windows-Computern in Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/03/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0219b3e35ed82795e9c799754cabdeb5d17096d6
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2020
ms.locfileid: "78267404"
---
# <a name="use-azure-to-host-and-run-sap-workload-scenarios"></a>Verwenden von Azure zum Hosten und Ausführen von SAP-Workloadszenarien

Mit Microsoft Azure können Sie Ihre geschäftskritischen SAP-Workloads und -Szenarien zuverlässig auf einer skalierbaren, konformen und in Unternehmen bewährten Plattform ausführen. Sie profitieren von der Skalierbarkeit, der Flexibilität und den Kosteneinsparungen von Azure. Dank der erweiterten Partnerschaft zwischen Microsoft und SAP können Sie SAP-Anwendungen in Entwicklungs-, Test- und Produktionsszenarien in Azure ausführen und erhalten dabei vollständigen Support. Wir können Sie in allen Belangen unterstützen, von SAP NetWeaver bis SAP S/4HANA, SAP BI unter Linux bis Windows und SAP HANA bis SQL.

Neben dem Hosten von SAP NetWeaver-Szenarien mit verschiedenen DBMS-Lösungen in Azure können Sie auch andere SAP-Workloadszenarien hosten, z. B. SAP BI unter Azure. 

Die Einzigartigkeit von Azure für SAP HANA ist ein Angebot, mit dem sich Azure absetzt. Um das Hosting von SAP-Szenarien mit höherem Arbeitsspeicher- und CPU-Ressourcenbedarf mit SAP HANA zu ermöglichen, bietet Azure für den Kunden dedizierte Bare-Metal-Hardware an. Verwenden Sie diese Lösung zum Ausführen von SAP HANA-Bereitstellungen, die bis zu 24 TB (120 TB bei horizontaler Hochskalierung) Arbeitsspeicher für S/4HANA- oder andere SAP HANA-Workloads erfordern. 

Durch das Hosten von SAP-Workloadszenarien in Azure können auch Anforderungen an Identitätsintegration und einmaliges Anmelden entstehen. Diese Situation kann auftreten, wenn Sie Azure Active Directory (Azure AD) verwenden, um verschiedene SAP-Komponenten und SaaS- (Software-as-a-Service) oder PaaS-Angebote (Platform as a Service) von SAP zu verbinden. Eine Liste solcher Integrations- und SSO-Szenarien mit Azure AD und SAP-Entitäten mit den entsprechenden Beschreibungen finden Sie im Abschnitt „AAD-SAP-Identitätsintegration und einmaliges Anmelden“.

## <a name="changes-to-the-sap-workload-section"></a>Änderungen am SAP-Workload-Abschnitt
Änderungen an Dokumenten im Abschnitt „SAP in Azure-Workload“ werden am Ende dieses Artikels aufgelistet. Die Einträge im Änderungsprotokoll werden ca. 180 Tage lang beibehalten.

## <a name="you-want-to-know"></a>Sie möchten Folgendes wissen:
Falls Sie bestimmte Fragen haben, verweisen wir Sie auf bestimmte Dokumente oder Flows in diesem Abschnitt der Startseite. Sie möchten Folgendes wissen:

- Welche Azure VMs und HANA-Einheiten (große Instanzen) werden bei welchen SAP-Softwarereleases und welchen Betriebssystemversionen unterstützt? Antworten und den erforderlichen Vorgang zum Suchen nach den jeweiligen Informationen finden Sie im Dokument [Welche SAP-Software wird für Azure-Bereitstellungen unterstützt?](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure).
- Welche SAP-Bereitstellungsszenarien werden bei Azure VMs und HANA (große Instanzen) unterstützt? Informationen zu den unterstützten Szenarien finden Sie in den folgenden Dokumenten:
    - [SAP-Workload in Szenarien mit Unterstützung von virtuellen Azure-Computern](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations)
    - [Unterstützte Szenarien für HANA (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario)

 
## <a name="sap-hana-on-azure-large-instances"></a>SAP HANA in Azure (große Instanzen)

Eine Dokumentationsreihe führt Sie durch SAP HANA in Azure (große Instanzen), Kurzform: HANA (große Instanzen). Wenn Sie sich zu HANA (große Instanzen) informieren möchten, lesen Sie zuerst das Dokument [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) und anschließend die zugehörige Dokumentation im Abschnitt „HANA (große Instanzen)“.



## <a name="sap-hana-on-azure-virtual-machines"></a>SAP HANA auf virtuellen Azure-Computern
In diesem Abschnitt der Dokumentation werden verschiedene Aspekte von SAP HANA behandelt. Dabei wird vorausgesetzt, dass Sie mit den wichtigsten Diensten von Azure vertraut sind, die elementare Dienste von Azure IaaS bereitstellen. Sie benötigen somit hauptsächlich Kenntnisse über Azure-Compute, -Speicher und -Netzwerke. Viele dieser Themen werden im SAP NetWeaver-bezogenen [Azure-Planungsleitfaden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) behandelt. 

Informationen zu HANA in Azure finden Sie in den folgenden Artikeln und den jeweiligen Unterartikeln:

- [SAP HANA-Infrastrukturkonfigurationen und -Vorgänge in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [Hochverfügbarkeit von SAP HANA für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)
- [Hochverfügbarkeit von SAP HANA auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Sicherungsanleitung für SAP HANA auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)


 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>Bereitstellung von SAP NetWeaver auf virtuellen Azure-Computern
In diesem Abschnitt finden Sie die Dokumentation zur Planung und Bereitstellung von SAP NetWeaver und Business One in Azure. Die Dokumentation behandelt in erster Linie die Grundlagen und die Verwendung von Nicht-HANA-Datenbanken mit einer SAP-Workload in Azure. Die Dokumente und Artikel über Hochverfügbarkeit bilden auch die Grundlage für die Hochverfügbarkeit von HANA in Azure, z. B.:

- [Azure-Planungshandbuch](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide). 
- [SAP Business One auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/business-one-azure)
- [Schützen einer SAP NetWeaver-Anwendungsbereitstellung mit mehreren Ebenen mit Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)
- [SAP LaMa-Connector für Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/lama-installation)

Informationen zu Nicht-HANA-Datenbanken unter einer SAP-Workload in Azure finden Sie unter:

- [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [Azure Virtual Machines – SQL Server-DBMS-Bereitstellung für SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)
- [Azure Virtual Machines – Oracle-DBMS-Bereitstellung für SAP-Workload](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_oracle)
- [Azure Virtual Machines – IBM DB2-DBMS-Bereitstellung für SAP-Workload](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
- [Azure Virtual Machines – SAP ASE-DBMS-Bereitstellung für SAP-Workload](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- [Bereitstellung von SAP MaxDB, SAP liveCache und SAP Content Server in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)

Informationen zu SAP HANA-Datenbanken in Azure finden Sie im Abschnitt „SAP HANA auf virtuellen Azure-Computern“.

Informationen zur Hochverfügbarkeit von SAP-Workloads in Azure finden Sie unter:

- [Hochverfügbarkeit von Azure Virtual Machines für SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)

Dieses Dokument verweist auf verschiedene andere Dokumente zur Architektur und zu ähnlichen Szenarien. In nachfolgenden Szenariodokumenten werden Links zu ausführlichen technischen Dokumenten bereitgestellt, in denen die Bereitstellung und Konfiguration der verschiedenen Hochverfügbarkeitsmethoden erläutert werden. In den anderen Dokumenten zum Einrichten und Konfigurieren von Hochverfügbarkeit für SAP NetWeaver-Workloads werden Linux- und Windows-Betriebssysteme behandelt.


Informationen zur Integration von Azure Active Directory (Azure AD) und SAP-Diensten mit einmaligem Anmelden finden Sie unter:

- [Tutorial: Azure Active Directory-Integration mit SAP Cloud for Customer](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Azure Active Directory-Integration mit SAP Cloud Platform Identity Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Azure Active Directory-Integration mit SAP Cloud Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Azure Active Directory-Integration mit SAP NetWeaver](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Azure Active Directory-Integration mit SAP Business ByDesign](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Tutorial: Azure Active Directory-Integration mit SAP HANA](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Ihre S/4HANA-Umgebung: SAML-SSO für Fiori Launchpad mit Azure AD](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

Informationen zur Integration von Azure-Diensten in SAP-Komponenten finden Sie unter:

- [Verwenden von SAP HANA in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery und SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [Verwenden des SAP BW-Connectors in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [SAP HANA- und Business Warehouse-Datenintegration mit Azure Data Factory](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)


## <a name="change-log"></a>Änderungsprotokoll
- 03.03.2020: Änderung in [Hochverfügbarkeit für SAP NW auf virtuellen Azure-Computern in SLES mit ANF für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files), um zu einem effizienteren ANF-Volumelayout zu wechseln.
- 01.03.2020: Überarbeitung von [Sicherungsanleitung für SAP HANA in Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide), um den Azure Backup-Dienst zu berücksichtigen. Reduzierter und komprimierter Inhalt in [SAP HANA Azure Backup auf Dateiebene](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level) und ein drittes Dokument gelöscht, das die Sicherung über eine Datenträgermomentaufnahme behandelt. Inhalt wird im Leitfaden zur Sicherung von SAP HANA in Azure Virtual Machines behandelt. 
- 27.02.2020: Änderung in [Hochverfügbarkeit für SAP NW auf virtuellen Azure-Computern in SLES für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse) und [Hochverfügbarkeit für SAP NW auf virtuellen Azure-Computern in SLES mit ANF für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) sowie [Hochverfügbarkeit von SAP NetWeaver auf virtuellen Azure-Computern in SLES, Multi-SID-Leitfaden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid), um den Clusterparameter „on fail“ (bei Fehler) anzupassen.
- 26.02.2020: Änderung in [SAP HANA: Speicherkonfigurationen für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage), um die Dateisystemauswahl für HANA in Azure zu verdeutlichen.
- 26.02.2020: Änderung in [Hochverfügbarkeitsarchitektur und Szenarien für SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios), um den Link zum Multi-SID-Leitfaden für Hochverfügbarkeit für SAP NetWeaver auf virtuellen Azure-Computern für RHEL einzubeziehen.
- 26.02.2020: Änderung in [Hochverfügbarkeit für SAP NW auf virtuellen Azure-Computern in SLES für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse), [Hochverfügbarkeit für SAP NW auf virtuellen Azure-Computern in SLES mit ANF für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files), [Hochverfügbarkeit von Azure-VMs für SAP NetWeaver unter RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) und [Hochverfügbarkeit von Azure-VMs für SAP NetWeaver unter RHEL mit Azure NetApp Files](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files), um die Aussage zu entfernen, dass Multi-SID ASCS/ERS-Cluster nicht unterstützt werden.
- 26.02.2020: Änderung in [Hochverfügbarkeit virtueller Azure-Computer für SAP NetWeaver unter RHEL: Multi-SID-Leitfaden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-multi-sid), um einen Link zum SUSE-Multi-SID-Clusterhandbuch hinzuzufügen.
- 25.02.2020: Änderung an [Hochverfügbarkeitsarchitektur und Szenarien für SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios), um Links zu neueren HA-Artikeln (High Availability, Hochverfügbarkeit) hinzuzufügen
- 25.02.2020: Änderung an [Hochverfügbarkeit von IBM Db2 LUW auf Azure-VMs unter SUSE Linux Enterprise Server mit Pacemaker](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm), um auf das Dokument zu verweisen, in dem der Zugriff auf den öffentlichen Endpunkt mit Azure Load Balancer Standard beschrieben wird
- 21.02.2020: Vollständige Überarbeitung des Artikels [Azure Virtual Machines – SAP ASE-DBMS-Bereitstellung für SAP-Workload](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- 21.02.2020: Änderung an [SAP HANA: Speicherkonfigurationen für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage), um eine neue Empfehlung in Stripegröße für „/hana/data“ darzustellen und eine Einstellung für E/A-Scheduler hinzuzufügen
- 21.02.2020: Änderungen an Dokumenten zu HANA (große Instanzen) zur Darstellung von neu zertifizierten SKUs von S224 und S224m
- 21.02.2020: Änderung an [Hochverfügbarkeit von Azure VMs für SAP NetWeaver unter RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) und [Hochverfügbarkeit von Azure VMs für SAP NetWeaver unter RHEL mit Azure NetApp Files](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files), um die Clustereinschränkungen für die Replikation von Enqueue-Server 2-Architektur (ENSA2) anzupassen
- 20.02.2020: Änderung an [Hochverfügbarkeit virtueller Azure-Computer für SAP NetWeaver unter SLES: Multi-SID-Leitfaden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid), um einen Link zum „SUSE-Multi-SID-Cluster-Handbuch“ hinzuzufügen
- 13.2.2020: Änderungen an [Azure Virtual Machines – Planung und Implementierung für SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), um Links zu neuen Dokumenten zu implementieren
- 13.2.2020: Neues Dokument [SAP-Workload in Szenarien mit Unterstützung von virtuellen Azure-Computern](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations) wurde hinzugefügt.
- 13.2.2020: Neues Dokument [Welche SAP-Software wird für Azure-Bereitstellungen unterstützt?](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure) wurde hinzugefügt.
- 13.2.2020: Änderung an [Hochverfügbarkeit von IBM DB2 LUW auf virtuellen Azure-Computern unter Red Hat Enterprise Linux Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-ibm-db2-luw), um auf das Dokument zu verweisen, in dem der Zugriff auf den öffentlichen Endpunkt mit Azure Load Balancer Standard beschrieben wird
- 13.2.2020: Die neuen VM-Typen wurden zu [SAP-Zertifizierungen und -Konfigurationen in Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-certifications) hinzugefügt.
- 13.2.2020: Neue SAP-Supporthinweise wurden zu [Prüfliste für die Planung und Bereitstellung von SAP-Workloads in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist) hinzugefügt.
- 13.2.2020: Änderung an [Hochverfügbarkeit von Azure VMs für SAP NetWeaver unter RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) und [Hochverfügbarkeit von Azure VMs für SAP NetWeaver unter RHEL mit Azure NetApp Files](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files), um die Timeoutwerte der Clusterressourcen an die Red Hat-Timeoutempfehlungen anzupassen
- 11.2.2020: Veröffentlichung von [SAP HANA in Azure (große Instanzen) – Migration zu Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-large-instance-virtual-machine-migration)
- 7\.2.2020: Änderung in [Konnektivität öffentlicher Endpunkte für VMs, die Azure Load Balancer Standard in SAP-Hochverfügbarkeitsszenarien verwenden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections) zum Aktualisieren des NSG-Beispielscreenshots
- 3\.2.2020: Änderung an der [Hochverfügbarkeit für SAP NW auf virtuellen Azure-Computern in SLES für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse) und [Hochverfügbarkeit für SAP NW auf virtuellen Azure-Computern in SLES mit ANF für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files), um die Warnung zur Verwendung von Gedankenstrichen in den Hostnamen von Clusterknoten in SLES zu entfernen.
- 28.1.2020: Änderung in [Hochverfügbarkeit von SAP HANA auf Azure-VMs unter RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability-rhel), um die SAP HANA-Clusterressourcentimeouts an den Empfehlungen für Red Hat-Timeouts auszurichten.
- 17.1.2020: Änderung in [Azure-Näherungsplatzierungsgruppen für optimale Netzwerklatenz mit SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios) zur Änderung des Abschnitts der Verschiebung vorhandener VMs in eine Näherungsplatzierungsgruppe.
- 17.1.2020: Änderung in [SAP-Workloadkonfigurationen mit Azure-Verfügbarkeitszonen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones), um auf die Prozedur zu verweisen, mit der Latenzmessungen zwischen Verfügbarkeitszonen automatisiert werden.
- 16.1.2020: Ändern in [Vorgehensweise: Installieren und Konfigurieren von SAP Hana (Large Instances) in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation) zum Anpassen von Betriebssystemreleases an das Hana-IaaS-Hardwareverzeichnis.
- 16.1.2020: Änderungen in [Hochverfügbarkeit von SAP NetWeaver auf Azure-VMs für SLES Multi-SID: Leitfaden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) zum Hinzufügen von Anweisungen für SAP-Systeme unter Verwendung der ENSA2-Architektur (Enqueue Server 2).
- 10.1.2020: Änderungen wurden an den Artikeln [Bereitstellen eines Systems für horizontale SAP HANA-Skalierung mit Standbyknoten auf Azure-VMs mithilfe von Azure NetApp Files auf SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) und [Bereitstellen eines Systems für horizontale SAP HANA-Skalierung mit Standbyknoten auf Azure-VMs mithilfe von Azure NetApp Files auf Red Hat Enterprise Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel) vorgenommen, um Anweisungen zum permanenten Durchführen von `nfs4_disable_idmapping`-Änderungen hinzuzufügen.
- 10.1.2020: Änderungen wurden an den Artikeln [Hochverfügbarkeit für SAP NetWeaver auf Azure-VMs unter SUSE Linux Enterprise Server mit Azure NetApp Files für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) und [Hochverfügbarkeit von Azure Virtual Machines für SAP NetWeaver unter Red Hat Enterprise Linux mit Azure NetApp Files für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) vorgenommen, um Anweisungen zum Einbinden von Azure NetApp Files-NFSv4-Volumes hinzuzufügen.
- 23.12.2019: Veröffentlichung von [Hochverfügbarkeit virtueller Azure-Computer für SAP NetWeaver unter SLES: Multi-SID-Leitfaden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid)
- 18.12.2019: Veröffentlichung von [Horizontale SAP HANA-Skalierung mit Standbyknoten auf Azure-VMs mit Azure NetApp Files auf RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)
- 21.11.2019: Änderungen an [Horizontale SAP HANA-Skalierung mit Standbyknoten auf Azure-VMs mit Azure NetApp Files auf SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) zum Vereinfachen der Konfiguration für NFS-ID-Zuordnung und Ändern der empfohlenen primäre Netzwerkschnittstelle, um das Routing zu vereinfachen.
- 15.11.2019: Kleinere Änderungen an [Hochverfügbarkeit für SAP NetWeaver unter SUSE Linux Enterprise Server mit Azure NetApp Files für SAP-Anwendungen](high-availability-guide-suse-netapp-files.md) und [Hochverfügbarkeit für SAP NetWeaver unter Red Hat Enterprise Linux mit Azure NetApp Files für SAP-Anwendungen](high-availability-guide-rhel-netapp-files.md) zum Klarstellen der Kapazitätseinschränkungen bei der Poolgröße und Entfernen der Aussage, dass nur die NFSv3-Version unterstützt wird.
- 12.11.2019: Freigabe der [Hochverfügbarkeit für SAP NetWeaver unter Windows mit Azure NetApp Files (SMB)](high-availability-guide-windows-netapp-files-smb.md)
- 08.11.2019: Änderungen an [Hochverfügbarkeit von SAP HANA auf virtuellen Azure-Computern auf SUSE Linux Enterprise Server](sap-hana-high-availability.md), [Einrichten der SAP HANA-Systemreplikation auf virtuellen Azure-Computern (VMs)](sap-hana-high-availability-rhel.md)[Hochverfügbarkeit von Azure Virtual Machines für SAP NetWeaver auf SUSE Linux Enterprise Server für SAP-Anwendungen](high-availability-guide-suse.md), [Hochverfügbarkeit von Azure Virtual Machines für SAP NetWeaver auf SUSE Linux Enterprise Server mit Azure NetApp Files](high-availability-guide-suse-netapp-files.md), [Hochverfügbarkeit von Azure Virtual Machines für SAP NetWeaver auf Red Hat Enterprise Linux](high-availability-guide-rhel.md), [Hochverfügbarkeit von Azure Virtual Machines für SAP NetWeaver auf Red Hat Enterprise Linux mit Azure NetApp Files](high-availability-guide-rhel-netapp-files.md), [Hochverfügbarkeit für NFS auf virtuellen Azure-Computern auf SUSE Linux Enterprise Server](high-availability-guide-suse-nfs.md), [GlusterFS auf virtuellen Azure-Computern auf Red Hat Enterprise Linux für SAP NetWeaver](high-availability-guide-rhel-glusterfs.md), um Azure Load Balancer Standard zu empfehlen  
- 08.11.2019: Änderungen in der [Prüfliste für SAP-Workloadplanung und -bereitstellung](sap-deployment-checklist.md) zur Verdeutlichung der Verschlüsselungsempfehlung  
- 04.11.2019: Änderungen beim [Einrichten von Pacemaker unter SUSE Linux Enterprise Server in Azure](high-availability-guide-suse-pacemaker.md) zum direkten Erstellen des Clusters mit Unicast-Konfiguration  
- 29.10.2019: Veröffentlichung von [Konnektivität öffentlicher Endpunkte für VMs, die Azure Load Balancer Standard in SAP-Hochverfügbarkeitsszenarien verwenden](high-availability-guide-standard-load-balancer-outbound-connections.md)
- 25.10.2019: Änderungen in [SAP HANA: Speicherkonfigurationen für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) und [Horizontale SAP HANA-Skalierung mit Standbyknoten auf Azure-VMs mit Azure NetApp Files auf SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse), um das NFS-Protokoll für das Volume /hana/shared zu verdeutlichen
- 22.10.2019: Änderung in [Hochverfügbarkeit für SAP NetWeaver auf Azure-VMs unter SUSE Linux Enterprise Server für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse), [Hochverfügbarkeit für SAP NetWeaver auf Azure-VMs unter SUSE Linux Enterprise Server mit Azure NetApp Files für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files), [Hochverfügbarkeit für NFS auf Azure-VMs unter SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs), [Einrichten von Pacemaker unter SUSE Linux Enterprise Server in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker), [Hochverfügbarkeit von IBM DB2 LUW auf Azure-VMs unter SUSE Linux Enterprise Server mit Pacemaker](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm) und [Hochverfügbarkeit von SAP HANA auf Azure-VMs unter SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability) für Azure Load Balancer-Erkennungshärtung
- Änderungen im ANF-Abschnitt und im Header-Abschnitt in [SAP HANA: Speicherkonfigurationen für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)
- 21.10.2019: Veröffentlichung von [Horizontale SAP HANA-Skalierung mit Standbyknoten auf Azure-VMs mit Azure NetApp Files auf SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)
- 16.10.2019: Korrektur fehlerhafter Links in [Sichern und Wiederherstellen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-backup-restore)
- 16.10.2019: Änderung des mindestens empfohlenen Betriebssystems von SLES 12 SP3 in SLES 12 SP4 in [Hochverfügbarkeit von IBM Db2 LUW auf Azure-VMs unter SUSE Linux Enterprise Server mit Pacemaker](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm)
- 11.10.2019: Änderungen an Disk Storage Ultra-Speicherkonfigurationen und Einführung von ANF in [SAP HANA: Speicherkonfigurationen für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)
- 01.10.2019: Änderung in der Abbildung [Azure-Näherungsplatzierungsgruppen für optimale Netzwerklatenz mit SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios), um eine klarere Darstellung zu erzielen.
- 01.10.2019: Änderung der [SAP HANA-Infrastrukturkonfigurationen und -Vorgänge in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations), um Aussagen über hochverfügbare NFS-Freigabe für /hana/shared zu korrigieren. 
- 28.09.2019: Änderung in [Einrichten von Pacemaker unter Red Hat Enterprise Linux in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) zur Verdeutlichung, dass SBD als Fencingmechanismus nicht von RHEL-Clustern unterstützt wird.  
- 17.09.2019: Änderung im NetWeaver-Planungs- und Bereitstellungsleitfaden, um die Begriffe im Zusammenhang mit der VM-Erweiterung für SAP zu vereinheitlichen  
- 22.08.2019: Änderungen beim [Einrichten von Pacemaker unter SUSE Linux Enterprise Server in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) zum Aktualisieren der URLs für die benutzerdefinierte Rollenerstellung  
- 16.08.2019: Änderungen an [Einrichten von Pacemaker unter Red Hat Enterprise Linux in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker), um Kunden daran zu erinnern, dass sie die Aktionen in der benutzerdefinierten Rolle aktualisieren müssen, wenn sie auf die neue Version des Azure Fence-Agents aktualisiert haben  
- 15.08.2019: Änderungen an [SAP HANA: Speicherkonfigurationen für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage), um die allgemeine Verfügbarkeit von Ultra-Datenträgern (früher SSD Ultra) zu berücksichtigen
- 01.08.2019: Änderungen an [Einrichtung von Pacemaker auf SuSE Linux Enterprise Server in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) zum Integrieren von Änderungen speziell für SLES 15 


