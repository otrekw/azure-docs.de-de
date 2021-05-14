---
title: Einführung in Active Directory-Dienstkonten | Azure Active Directory
description: Eine Einführung in die verschiedenen Arten von Dienstkonten in Active Directory und deren Schutz.
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
ms.openlocfilehash: 4551050cd8606c577edbbdfd85debc06ac12020c
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108206491"
---
# <a name="introduction-to-active-directory-service-accounts"></a>Einführung in Active Directory-Dienstkonten

Ein Dienst verfügt über eine primäre Sicherheitsidentität, mit der die Zugriffsrechte für lokale und Netzwerkressourcen bestimmt werden. Der Sicherheitskontext für einen Microsoft Win32-Dienst wird durch das Dienstkonto bestimmt, das zum Starten des Diensts verwendet wird. Ein Dienstkonto wird für Folgendes verwendet:
* Identifizieren und Authentifizieren eines Diensts
* Erfolgreiches Starten eines Diensts
* Zugreifen auf oder Ausführen von Code oder Anwendungen
* Starten eines Prozesses 

## <a name="types-of-on-premises-service-accounts"></a>Typen von lokalen Dienstkonten

Je nach Anwendungsfall können Sie ein verwaltetes Dienstkonto (Managed Service Account, MSA), ein Computerkonto oder ein Benutzerkonto verwenden, um einen Dienst auszuführen. Ein Dienst muss zuerst getestet werden, um zu ermitteln, ob er für die Verwendung eines verwalteten Dienstkontos geeignet ist. Falls ja, sollten Sie ein MSA verwenden.

### <a name="group-managed-service-accounts"></a>Über Gruppen verwaltete Dienstkonten

Verwenden Sie für Dienste, die in Ihrer lokalen Umgebung ausgeführt werden, nach Möglichkeit [gruppenverwaltete Dienstkonten](service-accounts-group-managed.md) (group managed service accounts, gMSAs). gMSAs bieten eine Einzelidentitätslösung für Dienste, die in einer Serverfarm oder hinter einem Netzwerklastenausgleich ausgeführt wird. gMSAs eignen sich auch für Dienste, die auf einem einzelnen Server ausgeführt werden. Weitere Informationen zu den Anforderungen für gMSAs finden Sie unter [Erste Schritte mit gruppenverwalteten Dienstkonten](/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts).

### <a name="standalone-managed-service-accounts"></a>Eigenständige verwaltete Dienstkonten

Sollten Sie kein gMSA verwenden können, verwenden Sie ein [eigenständiges verwaltetes Dienstkonto](service-accounts-standalone-managed.md) (standalone managed service account, sMSA). Für sMSAs ist mindestens Windows Server 2008 R2 erforderlich. Im Gegensatz zu gMSAs werden sMSAs nur auf einem einzelnen Server ausgeführt. Sie können für mehrere Dienste auf diesem Server verwendet werden.

### <a name="computer-accounts"></a>Computerkonten

Sollten Sie kein MSA verwenden können, empfiehlt sich ggf. die Verwendung eines [Computerkontos](service-accounts-computer.md). Das LocalSystem-Konto ist ein vordefiniertes lokales Konto, das umfangreiche Berechtigungen für den lokalen Computer umfasst und als Computeridentität im Netzwerk fungiert.

Unter LocalSystem-Konten ausgeführte Dienste greifen auf Netzwerkressourcen über die Anmeldeinformationen des Computerkontos im Format „<Domänenname>\\<Computername>“ zu. Der vordefinierte Name lautet „NT AUTHORITY\SYSTEM“. Er kann verwendet werden, um einen Dienst zu starten und einen Sicherheitskontext für diesen Dienst bereitzustellen.

> [!NOTE]
> Bei Verwendung eines Computerkontos können Sie nicht ermitteln, von welchem Dienst auf dem Computer das Konto verwendet wird. Somit können Sie nicht überprüfen, von welchem Dienst Änderungen vorgenommen werden. 

### <a name="user-accounts"></a>Benutzerkonten

Sollten Sie kein MSA verwenden können, empfiehlt sich ggf. die Verwendung eines [Benutzerkontos](service-accounts-user-on-premises.md). Ein Benutzerkonto kann ein *Domänenbenutzerkonto* oder ein *lokales Benutzerkonto* sein.

Bei einem Domänenbenutzerkonto können mit dem Dienst die Dienstsicherheitsfunktionen von Windows und Microsoft Active Directory Domain Services in vollem Umfang genutzt werden. Der Dienst verfügt über lokale und netzwerkbezogene Berechtigungen, die dem Konto gewährt wurden. Außerdem verfügt er über die Berechtigungen aller Gruppen, denen das Konto angehört. Domänendienstkonten unterstützen die gegenseitige Kerberos-Authentifizierung.

Ein lokales Benutzerkonto (Namensformat: *.\<Benutzername>* ) ist nur in der Datenbank des Sicherheitskonto-Managers des Hostcomputers vorhanden. Es verfügt über kein Benutzerobjekt in Active Directory Domain Services. Ein lokales Konto kann nicht in der Domäne authentifiziert werden. Ein Dienst, der im Sicherheitskontext eines lokalen Benutzerkontos ausgeführt wird, hat daher keinen Zugriff auf Netzwerkressourcen (außer als anonymer Benutzer). Im lokalen Benutzerkontext ausgeführte Dienste können die gegenseitige Kerberos-Authentifizierung nicht unterstützen, bei der der Dienst durch die zugehörigen Clients authentifiziert wird. Aus diesen Gründen eignen sich lokale Benutzerkonten üblicherweise nicht für verzeichnisfähige Dienste.

