---
title: Agent-basierte Migration bei der Azure Migrate-Servermigration
description: Bietet eine Übersicht über die Agent-basierte VMware VM-Migration in Azure Migrate.
author: rahulg1190
ms.author: rahugup
ms.manager: bsiva
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: f4f79725d0eda65ba00a44e9e7fc2a51c024eccf
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104864056"
---
# <a name="agent-based-migration-architecture"></a>Agent-basierte Migrationsarchitektur

Dieser Artikel gibt einen Überblick über die Architektur und die Prozesse, die für die Agent-basierte Replikation von virtuellen VMware-Computern mit dem folgenden Tool verwendet werden: [Azure Migrate: Servermigration](migrate-services-overview.md#azure-migrate-server-migration-tool).

Verwenden von Azure Migrate: Servermigration – Sie können virtuelle VMware-Computer mit einer Reihe von Optionen replizieren:

- Migration von virtuellen Computern mit Agent-basierter Replikation, wie in diesem Artikel beschrieben.
- Migration von virtuellen VMware-Computern ohne Agents. Dabei werden virtuelle Computer migriert, ohne dass darauf Software installiert werden muss.

Erfahren Sie mehr über das [Auswählen und Vergleichen](server-migrate-overview.md) von Migrationsmethoden für virtuelle VMware-Computer. 


## <a name="agent-based-migration"></a>Agent-basierte Migration

Die Agent-basierte Migration wird zum Migrieren von lokalen virtuellen VMware-Computern und physischen Servern in Azure verwendet. Sie kann auch zur Migration anderer lokaler virtueller Server sowie virtueller Computer (VMs) in privaten und öffentlichen Clouds (einschließlich AWS-Instanzen und GCP-VMs) verwendet werden. Die Agent-basierte Migration in Azure Migrate verwendet einige Back-End-Funktionen des [Azure Site Recovery](../site-recovery/site-recovery-overview.md)-Diensts.


## <a name="architectural-components"></a>Komponenten der Architektur

Das Diagramm veranschaulicht die Komponenten, die bei der Agent-basierten Migration beteiligt sind.

![Das Diagramm zeigt die Komponenten für die Agent-basierte Migration, die in einer Tabelle erläutert werden.](./media/agent-based-replication-architecture/architecture.png)

In der Tabelle sind die Komponenten zusammengefasst, die für die Agent-basierte Migration verwendet werden.

**Komponente** | **Details** | **Installation**
--- | --- | ---
**Replikationsappliance** | Die Replikationsappliance (Konfigurationsserver/Prozessserver) ist ein lokaler Server, der als Brücke zwischen der lokalen Umgebung und der Servermigration fungiert. Die Appliance erkennt den lokalen Serverbestand, sodass die Servermigration die Replikation und Migration orchestrieren kann. Die Appliance verfügt über zwei Komponenten:<br/><br/> **Konfigurationsserver**: Stellt eine Verbindung mit der Servermigration her und koordiniert die Replikation.<br/> **Prozessserver** Führt die Datenreplikation durch. Der Prozessserver empfängt die Serverdaten, komprimiert und verschlüsselt diese und sendet sie dann an Azure. In Azure schreibt die Servermigration die Daten auf verwaltete Datenträger. | Standardmäßig wird der Prozessserver zusammen mit dem Konfigurationsserver auf der Replikationsappliance installiert.
**Mobilitätsdienst** | Der Mobilitätsdienst ist ein Agent, der auf jedem Server installiert wird, den Sie replizieren und migrieren möchten. Er sendet Replikationsdaten vom Server an den Prozessserver. | Die Installationsdateien für verschiedene Versionen des Mobilitätsdiensts befinden sich auf der Replikationsappliance. Sie laden entsprechend dem Betriebssystem und der Version des zu replizierenden Servers den benötigten Agent herunter und installieren ihn.

## <a name="mobility-service-installation"></a>Installation des Mobilitätsdiensts

Sie können die den Mobilitätsdienst mithilfe der folgenden Methoden bereitstellen:

- **Pushinstallation:** Der Mobilitätsdienst wird vom Prozessserver installiert, wenn Sie den Schutz für einen Server aktivieren. 
- **Manuelle Installation:** Sie können den Mobilitätsdienst über die Benutzeroberfläche oder die Eingabeaufforderung auf jedem Server manuell installieren.

Der Mobilitätsdienst kommuniziert mit der Replikationsappliance und den replizierten Servern. Wenn Sie Antivirensoftware auf der Replikationsappliance, den Prozessservern oder den replizierten Servern ausführen, sollten die folgenden Ordner von der Überprüfung ausgeschlossen werden:


- C:\Programme\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- C:\Programme (x86)\Microsoft Azure Site Recovery
- C:\ProgramData\ASR\agent (auf Windows-Servern mit installiertem Mobilitätsdienst)

## <a name="replication-process"></a>Replikationsprozess

1. Wenn Sie die Replikation für einen virtuellen Server aktivieren, beginnt die erste Replikation nach Azure.
2. Während der ersten Replikation liest der Mobilitätsdienst Daten von den Datenträgern des Servers und sendet diese an den Prozessserver.
3. Diese Daten werden verwendet, um eine Kopie des Datenträgers in Ihrem Azure-Abonnement zu speichern. 
4. Die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Die Replikation findet auf Blockebene und nahezu fortlaufend statt.
4. Der Mobilitätsdienst fängt Schreibzugriffe auf den Datenträgerspeicher ab, indem er in das Speichersubsystem des Betriebssystems integriert wird. Bei dieser Methode werden Datenträger-E/A-Vorgänge auf dem Replikationsserver bei der inkrementellen Replikation vermieden. 
5. Nachverfolgte Änderungen für einen Server werden über den HTTPS-Eingangsport 9443 an den Prozessserver gesendet. Dieser Port kann geändert werden. Der Prozessserver komprimiert und verschlüsselt sie und sendet sie an Azure. 

## <a name="ports"></a>Ports

**Device** | **Connection**
--- | --- 
**Replizieren der Server** | Der Mobility Service-Agent auf den virtuellen Computern kommuniziert mit der lokalen Replikationsappliance über den eingehenden Port HTTPS 443, um die Replikationsverwaltung auszuführen.<br/><br/> Server senden Replikationsdaten über den HTTPS-Port 9443 für eingehenden Datenverkehr an den Prozessserver. Dieser Port kann geändert werden.
**Replikationsappliance** | Die Replikationsappliance orchestriert die Replikation mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
**Prozessserver** | Der Prozessserver empfängt Replikationsdaten, optimiert und verschlüsselt sie und sendet sie über den ausgehenden Port 443 an den Azure-Speicher.


## <a name="performance-and-scaling"></a>Leistung und Skalierung

Standardmäßig wird eine einzelne Replikationsappliance bereitgestellt, die sowohl den Konfigurationsserver als auch den Prozessserver ausführt. Wenn Sie nur wenige Server replizieren, ist diese Bereitstellung ausreichend. Wenn Sie jedoch Hunderte Server replizieren und migrieren, ist ein einzelner Prozessserver möglicherweise nicht in der Lage, den gesamten Replikationsdatenverkehr zu bewältigen. In diesem Fall können Sie zusätzliche Prozessserver für die horizontale Skalierung einsetzen.

### <a name="plan-vmware-deployment"></a>Planen der VMware-Bereitstellung

Wenn Sie VMware-VMs replizieren, können Sie den [Site Recovery-Bereitstellungsplaner](../site-recovery/site-recovery-deployment-planner.md) für VMware verwenden, um die Leistungsanforderungen, einschließlich der täglichen Datenänderungsrate und der benötigten Prozessserver, zu ermitteln.

### <a name="replication-appliance-capacity"></a>Kapazität der Replikationsappliance

Verwenden Sie die Werte in dieser Tabelle, um herauszufinden, ob Sie in Ihrer Bereitstellung einen zusätzlichen Prozessserver benötigen.

- Wenn die tägliche Änderungsrate (Churn Rate) über 2 TB liegt, stellen Sie einen zusätzlichen Prozessserver bereit.
- Wenn Sie mehr als 200 Server replizieren, setzen Sie eine zusätzliche Replikationsappliance ein.

**CPU** | **Memory** | **Freier Speicherplatz für Datenzwischenspeicherung** | **Datenänderungsrate** | **Replikationsgrenzwerte**
--- | --- | --- | --- | ---
8 vCPUs (2 Sockets * 4 Kerne \@ 2,5 GHz) | 16 GB | 300 GB | 500 GB oder weniger | Weniger als 100 Server 
12 vCPUs (2 Sockets * 6 Kerne \@ 2,5 GHz) | 18 GB | 600 GB | 501 GB bis 1 TB | 100 bis 150 Server
16 vCPUs (2 Sockets * 8 Kerne \@ 2,5 GHz) | 32 GB |  1 TB | 1 TB bis 2 TB | 151 bis 200 Server

### <a name="sizing-scale-out-process-servers"></a>Festlegen der Größe für horizontal skalierte Prozessserver

Wenn Sie einen horizontal skalierten Prozessserver bereitstellen müssen, verwenden Sie diese Tabelle, um die Servergröße zu ermitteln.

**Prozessserver** | **Freier Speicherplatz für Datenzwischenspeicherung** | **Datenänderungsrate** | **Replikationsgrenzwerte**
--- | --- | --- | --- 
4 vCPUs (2 Sockets * 2 Kerne \@ 2,5 GHz), 8 GB Arbeitsspeicher | 300 GB | 250 GB oder weniger | Bis zu 85 Server 
8 vCPUs (2 Sockets * 4 Kerne \@ 2,5 GHz), 12 GB Arbeitsspeicher | 600 GB | 251 GB bis 1 TB | 86 bis 150 Server
12 vCPUs (2 Sockets * 6 Kerne \@ 2,5 GHz), 24 GB Arbeitsspeicher | 1 TB | 1–2 TB | 151 bis 225 Server

## <a name="throttle-upload-bandwidth"></a>Drosseln Sie die Bandbreite für den Upload.

VMware-Datenverkehr, der nach Azure repliziert wird, wird über einen speziellen Prozessserver geleitet. Sie können Durchsatz des Uploads einschränken, indem Sie die Bandbreite für die Server drosseln, die als Prozessserver ausgeführt werden. Sie können die Bandbreite mithilfe dieses Registrierungsschlüssels beeinflussen:

- Der Registrierungswert „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM“ gibt die Anzahl von Threads an, die für die Datenübertragung (erste Replikation oder Deltareplikation) eines Datenträgers verwendet werden. Bei einem höheren Wert wird die Netzwerkbandbreite für die Replikation erhöht. Der Standardwert ist 4. Der Höchstwert ist 32. Überwachen Sie den Datenverkehr, um den Wert zu optimieren.
- Darüber hinaus können Sie die Bandbreite der Prozessserver wie folgt drosseln:

    1. Öffnen Sie das Azure Backup-MMC-Snap-In auf dem Prozessserver. Auf dem Desktop oder im Ordner „C:\Programme\Microsoft Azure Recovery Services Agent\bin\“ ist eine Verknüpfung verfügbar. 
    2. Wählen Sie im Snap-In **Eigenschaften ändern** aus.
    3. Wählen Sie unter **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren** aus. Legen Sie die Grenzwerte für Arbeitsstunden und arbeitsfreie Stunden fest. Der gültige Bereich reicht von 512 KBit/s bis 1,023 MBit/s.


## <a name="next-steps"></a>Nächste Schritte

Testen Sie die [Agent-basierte Migration](tutorial-migrate-vmware-agent.md) für [VMware](tutorial-migrate-vmware-agent.md) oder [physische Server](tutorial-migrate-physical-virtual-machines.md).
