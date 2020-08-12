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
ms.date: 06/25/2019
ms.author: b-juche
ms.openlocfilehash: 5f88b4755c7b4c0b20f27065cf9de2351251bc1c
ms.sourcegitcommit: 29400316f0c221a43aff3962d591629f0757e780
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2020
ms.locfileid: "87513875"
---
# <a name="performance-considerations-for-azure-netapp-files"></a>Überlegungen zur Leistung für Azure NetApp Files

Das [Durchsatzlimit](azure-netapp-files-service-levels.md) für ein Volumen wird durch eine Kombination aus dem Kontingent, das dem Volumen zugewiesen ist, und dem gewählten Servicelevel bestimmt. Wenn Sie Leistungspläne zu Azure NetApp Files erstellen, müssen Sie einige Überlegungen vornehmen. 

## <a name="quota-and-throughput"></a>Kontingent und Durchsatz  

Das Durchsatzlimit ist nur eine Determinante der tatsächlichen Leistung, die erzielt werden kann.  

Typische Überlegungen zur Speicherleistung, einschließlich Lese- und Schreibmischung, Übertragungsgröße, zufällige oder sequenzielle Muster und viele andere Faktoren tragen zur erzielten Gesamtleistung bei.  

Der bei Tests beobachtete maximale empirische Durchsatz beträgt 4.500MiB/s.  Im Storage Premium-Tarif bietet ein Volumekontingent von 70,31TiB ein Durchsatzlimit, das hoch genug ist, um diese Leistung zu erzielen.  

Wenn Sie erwägen, Volumekontingente über 70,31TiB zuzuweisen, kann einem Volume zum Speichern zusätzlicher Daten ein zusätzliches Kontingent zugewiesen werden. Das hinzugefügte Kontingent führt jedoch nicht zu einer weiteren Zunahme des tatsächlichen Durchsatzes.  

## <a name="overprovisioning-the-volume-quota"></a>Überbereitstellung des Volumekontingents

Wenn die Leistung einer Workload an das Durchsatzlimit gebunden ist, ist eine Überbereitstellung des Volumekontingents möglich, um einen höheren Durchsatzlevel festzulegen und eine höhere Leistung zu erzielen.  

Wenn beispielsweise ein Volume im Storage Premium-Tarif nur 500GiB Daten hat, aber einen Durchsatz von 128MiB/s benötigt, können Sie das Kontingent auf 2TiB festlegen, sodass der Durchsatzlevel entsprechend festgelegt wird (64MiB/s pro TB * 2TiB = 128MiB/s).  

Wenn Sie ein Volume konsistent überbereitstellen, um einen höheren Durchsatz zu erzielen, überlegen Sie, stattdessen einen höheren Servicelevel zu verwenden.  Im obigen Beispiel können Sie das gleiche Durchsatzlimit mit dem halben Volumekontingent erreichen, indem Sie stattdessen die Ultra-Speicherebene verwenden (128MiB/s pro TiB * 1TiB = 128MiB/s).

## <a name="dynamically-increasing-or-decreasing-volume-quota"></a>Dynamisches Erhöhen oder Verringern eines Volumekontingents

Wenn Ihre Leistungsanforderungen vorübergehend sind oder Sie für einen bestimmten Zeitraum einen erhöhten Leistungsbedarf haben, können Sie das Volumekontingent dynamisch erhöhen oder verringern, um das Durchsatzlimit sofort anzupassen.  Beachten Sie die folgenden Überlegungen: 

* Das Volumekontingent kann erhöht oder verringert werden, ohne dass die E/A angehalten werden muss, und der Zugriff auf das Volume wird nicht unterbrochen oder beeinträchtigt.  

    Sie können das Kontingent während einer aktiven E/A-Transaktion für ein Volume anpassen.  Beachten Sie, dass das Volumekontingent niemals unter die Menge der logischen Daten gesenkt werden kann, die im Volume gespeichert sind.

* Wenn ein Volumekontingent geändert wird, wird die entsprechende Änderung nahezu sofort im Durchsatzlimit wirksam. 

    Die Änderung unterbricht oder beeinträchtigt weder den Zugriff auf das Volume noch die E/A.  

* Das Anpassen eines Volumekontingents erfordert eine Änderung der Kapazitätspoolgröße.  

    Die Kapazitätspoolgröße kann dynamisch und ohne Beeinträchtigung der Volumenverfügbarkeit oder E/A angepasst werden.

## <a name="next-steps"></a>Nächste Schritte

- [Dienstebenen für Azure NetApp Files](azure-netapp-files-service-levels.md)
- [Leistungsbenchmarks für Linux](performance-benchmarks-linux.md)