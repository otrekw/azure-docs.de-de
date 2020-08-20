---
title: Lösungsarchitekturen mit Azure NetApp Files | Microsoft-Dokumentation
description: Bietet Verweise auf bewährte Methoden für Lösungsarchitekturen unter Verwendung von Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/10/2020
ms.author: b-juche
ms.openlocfilehash: 48e3f895fb123429f1bedd106bd92113533c84ae
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88055801"
---
# <a name="solution-architectures-using-azure-netapp-files"></a>Lösungsarchitekturen mit Azure NetApp Files
Dieser Artikel enthält Verweise auf bewährte Methoden, die Ihnen helfen können, die Lösungsarchitekturen für die Verwendung von Azure NetApp Files zu verstehen.  

Das folgende Diagramm zeigt die Kategorien der Lösungsarchitekturen, die Azure NetApp Files bietet:

![Kategorien der Lösungsarchitekturen](../media/azure-netapp-files/solution-architecture-categories.png)

## <a name="linux-oss-apps-and-database-solutions"></a>Lösungen für Linux OSS-Apps und Datenbanken

In diesem Abschnitt finden Sie Referenzen zu Lösungen für Linux OSS-Anwendungen und Datenbanken. 

### <a name="oracle"></a>Oracle

* [Oracle on Azure deployment best practice guide using Azure NetApp Files](https://www.netapp.com/us/media/tr-4780.pdf) (Oracle auf Azure-Bereitstellung – Leitfaden mit bewährten Methoden unter Verwendung von Azure NetApp Files)
* [Oracle VM-Images und deren Bereitstellung in Microsoft Azure: Konfigurationsoptionen für freigegebenen Speicher](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-vm-solutions#shared-storage-configuration-options)
* [Vorteile der Verwendung von Azure NetApp Files mit Oracle Database](solutions-benefits-azure-netapp-files-oracle-database.md)

## <a name="windows-apps-and-sql-server-solutions"></a>Lösungen für Windows-Apps und SQL Server

In diesem Abschnitt finden Sie Referenzen zu Lösungen für Windows-Anwendungen und SQL Server.

### <a name="file-sharing-and-global-file-caching"></a>Dateifreigaben und globales Dateicaching

* [Build Your Own Azure NFS? Wrestling Linux File Shares into Cloud](https://cloud.netapp.com/blog/ma-anf-blg-build-your-own-linux-nfs-file-shares) (Erstellen eines eigenen NFS: Verschieben von Linux-Dateifreigaben in die Cloud)
* [Globaler Dateicache / Bereitstellen von Azure NetApp Files](https://youtu.be/91LKb1qsLIM)

### <a name="sql-server"></a>SQL Server

* [Deploy SQL Server Over SMB with Azure NetApp Files](https://www.youtube.com/watch?v=x7udfcYbibs) (Bereitstellen von SQL Server über SMB mit Azure NetApp Files)
* [Deploy SQL Server Always-On Failover Cluster over SMB with Azure NetApp Files](https://www.youtube.com/watch?v=zuNJ5E07e8Q) (Bereitstellen von SQL Server-Always-On-Failoverclustern über SMB mit Azure NetApp Files)
* [Deploy Always-On Availability Groups with Azure NetApp Files](https://www.youtube.com/watch?v=y3VQmzzeyvc) (Bereitstellen von Always-On-Verfügbarkeitsgruppen mit Azure NetApp Files)

## <a name="sap-on-azure-solutions"></a>Lösung für SAP in Azure

Dieser Abschnitt enthält Referenzen zu Lösungen für SAP in Azure. 

### <a name="generic-sap-and-sap-netweaver"></a>SAP allgemein und SAP Netweaver 

* [SAP applications on Microsoft Azure using Azure NetApp Files](https://www.netapp.com/us/media/tr-4746.pdf) (SAP-Anwendungen in Microsoft Azure mithilfe von Azure NetApp Files)
* [Hochverfügbarkeit für SAP NetWeaver auf Azure-VMs unter SUSE Linux Enterprise Server mit Azure NetApp Files für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)
* [Hochverfügbarkeit von SAP NetWeaver auf virtuellen Azure-Computern unter Red Hat Enterprise Linux mit Azure NetApp Files für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files)
* [Hochverfügbarkeit von SAP NetWeaver auf virtuellen Azure-Computern unter Windows mit Azure NetApp Files (SMB) für SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-windows-netapp-files-smb)
* [Hochverfügbarkeit für SAP NetWeaver auf virtuellen Azure-Computern unter Red Hat Enterprise Linux für SAP-Anwendungen: Multi-SID-Leitfaden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-multi-sid)

### <a name="sap-hana"></a>SAP HANA 

* [SAP HANA: Speicherkonfigurationen für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)
* [Horizontale SAP HANA-Skalierung mit Standbyknoten auf Azure VMs mit Azure NetApp Files auf SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)
* [Horizontale SAP HANA-Skalierung mit Standbyknoten auf Azure-VMs mithilfe von Azure NetApp Files auf RedHat Enterprise Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)

### <a name="sap-tech-community-and-blog-posts"></a>Technische SAP-Community und Blogbeiträge 

* [Azure NetApp Files – SAP HANA-Sicherung in Sekunden](https://blog.netapp.com/azure-netapp-files-sap-hana-backup-in-seconds/)
* [Azure NetApp Files – Wiederherstellen Ihrer HANA-Datenbank aus einer Momentaufnahmesicherung](https://blog.netapp.com/azure-netapp-files-backup-sap-hana)
* [Azure NetApp Files – SAP HANA-Abladung der Sicherung mit Cloudsynchronisierung](https://blog.netapp.com/azure-netapp-files-sap-hana)
* [Beschleunigen Sie Ihre SAP HANA-Systemkopien mithilfe von Azure NetApp Files](https://blog.netapp.com/sap-hana-faster-using-azure-netapp-files/)
* [Cloudvolumes ONTAP und unter Azure NetApp Files: SAP HANA-Systemmigration leicht gemacht](https://blog.netapp.com/cloud-volumes-ontap-and-azure-netapp-files-sap-hana-system-migration-made-easy/)

## <a name="virtual-desktop-infrastructure-solutions"></a>Lösungen für virtuelle Desktopinfrastrukturen

Dieser Abschnitt enthält Referenzen zu Lösungen für virtuelle Desktopinfrastrukturen.

### <a name="windows-virtual-desktop"></a>Windows Virtual Desktop

* [Speicheroptionen für FSLogix-Profilcontainer in Windows Virtual Desktop](https://docs.microsoft.com/azure/virtual-desktop/store-fslogix-profile#azure-platform-details)
* [Erstellen eines FSLogix-Profilcontainers für einen Hostpool mit Azure NetApp Files](https://docs.microsoft.com/azure/virtual-desktop/create-fslogix-profile-container)
* [Windows Virtual Desktop auf Unternehmensebene](https://docs.microsoft.com/azure/architecture/example-scenario/wvd/windows-virtual-desktop)

## <a name="hpc-solutions"></a>Lösungen für HPC

Dieser Abschnitt enthält Referenzen zu Lösungen für High Performance Computing (HPC). 

### <a name="generic-hpc"></a>HPC allgemein

* [Azure NetApp Files: Getting the most out of your cloud storage](https://cloud.netapp.com/hubfs/Resources/ANF%20PERFORMANCE%20TESTING%20IN%20TEMPLATE.pdf) (Azure NetApp Files: Optimale Nutzung Ihres Cloudspeichers)
* [Ausführen von MPI-Workloads mit Azure Batch und Azure NetApp Files](https://azure.microsoft.com/resources/run-mpi-workloads-with-azure-batch-and-azure-netapp-files/)
* [Azure CycleCloud: CycleCloud- HPC-Umgebungen in Azure NetApp Files](https://docs.microsoft.com/azure/cyclecloud/overview)

### <a name="oil-and-gas"></a>Öl- und Gasanlagen

* [High Performance Computing (HPC): Oil and gas in Azure](https://techcommunity.microsoft.com/t5/azure-global/high-performance-computing-hpc-oil-and-gas-in-azure/ba-p/824926) (High Performance Computing: Lösungen für Öl- und Gasanlagen in Azure)
* [Ausführen von Software zur Reservoirsimulation in Azure](https://docs.microsoft.com/azure/architecture/example-scenario/infrastructure/reservoir-simulation)

### <a name="electronic-design-automation-eda"></a>Electronic Design Automation (EDA)

* [Vorteile der Verwendung von Azure NetApp Files für die elektronische Entwurfsautomatisierung](solutions-benefits-azure-netapp-files-electronic-design-automation.md)
* [Azure CycleCloud: EDA HPC Lab with Azure NetApp Files](https://github.com/Azure/cyclecloud-hands-on-labs/blob/master/EDA/README.md) (Azure CycleCloud: EDA-HPC-Lab mit Azure NetApp Files)

### <a name="analytics"></a>Analytics

* [Azure NetApp Files: A new shared file system to use with SAS Grid on Microsoft Azure](https://communities.sas.com/t5/Architecture/Azure-NetApp-Files-A-new-shared-file-system-to-use-with-SAS-Grid/m-p/606978) (Azure NetApp Files: ein neues Dateifreigabesystem zur Verwendung mit SAS Grid in Microsoft Azure)

## <a name="azure-platform-services-solutions"></a>Lösungen für Azure-Plattformdienste

Dieser Abschnitt enthält Lösungen für Azure-Plattformdienste. 

### <a name="azure-kubernetes-services-and-kubernetes"></a>Azure Kubernetes Services und Kubernetes

* [Integrieren von Azure NetApp Files in Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/azure-netapp-files)
* [Out-of-This-World Kubernetes performance on Azure with Azure NetApp Files](https://cloud.netapp.com/blog/ma-anf-blg-configure-kubernetes-openshift) (Überirdische Leistung von Kubernetes in Azure mit Azure NetApp Files)
* [Trident - Storage Orchestrator for Containers](https://netapp-trident.readthedocs.io/en/stable-v20.04/kubernetes/operations/tasks/backends/anf.html) (Trident: Speicherorchestrator für Container)

### <a name="azure-batch"></a>Azure Batch

* [Ausführen von MPI-Workloads mit Azure Batch und Azure NetApp Files](https://azure.microsoft.com/resources/run-mpi-workloads-with-azure-batch-and-azure-netapp-files/)
 
