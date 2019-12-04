---
title: Trainingshandbuch für den Übergang von App-Registrierungen (Legacy) zur neuen App-Registrierungsumgebung im Azure-Portal
description: Einführung in die neue App-Registrierungsumgebung im Azure-Portal
services: active-directory
documentationcenter: ''
author: archieag
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: aragra
ms.reviewer: lenalepa, keyam
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01d543262a9eb358643c0860b24ac4306d2c5edf
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73927095"
---
# <a name="transitioning-from-app-registrations-legacy-to-the-new-app-registrations-experience-in-the-azure-portal"></a>Übergang von App-Registrierungen (Legacy) zur neuen App-Registrierungsumgebung im Azure-Portal

In der neuen Umgebung [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) im Azure-Portal finden Sie viele Verbesserungen. Wenn Ihnen die bisherige App-Registrierungsumgebung (Legacy) im Azure-Portal vertraut ist, können Sie mit diesem Trainingshandbuch erste Schritte in der neuen Umgebung ausführen.

In Azure Active Directory ist die hier beschriebene neue Oberfläche für die Anwendungsregistrierung allgemein verfügbar (Generally Available, GA). In Azure Active Directory B2C (Azure AD B2C) befindet sich diese Oberfläche in der Vorschauphase.

## <a name="key-changes"></a>Die wichtigsten Änderungen

- App-Registrierungen beschränken sich nicht auf *Web-App/API* oder auf eine *native* App. Sie können die gleiche App-Registrierung für all diese Apps verwenden, indem Sie die entsprechenden Umleitungs-URIs registrieren.

- In der bisherigen Umgebung wurden nur Apps unterstützt, die sich mit Organisationskonten (Azure AD) anmelden. Die Apps wurden als Einzelmandant registriert. Die Apps unterstützten nur Organisationskonten aus dem Verzeichnis, in dem die App registriert war. Die Apps konnten in mehrinstanzenfähige Apps geändert werden und alle Organisationskonten unterstützen. Die neue Umgebung ermöglicht die Registrierung von Apps, die diese beiden Optionen und sogar eine dritte Option unterstützen können: alle Organisationskonten und persönliche Microsoft-Konten.

- Die bisherige Benutzeroberfläche war nur verfügbar, wenn Sie sich im Azure-Portal mit einem Organisationskonto angemeldet hatten. In der neuen Umgebung können Sie persönliche Microsoft-Konten verwenden, die keinem Verzeichnis zugeordnet sind.

## <a name="list-of-applications"></a>Liste der Anwendungen

Die neue App-Liste enthält Anwendungen, die über die bisherige App-Registrierungsumgebung im Azure-Portal registriert wurden. Diese Apps melden sich mit Azure AD-Konten an. Die neue App-Liste enthält ebenso Apps, die über das Anwendungsregistrierungsportal registriert wurden. Diese Apps melden sich mit Azure AD- und persönlichen Microsoft-Konten an.

>[!NOTE]
>Das Anwendungsregistrierungsportal ist veraltet.

Die neue App-Liste enthält keine Spalte **Anwendungstyp** mehr, da eine einzelne App-Registrierung verschiedene Typen aufweisen kann. Die Liste enthält zwei zusätzliche Spalten: **Erstellt am** und **Zertifikate & Geheimnisse**. Die Spalte **Zertifikate & Geheimnisse** enthält den Status der Anmeldeinformationen, die für die App registriert wurden. Statuswerte sind **Aktuell**, **Läuft bald ab** und **Abgelaufen**.

## <a name="new-app-registration"></a>Neue App-Registrierung

In der bisherigen Umgebung mussten Sie zum Registrieren einer App Folgendes angeben: **Name**, **Anwendungstyp** und **Anmelde-URL/Umleitungs-URI**. Die erstellten Apps waren reine Azure AD-Anwendungen mit nur einem Mandanten. Unterstützt wurden nur Organisationskonten aus dem Verzeichnis, in dem die App registriert wurde.

In der neuen Umgebung müssen Sie einen **Namen** für die App angeben und die **unterstützten Kontotypen** auswählen. Sie können optional einen **Umleitungs-URI** angeben. Wenn Sie einen Umleitungs-URI angeben, müssen Sie den Typ „Web“ oder „Öffentlicher Client“ (Mobilgerät oder Desktop) angeben. Weitere Informationen finden Sie unter [Quickstart: Registrieren einer Anwendung bei der Microsoft Identity Platform](quickstart-register-app.md). Entsprechende Informationen zu Azure AD B2C finden Sie unter [Registrieren einer Anwendung in Azure Active Directory B2C](../../active-directory-b2c/tutorial-register-applications.md).

