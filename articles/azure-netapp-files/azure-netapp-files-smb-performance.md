---
title: Häufig gestellte Fragen zur Leistung von SMB für Azure NetApp Files | Microsoft-Dokumentation
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zur Leistung von SMB für Azure NetApp Files.
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
ms.date: 03/17/2020
ms.author: b-juche
ms.openlocfilehash: 24b3710861f0ee158619ae9103584dcdb181f3d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460448"
---
# <a name="faqs-about-smb-performance-for-azure-netapp-files"></a>Häufig gestellte Fragen zur Leistung von SMB für Azure NetApp Files

In diesem Artikel werden häufig gestellte Fragen zu bewährten Methoden für mehr Leistung von SMB für Azure NetApp Files beantwortet.

## <a name="is-smb-multichannel-enabled-in-smb-shares"></a>Ist SMB Multichannel bei SMB-Freigaben aktiviert? 

Ja, SMB Multichannel ist standardmäßig aktiviert, eine Änderung, die Anfang Januar 2020 erfolgt ist. Bei allen SMB-Freigaben, die vor vorhandenen SMB-Volumes erstellt wurden, wurde das Feature aktiviert, und bei allen neu erstellten Volumes ist das Feature zum Zeitpunkt der Erstellung ebenfalls aktiviert. 

Alle SMB-Verbindungen, die vor der Featureaktivierung erstellt wurden, müssen zurückgesetzt werden, um die Vorteile der SMB Multichannel-Funktionalität nutzen zu können. Zum Zurücksetzen können Sie die Verbindung mit der SMB-Freigabe trennen und wiederherstellen.

## <a name="is-rss-supported"></a>Wird RSS unterstützt?

Ja, Azure NetApp Files unterstützt die empfangsseitige Skalierung (Receive-Side Scaling, RSS).

Wenn SMB Multichannel aktiviert ist, stellt ein SMB3-Client mehrere TCP-Verbindungen mit dem SMB-Server von Azure NetApp Files über eine Netzwerkschnittstellenkarte (Network Interface Card, NIC) her, die Einzel-RSS-fähig ist. 

## <a name="which-windows-versions-support-smb-multichannel"></a>Welche Windows-Versionen unterstützen SMB Multichannel?

Windows unterstützt SMB Multichannel seit Windows 2012, um eine optimale Leistung zu erzielen.  Weitere Informationen finden Sie unter [Bereitstellen von SMB Multichannel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3Dws.11)) und [Grundlagen von SMB Multichannel](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/). 


## <a name="does-my-azure-virtual-machine-support-rss"></a>Unterstützt mein virtueller Azure-Computer RSS?

Wenn Sie wissen möchten, ob die NICs Ihres virtuellen Azure-Computers RSS unterstützen, führen Sie den Befehl `Get-SmbClientNetworkInterface` wie folgt aus, und überprüfen Sie das Feld `RSS Capable`: 

![RSS-Unterstützung für virtuelle Azure-Computer](../media/azure-netapp-files/azure-netapp-files-formance-rss-support.png)

## <a name="does-azure-netapp-files-support-smb-direct"></a>Unterstützt Azure NetApp Files SMB Direct?

Nein, Azure NetApp Files unterstützt SMB Direct nicht. 

## <a name="what-is-the-benefit-of-smb-multichannel"></a>Welche Vorteile bietet SMB Multichannel? 

Das SMB Multichannel-Feature ermöglicht einem SMB3-Client das Einrichten eines Pools von Verbindungen über eine einzelne Netzwerkschnittstellenkarte (NIC) oder über mehrere NICs und deren Verwendung zum Senden von Anforderungen für eine einzelne SMB-Sitzung. Im Gegensatz dazu erfordern SMB1 und SMB2, dass der Client eine Verbindung herstellt und den gesamten SMB-Datenverkehr für eine Sitzung über diese Verbindung sendet. Diese Einzelverbindung schränkt die allgemeine Protokollleistung ein, die mit einem einzelnen Client erreicht werden kann.

## <a name="should-i-configure-multiple-nics-on-my-client-for-smb"></a>Sollte ich mehrere NICs auf meinem Client für SMB konfigurieren?

Nein. Der SMB-Client passt die Anzahl der NICs an die vom SMB-Server zurückgegebene Anzahl an.  Auf jedes Speichervolume kann nur von genau einem einzigen Speicherendpunkt aus zugegriffen werden.  Das bedeutet, dass für jede SMB-Beziehung nur eine NIC verwendet wird.  

