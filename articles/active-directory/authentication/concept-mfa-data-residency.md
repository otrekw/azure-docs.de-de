---
title: Datenresidenz bei Azure AD Multi-Factor Authentication
description: Hier erfahren Sie, welche personen- und organisationsbezogenen Daten Azure AD Multi-Factor Authentication über Sie und Ihre Benutzer speichert und welche Daten im Ursprungsland/in der Ursprungsregion verbleiben.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: inbarc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 788a666e8ec509bbd29a8dbd503a60b3dddefd6b
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98208351"
---
# <a name="data-residency-and-customer-data-for-azure-ad-multifactor-authentication"></a>Datenresidenz und Kundendaten für Azure AD Multi-Factor Authentication

Kundendaten werden von Azure AD an einem geografischen Standort gespeichert, der auf der Adresse basiert, die Ihre Organisation beim Abonnieren eines Microsoft-Onlinediensts wie Microsoft 365 und Azure angegeben hat. Informationen darüber, wo Ihre Kundendaten gespeichert werden, finden Sie im Microsoft Trust Center im Abschnitt [Wo wir Ihre Daten speichern](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

Bei der cloudbasierten Azure AD Multi-Factor Authentication und auf den Servern für Azure AD Multi-Factor Authentication werden einige personen- und organisationsbezogene Daten verarbeitet und gespeichert. In diesem Artikel wird beschrieben, welche Daten wo gespeichert werden.

Für den Dienst „Azure AD Multi-Factor Authentication“ stehen Rechenzentren in den USA, in Europa und in der Region „Asien-Pazifik“ zur Verfügung. Sofern nichts anderes angegeben ist, werden die folgenden Aktivitäten in den regionalen Rechenzentren abgewickelt:

* Die mehrstufige Authentifizierung mit Telefonanrufen erfolgt in Rechenzentren in den USA und wird von globalen Anbietern weitergeleitet.
* Universelle Benutzerauthentifizierungsanforderungen aus anderen Regionen wie Europa oder Australien werden aktuell abhängig vom Standort des Benutzers verarbeitet.
* Von der Microsoft Authenticator-App gesendete Pushbenachrichtigungen werden derzeit in den regionalen Rechenzentren (basierend auf dem Standort des Benutzers) verarbeitet.
    * Geräte- und anbieterspezifische Dienste (beispielsweise Apple-Pushbenachrichtigungen) können sich abseits des Benutzerstandorts befinden.

## <a name="personal-data-stored-by-azure-ad-multifactor-authentication"></a>Von Azure AD Multi-Factor Authentication gespeicherte personenbezogene Daten

Bei personenbezogenen Daten handelt es sich um Informationen auf Benutzerebene, die einer bestimmten Person zugeordnet sind. Die folgenden Datenspeicher enthalten persönliche Informationen:

* Blockierte Benutzer
* Umgangene Benutzer
* Änderungsanforderungen für Microsoft Authenticator-Gerätetoken
* Multi-Factor Authentication-Aktivitätsberichte
* Microsoft Authenticator-Aktivierungen

Diese Informationen werden 90 Tage lang aufbewahrt.

Azure AD Multi-Factor Authentication protokolliert keine personenbezogenen Daten wie Benutzernamen, Telefonnummern oder IP-Adressen. Es gibt jedoch eine *UserObjectId*, mit der Multi-Factor Authentication-Versuche Benutzern zugeordnet werden können. Protokolldaten werden für 30 Tage gespeichert.

### <a name="azure-ad-multifactor-authentication"></a>Azure AD Multifactor Authentication

Für öffentliche Azure-Clouds (mit Ausnahme von Azure B2C-Authentifizierung, NPS-Erweiterung und Windows Server 2016- oder 2019-AD FS-Adapter) werden die folgenden personenbezogenen Daten gespeichert:

| Ereignistyp                           | Datenspeichertyp |
|--------------------------------------|-----------------|
| OATH-Token                           | In Multi-Factor Authentication-Protokollen     |
| Unidirektionale SMS                          | In Multi-Factor Authentication-Protokollen     |
| Anruf                           | In Multi-Factor Authentication-Protokollen<br />Datenspeicher für Multi-Factor Authentication-Aktivitätsberichte<br />Blockierte Benutzer bei Betrugsmeldungen |
| Microsoft Authenticator-Benachrichtigung | In Multi-Factor Authentication-Protokollen<br />Datenspeicher für Multi-Factor Authentication-Aktivitätsberichte<br />Blockierte Benutzer bei Betrugsmeldungen<br />Änderungsanforderungen bei Änderung des Microsoft Authenticator-Gerätetokens |

Für Microsoft Azure Government, Microsoft Azure Deutschland Microsoft Azure, betrieben von 21ViaNet, Azure B2C-Authentifizierung, NPS-Erweiterung und Windows Server 2016- oder 2019-AD FS-Adapter werden die folgenden personenbezogenen Daten gespeichert:

| Ereignistyp                           | Datenspeichertyp |
|--------------------------------------|-----------------|
| OATH-Token                           | In Multi-Factor Authentication-Protokollen<br />Datenspeicher für Multi-Factor Authentication-Aktivitätsberichte |
| Unidirektionale SMS                          | In Multi-Factor Authentication-Protokollen<br />Datenspeicher für Multi-Factor Authentication-Aktivitätsberichte |
| Anruf                           | In Multi-Factor Authentication-Protokollen<br />Datenspeicher für Multi-Factor Authentication-Aktivitätsberichte<br />Blockierte Benutzer bei Betrugsmeldungen |
| Microsoft Authenticator-Benachrichtigung | In Multi-Factor Authentication-Protokollen<br />Datenspeicher für Multi-Factor Authentication-Aktivitätsberichte<br />Blockierte Benutzer bei Betrugsmeldungen<br />Änderungsanforderungen bei Änderung des Microsoft Authenticator-Gerätetokens |

### <a name="multifactor-authentication-server"></a>Multi-Factor Authentication-Server

Wenn Sie Azure AD Multi-Factor Authentication-Server bereitstellen und ausführen, werden die folgenden personenbezogenen Daten gespeichert:

> [!IMPORTANT]
> Ab dem 1. Juli 2019 bietet Microsoft keine Multi-Factor Authentication-Server mehr für neue Bereitstellungen an. Neue Kunden, die für ihre Benutzer eine mehrstufige Authentifizierung einrichten möchten, können stattdessen cloudbasierte Azure AD Multi-Factor Authentication verwenden. Bestehende Kunden, die ihren Multi-Factor Authentication-Server vor dem 1. Juli aktiviert haben, können weiterhin die neuesten Versionen und zukünftige Updates herunterladen sowie Anmeldedaten zur Aktivierung generieren.

| Ereignistyp                           | Datenspeichertyp |
|--------------------------------------|-----------------|
| OATH-Token                           | In Multi-Factor Authentication-Protokollen<br />Datenspeicher für Multi-Factor Authentication-Aktivitätsberichte |
| Unidirektionale SMS                          | In Multi-Factor Authentication-Protokollen<br />Datenspeicher für Multi-Factor Authentication-Aktivitätsberichte |
| Anruf                           | In Multi-Factor Authentication-Protokollen<br />Datenspeicher für Multi-Factor Authentication-Aktivitätsberichte<br />Blockierte Benutzer bei Betrugsmeldungen |
| Microsoft Authenticator-Benachrichtigung | In Multi-Factor Authentication-Protokollen<br />Datenspeicher für Multi-Factor Authentication-Aktivitätsberichte<br />Blockierte Benutzer bei Betrugsmeldungen<br />Änderungsanforderungen bei Änderung des Microsoft Authenticator-Gerätetokens |

## <a name="organizational-data-stored-by-azure-ad-multifactor-authentication"></a>Von Azure AD Multi-Factor Authentication gespeicherte organisationsbezogene Daten

Organisationsdaten sind Informationen auf Mandantenebene, die die Konfiguration oder das Umgebungssetup offenlegen können. In den Mandanteneinstellungen auf den folgenden Azure-Portal-Seiten zur Multi-Factor Authentication können Organisationsdaten wie Informationen zu Sperrschwellenwerten oder Aufrufer-ID für eingehende Anforderungen zur Telefonauthentifizierung gespeichert werden:

* Kontosperrung
* Betrugswarnung
* Benachrichtigungen
* Einstellungen für Telefonanruf

Und für Azure AD Multi-Factor Authentication-Server können die folgenden Azure-Portal-Seiten organisationsbezogene Daten enthalten:

* Servereinstellungen
* Einmalumgehung
* Cacheregeln
* Status des Multi-Factor Authentication-Servers

## <a name="multifactor-authentication-logs-location"></a>Speicherort der Multi-Factor Authentication-Protokolle

Die folgende Tabelle enthält den Speicherort für Dienstprotokolle für öffentliche Clouds:

| Öffentliche Cloud| Anmeldeprotokolle | Multi-Factor Authentication-Aktivitätsbericht        | Multi-Factor Authentication-Dienstprotokolle       |
|-------------|--------------|----------------------------------------|------------------------|
| US          | US           | US                                     | US                     |
| Europa      | Europa       | USA                                     | Europa<sup>2</sup>    |
| Australien   | Australien    | USA<sup>1</sup>                         | Australien<sup>2</sup> |

<sup>1</sup>Oath-Codeprotokolle werden in Australien gespeichert.

<sup>2</sup>Dienstprotokolle der Multi-Factor Authentication mit Sprachanrufen werden in den USA gespeichert.

Die folgende Tabelle enthält den Speicherort für Dienstprotokolle für Sovereign Clouds:

| Sovereign Cloud                      | Anmeldeprotokolle                         | Multi-Factor Authentication-Aktivitätsbericht (enthält personenbezogene Daten)| Multi-Factor Authentication-Dienstprotokolle |
|--------------------------------------|--------------------------------------|-------------------------------|------------------|
| Microsoft Azure Deutschland              | Deutschland                              | US                            | US               |
| Microsoft Azure, betrieben von 21Vianet | China                                | US                            | US               |
| Microsoft Government Cloud           | US                                   | US                            | US               |

Die Daten der Multi-Factor Authentication-Aktivitätsberichte beinhalten personenbezogene Daten, etwa den Benutzerprinzipalnamen (User Principal Name, UPN) und die vollständige Telefonnummer.

Die Multi-Factor Authentication-Dienstprotokolle werden für den Betrieb des Diensts verwendet.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Benutzerinformationen, die bei der cloudbasierten Azure AD Multi-Factor Authentication und von Azure AD Multi-Factor Authentication-Servern gesammelt werden, finden Sie unter [Azure AD Multi-Factor Authentication: Benutzerdatensammlung](howto-mfa-reporting-datacollection.md).