## <a name="differences-between-the-application-registration-portal-and-app-registrations-page"></a>Unterschiede zwischen dem Anwendungsregistrierungsportal und der Seite „App-Registrierungen“

### <a name="the-legacy-properties-page"></a>Die bisherige Seite „Eigenschaften“

Die bisherige Umgebung enthielt die Seite **Eigenschaften**. Die Seite **Eigenschaften** enthielt die folgenden Felder:

- **Name**
- **Objekt-ID**
- **Anwendungs-ID**
- **App-ID-URI**
- **Logo**
- **URL der Startseite**
- **Abmelde-URL**
- **URL zu den Vertragsbedingungen**
- **URL zur Datenschutzerklärung**
- **Anwendungstyp**
- **Mehrere Mandanten**

Diese Seite gibt es in der neuen Umgebung nicht. Die jeweilige Funktionalität finden Sie an folgenden Stellen:

- **Name**, **Logo**, **URL der Startseite**, **URL zu den Vertragsbedingungen** und **URL zur Datenschutzerklärung** befinden sich jetzt auf der Seite **Branding** der App.
- **Objekt-ID** und **Anwendungs-ID (Client)** befinden sich auf der Seite **Übersicht**.
- Die Funktionalität, die auf der bisherigen Benutzeroberfläche über den Schalter **Mehrinstanzenfähig** gesteuert wurde, wurde durch die Option **Unterstützte Kontotypen** ersetzt, die sich auf der Seite **Authentifizierung** befindet. Weitere Informationen finden Sie unter [Quickstart: Ändern der von einer Anwendung unterstützten Konten](quickstart-modify-supported-accounts.md).
- Die Option **Abmelde-URL** befindet sich jetzt auf der Seite **Authentifizierung**.
- **Anwendungstyp** ist kein gültiges Feld mehr. Stattdessen bestimmen die Umleitungs-URIs (auf der Seite **Authentifizierung**), welche App-Typen unterstützt werden.
- Die Option **App-ID-URI** wurde umbenannt in **Anwendungs-ID-URI** und befindet sich jetzt unter **Eine API verfügbar machen**. In der bisherigen Umgebung wurde diese Eigenschaft automatisch mit folgendem Format registriert: `https://{tenantdomain}/{appID}` (z. B. `https://microsoft.onmicrosoft.com/492439af-3282-44c3-b297-45463339544b`). In der neuen Umgebung wird sie automatisch als `api://{appID}` generiert, muss aber explizit gespeichert werden. In Azure AD B2C-Mandanten wird weiterhin das Format `https://{tenantdomain}/{appID}` verwendet.

### <a name="reply-urlsredirect-urls"></a>Antwort-URLs/Umleitungs-URIs

Auf der bisherigen Benutzeroberfläche gab es für eine App die Seite **Antwort-URLs**. In der neuen Umgebung befinden sich die Antwort-URLs auf der Seite **Authentifizierung** der App. Sie werden nun als **Umleitungs-URIs** bezeichnet.

