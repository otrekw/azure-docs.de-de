---
title: Schützen von gruppenverwalteten Dienstkonten | Azure Active Directory
description: Leitfaden zum Schützen von gruppenverwalteten Dienstkonten.
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
ms.openlocfilehash: 5c73bcd8fb4c6b594633abd1ac268bd8dfd78202
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100416559"
---
# <a name="securing-group-managed-service-accounts"></a>Schützen von gruppenverwalteten Dienstkonten

Gruppenverwaltete Dienstkonten (Group Managed Service Accounts, gMSAs) sind verwaltete Domänenkonten, die zum Schutz von Diensten verwendet werden. gMSAs können auf einem einzelnen Server oder in einer Serverfarm ausgeführt werden, z. B. in Systemen hinter einem Network Load Balancer (NLB) oder einem IIS-Server (Internet Information Services). Nachdem Sie Ihre Dienste zur Verwendung eines gMSA-Prinzipals konfiguriert haben, erfolgt die Verarbeitung der Kennwortverwaltung in Windows.

## <a name="benefits-of-using-gmsas"></a>Vorteile der Verwendung von gMSAs

gMSAs bieten eine Einzelidentitätslösung mit höherer Sicherheit bei gleichzeitiger Reduzierung des Verwaltungsaufwands durch Folgendes:

* **Festlegen sicherer Kennwörter:** gMSAs verwenden nach dem Zufallsprinzip generierte komplexe Kennwörter mit 240 Byte. Die Komplexität und Länge von gMSA-Kennwörtern minimiert die Wahrscheinlichkeit, dass ein Dienst durch Brute-Force- oder Wörterbuchangriffe kompromittiert wird.

* **Regelmäßiges Rotieren der Kennwörter:** Bei gMSAs wird die Kennwortverwaltung in Windows verlagert, sodass das Kennwort alle 30 Tage geändert wird. Dienst- und Domänenadministratoren müssen keine Kennwortänderungen mehr planen oder Dienstausfälle verwalten, um Dienstkonten zu schützen. 

* **Unterstützung der Bereitstellung in Serverfarmen:** Die Möglichkeit, gMSAs auf mehreren Servern bereitzustellen, ermöglicht die Unterstützung von Lösungen mit Lastenausgleich, bei denen mehrere Hosts denselben Dienst ausführen. 

* **Unterstützung der vereinfachten Verwaltung von Serverprinzipalnamen (SPN):** Sie können den Serverprinzipalnamen zum Zeitpunkt der Kontoerstellung über PowerShell einrichten. Außerdem kann dies durch Dienste, die automatische SPN-Registrierungen unterstützen, für das gMSA durchgeführt werden, wenn die gMSA-Berechtigungen ordnungsgemäß festgelegt sind. 

## <a name="when-to-use-gmsas"></a>Einsatzgebiete für gMSAs

Verwenden Sie gMSAs als bevorzugten Kontotyp für lokale Dienste, es sei denn, dies wird in einem Dienst (z. B. Failoverclustering) nicht unterstützt.

