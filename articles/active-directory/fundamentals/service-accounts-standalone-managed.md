---
title: Schützen von eigenständigen verwalteten Dienstkonten | Azure Active Directory
description: Eine Anleitung zum Schützen von eigenständigen verwalteten Dienstkonten.
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
ms.openlocfilehash: e2b3079407774c3d36fe5515b39e964018f9087e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102548851"
---
# <a name="securing-standalone-managed-service-accounts"></a>Schützen von eigenständigen verwalteten Dienstkonten

Eigenständige verwaltete Dienstkonten (Standalone Managed Service Accounts, sMSAs) sind verwaltete Domänenkonten, die zum Schutz eines oder mehrerer Dienste verwendet werden, die auf einem Server ausgeführt werden. Sie können nicht auf mehreren Servern erneut verwendet werden. sMSAs bieten eine automatische Kennwortverwaltung, eine einfachere Verwaltung des Dienstprinzipalnamens und die Möglichkeit, die Verwaltung an andere Administratoren zu delegieren. 

In Active Directory sind sMSAs an einen bestimmten Server gebunden, der einen Dienst ausführt. Diese Konten sind in der Microsoft Management Console im Snap-in „Active Directory-Benutzer und -Computer“ aufgeführt.

![Screenshot: Snap-In „Active Directory-Benutzer und -Computer“ mit der Organisationseinheit „Verwaltete Dienstkonten“](./media/securing-service-accounts/secure-standalone-msa-image-1.png)

Verwaltete Dienstkonten wurden mit Windows Server 2008 R2 und dem Active Directory-Schema eingeführt und setzen als Betriebssystem mindestens Windows Server 2008 R2 voraus. 

## <a name="benefits-of-using-smsas"></a>Vorteile der Verwendung von sMSAs

sMSAs bieten bei der Verwendung als Dienstkonten mehr Sicherheit als Benutzerkonten und reduzieren gleichzeitig den Verwaltungsaufwand durch folgende Features:

* Festlegen sicherer Kennwörter. sMSAs verwenden nach dem Zufallsprinzip generierte komplexe Kennwörter mit 240 Byte. Durch Komplexität und Länge der sMSA-Kennwörter sinkt die Wahrscheinlichkeit, dass ein Dienst durch Brute-Force- oder Wörterbuchangriffe kompromittiert wird.

* Regelmäßiges Wechseln der Kennwörter. Das sMSA-Kennwort wird von Windows automatisch alle 30 Tage geändert. Dienst- und Domänenadministratoren brauchen keine Kennwortänderungen zu planen oder damit verbundene Ausfallzeiten zu verwalten.

* Vereinfachen der Verwaltung von Dienstprinzipalnamen. Dienstprinzipalnamen werden automatisch aktualisiert, wenn Windows Server 2008 R2 die Domänenfunktionsebene ist. Zum Beispiel wird der Dienstprinzipalname in den folgenden Szenarien automatisch aktualisiert:

   * Das Hostcomputerkonto wird umbenannt. 

   * Der DNS-Name des Hostcomputers wird geändert.

   * Ein zusätzlicher Parameter „sam-accountname“ oder „dns-hostname“ wird mithilfe von [PowerShell](/powershell/module/addsadministration/set-adserviceaccount) hinzugefügt oder entfernt.

## <a name="when-to-use-smsas"></a>Wann sind sMSAs zu verwenden?

sMSAs können Verwaltungs- und Sicherheitsaufgaben vereinfachen. Verwenden Sie sMSAs, wenn Sie einen oder mehrere Dienste auf einem einzelnen Server bereitgestellt haben und Sie kein gruppenverwaltetes Dienstkonto (Group Managed Service Account, gMSA) verwenden können. 

> [!NOTE] 
> Sie können sMSAs zwar für mehr als einen Dienst verwenden, zu Überwachungszwecken ist es jedoch empfehlenswert, für jeden Dienst eine eigene Identität zu verwenden. 

Wenn der Ersteller der Software nicht sagen kann, ob Sie ein verwaltetes Dienstkonto verwenden können, müssen Sie die Anwendung testen. Erstellen Sie hierzu eine Testumgebung, und stellen Sie sicher, dass Sie auf alle erforderlichen Ressourcen zugreifen können. Eine schrittweise Anleitung finden Sie unter [Erstellen und Installieren eines sMSAs](/archive/blogs/askds/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting).

### <a name="assess-security-posture-of-smsas"></a>Bewerten des Sicherheitsstatus von sMSAs

sMSAs sind grundsätzlich sicherer als Standardbenutzerkonten, die eine ständige Kennwortverwaltung erfordern. Bei der Überprüfung des allgemeinen Sicherheitsstatus ist es jedoch wichtig, den Zugriffsbereich von sMSAs zu berücksichtigen.

