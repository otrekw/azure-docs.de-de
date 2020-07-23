---
title: Anbieter von Azure Monitor für SAP-Lösungen | Microsoft-Dokumentation
description: Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Azure Monitor für SAP-Lösungen.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/30/2020
ms.author: radeltch
ms.openlocfilehash: 267d600270e834cf4f1f077452fda7459fac3029
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86525448"
---
# <a name="azure-monitor-for-sap-solutions-providers-preview"></a>Anbieter von Azure Monitor für SAP-Lösungen (Vorschau)

## <a name="overview"></a>Übersicht  

Im Kontext von Azure Monitor für SAP-Lösungen bezieht sich ein *Anbietertyp* auf einen bestimmten *Anbieter*. Beispiel: *SAP HANA*, das für eine bestimmte Komponente innerhalb der SAP-Landschaft konfiguriert ist, wie die SAP HANA-Datenbank. Ein Anbieter enthält die Verbindungsinformationen für die entsprechende Komponente und hilft, Telemetriedaten von dieser Komponente zu sammeln. Eine Ressource von Azure Monitor für SAP-Lösungen (auch als SAP-Monitorressource bezeichnet) kann mit mehreren Anbietern desselben Anbietertyps oder mehreren Anbietern verschiedener Anbietertypen konfiguriert werden.
   
Kunden können verschiedene benutzerdefinierte Anbietertypen konfigurieren, um die Datensammlung aus der entsprechenden Komponente in ihrer SAP-Landschaft zu ermöglichen. Beispielsweise können Kunden einen Anbieter für den Anbietertyp „SAP HANA“, einen anderen Anbieter für den Anbietertyp „Hochverfügbarkeitscluster“ usw. konfigurieren.  

Kunden können auch mehrere Anbieter eines bestimmten Anbietertyps konfigurieren, um dieselbe SAP-Monitorressource und die zugehörige verwaltete Gruppe wiederzuverwenden. Erfahren Sie mehr über die verwaltete Ressourcengruppe. Für die öffentliche Vorschau werden die folgenden Anbietertypen unterstützt:   
- SAP HANA
- Hochverfügbarkeitscluster
- Microsoft SQL Server

![Anbieter von Azure Monitor für SAP-Lösungen](./media/azure-monitor-sap/azure-monitor-providers.png)

Kunden wird empfohlen, zum Zeitpunkt der Bereitstellung der SAP-Monitorressource mindestens einen Anbieter aus den verfügbaren Anbietertypen zu konfigurieren. Durch die Konfiguration eines Anbieters initiieren Kunden die Datenerfassung aus der entsprechenden Komponente, für die der Anbieter konfiguriert ist.   

Wenn Kunden zum Zeitpunkt der Bereitstellung der SAP-Monitorressource keine Anbieter konfigurieren, werden keine Telemetriedaten erfasst, obwohl die SAP-Monitorressource erfolgreich bereitgestellt wird. Kunden haben die Möglichkeit, Anbieter nach der Bereitstellung über die SAP-Monitorressource im Azure-Portal hinzuzufügen. Kunden können jederzeit Anbieter in der SAP-Monitorressource hinzufügen oder löschen.

> [!Tip]
> Wenn Sie möchten, dass Microsoft einen bestimmten Anbieter implementiert, hinterlassen Sie Ihr Feedback über den Link am Ende dieses Dokuments, oder wenden Sie sich an Ihr Kontoteam.  

## <a name="provider-type-sap-hana"></a>Anbietertyp „SAP HANA“

Kunden können einen oder mehrere Anbieter des Anbietertyps *SAP HANA* konfigurieren, um die Datenerfassung aus der SAP HANA-Datenbank zu ermöglichen. Der SAP HANA-Anbieter stellt über den SQL-Port eine Verbindung mit der SAP HANA-Datenbank her, pullt Telemetriedaten aus der Datenbank und pusht sie in den Log Analytics-Arbeitsbereich des Kundenabonnements. Der SAP HANA-Anbieter führt die Erfassung aus der SAP HANA-Datenbank jede Minute durch.  

In der öffentlichen Vorschau können Kunden die folgenden Daten mit dem SAP HANA-Anbieter erwarten: Nutzung der zugrundeliegenden Infrastruktur, Status des SAP HANA-Hosts, SAP HANA-Systemreplikation und Telemetriedaten für SAP HANA-Sicherungen. Zur Konfiguration des SAP HANA-Anbieters sind die Host-IP-Adresse, die HANA SQL-Portnummer und der SYSTEMDB-Benutzername sowie das Kennwort erforderlich. Kunden wird empfohlen, den SAP-HANA-Anbieter für SYSTEMDB zu konfigurieren. Zusätzliche Anbieter können jedoch auch für andere Datenbankmandanten konfiguriert werden.

