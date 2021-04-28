---
title: SDKs und REST-APIs für Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Erfahren Sie mehr über Azure Communication Services SDKs und REST APIs.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/25/2021
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 21910718d837750db7ae87afa36a2e7576a729cf
ms.sourcegitcommit: 2f322df43fb3854d07a69bcdf56c6b1f7e6f3333
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108016561"
---
# <a name="sdks-and-rest-apis"></a>SDKs und REST-APIs

Azure Communication Services-Funktionen sind vom Prinzip her in sechs Bereiche unterteilt. Die meisten Bereichen weisen vollständig quelloffene SDKs auf, die gegen veröffentlichte REST-APIs programmiert sind, und die Sie direkt über das Internet nutzen können. Das Calling SDK verwendet proprietäre Netzwerkschnittstellen und ist derzeit Closed-Source. Beispiele und weitere technische Details für SDKs sind im [Azure Communication Services GitHub Repo](https://github.com/Azure/communication) veröffentlicht.

## <a name="rest-apis"></a>REST-APIs
Communication Services-APIs sind unter [docs.microsoft.com](/rest/api/azure/) neben anderen Azure-REST-APIs dokumentiert. Diese Dokumentation erläutert die Strukturierung Ihrer HTTP-Nachrichten und bietet eine Anleitung zur Verwendung von Postman. Diese Dokumentation wird auch im Swagger-Format auf [GitHub](https://github.com/Azure/azure-rest-api-specs) angeboten.


## <a name="sdks"></a>SDKs

| Assembly | Namespaces| Protokolle | Funktionen |
|------------------------|-------------------------------------|---------------------------------|--------------------------------------------------------------------------------------------|
| Azure Resource Manager | Azure.ResourceManager.Communication | [REST](https://docs.microsoft.com/rest/api/communication/communicationservice)| Dient zum Bereitstellen und Verwalten von Communication Services-Ressourcen|
| Allgemein | Azure.Communication.Common| REST | Stellt Basistypen für andere SDKs zur Verfügung |
| Identity | Azure.Communication.Identity| [REST](https://docs.microsoft.com/rest/api/communication/communicationidentity)| Verwalten von Benutzern, Zugriffstoken|
| Telefonnummern _(beta)_| Azure.Communication.PhoneNumbers| [REST](/rest/api/communication/phonenumbers)| Erfassen und Verwalten von Telefonnummern |
| Chat | Azure.Communication.Chat| [REST](https://docs.microsoft.com/rest/api/communication/) mit proprietärer Signalisierung | Dient zum Hinzufügen von textbasiertem Chat in Echtzeit zu Ihren Anwendungen |
| SMS| Azure.Communication.SMS | [REST](https://docs.microsoft.com/rest/api/communication/sms)| Dient zum Senden und Empfangen von SMS-Nachrichten|
| Aufrufen| Azure.Communication.Calling | Proprietärer Transport | Nutzung von Sprach-, Video-, Bildschirmfreigabe- und anderen Echtzeit-Datenkommunikationsfunktionen |

Die Azure Resource Manager-, Identity- und SMS-SDKs sind auf die Integration von Diensten ausgerichtet, und in vielen Fällen entstehen Sicherheitsprobleme, wenn Sie diese Funktionen in Endbenutzeranwendungen integrieren. Die SDKs Common und Chat sind für Service- und Client-Anwendungen geeignet. Das Calling SDK ist für Client-Anwendungen konzipiert. Ein SDK mit Fokus auf Service-Szenarien ist in der Entwicklung.


### <a name="languages-and-publishing-locations"></a>Sprachen und Veröffentlichungsspeicherorte

Die Veröffentlichungsstandorte für die einzelnen SDK-Pakete sind unten aufgeführt.

| Bereich           | JavaScript | .NET | Python | Java SE | iOS | Android | Andere                          |
| -------------- | ---------- | ---- | ------ | ---- | -------------- | -------------- | ------------------------------ |
| Azure Resource Manager | -         | [NuGet](https://www.nuget.org/packages/Azure.ResourceManager.Communication)    |   [PyPi](https://pypi.org/project/azure-mgmt-communication/)    |  -  | -              | -  | [Go über GitHub](https://github.com/Azure/azure-sdk-for-go/releases/tag/v46.3.0) |
| Allgemein         | [npm](https://www.npmjs.com/package/@azure/communication-common)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Common/)    | –      | [Maven](https://search.maven.org/search?q=a:azure-communication-common)   | [GitHub](https://github.com/Azure/azure-sdk-for-ios/releases)            | [Maven](https://search.maven.org/artifact/com.azure.android/azure-communication-common)             | -                              |
| Identity | [npm](https://www.npmjs.com/package/@azure/communication-identity)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Identity)    | [PyPi](https://pypi.org/project/azure-communication-identity/)      | [Maven](https://search.maven.org/search?q=a:azure-communication-identity)   | -              | -              | -                            |
| Telefonnummern | [npm](https://www.npmjs.com/package/@azure/communication-phone-numbers)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.PhoneNumbers)    | [PyPi](https://pypi.org/project/azure-communication-phonenumbers/)      | [Maven](https://search.maven.org/search?q=a:azure-communication-phonenumbers)   | -              | -              | -                            |
| Chat           | [npm](https://www.npmjs.com/package/@azure/communication-chat)        | [NuGet](https://www.nuget.org/packages/Azure.Communication.Chat)     | [PyPi](https://pypi.org/project/azure-communication-chat/)     | [Maven](https://search.maven.org/search?q=a:azure-communication-chat)   | [GitHub](https://github.com/Azure/azure-sdk-for-ios/releases)  | [Maven](https://search.maven.org/search?q=a:azure-communication-chat)   | -                              |
| SMS            | [npm](https://www.npmjs.com/package/@azure/communication-sms)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Sms)    | [PyPi](https://pypi.org/project/azure-communication-sms/)       | [Maven](https://search.maven.org/artifact/com.azure/azure-communication-sms)   | -              | -              | -                              |
| Aufrufen        | [npm](https://www.npmjs.com/package/@azure/communication-calling)         | -      | -      | -     | [GitHub](https://github.com/Azure/Communication/releases)     | [Maven](https://search.maven.org/artifact/com.azure.android/azure-communication-calling/)            | -                              |
| Referenzdokumentation     | [docs](https://azure.github.io/azure-sdk-for-js/communication.html)         | [docs](https://azure.github.io/azure-sdk-for-net/communication.html)      | -      | [docs](http://azure.github.io/azure-sdk-for-java/communication.html)     | [docs](/objectivec/communication-services/calling/)      | [docs](/java/api/com.azure.android.communication.calling)            | -                              |


## <a name="rest-api-throttles"></a>REST-API-Drosselungen
Bestimmte Rest-APIs und entsprechende SDK-Methoden haben Drosselungs-Limits, die Sie berücksichtigen sollten. Das Überschreiten dieser Drosselungs-Limits löst eine  `429 - Too Many Requests` Fehler Antwort aus. Diese Grenzwerte können anhand [einer Anforderung an den Azure-Support](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)erhöht werden.

| API                                                                                                                          | Drosselung            |
|------------------------------------------------------------------------------------------------------------------------------|---------------------|
| [Alle Such-Telefonnummernplan-APIs](/rest/api/communication/phonenumbers)         | 4 Anfragen/Tag      |
| [Telefonnummer-Plan erwerben](/rest/api/communication/phonenumbers/purchasephonenumbers) | 1 Kauf pro Monat  |
| [Senden einer SMS](https://docs.microsoft.com/rest/api/communication/sms/send)                                                       | 200 Anfragen/Minute |


## <a name="sdk-platform-support-details"></a>Details zur SDK-Plattformunterstützung

### <a name="ios-and-android"></a>iOS und Android 

- Die Communication Services iOS SDKs zielen auf iOS Version 13+, und Xcode 11+.
- Die Android Java SDKs zielen auf Android API Level 21+ und Android Studio 4.0+

### <a name="net"></a>.NET 

Mit Ausnahme von Calling zielen die Communication Services-Pakete auf den .NET-Standard 2.0, der die unten aufgeführten Plattformen unterstützt.

Unterstützung über .NET Framework 4.6.1
- Windows 10, 8.1, 8 und 7
- Windows Server 2012 R2, 2012 und 2008 R2 SP1

Unterstützung über .NET Core 2.0:
- Windows 10 (ab 1607), 7 SP1+, 8.1
- Windows Server 2008 R2 SP1+
- Mac OS X ab 10.12
- Mehrere Versionen/Distributionen von Linux
- UWP 10.0.16299 (RS3) September 2017
- Unity 2018.1
- Mono 5.4
- Xamarin iOS 10.14
- Xamarin Mac 3.8

## <a name="api-stability-expectations"></a>Erwartungen an die API-Stabilität

> [!IMPORTANT]
> Dieser Abschnitt enthält Anleitungen zu REST-APIs und SDKs, die als **stabil** gekennzeichnet sind. APIs, die als Vorabversion, Vorschau oder Beta gekennzeichnet sind, können ohne Vorankündigung geändert oder als **veraltet eingestuft** werden.

In der Zukunft werden wir möglicherweise Versionen der SDKs für Kommunikationsdienste außer Betrieb nehmen, und wir könnten bahnbrechende Veränderungen an unseren REST-APIs und veröffentlichten SDKs vornehmen. Für Azure Communication Services gelten *allgemein* zwei Richtlinien zur Unterstützung des Zurückziehens von Dienstversionen:

- Sie werden mindestens drei Jahre vorher benachrichtigt, ehe Sie aufgrund einer Änderung einer Communication Services-Schnittstelle Code ändern müssen. Bei allen dokumentierten REST-APIs und SDK-APIs wird in der Regel eine mindestens dreijährige Vorwarnzeit eingehalten, bevor Schnittstellen ausgemustert werden.
- Sie werden mindestens ein Jahr, bevor Sie SDK-Assemblies auf die neueste Minor-Version aktualisieren müssen, benachrichtigt. Diese erforderlichen Aktualisierungen sollten keine Codeänderungen erfordern, da sie zur gleichen Hauptversion gehören. Dies gilt insbesondere für die Bibliotheken „Calling“ und „Chat“, die über Echtzeitkomponenten verfügen, bei denen häufig Sicherheits- und Leistungsaktualisierungen erforderlich sind. Wir empfehlen Ihnen nachdrücklich, Ihre Communication Services-SDKs auf dem neuesten Stand zu halten.

### <a name="api-and-sdk-decommissioning-examples"></a>Beispiele für die Außerbetriebnahme von API und SDK

**Sie haben Version 24 der REST-API „SMS“ in Ihre Anwendung integriert. Azure Communication veröffentlicht Version 25.**

Sie erhalten eine dreijährige Vorwarnung, bevor diese APIs nicht mehr funktionieren und Sie gezwungen sind, auf v25 zu aktualisieren. Dieses Update erfordert möglicherweise eine Codeänderung.

**Sie haben die Version v2.02 des Calling SDK in Ihre Anwendung integriert. Azure Communication gibt die Version v2.05 frei.**

Es kann erforderlich sein, dass Sie innerhalb von 12 Monaten nach der Freigabe des v2.05 auf die Version v2.05 des Calling SDK aktualisieren müssen. Dabei sollte es sich um einen einfachen Austausch des Artefakts handeln, ohne dass eine Codeänderung erforderlich ist, da Version 2.05 zur Hauptversion 2 gehört und keine Breaking Changes aufweist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden SDK-Übersichten:

- [Calling SDK Übersicht](../concepts/voice-video-calling/calling-sdk-features.md)
- [Chat SDK Übersicht](../concepts/chat/sdk-features.md)
- [SMS SDK Übersicht](../concepts/telephony-sms/sdk-features.md)

Für den Einstieg in Azure Communication Services:

- [Erstellen von Azure Communication-Ressourcen](../quickstarts/create-communication-resource.md)
- Generieren von [Benutzerzugrifftoken](../quickstarts/access-tokens.md)
