---
title: Diagnostizieren von Problemen mit Windows Virtual Desktop – Azure
description: Verwendung der Diagnosefunktion von Windows Virtual Desktop zum Diagnostizieren von Problemen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: cffc6393ef6f5c1a33be615d9d5d4b8729ab711f
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611856"
---
# <a name="identify-and-diagnose-issues"></a>Identifizieren und Diagnostizieren von Problemen

>[!IMPORTANT]
>Dieser Inhalt gilt für das Update vom Frühjahr 2020 mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/diagnostics-role-service-2019.md).
>
> Das Windows Virtual Desktop-Update vom Frühjahr 2020 befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. 
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Windows Virtual Desktop verfügt über eine Diagnosefunktion, mit der der Administrator Probleme über eine zentrale Benutzeroberfläche identifizieren kann. Weitere Informationen zu den Diagnosefunktionen von Windows Virtual Desktop finden Sie unter [Verwenden von Log Analytics für die Diagnosefunktion](diagnostics-log-analytics.md).
  
Verbindungen, die Windows Virtual Desktop nicht erreichen, werden nicht in den Diagnoseergebnissen angezeigt, da der Diagnoserollendienst selbst Teil von Windows Virtual Desktop ist. Windows Virtual Desktop-Verbindungsprobleme können auftreten, wenn es für den Endbenutzer zu Problemen mit der Netzwerkkonnektivität kommt.

## <a name="common-error-scenarios"></a>Allgemeine Fehlerszenarien

Fehlerszenarien werden in die Bereiche „Dienstintern“ und „Außerhalb von Windows Virtual Desktop“ (extern) kategorisiert.

* Internes Problem: Betrifft Szenarien, die nicht vom Kunden gelöst werden können und als Supportfall behandelt werden müssen. Wenn Sie Feedback über die [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) bereitstellen, geben Sie die Korrelations-ID sowie den ungefähren Zeitrahmen an, in dem das Problem aufgetreten ist.
* Externes Problem: Betrifft Szenarien, die vom Kunden gelöst werden können. Dies erfolgt außerhalb von Windows Virtual Desktop.

In der folgenden Tabelle sind häufige Fehler aufgeführt, die für Ihre Administratoren ggf. auftreten können.

>[!NOTE]
>Diese Liste enthält die häufigsten Fehler und wird in regelmäßigen Abständen aktualisiert. Damit Sie immer über die aktuellen Informationen verfügen, ist es ratsam, diesen Artikel mindestens einmal pro Monat aufzurufen.

## <a name="management-errors"></a>Verwaltungsfehler

|Fehlermeldung|Vorgeschlagene Lösung|
|---|---|
|Fehler beim Erstellen des Registrierungsschlüssels |Es konnte kein Registrierungstoken erstellt werden. Versuchen Sie, es mit einer kürzeren Ablaufzeit (zwischen 1 Stunde und 1 Monat) erneut zu erstellen. |
|Fehler beim Löschen des Registrierungsschlüssels|Das Registrierungstoken konnte nicht gelöscht werden. Versuchen Sie, es erneut zu löschen. Wenn dies immer noch nicht funktioniert, überprüfen Sie mithilfe von PowerShell, ob das Token immer noch vorhanden ist. Wenn dies der Fall ist, löschen Sie es mit PowerShell.|
|Fehler beim Ändern des Ausgleichsmodus für den Sitzungshost |Der Ausgleichsmodus für die VM konnte nicht geändert werden. Überprüfen Sie den VM-Status. Wenn die VM nicht verfügbar ist, kann der Ausgleichsmodus nicht geändert werden.|
|Fehler beim Trennen von Benutzersitzungen |Die Verbindung des Benutzers mit der VM konnte nicht getrennt werden. Überprüfen Sie den VM-Status. Wenn die VM nicht verfügbar ist, kann die Benutzersitzung nicht getrennt werden. Wenn die VM verfügbar ist, überprüfen Sie den Status der Benutzersitzung, um festzustellen, ob sie getrennt ist. |
|Fehler beim Abmelden aller Benutzer im Sitzungshost |Benutzer konnten nicht von der VM abgemeldet werden. Überprüfen Sie den VM-Status. Wenn sie nicht verfügbar ist, können Benutzer nicht abgemeldet werden. Prüfen Sie den Status der Benutzersitzung, um festzustellen, ob die Benutzer bereits abgemeldet sind. Sie können die Abmeldung mit PowerShell erzwingen. |
|Fehler beim Aufheben der Zuweisung des Benutzers zur Anwendungsgruppe|Die Veröffentlichung einer App-Gruppe für einen Benutzer konnte nicht aufgehoben werden. Überprüfen Sie, ob der Benutzer in Azure AD verfügbar ist. Überprüfen Sie, ob der Benutzer einer Benutzergruppe angehört, in der die App-Gruppe veröffentlicht wurde. |
|Fehler beim Abrufen der verfügbaren Standorte |Überprüfen Sie den Standort der VM, der im Assistenten zum Erstellen von Hostpools verwendet wurde. Wenn das Image an diesem Standort nicht verfügbar ist, fügen Sie dort ein Image hinzu oder wählen einen anderen VM-Standort aus. |

