---
title: Diagnostizieren von Problemen mit Windows Virtual Desktop – Azure
description: Verwendung der Diagnosefunktion von Windows Virtual Desktop zum Diagnostizieren von Problemen.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 09/21/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 70676bd1a07acdfcbba071a906b390ed66d70074
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91279857"
---
# <a name="identify-and-diagnose-windows-virtual-desktop-issues"></a>Identifizieren und Diagnostizieren von Problemen mit Windows Virtual Desktop

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/diagnostics-role-service-2019.md).

Windows Virtual Desktop verfügt über eine Diagnosefunktion, mit der der Administrator Probleme über eine zentrale Benutzeroberfläche identifizieren kann. Weitere Informationen zu den Diagnosefunktionen von Windows Virtual Desktop finden Sie unter [Verwenden von Log Analytics für die Diagnosefunktion](diagnostics-log-analytics.md).

Verbindungen, die Windows Virtual Desktop nicht erreichen, werden nicht in den Diagnoseergebnissen angezeigt, da der Diagnoserollendienst selbst Teil von Windows Virtual Desktop ist. Windows Virtual Desktop-Verbindungsprobleme können auftreten, wenn es für den Endbenutzer zu Problemen mit der Netzwerkkonnektivität kommt.

## <a name="common-error-scenarios"></a>Allgemeine Fehlerszenarien

In der Tabelle „WVDErrors“ werden Fehler für alle Aktivitätstypen nachverfolgt. Die Spalte „ServiceError“ enthält ein zusätzliches Flag („True“ oder „False“). Dieses Flag gibt Aufschluss darüber, ob der Fehler mit dem Dienst zusammenhängt.

* Lautet der Wert „True“, hat das Dienstteam dieses Problem möglicherweise bereits untersucht. Falls sich das Problem auf die Benutzerfreundlichkeit auswirkt und häufig auftritt, empfiehlt es sich, ein Supportticket für Windows Virtual Desktop zu erstellen.
* Lautet der Wert „False“, handelt es sich möglicherweise um eine Fehlkonfiguration, die Sie selbst beheben können. Die Fehlermeldung kann Aufschluss über einen möglichen Ansatzpunkt geben.

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

### <a name="connection-error-codes"></a>Verbindungsfehlercodes

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

## <a name="error-cant-add-user-assignments-to-an-app-group"></a>Error: App-Gruppen können keine Benutzerzuweisungen hinzugefügt werden

Nach dem Zuweisen eines Benutzers zu einer App-Gruppe zeigt das Azure-Portal die Warnung „Sitzungsende“ oder „Probleme bei der Authentifizierung – Erweiterung Microsoft_Azure_WVD“ an. Die Zuweisungsseite wird nicht geladen, und danach werden die Seiten im gesamten Azure-Portal nicht mehr geladen (z. B. Azure Monitor, Log Analytics, Service Health).

**Ursache:** Es liegt ein Problem mit der Richtlinie für bedingten Zugriff vor. Das Azure-Portal versucht, ein Token für Microsoft Graph abzurufen, das von SharePoint Online abhängig ist. Der Kunde besitzt eine Richtlinie für bedingten Zugriff namens „Microsoft Office 365 Data Storage Terms of Use“ (Nutzungsbedingungen für den Microsoft Office 365-Datenspeicher), die voraussetzt, dass der Benutzer die Nutzungsbedingungen akzeptiert, um auf den Datenspeicher zugreifen zu können. Er ist jedoch noch nicht angemeldet, sodass das Azure-Portal das Token nicht abrufen kann.

**Behebung:** Vor der Anmeldung beim Azure-Portal muss sich der Administrator zuerst bei SharePoint anmelden und die Nutzungsbedingungen akzeptieren. Danach sollte dieser sich wie gewohnt beim Azure-Portal anmelden können.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Rollen in Windows Virtual Desktop finden Sie unter [Windows Virtual Desktop-Umgebung](environment-setup.md).

Eine Liste mit verfügbaren PowerShell-Cmdlets für Windows Virtual Desktop finden Sie in der [PowerShell-Referenz](/powershell/windows-virtual-desktop/overview).