![Anbieter von Azure Monitor für SAP-Lösungen – SAP HANA](./media/azure-monitor-sap/azure-monitor-providers-hana.png)

## <a name="provider-type-high-availability-cluster"></a>Anbietertyp „Hochverfügbarkeitscluster“
Kunden können einen oder mehrere Anbieter des Anbietertyps *Hochverfügbarkeitscluster* konfigurieren, um die Datenerfassung aus Pacemaker-Clustern innerhalb der SAP-Landschaft zu ermöglichen. Der Anbieter des Hochverfügbarkeitsclusters stellt über den Endpunkt [ha_cluster_exporter](https://github.com/ClusterLabs/ha_cluster_exporter) eine Verbindung mit Pacemaker her, pullt Telemetriedaten aus der Datenbank und pusht sie in den Log Analytics-Arbeitsbereich des Kundenabonnements. Der Anbietertyp „Hochverfügbarkeitscluster“ erfasst alle 60 Sekunden Daten von Pacemaker.  

In der öffentlichen Vorschau können benutzerdefinierte Kunden die folgenden Daten mit Anbietern von Hochverfügbarkeitsclustern erwarten:   
 - Clusterstatus dargestellt als Rollup des Knoten- und Ressourcenstatus 
 - [Sonstige](https://github.com/ClusterLabs/ha_cluster_exporter/blob/master/doc/metrics.md) 

![Anbieter von Azure Monitor für SAP-Lösungen – Hochverfügbarkeitscluster](./media/azure-monitor-sap/azure-monitor-providers-pacemaker-cluster.png)

Die Konfiguration eines Anbieters von Hochverfügbarkeitsclustern erfolgt in zwei primären Schritten: 
1. Installieren von [ha_cluster_exporter](https://github.com/ClusterLabs/ha_cluster_exporter) in *jedem* Knoten innerhalb des Pacemaker-Clusters 
    - Kunden können benutzerdefinierte Azure Automation-Skripts verwenden, um Hochverfügbarkeitscluster bereitzustellen. Die Skripts installieren [ha_cluster_exporter](https://github.com/ClusterLabs/ha_cluster_exporter) auf jedem Clusterknoten.  
    - Oder Kunden können eine manuelle Installation durchführen, indem sie den Schritten auf [dieser Seite](https://github.com/ClusterLabs/ha_cluster_exporter) folgen. 
2. Konfigurieren des Anbieters von Hochverfügbarkeitsclustern in *jedem* Knoten innerhalb des Pacemaker-Clusters  
  Zur Konfiguration des Anbieters von Hochverfügbarkeitsclustern werden die Prometheus-URL, der Clustername, der Hostname und die System-ID benötigt.   
  Kunden wird empfohlen, einen Anbieter pro Clusterknoten zu konfigurieren.   

## <a name="provider-type-microsoft-sql-server"></a>Anbietertyp „Microsoft SQL Server“

Kunden können einen oder mehrere Anbieter des Anbietertyps *Microsoft SQL Server* konfigurieren, um die Datenerfassung von [SQL Server auf virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/) zu ermöglichen. Der SQL Server-Anbieter stellt über den SQL-Port eine Verbindung mit Microsoft SQL Server her, pullt Telemetriedaten aus der Datenbank und pusht sie in den Log Analytics-Arbeitsbereich des Kundenabonnements. Der SQL Server muss für die SQL-Authentifizierung konfiguriert werden, und es muss eine SQL Server-Anmeldung mit der SAP DB als Standarddatenbank für den Anbieter erstellt werden. Der SQL Server-Anbieter erfasst Daten zwischen alle 60 Sekunden bis zu jeder Stunde von SQL Server.  

In der öffentlichen Vorschau können Kunden die folgenden Daten mit dem SQL Server-Anbieter erwarten: Nutzung der zugrunde liegenden Infrastruktur, wichtigste SQL-Anweisungen, größte Tabelle, Aufzeichnung von Problemen in den SQL Server-Fehlerprotokollen, Blockierung von Prozessen und andere.  

Zur Konfiguration des Microsoft SQL Server-Anbieters sind die SAP-System-ID, die Host-IP-Adresse, die SQL Server-Portnummer sowie der SQL Server-Anmeldename und das Kennwort erforderlich.

![Anbieter von Azure Monitor für SAP-Lösungen – SQL](./media/azure-monitor-sap/azure-monitor-providers-sql.png)

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie Ihre erste Ressource für Azure Monitor für SAP-Lösungen.
- Haben Sie Fragen zu Azure Monitor für SAP-Lösungen? Weitere Informationen finden Sie im [FAQ](./azure-monitor-faq.md)-Abschnitt.
