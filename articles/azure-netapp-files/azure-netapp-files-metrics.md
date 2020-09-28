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
ms.date: 09/10/2020
ms.author: b-juche
ms.openlocfilehash: 1690a844ff700a2975be8e972fd90ba71eeb937c
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707780"
---
# <a name="metrics-for-azure-netapp-files"></a>Metriken für Azure NetApp Files

Azure NetApp Files verfügt über Metriken für Folgendes: zugeteilter Speicher, tatsächliche Speichernutzung, IOPS pro Volume und Latenz. Durch die Analyse dieser Metriken können Sie ein besseres Verständnis des Nutzungsmusters und der Volumeleistung Ihrer NetApp-Konten entwickeln.  

## <a name="usage-metrics-for-capacity-pools"></a><a name="capacity_pools"></a>Nutzungsmetriken für Kapazitätspools

- *Zugeordnete Poolgröße*   
    Die bereitgestellte Größe des Pools.

- *Zu Volumegröße zugeordneter Pool*  
    Die Gesamtsumme des Volumekontingents (GiB) eines bestimmten Kapazitätspools (Gesamtsumme der bereitgestellten Größen der Volumes im Kapazitätspool)  
    Diese Größe haben Sie beim Erstellen des Volumes ausgewählt.  

- *Vom Pool genutzte Größe*  
    Die Gesamtsumme des logischen Speichers (GiB), der in einem Kapazitätspool volumeübergreifend genutzt wird.  

- *Gesamtgröße der Momentaufnahme für den Pool*    
    Die Summe der Momentaufnahmegröße aller Volumes in dem Pool.

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

- *Durchschnittliche Wartezeit beim Lesevorgang*   
    Die durchschnittliche Dauer von Lesevorgängen vom Volume in Millisekunden.
- *Durchschnittliche Wartezeit beim Schreibvorgang*   
    Die durchschnittliche Dauer von Schreibvorgängen vom Volume in Millisekunden.
- *Lese-IOPS*   
    Die Anzahl der Lesevorgänge auf dem Volume pro Sekunde.
- *Schreib-IOPS*   
    Die Anzahl der Schreibvorgänge auf dem Volume pro Sekunde.

## <a name="volume-replication-metrics"></a><a name="replication"></a>Metriken für die Volumereplikation

- *Ist Volumereplikationsstatus „Fehlerfrei“*    
    Der Zustand der Replikationsbeziehung. 

- *Wird Volumereplikation übertragen*    
    Gibt an, ob der Status der Volumereplikation „Wird übertragen“ lautet. 
 
- *Verzögerungszeit bei der Volumereplikation*   
    Die Zeit in Sekunden, um die die Daten in der Spiegelung im Vergleich zur Quelle verzögert sind 

- *Übertragungsdauer der letzten Volumereplikation*   
    Die Dauer der letzten Übertragung in Sekunden 

- *Übertragungsgröße der letzten Volumereplikation*    
    Bei der letzten Übertragung insgesamt übertragene Bytes 

- *Fortschritt der Volumereplikation*    
    Die insgesamt beim aktuellen Übertragungsvorgang übertragenen Daten. 

- *Insgesamt bei der Volumereplikation übertragene Bytes*   
    Die kumulative Anzahl der für die Beziehung übertragenen Bytes. 

## <a name="next-steps"></a>Nächste Schritte

* [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)
* [Erstellen eines Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
