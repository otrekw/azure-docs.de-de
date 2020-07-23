---
title: include file
description: include file
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 07/08/2020
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 60012f79c3c04a4ff14c4a7f0609b6940d3402c4
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86544479"
---
**Anforderungen an den Konfigurations- und Prozessserver**


## <a name="hardware-requirements"></a>Hardwareanforderungen

**Komponente** | **Anforderung** 
--- | ---
CPU-Kerne | 8 
RAM | 16 GB
Anzahl der Datenträger | 3, einschließlich Betriebssystem-Datenträger, Prozessservercache-Datenträger und Aufbewahrungslaufwerk für Failback 
Freier Speicherplatz (Prozessservercache) | 600 GB
Freier Speicherplatz (Aufbewahrungslaufwerk) | 600 GB
 | 

## <a name="software-requirements"></a>Softwareanforderungen

**Komponente** | **Anforderung** 
--- | ---
Betriebssystem | Windows Server 2012 R2 <br> Windows Server 2016
Gebietsschema des Betriebssystems | Englisch (en-*)
Windows Server-Rollen | Aktivieren Sie die folgenden Rollen nicht: <br> - Active Directory Domain Services <br>- Internetinformationsdienste <br> - Hyper-V 
Gruppenrichtlinien | Aktivieren Sie die folgenden Gruppenrichtlinien nicht: <br> - Zugriff auf Eingabeaufforderung verhindern <br> - Zugriff auf Programme zum Bearbeiten der Registrierung verhindern <br> - Vertrauenslogik für Dateianlagen <br> - Skriptausführung aktivieren <br> [Weitere Informationen](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | - Keine bereits vorhandene Standardwebsite <br> - Keine bereits vorhandene Website/Anwendung sollte an Port 443 lauschen <br>- Die [anonyme Authentifizierung](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) ist aktiviert. <br> - Aktivieren der Einstellung [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) 
FIPS (Federal Information Processing Standard) | FIPS-Modus nicht aktivieren
|

## <a name="network-requirements"></a>Netzwerkanforderungen

**Komponente** | **Anforderung** 
--- | --- 
Art der IP-Adresse | statischen 
Ports | 443 (Steuerkanalorchestrierung)<br>9443 (Datentransport) 
NIC-Typ | VMXNET3 (wenn der Konfigurationsserver eine VMware-VM ist)
 |
**Internetzugriff** (der Server benötigt Zugriff auf diese URLs – direkt oder über einen Proxy):|
\*.backup.windowsazure.com | Wird für die Übertragung und Koordinierung replizierter Daten verwendet
\*.blob.core.windows.net | Wird für den Zugriff auf das Speicherkonto verwendet, in dem replizierte Daten gespeichert werden. Sie können die spezifische URL Ihres Cachespeicherkontos bereitstellen.
\*.hypervrecoverymanager.windowsazure.com | Für Vorgänge und Koordinierung der Replikationsverwaltung verwendet
https:\//login.microsoftonline.com | Für Vorgänge und Koordinierung der Replikationsverwaltung verwendet 
time.nist.gov | Wird zum Überprüfen der Zeitsynchronisierung zwischen der Systemzeit und der globalen Zeit verwendet
time.windows.com | Wird zum Überprüfen der Zeitsynchronisierung zwischen der Systemzeit und der globalen Zeit verwendet
| <ul> <li> https:\//management.azure.com </li><li> https:\//secure.aadcdn.microsoftonline-p.com </li><li> https:\//login.live.com </li><li> https:\//graph.windows.net </li><li> https:\//login.windows.net </li><li> *.services.visualstudio.com (optional) </li><li> https:\//www.live.com </li><li> https:\//www.microsoft.com </li></ul> | Bei der OVF-Einrichtung ist Zugriff auf diese zusätzlichen URLs erforderlich. Die URLs werden für die Zugriffssteuerung und die Identitätsverwaltung durch Azure Active Directory verwendet.
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi  | So schließen Sie den MySQL-Download ab </br> In einigen Regionen wird der Download möglicherweise an die CDN-URL umgeleitet. Stellen Sie ggf. sicher, dass die CDN-URL auch in der Whitelist enthalten ist.
|

> [!NOTE]
> Wenn eine [Private Link-Verbindung](../articles/site-recovery/hybrid-how-to-enable-replication-private-endpoints.md) mit einem Site Recovery-Tresor besteht, benötigen Sie keinen zusätzlichen Internetzugriff für den Konfigurationsserver. Es gibt jedoch eine Ausnahme: Wenn Sie den CS-Computer mithilfe einer OVA-Vorlage einrichten, benötigen Sie neben Private Link-Zugriff auch Zugriff auf die folgenden URLs: https://management.azure.com, https://www.live.com und https://www.microsoft.com. Wenn Sie den Zugriff auf diese URLs nicht zulassen möchten, richten Sie den CS-Computer mithilfe von Unified Installer ein.

## <a name="required-software"></a>Erforderliche Software

**Komponente** | **Anforderung** 
--- | ---
VMware vSphere PowerCLI | [PowerCLI Version 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) sollte installiert werden, wenn der Konfigurationsserver auf einer VMware-VM ausgeführt wird.
MYSQL | MySQL sollte installiert sein. Sie können es manuell installieren oder durch Site Recovery installieren lassen. (Weitere Informationen finden Sie unter [Konfigurieren von Einstellungen](../articles/site-recovery/vmware-azure-deploy-configuration-server.md#configure-settings)).
|

## <a name="sizing-and-capacity-requirements"></a>Anforderungen an Größenanpassung und Kapazität

Die folgende Tabelle enthält die Kapazitätsanforderungen für den Konfigurationsserver. Wenn Sie mehrere virtuelle VMware-Computer replizieren, lesen Sie sich den Artikel [Überlegungen zur Kapazitätsplanung](../articles/site-recovery/site-recovery-plan-capacity-vmware.md) durch, und führen Sie das Tool [Azure Site Recovery-Bereitstellungsplaner](../articles/site-recovery/site-recovery-deployment-planner.md) aus.


**CPU** | **Memory** | **Cachedatenträger** | **Datenänderungsrate** | **Replizierte Computer**
--- | --- | --- | --- | ---
8 vCPUs<br/><br/> 2 Sockets * 4 Kerne \@ 2,5GHz | 16 GB | 300 GB | 500 GB oder weniger | < 100 Computer
12 vCPUs<br/><br/> 2 Sockets * 6 Kerne \@ 2,5GHz | 18 GB | 600 GB | 500 GB bis 1 TB | 100 bis 150 Computer
16 vCPUs<br/><br/> 2 Sockets * 8 Kerne \@ 2,5GHz | 32 GB | 1 TB | 1–2 TB | 150–200 Computer
|

