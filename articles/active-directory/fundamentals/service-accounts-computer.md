---
title: Schützen von Computerkonten | Azure Active Directory
description: Eine Anleitung zum Schützen von lokalen Computerkonten.
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
ms.openlocfilehash: 22faba20cb12ae755f19fe43c295d98f9b364cbe
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100416564"
---
# <a name="securing-computer-accounts"></a>Schützen von Computerkonten

Das Computerkonto oder auch LocalSystem-Konto ist ein integriertes Konto mit hohen Berechtigungen und Zugriff auf praktisch alle Ressourcen auf dem lokalen Computer. Dieses Konto ist keinem angemeldeten Benutzerkonto zugeordnet. Als „LocalSystem“ ausgeführte Dienste greifen auf Netzwerkressourcen zu, indem sie den Remoteservern die Anmeldeinformationen des Computers bereitstellen. Die Anmeldeinformationen werden im Format „<Domänenname>\<computer_name>$“ bereitgestellt. Der vordefinierte Name eines Computerkontos lautet „NT AUTHORITY\SYSTEM“. Er kann verwendet werden, um einen Dienst zu starten und Sicherheitskontext für diesen Dienst bereitzustellen.

![[Abb. 4](.\media\securing-service-accounts\secure-computer-accounts-image-1.png)](.\media\securing-service-accounts\secure-computer-accounts-image-1.png)

## <a name="benefits-of-using-the-computer-account"></a>Vorteile der Verwendung des Computerkontos

Das Computerkonto bietet folgende Vorteile:

* **Uneingeschränkter lokaler Zugriff**: Das Computerkonto bietet vollständigen Zugriff auf die lokalen Ressourcen des Computers.

* **Automatische Kennwortverwaltung**: Für das Computerkonto brauchen Sie Kennwörter nicht mehr manuell zu ändern. Stattdessen ist dieses Konto Mitglied von Active Directory, und das Kontokennwort wird automatisch geändert. Außerdem brauchen Sie auch den Dienstprinzipalnamen für den Dienst nicht zu registrieren.

* **Eingeschränkte Zugriffsrechte außerhalb des Computers**: Die standardmäßige Zugriffssteuerungsliste in Active Directory Domain Services gewährt Computerkonten nur minimalen Zugriff. Sollte dieser Dienst gehackt werden, hätte er nur eingeschränkten Zugriff auf Ressourcen in Ihrem Netzwerk.

## <a name="assess-security-posture-of-computer-accounts"></a>Bewerten des Sicherheitsstatus von Computerkonten

Mögliche Probleme und entsprechende Gegenmaßnahmen bei der Verwendung von Computerkonten: 

| Probleme| Gegenmaßnahmen |
| - | - |
| Computerkonten können gelöscht und wieder hergestellt werden, wenn der Computer die Domäne verlässt und wieder beitritt.| Überprüfen Sie, ob ein Computer einer AD-Gruppe hinzugefügt werden muss. Überprüfen Sie außerdem anhand der auf dieser Seite bereitgestellten Beispielskripts, welches Computerkonto einer Gruppe hinzugefügt wurde.| 
| Wenn Sie ein Computerkonto einer Gruppe hinzufügen, erhalten alle auf diesem Computer als „LocalSystem“ ausgeführten Dienste die Zugriffsrechte der Gruppe.| Gehen Sie bei der Auswahl der Gruppenmitgliedschaften Ihres Computerkontos gezielt vor. Vermeiden Sie die Mitgliedschaft von Computerkonten in Domänenadministratorgruppen, da dann der zugehörige Dienst über vollständigen Zugriff auf Active Directory Domain Services verfügt. |
| Ungeeignete Netzwerkstandardeinstellungen für „LocalSystem“| Gehen Sie nicht davon aus, dass das Computerkonto über den eingeschränkten Standardzugriff auf Netzwerkressourcen verfügt. Überprüfen Sie stattdessen die Gruppenmitgliedschaften für dieses Konto sorgfältig. |
| Unbekannte Dienste, die als „LocalSystem“ ausgeführt werden| Stellen Sie sicher, dass alle Dienste, die unter dem LocalSystem-Konto ausgeführt werden, Microsoft-Dienste oder vertrauenswürdige Dienste von Drittanbietern sind. |


## <a name="find-services-running-under-the-computer-account"></a>Suchen nach unter dem Computerkonto ausgeführten Diensten

Suchen Sie mit dem folgenden PowerShell-Cmdlet nach Diensten, die im LocalSystem-Kontext ausgeführt werden.

```powershell

Get-WmiObject win32_service | select Name, StartName | Where-Object {($_.StartName -eq "LocalSystem")}
```

**Suchen nach Computerkonten, die Mitglied einer bestimmten Gruppe sind**

Suchen Sie mit dem folgenden PowerShell-Cmdlet nach Computerkonten, die Mitglied einer bestimmten Gruppe sind.

```powershell

```Get-ADComputer -Filter {Name -Like "*"} -Properties MemberOf | Where-Object {[STRING]$_.MemberOf -like "Your_Group_Name_here*"} | Select Name, MemberOf
```

**Suchen nach Computerkonten, die Mitglied privilegierter Gruppen sind**

Suchen Sie mit dem folgenden PowerShell-Cmdlet nach Computerkonten, die Mitglied der Gruppe „Identitätsadministrator“ (Domänenadministratoren, Unternehmensadministratoren, Administratoren) sind.

```powershell
Get-ADGroupMember -Identity Administrators -Recursive | Where objectClass -eq "computer"
```
## <a name="move-from-computer-accounts"></a>Weitgehendes Vermeiden von Computerkonten

> [!IMPORTANT]
> Computerkonten sind Konten mit hohen Berechtigungen und sollten nur verwendet werden, wenn Ihr Dienst uneingeschränkten Zugriff auf lokale Ressourcen auf dem Computer benötigt und Sie kein verwaltetes Dienstkonto (MSA) verwenden können.

* Überprüfen Sie zusammen mit dem Dienstbesitzer, ob dessen Dienst mit einem verwalteten Dienstkonto (MSA) ausgeführt werden kann. Wenn der Dienst dies unterstützt, können Sie ein gruppenverwaltetes Dienstkonto (Group Managed Service Account, gMSA) oder ein eigenständiges verwaltetes Dienstkonto (Standalone Managed Service Account, sMSA) verwenden.

* Verwenden Sie ein Domänenbenutzerkonto, das nur über die Berechtigungen verfügt, die zum Ausführen des Diensts benötigt werden.

## <a name="next-steps"></a>Nächste Schritte 

Lesen Sie die folgenden Artikel zum Schützen von Dienstkonten:

* [Einführung in lokale Dienstkonten](service-accounts-on-premises.md)

* [Schützen von gruppenverwalteten Dienstkonten](service-accounts-group-managed.md)

* [Schützen von eigenständigen verwalteten Dienstkonten](service-accounts-standalone-managed.md)

* [Schützen von Computerkonten](service-accounts-computer.md)

* [Schützen von Benutzerkonten](service-accounts-user-on-premises.md)

* [Steuern lokaler Dienstkonten](service-accounts-govern-on-premises.md)

 

 
