---
title: Metriken für Azure NetApp Files | Microsoft-Dokumentation
description: 'Azure NetApp Files verfügt über Metriken für Folgendes: zugeteilter Speicher, tatsächliche Speichernutzung, IOPS pro Volume und Latenz. Anhand dieser Metriken können Sie Verbrauch und Leistung analysieren.'
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
ms.openlocfilehash: 7e6ab90010d4379c1640f73a8deeba874e601daf
ms.sourcegitcommit: 29400316f0c221a43aff3962d591629f0757e780
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2020
ms.locfileid: "87513807"
---
# <a name="metrics-for-azure-netapp-files"></a>Metriken für Azure NetApp Files

Azure NetApp Files verfügt über Metriken für Folgendes: zugeteilter Speicher, tatsächliche Speichernutzung, IOPS pro Volume und Latenz. Durch die Analyse dieser Metriken können Sie ein besseres Verständnis des Nutzungsmusters und der Volumeleistung Ihrer NetApp-Konten entwickeln.  

## <a name="usage-metrics-for-capacity-pools"></a><a name="capacity_pools"></a>Nutzungsmetriken für Kapazitätspools

<!-- 
- *Pool Provisioned Size*  
    The logical space (GiB) the capacity pool is provisioned with.  
    This size is the size you selected during capacity pool creation. 
--> 
- *Zu Volumegröße zugeordneter Pool*  
    Die Gesamtsumme des Volumekontingents (GiB) eines bestimmten Kapazitätspools (Gesamtsumme der bereitgestellten Größen der Volumes im Kapazitätspool)  
    Diese Größe haben Sie beim Erstellen des Volumes ausgewählt.  
- *Vom Pool genutzte Größe*  
    Die Gesamtsumme des logischen Speichers (GiB), der in einem Kapazitätspool volumeübergreifend genutzt wird.  
<!-- 
- *Pool Consumed Snapshot Size*  
    The total of logical space (GiB) used by snapshots across all volumes in a capacity pool. 
-->

## <a name="usage-metrics-for-volumes"></a><a name="volumes"></a>Nutzungsmetriken für Volumes

<!--
- *Volume Quota Size*    
    The quota size (GiB) the volume is provisioned with.   
    This size is the size you selected during capacity pool creation. 
-->
- *Vom Volume genutzte Größe*   
    Der gesamte logische Speicher (GiB), der auf einem Volume genutzt wird.  
    In diesem Größenwert ist der logische Speicherplatz enthalten, der von aktiven Dateisystemen und Momentaufnahmen verwendet wird.  
- *Größe der Volumemomentaufnahme*   
   Der inkrementelle logische Speicherplatz, der von Momentaufnahmen auf einem Volume belegt wird.  

## <a name="performance-metrics-for-volumes"></a>Leistungsmetriken für Volumes

- *AverageReadLatency*   
    Die durchschnittliche Dauer von Lesevorgängen vom Volume in Millisekunden.
- *AverageWriteLatency*   
    Die durchschnittliche Dauer von Schreibvorgängen vom Volume in Millisekunden.
- *ReadIops*   
    Die Anzahl der Lesevorgänge auf dem Volume pro Sekunde.
- *WriteIops*   
    Die Anzahl der Schreibvorgänge auf dem Volume pro Sekunde.

## <a name="next-steps"></a>Nächste Schritte

* [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)
* [Erstellen eines Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