> [!IMPORTANT]
> Sie müssen den Dienst mit gMSAs testen, bevor Sie ihn in der Produktionsumgebung bereitstellen. Richten Sie dazu eine Testumgebung ein, und stellen Sie sicher, dass die Anwendung das gMSA verwenden und auf die erforderlichen Ressourcen zugreifen kann. Weitere Informationen finden Sie unter [Unterstützung für gruppenverwaltete Dienstkonten](https://docs.microsoft.com/system-center/scom/support-group-managed-service-accounts?view=sc-om-2019).


Wenn ein Dienst die Verwendung von gMSAs nicht unterstützt, kann als nächstbeste Alternative ein eigenständiges verwaltetes Dienstkonto (Standalone Managed Service Account, sMSA) verwendet werden. sMSAs bieten die gleichen Funktionen wie ein gMSA, sind jedoch nur für die Bereitstellung auf einem einzelnen Server vorgesehen.

Wenn Sie kein gMSA verwenden können und ein sMSA in Ihrem Dienst nicht unterstützt wird, muss der Dienst so konfiguriert werden, dass er als Standardbenutzerkonto ausgeführt wird. Dienst- und Domänenadministratoren müssen Verwaltungsprozesse für sichere Kennwörter durchführen, um das Konto zu schützen.

## <a name="assess-the-security-posture-of-gmsas"></a>Bewerten des Sicherheitsstatus von gMSAs

gMSAs sind grundsätzlich sicherer als Standardbenutzerkonten, die eine kontinuierliche Kennwortverwaltung erfordern. Bei der Überprüfung des allgemeinen Sicherheitsstatus ist es jedoch wichtig, den Zugriffsbereich von gMSAs zu berücksichtigen.

In der folgenden Tabelle sind potenzielle Sicherheitsprobleme und entsprechende Maßnahmen bei der Verwendung von gMSAs aufgeführt.

| Sicherheitsprobleme| Gegenmaßnahmen |
| - | - |
| Das gMSA ist Mitglied von privilegierten Gruppen. | Überprüfen Sie die Gruppenmitgliedschaften. Dazu können Sie ein PowerShell-Skript erstellen, um alle Gruppenmitgliedschaften aufzulisten, und dann die resultierende CSV-Datei nach den Namen der gMSA-Dateien filtern. <br>Entfernen Sie das gMSA aus privilegierten Gruppen.<br> Weisen Sie dem gMSA nur die Rechte und Berechtigungen zu, die zum Ausführen des zugehörigen Diensts erforderlich sind (wenden Sie sich an den Dienstanbieter). 
| Das gMSA verfügt über Lese-/Schreibzugriff auf vertrauliche Ressourcen. | Überwachen Sie den Zugriff auf vertrauliche Ressourcen. Archivieren Sie Überwachungsprotokolle zur Analyse in einem SIEM-System, z. B. Azure Log Analytics oder Azure Sentinel. Entfernen Sie unnötige Ressourcenberechtigungen, wenn eine unerwünschte Zugriffsebene erkannt wird. |


## <a name="find-gmsas"></a>Suchen von gMSAs

Möglicherweise wurden in Ihrer Organisation bereits gMSAs erstellt. Führen Sie das folgende PowerShell-Cmdlet aus, um diese Konten abzurufen:

Um effektiv verwendet werden zu können, müssen sich gMSAs in der Organisationseinheit (OE) für verwaltete Dienstkonten befinden.

  
![Screenshot der Organisationseinheit des verwalteten Dienstkontos](./media/securing-service-accounts/secure-gmsa-image-1.png)

Um MSAs zu finden, die möglicherweise nicht angezeigt werden, können Sie die folgenden Befehle ausführen.

**So suchen Sie alle Dienstkonten, einschließlich gMSAs und sMSAs**


```powershell

Get-ADServiceAccount -Filter *

# This PowerShell cmdlet will return all Managed Service Accounts (both gMSAs and sMSAs). An administrator can differentiate between the two by examining the ObjectClass attribute on returned accounts.

# For gMSA accounts, ObjectClass = msDS-GroupManagedServiceAccount

# For sMSA accounts, ObjectClass = msDS-ManagedServiceAccount

# To filter results to only gMSAs:

Get-ADServiceAccount –Filter * | where $_.ObjectClass -eq "msDS-GroupManagedServiceAccount”}
```

## <a name="manage-gmsas"></a>Verwalten von gMSAs

Sie können die folgenden PowerShell-Cmdlets für Active Directory zum Verwalten von gMSAs verwenden:

`Get-ADServiceAccount`

`Install-ADServiceAccount`

`New-ADServiceAccount`

`Remove-ADServiceAccount`

`Set-ADServiceAccount`

`Test-ADServiceAccount`

`Uninstall-ADServiceAccount`

> [!NOTE]
> Ab Windows Server 2012 funktionieren die „*-ADServiceAccount“-Cmdlets standardmäßig mit gMSAs. Weitere Informationen zur Verwendung dieser Cmdlets Sie unter [**Erste Schritte mit gruppenverwalteten Dienstkonten**](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts).

## <a name="move-to-a-gmsa"></a>Wechseln zu einem gMSA
gMSAs sind die sicherste Art von Dienstkonten für lokale Anforderungen. Wenn der Wechsel zu einem gMSA möglich ist, sollten Sie dies tun. Außerdem können Sie Ihre Dienste zu Azure und Ihre Dienstkonten zu Azure Active Directory migrieren.

1.  Stellen Sie sicher, dass der [KDS-Stammschlüssel in der Gesamtstruktur bereitgestellt wird](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/create-the-key-distribution-services-kds-root-key). Dies ist ein einmaliger Vorgang.

2. [Erstellen Sie ein neues gMSA.](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)

3. Installieren Sie das neue gMSA auf jedem Host, auf dem der Dienst ausgeführt wird.
   > [!NOTE] 
   > Weitere Informationen zum Erstellen und Installieren von gMSAs auf einem Host, bevor Sie den Dienst zur Verwendung von gMSAs konfigurieren, finden Sie unter [Erste Schritte mit gruppenverwalteten Dienstkonten](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj128431(v=ws.11)).

 
4. Ändern Sie die Dienstidentität in gMSA, und geben Sie ein leeres Kennwort an.

5. Überprüfen Sie, ob der Dienst unter der neuen gMSA-Identität ausgeführt wird.

6. Löschen Sie die alte Identität des Dienstkontos.

 

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Artikel zum Schützen von Dienstkonten:

* [Einführung in lokale Dienstkonten](service-accounts-on-premises.md)

* [Schützen von gruppenverwalteten Dienstkonten](service-accounts-group-managed.md)

* [Schützen von eigenständigen verwalteten Dienstkonten](service-accounts-standalone-managed.md)

* [Schützen von Computerkonten](service-accounts-computer.md)

* [Schützen von Benutzerkonten](service-accounts-user-on-premises.md)

* [Steuern lokaler Dienstkonten](service-accounts-govern-on-premises.md)