Wie die Ausgabe von `Get-SmbClientNetworkInterace` unten zeigt, verfügt der virtuelle Computer über zwei Netzwerkschnittstellen: 15 und 12.  Wie unter dem Befehl `Get-SmbMultichannelConnection` gezeigt, wird trotz zweier RSS-fähiger NICs nur die Schnittstelle 12 in Verbindung mit der SMB-Freigabe verwendet. Schnittstelle 15 wird nicht verwendet.

![RSS-fähige NICs](../media/azure-netapp-files/azure-netapp-files-rss-capable-nics.png)

## <a name="is-nic-teaming-supported-in-azure"></a>Wird NIC-Teaming von Azure unterstützt?

NIC-Teaming wird von Azure nicht unterstützt. Auch wenn auf virtuellen Azure-Computern mehrere Netzwerkschnittstellen unterstützt werden, stellen sie eher ein logisches als ein physisches Konstrukt dar. Daher bieten sie keine Fehlertoleranz.  Außerdem wird die Bandbreite, die für einen virtuellen Azure-Computer verfügbar ist, für den Computer selbst und nicht für einzelne Netzwerkschnittstellen berechnet.

## <a name="whats-the-performance-like-for-smb-multichannel"></a>Wie hoch ist die Leistung bei SMB Multichannel?

Die folgenden Tests und Diagramme veranschaulichen die Leistung von SMB Multichannel bei Einzelinstanzworkloads.

### <a name="random-io"></a>Zufällige E/A  

Auf dem Client war SMB Multichannel deaktiviert, während die reinen 8-KiB-Lese- und -Schreibtests mit FIO und einem 40-GiB-Arbeitssatz durchgeführt wurden.  Die SMB-Freigabe wurde zwischen den einzelnen Tests getrennt, wobei die Anzahl der SMB-Clientverbindungen pro RSS-Netzwerkschnittstellen in Schritten zwischen den Einstellungen `1`, `4`, `8`, `16` und `set-SmbClientConfiguration -ConnectionCountPerRSSNetworkInterface <count>` erhöht wurde. Die Tests zeigen, dass die Standardeinstellung `4` für E/A-intensive Workloads ausreichend ist. Das Erhöhen auf `8` und `16` hatte keine Auswirkungen. 

Der Befehl `netstat -na | findstr 445` belegte, dass bei den Schritten von `1` auf `4` auf `8` und auf `16` zusätzliche Verbindungen hergestellt wurden.  Bei jedem Test wurden vier CPU-Kerne vollständig für SMB genutzt, was auch von der Statistik `Per Processor Network Activity Cycles` von perfmon bestätigt wurde (nicht in diesem Artikel enthalten).

![Tests mit zufälligen E/A](../media/azure-netapp-files/azure-netapp-files-random-io-tests.png)

Der virtuelle Azure-Computer wirkt sich nicht auf die Speicher-E/A-Limits von SMB (und NFS) aus.  Wie unten gezeigt, ist beim Instanztyp D16 die Rate für Speicher-IOPS mit Zwischenspeicherung auf 32.000 und für Speicher-IOPS ohne Zwischenspeicherung auf 25.600 beschränkt.  Das obige Diagramm zeigt jedoch deutlich mehr E/A-Vorgänge über SMB.

![Vergleich mit zufälligen E/A](../media/azure-netapp-files/azure-netapp-files-random-io-tests-list.png)

### <a name="sequential-io"></a>Sequenzielle E/A 

Auch mit sequenziellen 64-KiB-E/A wurden Tests durchgeführt, die den oben beschriebenen Tests mit zufälligen E/A ähneln. Obwohl die Zunahme der Anzahl der Clientverbindungen pro RSS-Netzwerkschnittstelle über 4 hinaus keine merkliche Auswirkung auf zufällige E/A hatte, gilt dies nicht für die sequenzielle E/A. Wie im folgenden Diagramm dargestellt, führt jede Erhöhung zu einer entsprechenden Steigerung des Lesedurchsatzes. Der Schreibdurchsatz blieb aufgrund der Einschränkungen der Netzwerkbandbreite durch Azure für jeden Instanztyp und jede Instanzgröße gleich. 

![Tests mit sequenziellen E/A](../media/azure-netapp-files/azure-netapp-files-sequential-io-tests.png)