In der folgenden Tabelle wird gezeigt, wie Sie potenzielle Sicherheitsprobleme minimieren können, die von sMSAs verursacht werden.

| Sicherheitsprobleme| Gegenmaßnahmen |
| - | - |
| Das sMSA ist Mitglied von privilegierten Gruppen|Entfernen Sie das sMSA aus Gruppen mit erhöhten Rechten (z. B. Domänenadministratoren). <br> Verwenden Sie das Modell mit den geringsten Berechtigungen, und erteilen Sie dem sMSA nur die Rechte und Berechtigungen, die zum Ausführen des Diensts (oder der Dienste) erforderlich sind. <br> Wenn Sie nicht sicher sind, welche Berechtigungen erforderlich sind, wenden Sie sich an den Ersteller des Diensts. |
| Das sMSA verfügt über Lese-/Schreibzugriff auf vertrauliche Ressourcen.|Überwachen Sie den Zugriff auf vertrauliche Ressourcen. Archivieren Sie Überwachungsprotokolle zur Analyse in einem SIEM-System (Azure Log Analytics oder Azure Sentinel). <br> Korrigieren Sie Ressourcenberechtigungen, wenn eine unerwünschte Zugriffsebene erkannt wird. |
| Standardmäßig beträgt das Kennwortrolloverintervall von sMSAs 30 Tage.| Mit einer Gruppenrichtlinie können Sie die Dauer entsprechend den Sicherheitsanforderungen des Unternehmens optimieren. <br> *Sie können die Gültigkeitsdauer von Kennwörtern über den folgenden Pfad festlegen: <br>Computerkonfiguration\Richtlinien\Windows-Einstellungen\Sicherheitseinstellungen\Sicherheitsoptionen\Domänenmitglied: Maximales Alter von Computerkontenkennwörtern |



### <a name="challenges-with-smsas"></a>Probleme im Zusammenhang mit sMSAs

Im Zusammenhang mit sMSAs gibt es folgende Herausforderungen:

| Herausforderungen| Gegenmaßnahmen |
| - | - |
| Sie können nur auf einem einzelnen Server verwendet werden.| Verwenden Sie gMSAs, wenn Sie das Konto serverübergreifend verwenden müssen. |
| Sie können nicht domänenübergreifend verwendet werden.| Verwenden Sie gMSAs, wenn Sie das Konto domänenübergreifend verwenden müssen. |
| Nicht alle Anwendungen unterstützen sMSAs.| Verwenden Sie nach Möglichkeit gMSAs. Wenn das nicht möglich ist, verwenden Sie ein Standardbenutzerkonto oder ein Computerkonto, wie vom Anwendungsersteller empfohlen. |


## <a name="find-smsas"></a>Suchen nach sMSAs

Führen Sie auf jedem Domänencontroller DSA.msc aus, und erweitern Sie den Container „Verwaltete Dienstkonten“, um alle sMSAs anzuzeigen. 

Mit dem folgenden PowerShell-Befehl werden alle sMSAs und gMSAs in der Active Directory-Domäne zurückgegeben. 

`Get-ADServiceAccount -Filter *`

Mit dem folgenden Befehl werden nur sMSAs in der Active Directory-Domäne zurückgegeben.

`Get-ADServiceAccount -Filter * | where { $_.objectClass -eq "msDS-ManagedServiceAccount" }`

## <a name="manage-smsas"></a>Verwalten von sMSAs

Mit den folgenden Active Directory PowerShell-Cmdlets können Sie sMSAs verwalten:

`Get-ADServiceAccount`

` Install-ADServiceAccount`

` New-ADServiceAccount`

` Remove-ADServiceAccount`

`Set-ADServiceAccount`

`Test-ADServiceAccount`

`Ininstall-ADServiceAccount`

## <a name="move-to-smsas"></a>Wechseln zu sMSAs

Wenn ein Anwendungsdienst ein sMSA, aber kein gMSA unterstützt und derzeit ein Benutzerkonto oder ein Computerkonto für den Sicherheitskontext verwendet wird, [erstellen und installieren Sie ein sMSA](/archive/blogs/askds/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting) auf dem Server. 

Im Idealfall verschieben Sie Ressourcen zu Azure und verwenden verwaltete Azure-Identitäten oder Dienstprinzipale.

 

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Artikel zum Schützen von Dienstkonten:

* [Einführung in lokale Dienstkonten](service-accounts-on-premises.md)

* [Schützen von gruppenverwalteten Dienstkonten](service-accounts-group-managed.md)

* [Schützen von eigenständigen verwalteten Dienstkonten](service-accounts-standalone-managed.md)

* [Schützen von Computerkonten](service-accounts-computer.md)

* [Schützen von Benutzerkonten](service-accounts-user-on-premises.md)

* [Steuern lokaler Dienstkonten](service-accounts-govern-on-premises.md)