Das Format der Umleitungs-URIs hat sich geändert. Sie müssen einem App-Typ („Web“ oder „Öffentlicher Client“) zugeordnet werden. Aus Sicherheitsgründen werden Platzhalter und `http://`-Schemas nicht unterstützt (mit Ausnahme von *http://localhost* ).

### <a name="keyscertificates--secrets"></a>Schlüssel/Zertifikate und Geheimnisse

Auf der bisherigen Benutzeroberfläche gab es für eine App die Seite **Schlüssel**. In der neuen Umgebung wurde daraus **Zertifikate & Geheimnisse**.

**Öffentliche Schlüssel** werden nun als **Zertifikate** bezeichnet. **Kennwörter** werden jetzt als **Geheime Clientschlüssel** bezeichnet.

### <a name="required-permissionsapi-permissions"></a>Erforderliche Berechtigungen/API-Berechtigungen

Auf der bisherigen Benutzeroberfläche gab es für eine App die Seite **Erforderliche Berechtigungen**. In der neuen Umgebung wurde die Option umbenannt in **API-Berechtigungen**.

Wenn Sie in der bisherigen Umgebung eine API ausgewählt haben, stand eine kleine Liste von Microsoft-APIs zur Auswahl. Sie konnten auch die Dienstprinzipale im Mandanten durchsuchen. In der neuen Umgebung stehen mehrere Registerkarten zur Auswahl: **Microsoft-APIs**, **Von meiner Organisation verwendete APIs** oder **Meine APIs**. Mit der Suchleiste auf der Registerkarte **Von meiner Organisation verwendete APIs** können Sie nach den Dienstprinzipalen im Mandanten suchen.

> [!NOTE]
> Diese Registerkarte wird nicht angezeigt, wenn Ihre Anwendung keinem Mandanten zugeordnet ist. Weitere Informationen zum Anfordern von Berechtigungen finden Sie unter [Schnellstart: Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](quickstart-configure-app-access-web-apis.md).

Auf der bisherigen Benutzeroberfläche gab es am oberen Rand der Seite **Angeforderte Berechtigungen** die Schaltfläche **Berechtigungen erteilen**. Die neue Umgebung enthält im Bereich **API-Berechtigungen** für eine App die Seite **Einwilligung erteilen** mit der Schaltfläche **Administratoreinwilligung erteilen**. Außerdem gibt es einige Unterschiede in der Funktionsweise der Schaltflächen.

In der bisherigen Umgebung veränderte sich die Logik in Abhängigkeit vom angemeldeten Benutzer und von den angeforderten Berechtigungen. Folgende Logik kam zum Einsatz:

- Wenn nur benutzerzustimmungsfähige Berechtigungen angefordert wurden und der angemeldete Benutzer kein Administrator war, konnte der Benutzer die Benutzerzustimmung für die angeforderten Berechtigungen erteilen.
- Wenn mindestens eine angeforderte Berechtigung die Zustimmung eines Administrators erforderte und der angemeldete Benutzer kein Administrator war, wurde dem Benutzer beim Versuch, die Benutzerzustimmung zu erteilen, ein Fehler angezeigt.
- War der angemeldete Benutzer jedoch ein Administrator, wurde für alle angeforderten Berechtigungen Administratorzustimmung erteilt.

In der neuen Umgebung kann nur ein Administrator die Zustimmung erteilen. Wenn ein Administrator **Administratorzustimmung erteilen** auswählt, wird für alle angeforderten Berechtigungen Administratorzustimmung erteilt.

## <a name="deleting-an-app-registration"></a>Löschen einer App-Registrierung

In der bisherigen Umgebung konnten Sie nur Apps mit einem Mandanten löschen. Bei mehrinstanzenfähigen Apps war die Schaltfläche „Löschen“ deaktiviert. In der neuen Umgebung können Apps in jedem Zustand gelöscht werden. Die Aktion muss jedoch bestätigt werden. Weitere Informationen finden Sie unter [Quickstart: Entfernen einer bei Microsoft Identity Platform registrierten Anwendung](quickstart-remove-app.md).

## <a name="application-manifest"></a>Anwendungsmanifest

In der bisherigen und der neuen Umgebung werden im Manifest-Editor unterschiedliche Formatversionen für die JSON-Datei verwendet. Weitere Informationen finden Sie unter [Azure Active Directory-App-Manifest](reference-app-manifest.md).

## <a name="new-ui"></a>Neue Benutzeroberfläche

In der neuen Umgebung wurden der Benutzeroberfläche Steuerelemente für die folgenden Eigenschaften hinzugefügt:

- Die Seite **Authentifizierung** enthält das Element **Flow für implizite Genehmigungen** (`oauth2AllowImplicitFlow`). Im Gegensatz zur bisherigen Umgebung können Sie **Zugriffstoken** und/oder **ID-Token** aktivieren.
- Die Seite **Eine API verfügbar machen** enthält die Elemente **Durch diese API definierte Bereiche** (`oauth2Permissions`) und **Autorisierte Clientanwendungen** (`preAuthorizedApplications`). Weitere Informationen zum Konfigurieren einer App als Web-API und zum Verfügbarmachen von Berechtigungen/Bereichen finden Sie unter [Schnellstart: Konfigurieren einer Anwendung für das Verfügbarmachen von Web-APIs](quickstart-configure-app-expose-web-apis.md).
- Die Seite **Branding** enthält das Element **Herausgeberdomäne**. Die Herausgeberdomäne wird dem Benutzer in der [Zustimmungsaufforderung der App](application-consent-experience.md) angezeigt. Weitere Informationen finden Sie unter [Gewusst wie: Konfigurieren der Herausgeberdomäne einer Anwendung](howto-configure-publisher-domain.md).

## <a name="limitations"></a>Einschränkungen

Für die neue Umgebung gelten die folgenden Einschränkungen:

- Das Format von geheimen Clientschlüsseln (App-Kennwörtern) unterscheidet sich von dem auf der Legacyoberfläche verwendeten Format und kann zu CLI-Unterbrechungen führen.
- Das Ändern des Werts für unterstützte Konten wird auf der Benutzeroberfläche nicht unterstützt. Sie müssen dazu das App-Manifest verwenden, sofern Sie nicht zwischen Azure AD mit einem Mandanten und mehreren Mandanten wechseln.
