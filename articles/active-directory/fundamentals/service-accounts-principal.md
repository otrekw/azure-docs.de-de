---
title: Schützen von Dienstprinzipalen in Azure Active Directory
description: Es wird beschrieben, wie Sie Dienstprinzipale suchen, bewerten und schützen können.
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
ms.openlocfilehash: 297c0a53fb2ab4ee0b2c5291cabf5a63c8841664
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105604701"
---
# <a name="securing-service-principals"></a>Sichern von Dienstprinzipalen

Bei einem Azure AD-[Dienstprinzipal](../develop/app-objects-and-service-principals.md) (Azure Active Directory) handelt es sich um die lokale Darstellung eines Anwendungsobjekts in einem einzelnen Mandanten oder Verzeichnis.  ‎Er dient als Identität der Anwendungsinstanz. Mit Dienstprinzipalen wird definiert, wer auf die Anwendung zugreifen kann und auf welche Ressourcen die Anwendung Zugriff hat. Ein Dienstprinzipal wird in jedem Mandanten erstellt, in dem die Anwendung verwendet wird, und es verweist auf das global eindeutige Anwendungsobjekt. Mit dem Mandanten werden die Anmeldung des Dienstprinzipals und den Zugriff auf Ressourcen geschützt.  

### <a name="tenant-service-principal-relationships"></a>Beziehungen zwischen Mandanten und Dienstprinzipalen
Eine Anwendung mit nur einem Mandanten verfügt auf diesem Basismandanten nur über einen Dienstprinzipal. Für eine mehrinstanzenfähige Webanwendung oder API wird ein Dienstprinzipal auf jedem Mandanten benötigt. Ein Dienstprinzipal wird erstellt, wenn ein Benutzer des entsprechenden Mandanten seine Einwilligung zur Verwendung der Anwendung bzw. API erteilt hat. Aufgrund dieser Einwilligung wird eine 1:n-Beziehung zwischen der mehrinstanzenfähigen Anwendung und den zugehörigen Dienstprinzipalen erstellt.

Eine mehrinstanzenfähige Anwendung verfügt über einen Mandanten als Basis und kann zusätzlich Instanzen auf anderen Mandanten aufweisen. Die meisten SaaS-Anwendungen (Software-as-a-Service) sind für Mehrinstanzenfähigkeit konzipiert. Verwenden Sie Dienstprinzipale, um sicherzustellen, dass für die Anwendung und ihre Benutzer sowohl bei Verwendung eines einzelnen als auch von mehreren Mandanten der passende Sicherheitsstatus gilt.

## <a name="applicationid-and-objectid"></a>ApplicationID und ObjectID

Eine bestimmte Anwendungsinstanz verfügt über zwei Eigenschaften: die ApplicationID (auch als ClientID bezeichnet) und die ObjectID.

> [!NOTE] 
> Es scheint so zu sein, als ob die Begriffe „Anwendung“ und „Dienstprinzipal“ austauschbar wären, wenn im Kontext von Authentifizierungsaufgaben grob auf eine Anwendung verwiesen wird. Es handelt sich hierbei aber um zwei unterschiedliche Darstellungen von Anwendungen in Azure AD.
 

Die ApplicationID steht für die globale Anwendung und ist für alle Anwendungsinstanzen mandantenübergreifend identisch. Die ObjectID ist ein eindeutiger Wert für ein Anwendungsobjekt und repräsentiert den Dienstprinzipal. Wie bei Benutzern, Gruppen und anderen Ressourcen auch, dient die ObjectID als Hilfe beim eindeutigen Identifizieren einer Anwendungsinstanz in Azure AD.

Ausführlichere Informationen zu diesem Thema finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](../develop/app-objects-and-service-principals.md).

Sie können auch Azure PowerShell, die Azure CLI, Microsoft Graph, das Azure-Portal und andere Tools verwenden, um eine Anwendung und das zugehörige Dienstprinzipalobjekt (ObjectID) auf einem Mandanten zu erstellen. 

