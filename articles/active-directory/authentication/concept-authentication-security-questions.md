---
title: Authentifizierungsmethode mit Sicherheitsfragen – Azure Active Directory
description: Erfahren Sie mehr über die Verwendung von Sicherheitsfragen zum Verbessern und Sichern von Anmeldeereignissen in Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 09/02/2020
ms.author: justinha
author: justinha
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 841391778e0fb8c00f503aa0cc79b5562661e309
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107530347"
---
# <a name="authentication-methods-in-azure-active-directory---security-questions"></a>Authentifizierungsmethoden in Azure Active Directory – Sicherheitsfragen

Sicherheitsfragen werden bei einem Anmeldeereignis nicht als Authentifizierungsmethode verwendet. Sicherheitsfragen können vielmehr bei der Self-Service-Kennwortzurücksetzung (SSPR) verwendet werden, um die eigene Identität zu bestätigen. Die Verwendung von Sicherheitsfragen bei Administratorkonten sind bei der Self-Service-Kennwortzurücksetzung nicht als Prüfmethode zulässig.

Wenn sich Benutzer für die Self-Service-Kennwortzurücksetzung registrieren, werden sie aufgefordert, die gewünschten Authentifizierungsmethoden auszuwählen. Bei der Auswahl von Sicherheitsfragen müssen die Benutzer aus den diversen verfügbaren Fragen für die Eingabeaufforderung eine Auswahl treffen und ihre Antworten bereitstellen.

![Screenshot des Azure-Portals mit Authentifizierungsmethoden und Optionen für Sicherheitsfragen](media/concept-authentication-methods/security-questions-authentication-method.png)

> [!NOTE]
> Sicherheitsfragen werden privat und sicher in einem Benutzerobjekt im Verzeichnis gespeichert und können nur während der Registrierung von Benutzern beantwortet werden. Ein Administrator hat keine Möglichkeit, die Fragen oder Antworten eines Benutzers zu lesen oder zu ändern.

Sicherheitsfragen können weniger sicher als andere Methoden sein, da einige Personen unter Umständen die Antworten auf die Fragen eines anderen Benutzers kennen. Wenn Sie Sicherheitsfragen bei der Self-Service-Kennwortzurücksetzung verwenden, wird empfohlen, diese in Verbindung mit einer anderen Methode zu verwenden. Ein Benutzer kann aufgefordert werden, die Microsoft Authenticator-App oder die Authentifizierung über Telefon zu verwenden, um beim SSPR-Prozess seine Identität nachzuweisen, und Sicherheitsfragen nur dann auszuwählen, wenn sein Telefon oder das registrierte Gerät nicht verfügbar ist.

## <a name="predefined-questions"></a>Vordefinierte Fragen

Die folgenden vordefinierten Sicherheitsfragen stehen als Prüfmethode bei der Self-Service-Kennwortzurücksetzung zur Verfügung. Alle Sicherheitsfragen sind in alle Microsoft 365-Sprachen übersetzt und lokalisiert und basieren auf dem Gebietsschema, das im Browser des Benutzers eingestellt ist:

