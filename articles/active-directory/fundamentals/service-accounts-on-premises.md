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
ms.openlocfilehash: 55de24975dadf27293f305611c6ba07522e8aa90
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100416558"
---
# <a name="introduction-to-active-directory-service-accounts"></a>Einführung in Active Directory-Dienstkonten

Ein Dienst verfügt über eine primäre Sicherheitsidentität, mit der die Zugriffsrechte für lokale und Netzwerkressourcen bestimmt werden. Der Sicherheitskontext für einen Microsoft Win32-Dienst wird durch das Dienstkonto vorgegeben, das zum Starten des Diensts verwendet wird. Ein Dienstkonto wird für folgende Zwecke verwendet:
* Identifizieren und Authentifizieren eines Diensts
* Erfolgreiches Starten eines Diensts
* Zugreifen auf oder Ausführen von Code oder Anwendungen
* Starten eines Prozesses 

## <a name="types-of-on-premises-service-accounts"></a>Typen von lokalen Dienstkonten

Basierend auf Ihrem Anwendungsfall können Sie ein verwaltetes Dienstkonto (Managed Service Account, MSA), ein Computerkonto oder ein Benutzerkonto verwenden, um einen Dienst auszuführen. Dienste müssen getestet werden, um zu prüfen, ob ein verwaltetes Dienstkonto verwendet werden kann. Wenn dies möglich ist, sollten Sie es verwenden.

### <a name="group-msa-accounts"></a>MSA-Gruppenkonten

Verwenden Sie nach Möglichkeit [gruppenverwaltete Dienstkonten](service-accounts-group-managed.md) (gMSAs) für Dienste, die in der lokalen Umgebung ausgeführt werden. gMSAs bieten eine Einzelidentitätslösung für einen Dienst, der in einer Serverfarm oder hinter einem Lastenausgleich im Netzwerk ausgeführt wird. Sie können auch für einen Dienst verwendet werden, der auf einem einzelnen Server ausgeführt wird. [Für gMSAs gelten bestimmte Anforderungen, die erfüllt sein müssen.](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)

### <a name="standalone-msa-accounts"></a>Eigenständige MSA-Konten

Wenn die Verwendung eines gMSA nicht möglich ist, verwenden Sie ein [eigenständiges verwaltetes Dienstkonto](service-accounts-standalone-managed.md) (sMSA). Für sMSAs ist mindestens Windows Server 2008 R2 erforderlich. Im Unterschied zu gMSAs werden sMSAs nur auf einem Server ausgeführt. Sie können für mehrere Dienste auf diesem Server verwendet werden.

### <a name="computer-account"></a>Computerkonto

Wenn die Verwendung eines MSA-Kontos nicht möglich ist, prüfen Sie die Verwendung eines [Computerkontos](service-accounts-computer.md). Das LocalSystem-Konto ist ein vordefiniertes lokales Konto, das umfangreiche Berechtigungen für den lokalen Computer umfasst und als Computeridentität im Netzwerk fungiert.   
‎Unter LocalSystem-Konten ausgeführte Dienste greifen auf Netzwerkressourcen über die Anmeldeinformationen des Computerkontos im Format „<Domänenname>\<computer_name>“ zu.

Als Name für das LocalSystem-Konto ist „NT-AUTORITÄT\SYSTEM“ vordefiniert. Das Konto kann verwendet werden, um einen Dienst zu starten und Sicherheitskontext für diesen Dienst bereitzustellen.

> [!NOTE]
> Bei Verwendung eines Computerkontos kann nicht festgestellt werden, welcher Dienst auf dem Computer dieses Konto verwendet. Daher kann nicht überwacht werden, welcher Dienst Änderungen vornimmt. 

### <a name="user-account"></a>Benutzerkonto

Wenn die Verwendung eines MSA-Kontos nicht möglich ist, prüfen Sie die Verwendung eines [Benutzerkontos](service-accounts-user-on-premises.md). Ein Benutzerkonto kann ein Domänenbenutzerkonto oder ein lokales Benutzerkonto sein.

Bei einem Domänenbenutzerkonto können mit dem Dienst die Dienstsicherheitsfunktionen von Windows und Microsoft Active Directory Domain Services in vollem Umfang genutzt werden. Der Dienst verfügt über den lokalen Zugriff und den Netzwerkzugriff, die dem Konto gewährt wurden. Außerdem verfügt er über die Berechtigungen aller Gruppen, denen das Konto angehört. Domänendienstkonten unterstützen die gegenseitige Kerberos-Authentifizierung.

Ein lokales Benutzerkonto (Namensformat: „.\Benutzername“) ist nur in der SAM-Datenbank des Hostcomputers vorhanden. In Active Directory Domain Services ist kein zugehöriges Benutzerobjekt vorhanden. Ein lokales Konto kann nicht in der Domäne authentifiziert werden. Ein Dienst, der im Sicherheitskontext eines lokalen Benutzerkontos ausgeführt wird, hat daher keinen Zugriff auf Netzwerkressourcen (außer als anonymer Benutzer). Dienste, die im lokalen Benutzerkontext ausgeführt werden, können die gegenseitige Kerberos-Authentifizierung nicht unterstützen, bei der der Dienst von den zugehörigen Clients authentifiziert wird. Aus diesen Gründen eignen sich lokale Benutzerkonten normalerweise nicht für verzeichnisfähige Dienste.

> [!IMPORTANT]
> Dienstkonten sollten nicht Mitglied einer privilegierten Gruppe sein, da die Mitgliedschaft in einer privilegierten Gruppe Berechtigungen beinhaltet, die ein Sicherheitsrisiko darstellen können. Jeder Dienst muss über ein eigenes Dienstkonto für die Überwachung und Sicherheit verfügen.

