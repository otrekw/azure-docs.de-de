---
title: Überlegungen zur Leistung für Azure NetApp Files | Microsoft-Dokumentation
description: Erfahren Sie etwas über die Leistung für Azure NetApp Files, einschließlich des Verhältnisses zwischen Kontingent und Durchsatzlimit und wie sich das Volumekontingent dynamisch erhöhen und verringern lässt.
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
ms.date: 02/19/2021
ms.author: b-juche
ms.openlocfilehash: f963c87148c08a4855befc5afb79d9c5ea0f4481
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101713387"
---
# <a name="performance-considerations-for-azure-netapp-files"></a>Überlegungen zur Leistung für Azure NetApp Files

Das [Durchsatzlimit](azure-netapp-files-service-levels.md) für ein Volume mit automatischer QoS wird durch eine Kombination aus dem Kontingent, das dem Volumen zugewiesen ist, und dem gewählten Servicelevel bestimmt. Für Volumes mit manueller QoS kann das Durchsatzlimit individuell definiert werden. Wenn Sie Leistungspläne zu Azure NetApp Files erstellen, müssen Sie einige Überlegungen vornehmen. 

## <a name="quota-and-throughput"></a>Kontingent und Durchsatz  

Das Durchsatzlimit ist nur eine Determinante der tatsächlichen Leistung, die erzielt werden kann.  

Typische Überlegungen zur Speicherleistung, einschließlich Lese- und Schreibmischung, Übertragungsgröße, zufällige oder sequenzielle Muster und viele andere Faktoren tragen zur erzielten Gesamtleistung bei.  

Der bei Tests beobachtete maximale empirische Durchsatz beträgt 4.500MiB/s.  Im Storage Premium-Tarif bietet ein Kontingent für ein Volume mit automatischer QoS von 70,31 TiB ein Durchsatzlimit, das hoch genug ist, um diese Leistung zu erzielen.  

Wenn Sie im Falle von Volumes mit automatischer QoS erwägen, Kontingente von mehr als 70,31 TiB zuzuweisen, kann einem Volume zum Speichern zusätzlicher Daten ein zusätzliches Kontingent zugewiesen werden. Das hinzugefügte Kontingent führt jedoch nicht zu einer weiteren Zunahme des tatsächlichen Durchsatzes.  

Dieselbe empirische Durchsatzobergrenze gilt für Volumes mit manueller QoS. Der maximale Durchsatz, der einem Volume zugewiesen werden kann beträgt 4.500 MiB/s.

## <a name="automatic-qos-volume-quota-and-throughput"></a>Kontingent und Durchsatz für Volumes mit automatischer QoS

In diesem Abschnitt werden die Kontingentverwaltung und der Durchsatz für Volumes mit automatischer QoS beschrieben.

### <a name="overprovisioning-the-volume-quota"></a>Überbereitstellung des Volumekontingents

Wenn die Leistung einer Workload an das Durchsatzlimit gebunden ist, ist eine Überbereitstellung des Kontingents für ein Volume mit automatischer QoS möglich, um einen höheren Durchsatzlevel festzulegen und eine höhere Leistung zu erzielen.  

Wenn beispielsweise ein Volume mit automatischer QoS im Storage Premium-Tarif nur 500 GiB Daten hat, aber einen Durchsatz von 128 MiB/s benötigt, können Sie das Kontingent auf 2 TiB festlegen, sodass der Durchsatzlevel entsprechend festgelegt wird (64 MiB/s pro TB * 2 TiB = 128 MiB/s).  

Wenn Sie ein Volume konsistent überbereitstellen, um einen höheren Durchsatz zu erzielen, überlegen Sie, stattdessen die Volumes mit manueller QoS oder einen höheren Servicelevel zu verwenden.  Im obigen Beispiel können Sie dasselbe Durchsatzlimit mit dem halben Kontingent für ein Volume mit automatischer QoS erreichen, indem Sie stattdessen die Ultra-Speicherebene verwenden (128MiB/s pro TiB * 1TiB = 128MiB/s).

### <a name="dynamically-increasing-or-decreasing-volume-quota"></a>Dynamisches Erhöhen oder Verringern eines Volumekontingents

Wenn Ihre Leistungsanforderungen vorübergehend sind oder Sie für einen bestimmten Zeitraum einen erhöhten Leistungsbedarf haben, können Sie das Volumekontingent dynamisch erhöhen oder verringern, um das Durchsatzlimit sofort anzupassen.  Beachten Sie die folgenden Überlegungen: 

* Das Volumekontingent kann erhöht oder verringert werden, ohne dass die E/A angehalten werden muss, und der Zugriff auf das Volume wird nicht unterbrochen oder beeinträchtigt.  

    Sie können das Kontingent während einer aktiven E/A-Transaktion für ein Volume anpassen.  Beachten Sie, dass das Volumekontingent niemals unter die Menge der logischen Daten gesenkt werden kann, die im Volume gespeichert sind.

* Wenn ein Volumekontingent geändert wird, wird die entsprechende Änderung nahezu sofort im Durchsatzlimit wirksam. 

    Die Änderung unterbricht oder beeinträchtigt weder den Zugriff auf das Volume noch die E/A.  

* Das Anpassen eines Volumekontingents kann eine Änderung der Kapazitätspoolgröße erfordern.  

    Die Kapazitätspoolgröße kann dynamisch und ohne Beeinträchtigung der Volumenverfügbarkeit oder E/A angepasst werden.

## <a name="manual-qos-volume-quota-and-throughput"></a>Kontingent und Durchsatz für Volumes mit manueller QoS 

Wenn Sie Volumes mit manueller QoS verwenden, müssen Sie das Volumekontingent nicht überbereitstellen, um einen höheren Durchsatz zu erzielen, da der Durchsatz jedem Volume unabhängig zugewiesen werden kann. Sie müssen jedoch trotzdem sicherstellen, dass der Kapazitätspool mit ausreichendem Durchsatz für Ihre Leistungsanforderungen vorab bereitgestellt wird. Der Durchsatz eines Kapazitätspools wird entsprechend seiner Größe und seinem Servicelevel bereitgestellt. Unter [Servicelevels für Azure NetApp Files](azure-netapp-files-service-levels.md) finden Sie weitere Detailinformationen.


## <a name="next-steps"></a>Nächste Schritte

- [Azure NetApp Files Performance Calculator](https://cloud.netapp.com/azure-netapp-files/tco?hs_preview=tIKQbfoF-41214739590) (Azure NetApp Files-Leistungsrechner)
- [Dienstebenen für Azure NetApp Files](azure-netapp-files-service-levels.md)
- [Leistungsbenchmarks für Linux](performance-benchmarks-linux.md)
