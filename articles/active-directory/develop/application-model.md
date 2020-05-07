---
title: Anwendungsmodell | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie etwas über das Registrieren Ihrer Anwendung, damit diese mit Microsoft Identity Platform (v2.0) integriert werden kann.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/28/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: 774822eae5ab327f57da3eca4499ca14d3e0c7a1
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82584166"
---
# <a name="application-model"></a>Anwendungsmodell

Anwendungen können Benutzer selbst anmelden oder die Anmeldung an einen Identitätsanbieter delegieren. In diesem Thema werden die erforderlichen Schritte zum Registrieren einer Anwendung bei Microsoft Identity Platform erläutert.

## <a name="registering-an-application"></a>Registrieren einer Anwendung

Damit ein Identitätsanbieter weiß, dass ein Benutzer Zugriff auf eine bestimmte App hat, müssen sowohl der Benutzer als auch die Anwendung beim Identitätsanbieter registriert werden. Wenn Sie Ihre Anwendung bei Azure AD registrieren, stellen Sie eine Identitätskonfiguration für Ihre Anwendung bereit, die die Integration mit Microsoft Identity Platform ermöglicht. Die Registrierung der App ermöglicht Ihnen außerdem Folgendes:

* Anpassen des Brandings Ihrer Anwendung im Anmeldedialog. Dies ist wichtig, da dies der erste Eindruck ist, den ein Benutzer von Ihrer App erhält.
* Entscheiden, ob Sie die Anmeldung nur für Benutzer zulassen, die Ihrer Organisation angehören. Hierbei handelt es sich um eine Einzelinstanzanwendung. Oder lassen Sie für Benutzer die Anmeldung per Geschäfts-, Schul-oder Unikonto zu. Hierbei handelt es sich um eine mehrinstanzenfähige Anwendung. Sie können auch persönliche Microsoft-Konten oder ein Social Media-Konto zulassen, z. B. von LinkedIn, Google usw.
* Anfordern von Bereichsberechtigungen. Sie können beispielsweise den Bereich „user.read“ anfordern, mit dem die Berechtigung zum Lesen des Profils des angemeldeten Benutzers gewährt wird.
* Definieren von Bereichen, mit denen der Zugriff auf Ihre Web-API definiert wird. Wenn eine App auf Ihre API zugreifen möchte, muss sie normalerweise Berechtigungen für die von Ihnen definierten Bereiche anfordern.
* Austauschen eines Geheimnisses mit Microsoft Identity Platform, um die Identität der App nachzuweisen.  Dies ist relevant für den Fall, dass es sich bei der App um eine vertrauliche Clientanwendung handelt. In einer vertraulichen Clientanwendung können Anmeldeinformationen sicher aufbewahrt werden. Zum Speichern der Anmeldeinformationen wird ein vertrauenswürdiger Back-End-Server benötigt.

