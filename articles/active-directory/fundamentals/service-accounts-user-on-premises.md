---
title: Schützen von benutzerbasierten Dienstkonten | Azure Active Directory
description: Leitfaden zum Schützen von lokalen Benutzerkonten.
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
ms.openlocfilehash: e484bdda33142024f2067649eaa67042fe7776f8
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100416555"
---
# <a name="securing-user-based-service-accounts-in-active-directory"></a>Schützen von benutzerbasierten Dienstkonten in Active Directory

Lokale Benutzerkonten sind der herkömmliche Ansatz zum Schutz von Diensten, die unter Windows ausgeführt werden. Verwenden Sie diese Konten als letzte Möglichkeit, wenn globale verwaltete Dienstkonten (gMSAs) und eigenständige verwaltete Dienstkonten (sMSAs) in Ihrem Dienst nicht unterstützt werden. Informationen zur Auswahl des am besten geeigneten Kontotyps finden Sie in der Übersicht über die lokalen Dienstkonten. Prüfen Sie außerdem, ob Sie den Dienst verschieben können, sodass ein Azure-Dienstkonto verwendet wird, z. B. eine verwaltete Identität oder ein Dienstprinzipal. 

Lokale Benutzerkonten können erstellt werden, um einen Sicherheitskontext für Dienste bereitzustellen und für die Dienste die erforderlichen Berechtigungen für den Zugriff auf lokale und Netzwerkressourcen zu erteilen. Wie bei allen anderen Active Directory-Konten (AD) muss bei diesen Konten die Kennwortverwaltung manuell durchgeführt werden. Dienst- und Domänenadministratoren müssen Verwaltungsprozesse für sichere Kennwörter durchführen, um diese Konten zu schützen.

Wenn Sie ein Benutzerkonto als Dienstkonto verwenden, sollten Sie es nur für einen einzigen Dienst verwenden. Benennen Sie es so, dass deutlich wird, dass es sich um ein Dienstkonto handelt und für welchen Dienst es verwendet wird. 

## <a name="benefits-and-challenges"></a>Vorteile und Einschränkungen

Vorteile

Lokale Benutzerkonten gelten als der vielseitigste Kontotyp für die Verwendung mit Diensten. Benutzerkonten, die als Dienstkonten verwendet werden, können über alle Richtlinien gesteuert werden, über die normale Benutzerkonten gesteuert werden. Sie sollten sie jedoch nur verwenden, wenn die Verwendung eines MSA-Kontos nicht möglich ist. Überprüfen Sie außerdem, ob ein Computerkonto eine bessere Alternative ist. 

Einschränkungen bei lokalen Benutzerkonten

Bei der Verwendung von lokalen Benutzerkonten ergeben sich die folgenden Einschränkungen.

| Herausforderungen| Gegenmaßnahmen |
| - | - |
| Die Kennwortverwaltung wird manuell durchgeführt. Dies kann zu einer geringeren Sicherheit und Dienstausfallzeiten führen.| Stellen Sie sicher, dass Kennwortkomplexität und Kennwortänderungen durch einen robusten Prozess gesteuert werden, bei dem reguläre Aktualisierungen mit einem sicheren Kennwort gewährleistet sind. <br> Koordinieren Sie die Kennwortänderung mit einer Kennwortaktualisierung des Diensts, da diese Dienstausfallzeiten verursachen. |
| Die Identifizierung von lokalen Benutzerkonten, die als Dienstkonten fungieren, kann schwierig sein.| Dokumentieren und verwalten Sie Aufzeichnungen der Dienstkonten, die in Ihrer Umgebung bereitgestellt werden. <br> Verfolgen Sie den Kontonamen und die Ressourcen, denen sie zugewiesen sind. <br> Sie können beispielsweise allen als Dienstkonten verwendeten Benutzerkonten das Präfix „svc_“ anfügen. |


## <a name="find-on-premises-user-accounts-used-as-service-accounts"></a>Suchen von als Dienstkonten verwendeten lokalen Benutzerkonten

Lokale Benutzerkonten unterscheiden sich nicht von anderen AD-Benutzerkonten. Daher kann es schwierig sein, diese Benutzerkonten aufzufinden, da sie durch kein spezifisches Attribut als Dienstkonto identifiziert werden. 

Es empfiehlt sich, für alle Benutzerkonten, die als Dienstkonto verwendet werden, eine einfach zu erkennende Namenskonvention anzuwenden.

Fügen Sie z. B. „service-“ als Präfix hinzu, und nennen Sie den Dienst „service-HRDataConnector“.

Anhand der folgenden Kennzeichen können Sie diese Dienstkonten auffinden, jedoch möglicherweise nicht alle.

* Konten, die für die Delegierung als vertrauenswürdig gelten

* Konten mit Dienstprinzipalnamen

* Konten, deren Kennwort so festgelegt ist, dass es nie abläuft

Sie können die folgenden PowerShell-Befehle ausführen, um die für Dienste erstellten lokalen Benutzerkonten zu suchen.

