---
title: Problembehandlung bei der Herausgeberüberprüfung – Microsoft Identity Platform | Azure
description: Hier wird die Behandlung von Problemen bei der Herausgeberüberprüfung (Vorschauversion) für Microsoft Identity Platform durch Aufrufen von Microsoft Graph-APIs beschrieben.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: troubleshooting
ms.workload: identity
ms.date: 05/08/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jesakowi
ms.openlocfilehash: 3069e3caf81d9bb2f809b21c88383c419e3b90b3
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87282975"
---
# <a name="troubleshoot-publisher-verification-preview"></a>Problembehandlung bei der Herausgeberüberprüfung (Vorschauversion)
Wenn Sie den Vorgang nicht ausführen können oder bei der [Herausgeberüberprüfung (Vorschauversion)](publisher-verification-overview.md) unerwartetes Verhalten auftritt, sollten Sie zunächst die folgenden Schritte ausführen, wenn Sie Fehlermeldungen erhalten oder unerwartetes Verhalten feststellen: 

1. Überprüfen Sie die [Anforderungen](publisher-verification-overview.md#requirements), und stellen Sie sicher, dass alle Anforderungen erfüllt sind.

1. Lesen Sie die Anweisungen zum [Markieren einer App als durch den Herausgeber verifiziert](mark-app-as-publisher-verified.md), und stellen Sie sicher, dass alle Schritte erfolgreich ausgeführt wurden.

1. Sehen Sie sich die Liste [häufiger Probleme](#common-issues) an.

1. Reproduzieren Sie die Anforderung mit dem [Graph-Tester](#making-microsoft-graph-api-calls), um zusätzliche Informationen zu sammeln und Probleme auf der Benutzeroberfläche auszuschließen.

## <a name="common-issues"></a>Häufige Probleme
Im Folgenden finden Sie einige häufige Probleme, die während des Vorgangs auftreten können. 

- **Ich kenne meine Microsoft Partner Network-ID (MPN-ID) nicht, oder ich weiß nicht, wer der Hauptkontakt für das Konto ist** 
    1. Navigieren Sie zur [Seite für die MPN-Registrierung](https://partner.microsoft.com/dashboard/account/v3/enrollment/joinnow/basicpartnernetwork/new).
    1. Melden Sie sich mit einem Benutzerkonto im primären Azure AD-Mandanten der Organisation an. 
    1. Wenn bereits ein MPN-Konto vorhanden ist, wird dies erkannt, und Sie werden dem Konto hinzugefügt. 
    1. Navigieren Sie zur [Partnerprofilseite](https://partner.microsoft.com/pcv/accountsettings/connectedpartnerprofile), auf der die MPN-ID und der Hauptkontakt für das Konto aufgeführt sind.

- **Ich weiß nicht, wer mein globaler Azure AD-Administrator (auch als Unternehmensadministrator oder Mandantenadministrator bezeichnet) ist. Wie finde ich diese Informationen? Wie sieht es mit dem App-Administrator oder einer anderen Administratorrolle aus?**
    1. Melden Sie sich mit einem Benutzerkonto im primären Mandanten Ihrer Organisation beim [Azure AD-Portal](https://aad.portal.azure.com) an.
    1. Navigieren Sie zu [Rollenverwaltung](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators).
    1. Klicken Sie auf „Globaler Administrator“ oder auf die gewünschte Administratorrolle.
    1. Es wird eine Liste der Benutzer angezeigt, denen diese Rolle zugewiesen ist.

- **Ich weiß nicht, wer Administrator für mein MPN-Konto ist** Navigieren Sie zur [Seite für die MPN-Benutzerverwaltung](https://partner.microsoft.com/pcv/users), und filtern Sie die Benutzerliste, um die Benutzer in verschiedenen Administratorrollen anzuzeigen.

- **Ich erhalte eine Fehlermeldung, die besagt, dass meine MPN-ID ungültig ist oder dass ich keinen Zugriff darauf habe.**
    1. Navigieren Sie zu Ihrem [Partnerprofil](https://partner.microsoft.com/pcv/accountsettings/connectedpartnerprofile), und stellen Sie Folgendes sicher: 
        - Dass die MPN-ID korrekt ist. 
        - Dass keine Fehler oder „ausstehenden Aktionen“ angezeigt werden und der Überprüfungsstatus unter „Rechtliches Geschäftsprofil“ und „Partnerinformationen“ jeweils „autorisiert“ oder „erfolgreich“ lauten.
    1. Navigieren Sie zur [Seite für die MPN-Mandantenverwaltung](https://partner.microsoft.com/dashboard/account/v3/tenantmanagement), und vergewissern Sie sich, dass der Mandant, in dem die App registriert ist und von dem Sie sich mit einem Benutzerkonto anmelden, in der Liste der zugeordneten Mandanten aufgeführt ist.
    1. Navigieren Sie zur [Seite für die MPN-Benutzerverwaltung](https://partner.microsoft.com/pcv/users), und vergewissern Sie sich, dass der Benutzer, unter dem Sie sich anmelden, entweder „Globaler Administrator“, „MPN-Administrator“ oder „Kontoadministrator“ ist.

- **Wenn ich mich beim Azure AD-Portal anmelde, werden mir keine registrierten Apps angezeigt. Warum?** 
    Möglicherweise wurden Ihre App-Registrierungen mit einem anderen Benutzerkonto oder in einem anderen Mandanten erstellt. Stellen Sie sicher, dass Sie mit dem richtigen Konto in dem Mandanten angemeldet sind, in dem Ihre App-Registrierungen erstellt wurden.

- **Wie finde ich heraus, wer der Besitzer einer bei Azure AD registrierten App ist?** 
    Wenn Sie bei einem Mandanten angemeldet sind, in dem die App registriert ist, navigieren Sie zum Blatt „App-Registrierungen“, klicken Sie auf eine App, und klicken Sie dann auf „Besitzer“.

## <a name="making-microsoft-graph-api-calls"></a>Ausführen von Microsoft Graph-API-Aufrufen 

Wenn ein Problem aufgetreten ist, Sie aber basierend auf dem, was auf der Benutzeroberfläche angezeigt wird, nicht verstehen können, warum, kann es hilfreich sein, eine weitere Problembehandlung vorzunehmen, indem Sie mithilfe von Microsoft Graph-Aufrufen dieselben Vorgänge ausführen, die Sie auch im App-Registrierungsportal ausführen können. Während der Vorschauphase sind diese APIs nur am /Beta-Endpunkt von Microsoft Graph verfügbar.  

Am einfachsten können diese Anforderungen mit dem [Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) ausgeführt werden. Sie können auch andere Optionen in Erwägung ziehen und z. B. [Postman](https://www.postman.com/) oder PowerShell verwenden, um eine [Webanforderung aufzurufen](/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7).  

Sie können Microsoft Graph verwenden, um den verifizierten Herausgeber Ihrer App festzulegen und zu löschen und das Ergebnis zu überprüfen, nachdem Sie einen dieser Vorgänge ausgeführt haben. Das Ergebnis kann sowohl für das Ihrer App-Registrierung entsprechende [Anwendungsobjekt](/graph/api/resources/application?view=graph-rest-beta) als auch für alle von dieser App instanziierten [Dienstprinzipale](/graph/api/resources/serviceprincipal?view=graph-rest-beta) angezeigt werden. Weitere Informationen zur Beziehung zwischen diesen Objekten finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](app-objects-and-service-principals.md).  

Nachfolgend finden Sie einige nützliche Anforderungen:  

### <a name="set-verified-publisher"></a>Festlegen des verifizierten Herausgebers 

Anforderung

```
POST /applications/0cd04273-0d11-4e62-9eb3-5c3971a7cbec/setVerifiedPublisher 

{ 

    "verifiedPublisherId": "12345678" 

} 
```
 
Antwort 
```
204 No Content 
```
> [!NOTE]
> *verifiedPublisherID* ist Ihre MPN-ID. 

### <a name="unset-verified-publisher"></a>Löschen des verifizierten Herausgebers 

Anforderung:  
```
POST /applications/0cd04273-0d11-4e62-9eb3-5c3971a7cbec/unsetVerifiedPublisher 
```
 
Antwort 
```
204 No Content 
```
### <a name="get-verified-publisher-info-from-application"></a>Abrufen von Informationen des verifizierten Herausgebers von der Anwendung 
 
```
GET https://graph.microsoft.com/beta/applications/0cd04273-0d11-4e62-9eb3-5c3971a7cbec 

HTTP/1.1 200 OK 

{ 
    "id": "0cd04273-0d11-4e62-9eb3-5c3971a7cbec", 

    ... 

    "verifiedPublisher" : { 
        "displayName": "myexamplePublisher", 
        "verifiedPublisherId": "12345678", 
        "addedDateTime": "2019-12-10T00:00:00" 
    } 
} 
```

### <a name="get-verified-publisher-info-from-service-principal"></a>Abrufen von Informationen des verifizierten Herausgebers vom Dienstprinzipal 
```
GET https://graph.microsoft.com/beta/servicePrincipals/010422a7-4d77-4f40-9335-b81ef5c23dd4 

HTTP/1.1 200 OK 

{ 
    "id": "010422a7-4d77-4f40-9335-b81ef5c22dd4", 

    ... 

    "verifiedPublisher" : { 
        "displayName": "myexamplePublisher", 
        "verifiedPublisherId": "12345678", 
        "addedDateTime": "2019-12-10T00:00:00" 
    } 
} 
```

## <a name="error-reference"></a>Fehlerreferenz 

Nachfolgend finden Sie eine Liste möglicher Fehlercodes, die Sie entweder bei der Problembehandlung mit Microsoft Graph oder beim Durchlaufen des Prozesses im App-Registrierungsportal erhalten können.

### <a name="mpnaccountnotfoundornoaccess"></a>MPNAccountNotFoundOrNoAccess     

Die von Ihnen angegebene MPN-ID (<MPNID>) ist nicht vorhanden, oder Sie haben keinen Zugriff darauf. Geben Sie eine gültige MPN-ID an, und versuchen Sie es noch mal. 

### <a name="mpnglobalaccountnotfound"></a>MPNGlobalAccountNotFound     

Die von Ihnen angegebene MPN-ID (<MPNID>) ist ungültig. Geben Sie eine gültige MPN-ID an, und versuchen Sie es noch mal. 

### <a name="mpnaccountinvalid"></a>MPNAccountInvalid    

Die von Ihnen angegebene MPN-ID (<MPNID>) ist ungültig. Geben Sie eine gültige MPN-ID an, und versuchen Sie es noch mal. 

### <a name="mpnaccountnotvetted"></a>MPNAccountNotVetted  

Für die von Ihnen angegebene MPN-ID (<MPNID>) wurde der Überprüfungsvorgang nicht abgeschlossen. Führen Sie diesen Vorgang in Partner Center aus, und versuchen Sie es noch mal. 

### <a name="nopublisheridonassociatedmpnaccount"></a>NoPublisherIdOnAssociatedMPNAccount  

Die von Ihnen angegebene MPN-ID (<MPNID>) ist ungültig. Geben Sie eine gültige MPN-ID an, und versuchen Sie es noch mal. 

### <a name="mpniddoesnotmatchassociatedmpnaccount"></a>MPNIdDoesNotMatchAssociatedMPNAccount    

Die von Ihnen angegebene MPN-ID (<MPNID>) ist ungültig. Geben Sie eine gültige MPN-ID an, und versuchen Sie es noch mal. 

### <a name="applicationnotfound"></a>ApplicationNotFound  

Die Zielanwendung (<AppId>) konnte nicht gefunden werden. Geben Sie eine gültige Anwendungs-ID an, und versuchen Sie es noch mal. 

### <a name="b2ctenantnotallowed"></a>B2CTenantNotAllowed  

Diese Funktion wird in einem Azure AD B2C-Mandanten nicht unterstützt. 

### <a name="emailverifiedtenantnotallowed"></a>EmailVerifiedTenantNotAllowed    

Diese Funktion wird in einem per E-Mail verifizierten Mandanten nicht unterstützt. 

### <a name="nopublisherdomainonapplication"></a>NoPublisherDomainOnApplication   

Für die Zielanwendung (<AppId>) muss eine Herausgeberdomäne festgelegt sein. Legen Sie eine Herausgeberdomäne fest, und versuchen Sie es noch mal. 

### <a name="publisherdomainisnotdnsverified"></a>PublisherDomainIsNotDNSVerified  

Die Herausgeberdomäne (<publisherDomain>) der Zielanwendung ist keine überprüfte Domäne in diesem Mandanten. Überprüfen Sie eine Mandantendomäne mithilfe der DNS-Überprüfung, und versuchen Sie es noch mal. 

### <a name="publisherdomainmismatch"></a>PublisherDomainMismatch  

Die Herausgeberdomäne (<publisherDomain>) der Zielanwendung stimmt nicht mit der zum Durchführen der E-Mail-Überprüfung in Partner Center verwendeten Domäne (<pcDomain>) überein. Stellen Sie sicher, dass diese Domänen übereinstimmen, und versuchen Sie es noch mal. 

### <a name="notauthorizedtoverifypublisher"></a>NotAuthorizedToVerifyPublisher   

Sie sind nicht berechtigt, die Eigenschaft für den verifizierten Herausgeber für die Anwendung (<AppId>) festzulegen. 

### <a name="mpnidwasnotprovided"></a>MPNIdWasNotProvided  

Die MPN-ID wurde im Anforderungstext nicht angegeben, oder der Anforderungsinhaltstyp war nicht „application/json“. 

### <a name="msanotsupported"></a>MSANotSupported  

Diese Funktion wird für Microsoft-Kundenkonten nicht unterstützt. Es werden nur von einem Azure AD-Benutzer in Azure AD registrierte Anwendungen unterstützt. 

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie alle vorstehenden Informationen gelesen und entsprechend angewendet haben und immer noch einen Fehler von Microsoft Graph erhalten, sammeln Sie möglichst viele der folgenden Informationen in Bezug auf die fehlgeschlagene Anforderung, und [wenden Sie sich an den Microsoft-Support](developer-support-help-options.md#open-a-support-request).

- Timestamp 
- CorrelationId 
- ObjectID oder UserPrincipalName des angemeldeten Benutzers 
- ObjectId der Zielanwendung
- AppId der Zielanwendung
- ID des Mandanten (TenantId), bei dem die App registriert ist
- MPN-ID
- Die ausgeführte REST-Anforderung 
- Zurückgegebene(r) Fehlercode und Fehlermeldung 