Nach der Registrierung erhält die Anwendung einen eindeutigen Bezeichner, der von der App beim Anfordern von Token mit Microsoft Identity Platform ausgetauscht wird. Wenn es sich bei der App um eine [vertrauliche Clientanwendung](developer-glossary.md#client-application) handelt, wird auch das Geheimnis oder der öffentliche Schlüssel ausgetauscht. Dies hängt davon ab, ob Zertifikate oder Geheimnisse verwendet werden.

Bei Microsoft Identity Platform werden Anwendungen mit einem Modell dargestellt, das zwei Hauptfunktionen erfüllt:

* Identifizieren der App anhand der unterstützten Authentifizierungsprotokolle
* Bereitstellen aller Bezeichner, URLs, Geheimnisse und zugehörigen Informationen, die für die Authentifizierung erforderlich sind

Microsoft Identity Platform:

* Enthält alle Daten, die zur Unterstützung der Authentifizierung zur Laufzeit erforderlich sind
* Enthält alle Daten zum Treffen der Entscheidung, auf welche Ressourcen eine App ggf. zugreifen muss und unter welchen Umständen eine bestimmte Anforderung erfüllt werden soll
* Stellt Infrastruktur für die Implementierung der App-Bereitstellung innerhalb des Mandanten des App-Entwicklers und für andere Azure AD-Mandanten bereit
* Behandelt die Benutzereinwilligung während der Tokenanforderung und vereinfacht die dynamische mandantenübergreifenden Bereitstellung von Apps

Die **Einwilligung** ist der Prozess, bei dem ein Ressourcenbesitzer einer Clientanwendung durch spezifische Berechtigungen die Autorisierung für den Zugriff auf geschützte Ressourcen im Auftrag des Ressourcenbesitzers gewährt. Microsoft Identity Platform:

* Ermöglicht Benutzern und Administratoren, der App dynamisch die Zustimmung zum Zugriff auf Ressourcen in ihrem Namen zu erteilen oder zu verweigern.
* Ermöglicht es Administratoren, letztendlich zu entscheiden, welche Aktionen Apps ausführen und welche Benutzer bestimmte Apps verwenden dürfen und wie auf die Verzeichnisressourcen zugegriffen wird.

## <a name="multi-tenant-apps"></a>Mehrinstanzenfähige Apps

Bei Microsoft Identity Platform beschreibt ein [Anwendungsobjekt](developer-glossary.md#application-object) eine Anwendung. Zum Zeitpunkt der Bereitstellung verwendet Microsoft Identity Platform das Anwendungsobjekt als Blaupause zum Erstellen eines [Dienstprinzipals](developer-glossary.md#service-principal-object), der eine konkrete Instanz einer Anwendung in einem Verzeichnis oder Mandanten darstellt. Mit dem Dienstprinzipal wird definiert, welche Aktionen die App in einem bestimmten Zielverzeichnis ausführen darf, wer sie verwenden kann, auf welche Ressourcen sie Zugriff besitzt usw. Microsoft Identity Platform erstellt mittels [Einwilligung](developer-glossary.md#consent) einen Dienstprinzipal aus einem Anwendungsobjekt.

Die folgende Abbildung zeigt einen vereinfachten Microsoft Identity Platform-Bereitstellungsablauf, der durch die Einwilligung gesteuert wird. Sie enthält zwei Mandanten: *A* und *B*.

* *Mandant A* ist der Besitzer der Anwendung.
* *Mandant B* instanziiert die Anwendung über einen Dienstprinzipal.

![Vereinfachter Bereitstellungsablauf, der durch Zustimmung gesteuert wird](./media/authentication-scenarios/simplified-provisioning-flow-consent-driven.svg)

In diesem Bereitstellungsablauf geschieht Folgendes:

1. Ein Benutzer von Mandant B versucht, sich mit der App anzumelden, und der Autorisierungsendpunkt fordert ein Token für die Anwendung an.
1. Die Anmeldeinformationen des Benutzers werden abgerufen und für die Authentifizierung überprüft.
1. Der Benutzer wird aufgefordert, dem Zugriff auf den Mandanten B für die App zuzustimmen.
1. Microsoft Identity Platform verwendet das Anwendungsobjekt in Mandant A als Blaupause für das Erstellen eines Dienstprinzipals in Mandant B.
1. Der Benutzer erhält das angeforderte Token.

Sie können diesen Vorgang für weitere Mandanten wiederholen. Der Mandant A enthält die Blaupause für die App (Anwendungsobjekt). Benutzer und Administratoren aller anderen Mandanten, in denen der App die Zustimmung erteilt wird, behalten durch das entsprechende Dienstprinzipalobjekt in jedem Mandanten die Kontrolle darüber, welche Aktionen die Anwendung ausführen darf. Weitere Informationen finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Microsoft Identity Platform](app-objects-and-service-principals.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Themen zu den Grundlagen von Authentifizierung und Autorisierung:

* Unter [Authentifizierung im Vergleich zu Autorisierung](authentication-vs-authorization.md) lernen Sie die grundlegenden Konzepte von Authentifizierung und Autorisierung in Microsoft Identity Platform kennen.
* Unter [Sicherheitstoken](security-tokens.md) erfahren Sie, wie Sie auf Token zugreifen und Token aktualisieren. Erläutert wird auch, wie ID-Token bei Authentifizierung und Autorisierung verwendet werden.
* Unter [App-Anmeldefluss](app-sign-in-flow.md) lernen Sie den Anmeldeflow von Web-, Desktop- und mobilen Apps in Microsoft Identity Platform kennen.

Weitere Informationen zum Anwendungsmodell:

* Unter [Wie und warum werden Anwendungen zu Azure AD hinzugefügt?](active-directory-how-applications-are-added.md) erhalten Sie weitere Informationen zu Anwendungsobjekten und Dienstprinzipalen in Microsoft Identity Platform.
* Unter [Mandanten in Azure Active Directory](single-and-multi-tenant-apps.md) erhalten Sie weitere Informationen zu Einzelmandanten-Apps und mehrinstanzenfähigen Apps.
* In der [Azure Active Directory B2C-Dokumentation](https://docs.microsoft.com/azure/active-directory-b2c) erfahren Sie, wie Azure AD auch Azure Active Directory B2C bereitstellt, damit Organisationen für die Anmeldung von Benutzern (meist Kunden) Identitäten sozialer Netzwerke, z. B. ein Google-Konto, verwenden können.