* In welcher Stadt haben Sie Ihren ersten Partner bzw. Ihre erste Partnerin kennengelernt?
* In welcher Stadt haben sich Ihre Eltern kennengelernt?
* In welcher Stadt lebt Ihr Bruder/Ihre Schwester?
* Wie heißt die Geburtsstadt Ihres Vaters?
* In welcher Stadt haben Sie Ihre erste Arbeitsstelle angetreten?
* Wie heißt die Geburtsstadt Ihrer Mutter?
* In welcher Stadt haben Sie den Neujahrstag 2000 verbracht?
* Wie hieß Ihr Lieblingslehrer bzw. Ihre Lieblingslehrerin in der Grundschule mit Nachnamen?
* Bei welcher Universität haben Sie sich beworben, dort jedoch nicht studiert?
* An welchem Ort fand Ihre Hochzeitsfeier statt?
* Wie lautet der 2. Vorname Ihres Vaters?
* Was ist Ihr Lieblingsgericht?
* Wie lautet der Vor- und Nachname Ihrer Großmutter mütterlicherseits?
* Wie lautet der 2. Vorname Ihrer Mutter?
* In welchem Monat und Jahr ist Ihre älteste Schwester bzw. Ihr ältester Bruder geboren? (Beispiel: November 1985)
* Wie lautet der 2. Vorname Ihrer ältesten Schwester bzw. Ihres ältesten Bruders?
* Wie lautet der Vor- und Nachname Ihres Großvaters väterlicherseits?
* Wie lautet der 2. Vorname Ihrer jüngsten Schwester bzw. Ihres jüngsten Bruders?
* Auf welche Schule sind Sie in der sechsten Klasse gegangen?
* Wie lautet der Vor- und Nachname Ihres besten Freundes bzw. Ihrer besten Freundin in Ihrer Kindheit?
* Wie lautet der Vor- und Nachname Ihres ersten Partners bzw. Ihrer ersten Partnerin?
* Wie lautete der Nachname des Lehrers bzw. der Lehrerin Ihres Lieblingsfachs?
* Welche Marke und welches Modell hatte Ihr erstes Auto oder Motorrad?
* Wie heißt die erste Schule, die Sie besucht haben?
* In welchem Krankenhaus wurden Sie geboren?
* In welcher Straße haben Sie als Kind zuerst gewohnt?
* Wer war der Held Ihrer Kindheit?
* Wie hieß Ihr Lieblingskuscheltier?
* Wie hieß Ihr erstes Haustier?
* Welchen Spitznamen hatten Sie als Kind?
* Was war in der Schulzeit Ihre Lieblingssportart?
* Was war Ihr erster Job?
* Wie lauten die letzten vier Ziffern der Telefonnummer in Ihrer Kindheit?
* Was wollten Sie als Kind später einmal werden?
* Welches ist die berühmteste Person, die Sie kennen gelernt haben?

## <a name="custom-security-questions"></a>Benutzerdefinierte Sicherheitsfragen

Für noch mehr Flexibilität können Sie auch Ihre eigenen benutzerdefinierten Sicherheitsfragen definieren. Die Länge von benutzerdefinierten Sicherheitsfragen ist auf 200 Zeichen begrenzt.

Benutzerdefinierte Sicherheitsfragen werden nicht automatisch lokalisiert, wie das bei den Standardsicherheitsfragen der Fall ist. Alle benutzerdefinierten Fragen werden auch bei abweichendem Gebietsschema im Browser des Benutzers in der Sprache angezeigt, in der sie in der administrativen Benutzeroberfläche eingegeben wurden. Wenn Sie lokalisierte Fragen benötigen, verwenden Sie die vordefinierten Fragen.

## <a name="security-question-requirements"></a>Anforderungen an Sicherheitsfragen

Sowohl für Standardsicherheitsfragen als auch für benutzerdefinierte Sicherheitsfragen gelten die folgenden Anforderungen und Einschränkungen:

* Die Mindestanzahl von Zeichen für Antworten ist drei Zeichen.
* Die maximale Zeichenanzahl für Antworten ist 40 Zeichen.
* Benutzer können dieselbe Frage nicht mehrmals beantworten.
* Benutzer können nicht dieselbe Antwort für mehrere Fragen angeben.
* Bei der Definition von Fragen und Antworten kann ein beliebiger Zeichensatz verwendet werden (einschließlich Unicode).
* Die Anzahl der definierten Fragen muss größer als oder gleich der Anzahl der für die Registrierung erforderlichen Fragen sein.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Einstieg finden Sie im [Tutorial zur Self-Service-Kennwortzurücksetzung (SSPR)][tutorial-sspr].

Weitere Informationen zu SSPR-Konzepten finden Sie unter [So funktioniert's: Self-Service-Kennwortzurücksetzung in Azure AD][concept-sspr].

Lernen Sie mehr über die Konfiguration der Authentifizierungsmethoden mithilfe der [Microsoft Graph REST-API](/graph/api/resources/authenticationmethods-overview).

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[concept-sspr]: concept-sspr-howitworks.md