![Screenshot: Neue Anwendungsregistrierung mit hervorgehobenen Feldern mit ApplicationID und ObjectID](./media/securing-service-accounts/secure-principal-image-1.png)

## <a name="service-principal-authentication"></a>Dienstprinzipalauthentifizierung

Es gibt zwei Mechanismen für die Authentifizierung mithilfe von Dienstprinzipalen: Clientzertifikate und geheime Clientschlüssel. 

![ Screenshot: Seite „Neue App“ mit hervorgehobenen Bereichen für Zertifikate und geheime Clientschlüssel](./media/securing-service-accounts/secure-principal-certificates.png)

Zertifikate sind sicherer: Verwenden Sie nach Möglichkeit Clientzertifikate. Im Gegensatz zu geheimen Clientschlüsseln können Clientzertifikate nicht versehentlich in Code eingebettet werden. Nutzen Sie nach Möglichkeit Azure Key Vault für die Verwaltung von Zertifikaten und Geheimnissen, um die folgenden Ressourcen zu verschlüsseln, indem Sie durch Hardwaresicherheitsmodule geschützte Schlüssel verwenden:

* Authentifizierungsschlüssel

* Speicherkontoschlüssel

* Datenverschlüsselungsschlüssel

* PFX-Dateien

* Kennwörter 

Weitere Informationen zu Azure Key Vault und zur Verwendung für die Verwaltung von Zertifikaten und Schlüsseln finden Sie unter [Informationen zu Azure Key Vault](../../key-vault/general/overview.md) und [Zuweisen einer Key Vault-Zugriffsrichtlinie über das Azure-Portal](../../key-vault/general/assign-access-policy-portal.md). 

 ### <a name="challenges-and-mitigations"></a>Herausforderungen und Gegenmaßnahmen
Die folgende Tabelle enthält Gegenmaßnahmen für Herausforderungen, die bei der Verwendung von Dienstprinzipalen ggf. bewältigt werden müssen.


| Herausforderungen| Gegenmaßnahmen |
| - | - |
| Zugriffsüberprüfungen für Dienstprinzipale, die privilegierten Rollen zugewiesen sind| Diese Funktion befindet sich in der Vorschauphase und ist noch nicht flächendeckend verfügbar. |
| Überprüfung des Zugriffs von Dienstprinzipalen| Ermöglicht die manuelle Überprüfung der Zugriffssteuerungsliste einer Ressource über das Azure-Portal. |
| Dienstprinzipale mit zu vielen Berechtigungen| Geben Sie beim Erstellen von Automatisierungsdienstkonten oder Dienstprinzipalen nur die Berechtigungen an, die für die jeweilige Aufgabe benötigt werden. Evaluieren Sie die vorhandenen Dienstprinzipale, um zu ermitteln, ob Sie die Berechtigungen verringern können. |
|Ermittlung von Änderungen an den Anmeldeinformationen oder Authentifizierungsmethoden von Dienstprinzipalen |Verwenden Sie die Arbeitsmappe „Bericht zu vertraulichen Vorgängen“ als Hilfe beim Beheben dieses Problems. Eine Beschreibung finden Sie in [diesem Blogbeitrag](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/azure-ad-workbook-to-help-you-assess-solorigate-risk/ba-p/2010718).|

## <a name="find-accounts-using-service-principals"></a>Suchen nach Konten mit Dienstprinzipalen
Führen Sie die folgenden Befehle aus, um nach Konten mit Dienstprinzipalen zu suchen.

Verwenden der Azure-Befehlszeilenschnittstelle


`az ad sp list`

PowerShell

`Get-AzureADServicePrincipal -All:$true` 


Weitere Informationen finden Sie unter [Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal).

## <a name="assess-service-principal-security"></a>Bewerten der Sicherheit von Dienstprinzipalen

Stellen Sie sicher, dass Sie die Speicherung von Berechtigungen und Anmeldeinformationen evaluieren, um die Sicherheit Ihrer Dienstprinzipale zu bewerten.

Verwenden Sie die folgenden Informationen, um Gegenmaßnahmen für potenzielle Herausforderungen zu ergreifen.

