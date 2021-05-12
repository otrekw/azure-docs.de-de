---
title: Schützen von Computerkonten | Azure Active Directory
description: Ein Leitfaden zum Schutz lokaler Computerkonten.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 2/15/2021
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec2e8cddb93e2e6fdf9ff804bbc28fd283436131
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108206617"
---
# <a name="secure-on-premises-computer-accounts"></a>Schützen lokaler Computerkonten

Ein Computerkonto oder auch LocalSystem-Konto ist ein integriertes Konto mit hohen Berechtigungen und Zugriff auf nahezu alle Ressourcen auf dem lokalen Computer. Das Konto ist keinem angemeldeten Benutzerkonto zugeordnet. Als „LocalSystem“ ausgeführte Dienste greifen auf Netzwerkressourcen zu, indem sie Remoteservern die Anmeldeinformationen des Computers im Format „<Domänenname>\\<Computername>$“ präsentieren. Der vordefinierte Name des Computerkontos lautet „NT AUTHORITY\SYSTEM“. Er kann verwendet werden, um einen Dienst zu starten und Sicherheitskontext für diesen Dienst bereitzustellen.

![Screenshot: Liste mit lokalen Diensten eines Computerkontos](.\media\securing-service-accounts\secure-computer-accounts-image-1.png)

## <a name="benefits-of-using-a-computer-account"></a>Vorteile der Verwendung eines Computerkontos

Ein Computerkonto bietet folgende Vorteile:

* **Uneingeschränkter lokaler Zugriff**: Das Computerkonto bietet vollständigen Zugriff auf die lokalen Ressourcen des Computers.

* **Automatische Kennwortverwaltung**: Kennwörter müssen nicht mehr manuell geändert werden. Das Konto gehört zu Active Directory, und das Kontokennwort wird automatisch geändert. Bei Verwendung eines Computerkontos muss der Dienstprinzipalname nicht für den Dienst registriert werden.

* **Eingeschränkte Zugriffsrechte außerhalb des Computers**: Die standardmäßige Zugriffssteuerungsliste in Active Directory Domain Services (AD DS) gewährt Computerkonten nur minimalen Zugriff. Im Falle eines Zugriffs durch einen nicht autorisierten Benutzer hat der Dienst nur eingeschränkten Zugriff auf Ressourcen in Ihrem Netzwerk.

## <a name="assess-the-security-posture-of-computer-accounts"></a>Bewerten des Sicherheitsstatus von Computerkonten

Die folgende Tabelle enthält eine Liste mit einigen potenziellen Herausforderungen bei der Verwendung eines Computerkontos sowie entsprechende Abhilfen:
 
| Problem | Minderung |
| - | - |
| Computerkonten werden gelöscht und erneut erstellt, wenn der Computer die Domäne verlässt und ihr wieder beitritt. | Prüfen Sie, ob ein Computer einer Active Directory-Gruppe hinzugefügt werden muss, und verwenden Sie die im nächsten Abschnitt dieses Artikels bereitgestellten Beispielskripts, um zu überprüfen, welches Computerkonto einer Gruppe hinzugefügt wurde.| 
| Wenn Sie ein Computerkonto einer Gruppe hinzufügen, erhalten alle auf diesem Computer als „LocalSystem“ ausgeführten Dienste die Zugriffsrechte der Gruppe.| Gehen Sie bei der Wahl der Gruppenmitgliedschaften Ihres Computerkontos gezielt vor. Computerkonten sollten keiner Domänenadministratorgruppe hinzugefügt werden, da der zugeordnete Dienst uneingeschränkten Zugriff auf AD DS hat. |
| Ungeeignete Netzwerkstandardeinstellungen für „LocalSystem“ | Gehen Sie nicht davon aus, dass das Computerkonto über den eingeschränkten Standardzugriff auf Netzwerkressourcen verfügt. Überprüfen Sie stattdessen die Gruppenmitgliedschaften für das Konto sorgfältig. |
| Unbekannte Dienste, die als „LocalSystem“ ausgeführt werden | Stellen Sie sicher, dass alle Dienste, die unter dem LocalSystem-Konto ausgeführt werden, Microsoft-Dienste oder vertrauenswürdige Dienste von Drittanbietern sind. |
| | |

## <a name="find-services-that-run-under-the-computer-account"></a>Suchen nach unter dem Computerkonto ausgeführten Diensten

Verwenden Sie das folgende PowerShell-Cmdlet, um nach im LocalSystem-Kontext ausgeführten Diensten zu suchen:

```powershell
Get-WmiObject win32_service | select Name, StartName | Where-Object {($_.StartName -eq "LocalSystem")}
```

Führen Sie das folgende PowerShell-Cmdlet aus, um nach Computerkonten zu suchen, die einer bestimmten Gruppe angehören:

```powershell
Get-ADComputer -Filter {Name -Like "*"} -Properties MemberOf | Where-Object {[STRING]$_.MemberOf -like "Your_Group_Name_here*"} | Select Name, MemberOf
```

Führen Sie das folgende PowerShell-Cmdlet aus, um nach Computerkonten zu suchen, die einer Gruppe von Identitätsadministratoren (Domänenadministratoren, Unternehmensadministratoren und Administratoren) angehören:

```powershell
Get-ADGroupMember -Identity Administrators -Recursive | Where objectClass -eq "computer"
```

## <a name="move-from-computer-accounts"></a>Weitgehendes Vermeiden von Computerkonten

> [!IMPORTANT]
> Computerkonten sind Konten mit hohen Berechtigungen und sollten nur verwendet werden, wenn Ihr Dienst uneingeschränkten Zugriff auf lokale Ressourcen auf dem Computer benötigt und Sie kein verwaltetes Dienstkonto (managed service account, MSA) verwenden können.

* Überprüfen Sie zusammen mit dem Dienstbesitzer, ob dessen Dienst mit einem verwalteten Dienstkonto (MSA) verwendet werden kann, und verwenden Sie ein gruppenverwaltetes Dienstkonto (group managed service account, gMSA) oder ein eigenständiges verwaltetes Dienstkonto (standalone managed service account, sMSA), falls der Dienst dies unterstützt.

* Verwenden Sie ein Domänenbenutzerkonto nur mit den Berechtigungen, die Sie zum Ausführen Ihres Diensts benötigen.

## <a name="next-steps"></a>Nächste Schritte 

Weitere Informationen zum Schutz von Dienstkonten finden Sie in den folgenden Artikeln:

* [Einführung in lokale Dienstkonten](service-accounts-on-premises.md)
* [Schützen von gruppenverwalteten Dienstkonten](service-accounts-group-managed.md)
* [Schützen von eigenständigen verwalteten Dienstkonten](service-accounts-standalone-managed.md)
* [Schützen von Benutzerkonten](service-accounts-user-on-premises.md)  
* [Steuern lokaler Dienstkonten](service-accounts-govern-on-premises.md)