Azure legt für jeden Typ/jede Größe von virtuellen Computern Limits für die Netzwerkrate fest. Das Ratenlimit wird nur auf ausgehenden Datenverkehr angewandt. Die Anzahl der auf einem virtuellen Computer vorhandenen NICs hat keinen Einfluss auf die Gesamtbandbreite, die für den Computer verfügbar ist.  Für den Instanztyp D16 wird beispielsweise ein Netzwerklimit von 8.000 MBit/s (1.000 MiB/s) erzwungen.  Wie das Diagramm für sequenzielle E/A oben zeigt, wirkt sich das Limit auf den ausgehenden Datenverkehr (Schreibvorgänge), aber nicht auf Multichannel-Lesevorgänge aus.

![Vergleich mit sequenziellen E/A](../media/azure-netapp-files/azure-netapp-files-sequential-io-tests-list.png)

## <a name="is-accelerated-networking-recommended"></a>Wird beschleunigter Netzwerkbetrieb empfohlen?

Um die maximale Leistung zu erzielen, empfiehlt es sich, nach Möglichkeit [Beschleunigter Netzwerkbetrieb](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) zu konfigurieren. Beachten Sie dabei Folgendes:  

* Das Azure-Portal aktiviert „Beschleunigter Netzwerkbetrieb“ standardmäßig für virtuelle Computer, die dieses Feature unterstützen.  Bei anderen Bereitstellungsmethoden, wie z. B. Ansible und ähnliche Konfigurationstools, erfolgt dies jedoch möglicherweise nicht.  Wenn „Beschleunigter Netzwerkbetrieb“ nicht aktiviert wird, kann die Leistung eines Computers beeinträchtigt werden.  
* Wenn „Beschleunigter Netzwerkbetrieb“ für die Netzwerkschnittstelle eines virtuellen Computers nicht aktiviert ist (aufgrund des Instanztyps oder der Größe), bleibt dieses Feature auch bei größeren Instanztypen deaktiviert. In diesen Fällen ist ein manueller Eingriff erforderlich.

## <a name="are-jumbo-frames-supported"></a>Werden Großrahmen unterstützt?

Großrahmen werden für virtuelle Azure-Computer nicht unterstützt.

## <a name="is-smb-signing-supported"></a>Wird SMB Signing unterstützt? 

Das SMB-Protokoll stellt die Grundlage für die Datei- und Druckfreigabe sowie andere Netzwerkvorgänge wie die Windows-Remoteverwaltung dar. Um Man-in-the-Middle-Angriffe zu verhindern, die SMB-Pakete während der Übertragung ändern, unterstützt das SMB-Protokoll das digitale Signieren von SMB-Paketen. 

SMB Signing wird für alle SMB-Protokollversionen unterstützt, die von Azure NetApp Files unterstützt werden. 

## <a name="what-is-the-performance-impact-of-smb-signing"></a>Wie wirkt sich die Verwendung von SMB Signing auf die Leistung aus?  

SMB Signing wirkt sich negativ auf die SMB-Leistung aus. Neben anderen möglichen Ursachen von Leistungsminderungen beansprucht das digitale Signieren jedes Pakets eine zusätzliche clientseitige CPU, wie die folgende perfmon-Ausgabe zeigt. In diesem Fall ist Kern 0 für SMB verantwortlich, also auch für SMB Signing.  Ein Vergleich mit den Zahlen für sequenzielle Lesevorgänge ohne Multichannel im vorherigen Abschnitt zeigt, dass SMB Signing den Gesamtdurchsatz von 875 MiB/s auf ungefähr 250 MiB/s senkt. 

![Auswirkung auf die Leistung durch SMB Signing](../media/azure-netapp-files/azure-netapp-files-smb-signing-performance.png)


## <a name="next-steps"></a>Nächste Schritte  

- [Häufig gestellte Fragen zu Azure NetApp Files](azure-netapp-files-faqs.md)
- Weitere Informationen zur Verwendung von SMB-Dateifreigaben mit Azure NetApp Files finden Sie unter [Azure NetApp Files: Managed Enterprise File Shares for SMB Workloads](https://cloud.netapp.com/hubfs/Resources/ANF%20SMB%20Quickstart%20doc%20-%2027-Aug-2019.pdf?__hstc=177456119.bb186880ac5cfbb6108d962fcef99615.1550595766408.1573471687088.1573477411104.328&__hssc=177456119.1.1573486285424&__hsfp=1115680788&hsCtaTracking=cd03aeb4-7f3a-4458-8680-1ddeae3f045e%7C5d5c041f-29b4-44c3-9096-b46a0a15b9b1) (Azure NetApp Files: verwaltete Unternehmensdateifreigaben für SMB-Workloads).