> [!IMPORTANT]
> Dienstkonten sollten keiner privilegierten Gruppe angehören, da die Mitgliedschaft in einer privilegierten Gruppe Berechtigungen beinhaltet, die ein Sicherheitsrisiko darstellen können. Jeder Dienst muss über ein eigenes Dienstkonto für die Überwachung und Sicherheit verfügen.

## <a name="choose-the-right-type-of-service-account"></a>Auswählen des geeigneten Dienstkontotyps


| Kriterium| gMSA| sMSA| Computerkonto| Benutzerkonto |
| - | - | - | - | - |
| Ausführen der Anwendung auf einem einzelnen Server| Ja| Ja. Verwenden Sie nach Möglichkeit ein gMSA.| Ja. Verwenden Sie nach Möglichkeit ein MSA.| Ja. Verwenden Sie nach Möglichkeit ein MSA. |
| Ausführen der Anwendung auf mehreren Servern| Ja| Nein| Nein. Das Konto ist an den Server gebunden.| Ja. Verwenden Sie nach Möglichkeit ein MSA. |
| Ausführen der Anwendung hinter einem Lastenausgleich| Ja| Nein| Nein| Ja. Verwenden Sie diese Option nur, wenn Sie kein gMSA verwenden können. |
| Ausführen der Anwendung unter Windows Server 2008 R2| Nein| Ja| Ja. Verwenden Sie nach Möglichkeit ein MSA.| Ja. Verwenden Sie nach Möglichkeit ein MSA. |
| Ausführen der App unter Windows Server 2012| Ja| Ja. Verwenden Sie nach Möglichkeit ein gMSA.| Ja. Verwenden Sie nach Möglichkeit ein MSA.| Ja. Verwenden Sie nach Möglichkeit ein MSA. |
| Anforderung der Beschränkung des Dienstkontos auf einen einzelnen Server| Nein| Ja| Ja. Verwenden Sie nach Möglichkeit ein sMSA.| Nein |
| | |


### <a name="use-server-logs-and-powershell-to-investigate"></a>Verwenden von Serverprotokollen und PowerShell für Untersuchungen

Über Serverprotokolle können Sie ermitteln, auf welchen und auf wie vielen Servern eine Anwendung ausgeführt wird.

Mit dem folgenden PowerShell-Befehl können Sie eine Liste der Windows Server-Versionen für alle Server in Ihrem Netzwerk abrufen: 

```PowerShell

Get-ADComputer -Filter 'operatingsystem -like "*server*" -and enabled -eq "true"' `

-Properties Name,Operatingsystem,OperatingSystemVersion,IPv4Address |

sort-Object -Property Operatingsystem |

Select-Object -Property Name,Operatingsystem,OperatingSystemVersion,IPv4Address |

Out-GridView

```

## <a name="find-on-premises-service-accounts"></a>Suchen lokaler Dienstkonten

Es empfiehlt sich, alle als Dienstkonten verwendeten Konten mit einem Präfix wie „svc-“ zu versehen. Diese Namenskonvention vereinfacht die Kontosuche und -verwaltung. Des Weiteren empfiehlt sich ggf. die Verwendung eines Beschreibungsattributs für das Dienstkonto und den Besitzer des Dienstkontos. Dabei kann es sich um einen Teamalias oder um den Besitzer eines Sicherheitsteams handeln.

Das Auffinden lokaler Dienstkonten ist der Schlüssel zur Gewährleistung ihrer Sicherheit. Dies kann bei MSA-fremden Konten schwierig sein. Es empfiehlt sich, alle Konten, die Zugriff auf Ihre wichtigen lokalen Ressourcen haben, zu überprüfen sowie zu bestimmen, welche Computer- oder Benutzerkonten als Dienstkonten fungieren können. 

Informationen zur Dienstkontosuche finden Sie im Abschnitt [Nächste Schritte](#next-steps) im Artikel zu diesem Kontotyp.

## <a name="document-service-accounts"></a>Dokumentieren von Dienstkonten

Wenn Sie die Dienstkonten in Ihrer lokalen Umgebung gefunden haben, dokumentieren Sie die folgenden Informationen: 

* **Besitzer:** Die Person, die für die Verwaltung des Kontos verantwortlich ist.

* **Zweck:** Die Anwendung, die das Konto darstellt, oder ein anderer Zweck. 

* **Berechtigungsbereiche:** Die Berechtigungen, über die es verfügt oder verfügen soll, sowie alle Gruppen, denen es ggf. angehört.

* **Risikoprofil:** Welches Risiko besteht für Ihr Unternehmen, wenn dieses Konto kompromittiert wird? Ist das Risiko hoch, verwenden Sie ein MSA.

* **Erwartete Lebensdauer und regelmäßiger Nachweis:** Wie lange ist das Konto voraussichtlich aktiv, und wie oft muss der Besitzer das Konto überprüfen und nachweisen, dass es weiterhin benötigt wird?

* **Kennwortsicherheit:** Betrifft Benutzerkonten und lokale Computerkonten, für die das Kennwort gespeichert wird. Stellen Sie sicher, dass Kennwörter sicher aufbewahrt werden, und dokumentieren Sie, wer Zugriff hat. Verwenden Sie [Privileged Identity Management](../privileged-identity-management/pim-configure.md) für den Schutz der gespeicherten Kennwörter. 

  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Schutz von Dienstkonten finden Sie in den folgenden Artikeln:

* [Schützen von gruppenverwalteten Dienstkonten](service-accounts-group-managed.md)  
* [Schützen von eigenständigen verwalteten Dienstkonten](service-accounts-standalone-managed.md)  
* [Schützen von Computerkonten](service-accounts-computer.md)  
* [Schützen von Benutzerkonten](service-accounts-user-on-premises.md)  
* [Steuern lokaler Dienstkonten](service-accounts-govern-on-premises.md)

