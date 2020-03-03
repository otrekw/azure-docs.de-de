---
title: Benutzerdefinierte Steuerelemente beim bedingtem Zugriff in Azure AD
description: Hier erfahren Sie, wie benutzerdefinierte Steuerelemente beim bedingten Zugriff in Azure Active Directory funktionieren.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 02/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f1df037b66c72177a96f77231cee70782d04992
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77620712"
---
# <a name="custom-controls-preview"></a>Benutzerdefinierte Steuerelemente (Vorschau)

Benutzerdefinierte Steuerelemente sind eine Funktion der Azure Active Directory Premium P1 Edition. Wenn Sie benutzerdefinierte Steuerelemente verwenden, werden Ihre Benutzer zu kompatiblen Diensten umgeleitet, um weiteren Anforderungen außerhalb von Azure Active Directory zu genügen. Um die Bedingungen dieses Steuerelements zu erfüllen, wird der Browser eines Benutzers auf den externen Dienst umgeleitet, führt alle erforderlichen Authentifizierungs- oder Überprüfungsaktivitäten durch und wird dann wieder an Azure Active Directory umgeleitet. Azure Active Directory überprüft die Antwort, und wenn der Benutzer erfolgreich authentifiziert oder überprüft wurde, verbleibt der Benutzer im Vorgangsfluss des bedingten Zugriffs.

Diese Steuerelemente ermöglichen die Verwendung von bestimmten externen oder benutzerdefinierten Diensten als Steuerelemente für den bedingten Zugriff und erweitern in der Regel die Funktionen des bedingten Zugriffs.

Zu den Anbietern, die derzeit einen kompatiblen Dienst anbieten gehören:

- [Duo Security](https://duo.com/docs/azure-ca)
- [Entrust Datacard](https://www.entrustdatacard.com/products/authentication/intellitrust)
- [GSMA](https://mobileconnect.io/azure/)
- [Ping Identity](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_AzureADIntegration.html)
- [RSA](https://community.rsa.com/docs/DOC-81278)
- [SecureAuth](https://docs.secureauth.com/pages/viewpage.action?pageId=47238992#)
- [Silverfort](https://www.silverfort.io/company/using-silverfort-mfa-with-azure-active-directory/)
- [Symantec VIP](https://help.symantec.com/home/VIP_Integrate_with_Azure_AD)
- [Thales (Gemalto)](https://resources.eu.safenetid.com/help/AzureMFA/Azure_Help/Index.htm)
- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

Für weitere Informationen zu diesen Diensten wenden Sie sich direkt an die Anbieter.

## <a name="creating-custom-controls"></a>Erstellen von benutzerdefinierten Steuerelementen

Um ein benutzerdefiniertes Steuerelement zu erstellen, sollten Sie sich zuerst an den Anbieter wenden, den Sie verwenden möchten. Jeder Nicht-Microsoft-Anbieter hat seine eigenen Prozesse und Anforderungen für das Registrieren, das Abonnieren, oder um auf andere Weise Teil des Diensts zu werden, und um anzugeben, dass Sie sich in den bedingten Zugriff integrieren möchten. Zu diesem Zeitpunkt wird der Anbieter einen Block von Daten im JSON-Format für Sie bereitstellen. Diese Daten ermöglichen dem Anbieter und dem bedingten Zugriff, für Ihren Mandanten zusammenzuarbeiten. Diese Daten erstellen das neue Steuerelement und definieren, wie der bedingte Zugriff feststellen kann, ob Ihre Benutzer die Überprüfung mit dem Anbieter erfolgreich ausgeführt haben.

Benutzerdefinierte Steuerelemente können nicht mit der Identity Protection-Automatisierung verwendet werden, die eine mehrstufige Authentifizierung erfordert. Sie können auch nicht zum Erhöhen von Rollen im Privileged Identity Manager (PIM) eingesetzt werden.

Kopieren Sie die JSON-Daten, und fügen Sie sie in das entsprechende Textfeld ein. Verändern Sie JSON-Daten nicht, es sei denn, Sie verstehen explizit die Änderungen, die Sie vornehmen möchten. Jede Änderung kann die Verbindung zwischen dem Anbieter und Microsoft unterbrechen und möglicherweise den Zugriff auf Ihre Konten für Sie und Ihre Benutzer sperren.

Die Option zum Erstellen eines benutzerdefinierten Steuerelements befindet sich im Abschnitt **Verwalten** der Seite **Bedingter Zugriff**.

![Control](./media/controls/82.png)

Wenn Sie auf **New custom control** (Neues benutzerdefiniertes Steuerelement) klicken, wird ein Blatt geöffnet und ein Textfeld für die JSON-Daten des Steuerelements angezeigt.  

![Control](./media/controls/81.png)

## <a name="deleting-custom-controls"></a>Löschen von benutzerdefinierten Steuerelementen

Um ein benutzerdefiniertes Steuerelement zu löschen, müssen Sie zunächst sicherstellen, dass es in keiner der Richtlinien für bedingten Zugriff verwendet wird. Ist der Vorgang einmal abgeschlossen:

1. Wechseln Sie zur Liste der benutzerdefinierten Steuerelemente
1. Klicken Sie auf „...“  
1. Klicken Sie auf **Löschen**.

## <a name="editing-custom-controls"></a>Bearbeiten von benutzerdefinierten Steuerelementen

Um ein benutzerdefiniertes Steuerelement zu bearbeiten, müssen Sie das aktuelle Steuerelement löschen und ein neues Steuerelement mit den aktualisierten Informationen erstellen.

## <a name="session-controls"></a>Sitzungssteuerelemente

Sitzungssteuerelemente ermöglichen das Einschränken der Benutzeroberfläche innerhalb einer Cloud-App. Die Sitzungssteuerelemente werden von Cloud-Apps erzwungen und verlassen sich auf zusätzliche Informationen über die Sitzung, die der App von Azure AD bereitgestellt werden.

![Control](./media/controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Verwenden von App-erzwungenen Einschränkungen

Sie können dieses Steuerelement verwenden, um von Azure AD anzufordern, die Geräteinformationen an die ausgewählten Cloud-Apps zu übergeben. Mithilfe der Geräteinformationen können Cloud-Apps herausfinden, ob eine Verbindung von einem konformen oder einem in die Domäne eingebundenen Gerät initiiert wird. Diese Steuermöglichkeit unterstützt nur SharePoint Online und Exchange Online als ausgewählte Cloud-Apps. Bei Auswahl werden die Geräteinformationen von der Cloud-App dazu verwendet, Benutzern je nach Gerätezustand eine eingeschränkte oder vollständige Benutzeroberfläche zur Verfügung zu stellen.

Weitere Informationen finden Sie unter:

- [Aktivieren des eingeschränkten Zugriffs mit SharePoint Online](https://aka.ms/spolimitedaccessdocs)
- [Aktivieren des eingeschränkten Zugriffs mit Exchange Online](https://aka.ms/owalimitedaccess)

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, finden Sie Informationen unter [Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](app-based-mfa.md).
- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, helfen Ihnen die Informationen unter [Best Practices für den bedingten Zugriff in Azure Active Directory](best-practices.md) weiter.
