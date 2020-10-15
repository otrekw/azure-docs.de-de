---
author: baanders
description: Includedatei für mögliche zusätzliche Anforderungen bei der Einrichtung von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 7/22/2020
ms.author: baanders
ms.openlocfilehash: c2668bdda5002ebd2a34b8a2ffa5885263aec0c9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88009647"
---
Ihre Organisation erfordert möglicherweise zusätzliche Aktionen von Abonnementbesitzern/Administratoren, um eine App-Registrierung erfolgreich einzurichten (und so das Einrichten einer funktionsbereiten Instanz von Azure Digital Twins abzuschließen). Welche Schritte erforderlich sind, hängt von den spezifischen Einstellungen Ihrer Organisation ab.

Im Folgenden finden Sie einige häufig vorkommende Aktivitäten, die ein Besitzer/Administrator möglicherweise ausführen muss. Diese und andere Vorgänge können über die Seite [*Azure AD-App-Registrierungen*](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) im Azure-Portal ausgeführt werden.
* Erteilen Sie eine Administratoreinwilligung für die App-Registrierung. In Ihrer Organisation ist möglicherweise die Einstellung *Administratoreinwilligung erforderlich* global in Azure AD für alle App-Registrierungen in Ihrem Abonnement aktiviert. Wenn dies der Fall ist, muss der Besitzer/Administrator diese Schaltfläche für Ihr Unternehmen auf der Seite *API-Berechtigungen* der App-Registrierung auswählen, damit die App-Registrierung gültig ist:

    :::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/portal/grant-admin-consent.png" alt-text="Portalansicht der Schaltfläche „Administratoreinwilligung gewähren“ unter „API-Berechtigungen“":::
  - Wenn die Einwilligung erfolgreich gewährt wurde, sollte der Eintrag für Azure Digital Twins unter *Status* dann den Wert _Gewährt für **(Ihr Unternehmen)**_ anzeigen.
   
    :::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/portal/granted-admin-consent-done.png" alt-text="Portalansicht der Schaltfläche „Administratoreinwilligung gewähren“ unter „API-Berechtigungen“":::
* Aktivieren des öffentlichen Clientzugriffs
* Festlegen bestimmter Antwort-URLs für den Web- und Desktopzugriff
* Zulassen von impliziten OAuth2-Authentifizierungsflows

Weitere Informationen zur App-Registrierung und zu den verschiedenen diesbezüglichen Einrichtungsoptionen finden Sie unter [*Registrieren einer Anwendung bei der Microsoft Identity Platform*](https://docs.microsoft.com/graph/auth-register-app-v2).

Sie verfügen jetzt über eine einsatzbereite Azure Digital Twins-Instanz, Sie haben Berechtigungen für deren Verwaltung zugewiesen, und Sie haben Berechtigungen für eine Client-App eingerichtet, damit diese auf die Instanz zugreifen kann.