### <a name="find-accounts-trusted-for-delegation"></a>Suchen von Konten, die für die Delegierung als vertrauenswürdig gelten

```PowerShell

Get-ADObject -Filter {(msDS-AllowedToDelegateTo -like '*') -or (UserAccountControl -band 0x0080000) -or (UserAccountControl -band 0x1000000)} -prop samAccountName,msDS-AllowedToDelegateTo,servicePrincipalName,userAccountControl | select DistinguishedName,ObjectClass,samAccountName,servicePrincipalName, @{name='DelegationStatus';expression={if($_.UserAccountControl -band 0x80000){'AllServices'}else{'SpecificServices'}}}, @{name='AllowedProtocols';expression={if($_.UserAccountControl -band 0x1000000){'Any'}else{'Kerberos'}}}, @{name='DestinationServices';expression={$_.'msDS-AllowedToDelegateTo'}}

```

### <a name="find-accounts-with-service-principle-names"></a>Suchen von Konten mit Dienstprinzipalnamen

```PowerShell

Get-ADUser -Filter * -Properties servicePrincipalName | where {$_.servicePrincipalName -ne $null}

```

 

### <a name="find-accounts-with-passwords-set-to-never-expire"></a>Suchen von Konten, deren Kennwort so festgelegt ist, dass es nie abläuft

```PowerShell

Get-ADUser -Filter * -Properties PasswordNeverExpires | where {$_.PasswordNeverExpires -eq $true}

```


Sie können außerdem den Zugriff auf vertrauliche Ressourcen überwachen und Überwachungsprotokolle in einem SIEM-System (Security Information & Event Management) archivieren. In Systemen wie Azure Log Analytics oder Azure Sentinel können Sie Dienstkonten suchen und analysieren.

## <a name="assess-security-of-on-premises-user-accounts"></a>Bewerten der Sicherheit von lokalen Benutzerkonten

Bewerten Sie die Sicherheit Ihrer lokalen Benutzerkonten, die als Dienstkonten verwendet werden, anhand der folgenden Kriterien:

* Welche Richtlinie wird für die Kennwortverwaltung verwendet?

* Gehört das Konto privilegierten Gruppen an?

* Hat das Konto Lese-/Schreibzugriff auf wichtige Ressourcen?

### <a name="mitigate-potential-security-issues"></a>Beheben potenzieller Sicherheitsprobleme

In der folgenden Tabelle werden potenzielle Sicherheitsprobleme und entsprechende Maßnahmen bei lokalen Benutzerkonten beschrieben.

| Sicherheitsprobleme| Gegenmaßnahmen |
| - | - |
| Kennwortverwaltung|* Stellen Sie sicher, dass Kennwortkomplexität und Kennwortänderungen durch einen robusten Prozess gesteuert werden, bei dem reguläre Aktualisierungen mit Anforderungen für sichere Kennwörter gewährleistet sind. <br> * Koordinieren Sie die Kennwortänderung mit einer Kennwortaktualisierung, um die Dienstausfallzeit zu minimieren. |
| Das Konto ist Mitglied von privilegierten Gruppen.| Überprüfen Sie die Gruppenmitgliedschaften. Entfernen Sie das Konto aus privilegierten Gruppen. Weisen Sie dem Konto nur die Rechte und Berechtigungen zu, die zum Ausführen des zugehörigen Diensts erforderlich sind (wenden Sie sich an den Dienstanbieter). Beispielsweise können Sie möglicherweise die lokale oder die interaktive Anmeldung verweigern. |
| Das Konto verfügt über Lese-/Schreibzugriff auf vertrauliche Ressourcen.| Überwachen Sie den Zugriff auf vertrauliche Ressourcen. Archivieren Sie Überwachungsprotokolle zur Analyse in einem SIEM-System (Azure Log Analytics oder Azure Sentinel). Korrigieren Sie Ressourcenberechtigungen, wenn eine unerwünschte Zugriffsebene erkannt wird. |


## <a name="move-to-more-secure-account-types"></a>Wechseln zu sichereren Kontotypen

Microsoft rät davon ab, dass Kunden lokale Benutzerkonten als Dienstkonten verwenden. Überprüfen Sie für jeden Dienst, für den dieser Kontotyp verwendet wird, ob er stattdessen zur Verwendung eines gMSA oder sMSA konfiguriert werden kann.

Außerdem sollten Sie beurteilen, ob der Dienst selbst in Azure migriert werden kann, damit sicherere Dienstkontotypen verwendet werden können. 

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Artikel zum Schützen von Dienstkonten:

* [Einführung in lokale Dienstkonten](service-accounts-on-premises.md)

* [Schützen von gruppenverwalteten Dienstkonten](service-accounts-group-managed.md)

* [Schützen von eigenständigen verwalteten Dienstkonten](service-accounts-standalone-managed.md)

* [Schützen von Computerkonten](service-accounts-computer.md)

* [Schützen von Benutzerkonten](service-accounts-user-on-premises.md)

* [Steuern lokaler Dienstkonten](service-accounts-govern-on-premises.md)

 