|Herausforderungen | Gegenmaßnahmen|
| - | - |
| Ermitteln des Benutzers, der seine Einwilligung für eine mehrinstanzenfähige App erteilt hat, und Ermitteln von unrechtmäßigen Einwilligungserteilungen für eine mehrinstanzenfähige App | Führen Sie den folgenden PowerShell-Befehl aus, um nach mehrinstanzenfähigen Apps zu suchen.<br>`Get-AzureADServicePrincipal -All:$true ? {$_.Tags -eq WindowsAzureActiveDirectoryIntegratedApp"}`<br>Deaktivieren Sie die Benutzereinwilligung. <br>Zulassen der Benutzereinwilligung von verifizierten Herausgebern für ausgewählte Berechtigungen (empfohlen) <br> Verwenden Sie den bedingten Zugriff, um Dienstprinzipale von nicht vertrauenswürdigen Orten zu blockieren. Führen Sie die Konfiguration im Rahmen des Benutzerkontexts durch. Die zugehörigen Token sollten verwendet werden, um den Dienstprinzipal auszulösen.|
|Verwenden eines hartcodierten gemeinsamen geheimen Schlüssels in einem Skript mithilfe eines Dienstprinzipals.|Verwenden Sie ein Zertifikat oder Azure Key Vault.|
|Nachverfolgen, von wem das Zertifikat oder Geheimnis verwendet wird| Überwachen Sie die Anmeldungen für den Dienstprinzipal, indem Sie die Azure AD-Anmeldeprotokolle verwenden.|
Die Anmeldung von Dienstprinzipalen kann nicht per bedingtem Zugriff verwaltet werden.| Überwachen der Anmeldungen über die Azure AD-Anmeldeprotokolle
| Die standardmäßige Azure RBAC-Rolle lautet „Mitwirkender“. |Evaluieren Sie die Anforderungen, und wenden Sie die Rolle mit den geringstmöglichen Berechtigungen an, um diese Anforderungen zu erfüllen.|

## <a name="move-from-a-user-account-to-a-service-principal"></a>Wechseln von einem Benutzerkonto zu einem Dienstprinzipal  
‎Wenn Sie ein Azure-Benutzerkonto als Dienstprinzipal verwenden, sollten Sie evaluieren, ob die Umstellung auf eine [verwaltete Identität](../../app-service/overview-managed-identity.md?tabs=dotnet) oder einen Dienstprinzipal möglich ist. Falls Sie keine verwaltete Identität nutzen können, sollten Sie einen Dienstprinzipal bereitstellen, der über gerade ausreichende Berechtigungen und den entsprechenden Bereich für die Ausführung der erforderlichen Aufgaben verfügt. Sie können einen Dienstprinzipal erstellen, indem Sie eine [Anwendung registrieren](../develop/howto-create-service-principal-portal.md) oder [PowerShell](../develop/howto-authenticate-service-principal-powershell.md) verwenden.

Informieren Sie sich bei Verwendung von Microsoft Graph in der Dokumentation zur jeweiligen API ([wie in diesem Beispiel](/powershell/azure/create-azure-service-principal-azureps)), ‎und vergewissern Sie sich, dass der Berechtigungstyp für die Anwendung unterstützt wird.

## <a name="next-steps"></a>Nächste Schritte

**Weitere Informationen zu Dienstprinzipalen:**

[Erstellen eines Dienstprinzipals](../develop/howto-create-service-principal-portal.md)

 [Überwachen der Anmeldungen von Dienstprinzipalen](../reports-monitoring/concept-sign-ins.md#sign-ins-report)

**Weitere Informationen zum Schützen von Dienstkonten:**

[Einführung in Azure-Dienstkonten](service-accounts-introduction-azure.md)

[Sichern von verwalteten Identitäten](service-accounts-managed-identities.md)

[Steuern von Azure-Dienstkonten](service-accounts-governing-azure.md)

[Einführung in lokale Dienstkonten](service-accounts-on-premises.md)