## <a name="choose-the-right-type-of-service-account"></a>Auswählen des geeigneten Dienstkontotyps


| Kriterien| gMSA| sMSA| Computerkonto| Benutzerkonto |
| - | - | - | - | - |
| Ausführen der Anwendung auf einem einzelnen Server| Ja| Ja. Nach Möglichkeit Verwendung eines gMSA| Ja. Nach Möglichkeit Verwendung eines MSA| Ja. Nach Möglichkeit Verwendung eines MSA |
| Ausführen der Anwendung auf mehreren Servern| Ja| Nein| Nein. Das Konto ist an den Server gebunden.| Ja. Nach Möglichkeit Verwendung eines MSA |
| Ausführen der Anwendung hinter Load Balancern| Ja| Nein| Nein| Ja. Nur verwenden, wenn kein gMSA möglich ist |
| Ausführen der Anwendung unter Windows Server 2008 R2| Nein| Ja| Ja. Nach Möglichkeit Verwendung eines MSA| Ja. Nach Möglichkeit Verwendung eines MSA |
| Ausführung unter Windows Server 2012| Ja| Ja. Nach Möglichkeit Verwendung eines gMSA| Ja. Nach Möglichkeit Verwendung eines MSA| Ja. Nach Möglichkeit Verwendung eines MSA |
| Anforderung der Beschränkung des Dienstkontos auf einen einzelnen Server| Nein| Ja| Ja. Nach Möglichkeit Verwendung eines sMSA| Nein. |


 

### <a name="use-server-logs-and-powershell-to-investigate"></a>Verwenden von Serverprotokollen und PowerShell für Untersuchungen

Über Serverprotokolle können Sie ermitteln, auf welchen und auf wie vielen Servern eine Anwendung ausgeführt wird.

Sie können den folgenden PowerShell-Befehl ausführen, um eine Liste der Windows Server-Versionen für alle Server in Ihrem Netzwerk abzurufen. 

```PowerShell

Get-ADComputer -Filter 'operatingsystem -like "*server*" -and enabled -eq "true"' `

-Properties Name,Operatingsystem,OperatingSystemVersion,IPv4Address |

sort-Object -Property Operatingsystem |

Select-Object -Property Name,Operatingsystem,OperatingSystemVersion,IPv4Address |

Out-GridView

```

## <a name="find-on-premises-service-accounts"></a>Suchen lokaler Dienstkonten

Es wird empfohlen, allen als Dienstkonten verwendeten Konten ein Präfix hinzuzufügen, z. B. „svc.“. Diese Namenskonvention vereinfacht das Suchen und Verwalten der Konten. Es empfiehlt sich außerdem die Verwendung eines Beschreibungsattributs für das Dienstkonto und den Besitzer des Dienstkontos. Dabei kann es sich um einen Teamalias oder den Besitzer eines Sicherheitsteams handeln.

Das Auffinden lokaler Dienstkonten ist der Schlüssel zur Gewährleistung ihrer Sicherheit. Dies kann für Nicht-MSA-Konten schwierig sein. Es wird empfohlen, alle Konten, die Zugriff auf Ihre wichtigen lokalen Ressourcen haben, zu überprüfen und zu bestimmen, welche Computer- oder Benutzerkonten als Dienstkonten fungieren können. Sie können auch die folgenden Methoden anwenden, um die Konten aufzufinden.

* Die Artikel für die einzelnen Kontotypen enthalten ausführliche Schritte zum Auffinden des jeweiligen Kontotyps. Links zu diesen Artikeln finden Sie im Abschnitt „Nächste Schritte“ in diesem Artikel.

## <a name="document-service-accounts"></a>Dokumentieren von Dienstkonten

Nachdem Sie die Dienstkonten in der lokalen Umgebung gefunden haben, dokumentieren Sie die folgenden Informationen zu den einzelnen Konten. 

* Der Besitzer. Die Person, die für die Verwaltung des Kontos verantwortlich ist.

* Der Zweck. Die Anwendung, die das Konto darstellt, oder ein anderer Zweck. 

* Berechtigungsbereiche: Welche Berechtigungen hat das Konto, und welche Berechtigungen muss es haben? Welchen Gruppen gehört es gegebenenfalls an?

* Risikoprofil: Welches Risiko besteht für Ihr Unternehmen, wenn das Konto kompromittiert wird? Wenn ein hohes Risiko besteht, verwenden Sie ein MSA-Konto.

* Erwartete Lebensdauer und regelmäßiger Nachweis: Wie lange wird das Konto voraussichtlich aktiv sein? Wie oft muss der Besitzer den laufenden Bedarf überprüfen und nachweisen?

* Kennwortsicherheit: Betrifft Benutzerkonten und lokale Computerkonten, für die das Kennwort gespeichert wird. Stellen Sie sicher, dass die Kennwörter sicher aufbewahrt werden, und dokumentieren Sie, wer darauf Zugriff hat. Verwenden Sie [Privileged Identity Management](../privileged-identity-management/pim-configure.md) für den Schutz der gespeicherten Kennwörter. 

  

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel zum Schützen von Dienstkonten:

* [Einführung in lokale Dienstkonten](service-accounts-on-premises.md)

* [Schützen von gruppenverwalteten Dienstkonten](service-accounts-group-managed.md)

* [Schützen von eigenständigen verwalteten Dienstkonten](service-accounts-standalone-managed.md)

* [Schützen von Computerkonten](service-accounts-computer.md)

* [Schützen von Benutzerkonten](service-accounts-user-on-premises.md)

* [Steuern lokaler Dienstkonten](service-accounts-govern-on-premises.md)

 