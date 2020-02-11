---
title: 'Azure VMware Solutions (AVS): Ausweiten von AVS-Berechtigungen'
description: Es wird beschrieben, wie die AVS-Berechtigungen zum Ausführen von Verwaltungsfunktionen auf der vCenter-Instanz der privaten AVS-Cloud ausgeweitet werden.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 90dd61fc9856978bab0b68de19d48493a8e0c5fd
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025332"
---
# <a name="escalate-avs-privileges-to-perform-administrative-functions-in-avs-private-cloud-vcenter"></a>Ausweiten der AVS-Berechtigungen zum Ausführen von Verwaltungsfunktionen in vCenter in der privaten AVS-Cloud

Der Berechtigungsansatz von AVS ist so konzipiert, dass vCenter-Benutzer die Berechtigungen erhalten, die sie für die Durchführung normaler Vorgänge benötigen. In einigen Fällen benötigt ein Benutzer möglicherweise zusätzliche Berechtigungen, um eine bestimmte Aufgabe auszuführen. Sie können die Berechtigungen eines vCenter-SSO-Benutzers für einen begrenzten Zeitraum ausweiten.

Gründe für die Eskalation von Berechtigungen können Folgendes umfassen:

* Konfiguration von Identitätsquellen
* Benutzerverwaltung
* Löschen der verteilten Portgruppe
* Installieren von vCenter-Lösungen (z.B. Sicherungs-Apps)
* Erstellen von Dienstkonten

> [!WARNING]
> Aktionen, die während des ausgeweiteten Berechtigungszustands durchgeführt werden, können sich negativ auf Ihr System auswirken und dazu führen, dass Ihr System nicht mehr verfügbar ist. Führen Sie während des Eskalationszeitraums nur die erforderlichen Aktionen aus.

[Führen Sie im AVS-Portal die Ausweitung der Berechtigungen](escalate-private-cloud-privileges.md) für den lokalen Benutzer „CloudOwner“ für das einmalige Anmelden (SSO) von vCenter durch. Sie können die Berechtigung des Remotebenutzers nur ausweiten, wenn ein zusätzlicher Identitätsanbieter für vCenter konfiguriert ist. Die Ausweitung von Berechtigungen umfasst das Hinzufügen des ausgewählten Benutzers zur integrierten Gruppe „Administratoren“ von vSphere. Nur ein Benutzer kann über ausgeweitete Berechtigungen verfügen. Wenn Sie die Berechtigungen eines anderen Benutzers eskalieren müssen, schränken Sie zunächst die Berechtigungen der aktuellen Benutzer ein.

Benutzer aus zusätzlichen Identitätsquellen müssen als Mitglieder der Gruppe „CloudOwner“ hinzugefügt werden.

> [!CAUTION]
> Neue Benutzer müssen lediglich *Cloud-Owner-Group*, *Cloud-Global-Cluster-Admin-Group*, *Cloud-Global-Storage-Admin-Group*, *Cloud-Global-Network-Admin-Group* oder *Cloud-Global-VM-Admin-Group* hinzugefügt werden.  Benutzer, die der Gruppe *Administratoren* hinzugefügt wurden, werden automatisch entfernt.  Nur Dienstkonten dürfen der Gruppe *Administratoren* hinzugefügt werden, und Dienstkonten dürfen nicht für die Anmeldung bei der vSphere-Webbenutzeroberfläche verwendet werden.
Während des Zeitraums der Ausweitung verwendet AVS die automatisierte Überwachung mit zugehörigen Warnungsbenachrichtigungen, um unbeabsichtigte Änderungen an der Umgebung zu identifizieren.