### <a name="external-connection-error-codes"></a>Externe Verbindung: Fehlercodes

|Numerischer Code|Fehlercode|Vorgeschlagene Lösung|
|---|---|---|
|-2147467259|ConnectionFailedAdTrustedRelationshipFailure|Der Sitzungshost ist nicht richtig in Active Directory eingebunden.|
|-2146233088|ConnectionFailedUserHasValidSessionButRdshIsUnhealthy|Bei der Verbindungsherstellung ist ein Fehler aufgetreten, weil der Sitzungshost nicht verfügbar ist. Überprüfen Sie die Integrität des Sitzungshosts.|
|-2146233088|ConnectionFailedClientDisconnect|Wenn dieser Fehler häufig angezeigt wird, sollten Sie sicherstellen, dass der Computer des Benutzers mit dem Netzwerk verbunden ist.|
|-2146233088|ConnectionFailedNoHealthyRdshAvailable|Die Sitzung, mit der der Hostbenutzer eine Verbindung herstellen möchte, ist nicht fehlerfrei. Debuggen Sie den virtuellen Computer.|
|-2146233088|ConnectionFailedUserNotAuthorized|Der Benutzer verfügt nicht über die Berechtigung zum Zugreifen auf die veröffentlichte App oder den Desktop. Der Fehler wird unter Umständen angezeigt, nachdem der Administrator die veröffentlichten Ressourcen entfernt hat. Bitten Sie den Benutzer, den Feed in der Remotedesktopanwendung zu aktualisieren.|
|2|FileNotFound|Die Anwendung, auf die der Benutzer zugreifen möchte, ist entweder falsch installiert oder auf einen falschen Pfad festgelegt.<br><br>Wenn neue Apps veröffentlicht werden, während der Benutzer eine aktive Sitzung ausführt, kann der Benutzer nicht auf diese App zugreifen. Die Sitzung muss beendet und neu gestartet werden, bevor der Benutzer auf die App zugreifen kann. |
|3|InvalidCredentials|Der vom Benutzer eingegebene Benutzername oder das Kennwort stimmt nicht mit vorhandenen Benutzernamen bzw. Kennwörtern überein. Überprüfen Sie die Anmeldeinformationen auf Tippfehler, und wiederholen Sie den Vorgang.|
|8|ConnectionBroken|Die Verbindung zwischen dem Client und dem Gateway oder Server wurde getrennt. Sofern dieser Vorgang nicht unerwartet auftritt, ist keine Aktion erforderlich.|
|14|UnexpectedNetworkDisconnect|Die Verbindung mit dem Netzwerk wurde getrennt. Bitten Sie den Benutzer, die Verbindung wiederherzustellen.|
|24|ReverseConnectFailed|Der virtuelle Hostcomputer verfügt nicht über eine direkte Sichtlinie zum RD-Gateway. Stellen Sie sicher, dass die IP-Adresse des Gateways aufgelöst werden kann.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Rollen in Windows Virtual Desktop finden Sie unter [Windows Virtual Desktop-Umgebung](environment-setup.md).

Eine Liste mit verfügbaren PowerShell-Cmdlets für Windows Virtual Desktop finden Sie in der [PowerShell-Referenz](/powershell/windows-virtual-desktop/overview).
