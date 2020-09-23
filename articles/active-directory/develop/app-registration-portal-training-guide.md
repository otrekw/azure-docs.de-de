---
title: Neue Oberfläche für App-Registrierungen im Azure-Portal
titleSuffix: Microsoft identity platform
description: Einführung in die neue App-Registrierungsumgebung im Azure-Portal
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 11/8/2019
ms.author: marsma
ms.reviewer: lenalepa, alamaral
ms.custom: aaddev
ms.openlocfilehash: 13242a41e1d10b0df031bf10fd646d9ec3cf47c3
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89437767"
---
# <a name="the-new-azure-portal-app-registration-experience"></a>Die neue Oberfläche für App-Registrierungen im Azure-Portal

In der neuen Umgebung [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) im Azure-Portal gibt es viele Verbesserungen. Wenn Sie mit dem Anwendungsregistrierungsportal (apps.dev.microsoft.com) zum Registrieren oder Verwalten von Anwendungen (im Folgenden als alte Umgebung bezeichnet) gut vertraut sind, können Sie mit dieser Anleitung erste Schritte in der neuen Umgebung unternehmen.

## <a name="whats-not-changing"></a>Was ändert sich nicht?

- Ihre Anwendungen und die zugehörigen Konfigurationen finden Sie unverändert in der neuen Umgebung. Sie müssen die Anwendungen nicht erneut registrieren, und die Benutzer Ihrer Anwendungen müssen sich nicht erneut anmelden.

    > [!NOTE]
    > Sie müssen sich mit dem Konto anmelden, das Sie zum Registrieren der Anwendungen verwendet haben, damit diese im Azure-Portal angezeigt werden. Es wird empfohlen, zu überprüfen, ob der im Azure-Portal angemeldete Benutzer dem Benutzer entspricht, der im Anwendungsregistrierungsportal angemeldet war. Vergleichen Sie dazu die E-Mail-Adresse Ihres Profils.
    >
    > In einigen Fällen (besonders bei der Anmeldung mit persönlichen Microsoft-Konten wie Outlook, Live, Xbox usw. und einer Azure AD-E-Mail-Adresse) haben wir festgestellt, dass Sie beim Navigieren von der alten Umgebung zum Azure-Portal bei einem anderen Konto mit der gleichen E-Mail-Adresse im Azure AD-Mandanten angemeldet werden. Wenn Ihre Anwendungen weiterhin zu fehlen scheinen, melden Sie sich ab, und melden Sie sich mit dem richtigen Konto wieder an.

- Live SDK-Apps, die mit persönlichen Microsoft-Konten erstellt wurden, werden im Azure-Portal noch nicht unterstützt und sind auch in naher Zukunft noch in der alten Umgebung zu finden.

## <a name="key-changes"></a>Die wichtigsten Änderungen

-   In der alten Umgebung wurden Apps standardmäßig als *konvergierte* Apps registriert – Apps, die alle Organisationskonten (mehrinstanzenfähig) und persönliche Microsoft-Konten unterstützten. Dies konnte in der alten Umgebung nicht geändert werden, sodass es schwierig war, Apps zu erstellen, die nur Organisationskonten unterstützten (entweder mehrinstanzenfähig oder Einzelmandant).
    In der neuen Umgebung können Sie Apps registrieren, die all diese Optionen unterstützen. [Weitere Informationen zu App-Typen](active-directory-v2-registration-portal.md).

-   Wenn sich auch Ihr persönliches Microsoft-Konto in einem Azure AD-Mandanten befindet, werden in der neuen Umgebung drei Registerkarten angezeigt: alle Anwendungen im Mandanten, in Ihrem Besitz befindliche Anwendungen im Mandanten sowie Anwendungen für Ihr persönliches Konto. Wenn also die für Ihr persönliches Microsoft-Konto registrierten Anwendungen zu fehlen scheinen, schauen Sie sich die Registerkarte **Anwendungen für Ihr persönliches Konto** an.

-   In der neuen Umgebung können Sie problemlos zwischen Mandanten wechseln, indem Sie zu Ihrem Profil navigieren und zu einem anderen Verzeichnis wechseln.

## <a name="list-of-applications"></a>Liste der Anwendungen

-   Die neue App-Liste enthält die Anwendungen, die über die alte Anwendungsregistrierungsumgebung im Azure-Portal registriert wurden (nur Apps mit Anmeldung bei Azure AD-Konten) sowie die Apps, die über das [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/) registriert wurden (Apps mit Anmeldung bei Azure AD- und persönlichen Microsoft-Konten).

-   Die neue App-Liste enthält zwei zusätzliche Spalten: **Erstellt am** und **Zertifikate & Geheimnisse**. Letztere Spalte enthält den Status der Anmeldeinformationen („Aktuell“, „Läuft bald ab“ oder „Abgelaufen“), die für die App registriert wurden.

## <a name="new-app-registration"></a>Neue App-Registrierung

In der alten Umgebung mussten Sie zum Registrieren einer App nur einen Namen angeben. Die erstellten Apps wurden als *konvergierte* Apps registriert – Apps, die alle Organisationskonten (mehrinstanzenfähig) und persönliche Microsoft-Konten unterstützten.  Dies konnte in der alten Umgebung nicht geändert werden, sodass es schwierig war, Apps zu erstellen, die nur Organisationskonten unterstützten (mehrinstanzenfähig oder Einzelmandant). [Weitere Informationen zu unterstützten Kontotypen](v2-supported-account-types.md)

In der neuen Umgebung müssen Sie einen Namen für die App angeben und die unterstützten Kontotypen auswählen. Sie können optional einen Umleitungs-URI angeben.
Wenn Sie einen Umleitungs-URI angeben, müssen Sie den Typ „Web“ oder „Öffentlicher Client“ angeben (native Anwendung/Mobilgerät und Desktop). Weitere Informationen zum Registrieren einer App mithilfe der neuen App-Registrierungsumgebung finden Sie [in dieser Schnellstartanleitung](quickstart-register-app.md).

## <a name="app-management-page"></a>Seite für die App-Verwaltung

In der alten Umgebung gab es eine einzelne Seite für die App-Verwaltung mit folgenden Bereichen: „Eigenschaften“, „Anwendungsgeheimnisse“, „Plattformen“, „Besitzer“, „Microsoft Graph-Berechtigungen“, „Profil“ und „Erweiterte Optionen“.

Die neue Umgebung im Azure-Portal enthält diese Funktionen auf separaten Seiten. Die jeweilige Funktionalität finden Sie an folgenden Stellen:

- Eigenschaften: Name und Anwendungs-ID befinden sich auf der Seite „Übersicht“.
- Anwendungsgeheimnisse befinden sich auf der Seite „Zertifikate & Geheimnisse“.
- Die Plattformkonfiguration befindet sich auf der Seite „Authentifizierung“.
- Die Microsoft Graph-Berechtigungen finden Sie neben anderen Berechtigungen auf der Seite „API-Berechtigungen“.
- Das Profil befindet sich auf der Seite „Branding“.
- Erweiterte Optionen: Live SDK-Support befindet sich auf der Seite „Authentifizierung“.

## <a name="application-secretscertificates--secrets"></a>Anwendungsgeheimnisse/Zertifikate & Geheimnisse

In der neuen Umgebung wurden **Anwendungsgeheimnisse** in **Zertifikate & Geheimnisse** umbenannt. Außerdem werden **Öffentliche Schlüssel** als **Zertifikate** und **Kennwörter** als **Geheime Clientschlüssel** bezeichnet. Wir haben uns aus Sicherheitsgründen entschieden, die Funktionalität nicht in die neue Umgebung zu übernehmen. Daher können Sie kein neues Schlüsselpaar mehr erstellen.

## <a name="platformsauthentication-reply-urlsredirect-uris"></a>Plattformen/Authentifizierung Antwort-URLs/Umleitungs-URIs
In der alten Umgebung gab es für eine App die Plattformabschnitte „Web“, „nativ“ und „Web API“ zum Konfigurieren von „Antwort-URLs“, „Abmelde-URL“ und „Impliziter Flow“.

In der neuen Umgebung befinden sich die Antwort-URLs im Abschnitt „Authentifizierung“ der App. Zudem werden sie jetzt als Umleitungs-URIs bezeichnet und haben ein anderes Format erhalten. Sie müssen einem App-Typ („Web“ oder „Öffentlicher Client“, Mobilgerät und Desktop) zugeordnet werden. [Weitere Informationen](quickstart-register-app.md#add-a-redirect-uri)

Web-APIs werden auf der Seite „Eine API verfügbar machen“ konfiguriert.

> [!NOTE]
> Testen Sie die neue Umgebung für die Authentifizierungseinstellungen, in der Sie Einstellungen für Ihre Anwendung basierend auf der Zielplattform oder dem Zielgerät konfigurieren können. [Weitere Informationen](quickstart-register-app.md#configure-platform-settings)

## <a name="microsoft-graph-permissionsapi-permissions"></a>Microsoft Graph-Berechtigungen/API-Berechtigungen

-   Bei der Auswahl einer API in der alten Umgebung standen nur Microsoft Graph-APIs zur Auswahl. In der neuen Umgebung stehen viele Microsoft-APIs (einschließlich Microsoft Graph), APIs Ihrer Organisation und eigene APIs zur Auswahl. Dafür gibt es drei Registerkarten: „Microsoft-APIs“, „Von meiner Organisation verwendete APIs“ oder „Meine APIs“. Mit der Suchleiste auf der Registerkarte „Von meiner Organisation verwendete APIs“ können Sie die Dienstprinzipale im Mandanten durchsuchen.

    > [!NOTE]
    > Diese Registerkarte wird nicht angezeigt, wenn Ihre Anwendung keinem Mandanten zugeordnet ist. Weitere Informationen zum Anfordern von Berechtigungen in der neuen Umgebung finden Sie [in dieser Schnellstartanleitung](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/active-directory/develop/quickstart-configure-app-access-web-apis.md).

-   In der alten Umgebung gab es die Schaltfläche **Berechtigungen erteilen** nicht. Die neue Umgebung enthält im Abschnitt „API-Berechtigungen“ für eine App den Bereich „Einwilligung erteilen“ mit der Schaltfläche **Administratoreinwilligung erteilen**. Nur ein Administrator kann eine Einwilligung erteilen. Daher ist diese Schaltfläche nur bei Administratoren aktiviert. Wenn ein Administrator die Schaltfläche **Administratorzustimmung erteilen** auswählt, wird für alle angeforderten Berechtigungen Administratorzustimmung erteilt.

## <a name="profile"></a>Profil
In der alten Umgebung konnten auf der Seite „Profil“ die Optionen „Logo“, „URL der Startseite“, „URL zu den Vertragsbedingungen“ und „URL zur Datenschutzerklärung“ konfiguriert werden. In der neuen Umgebung befinden sich diese Optionen auf der Seite „Branding“.

## <a name="application-manifest"></a>Anwendungsmanifest
In der neuen Umgebung können Sie auf der Seite „Manifest“ die Attribute der App bearbeiten und aktualisieren. Weitere Informationen finden Sie unter [Anwendungsmanifest](reference-app-manifest.md).

## <a name="new-ui"></a>Neue Benutzeroberfläche
Es gibt eine neue Benutzeroberfläche für die Eigenschaften, die bisher nur mit dem Manifest-Editor oder der API festgelegt werden konnten (oder nicht vorhanden waren).

-   Die Option „Impliziter Gewährungsflow“ (oauth2AllowImplicitFlow) befindet sich auf der Seite „Authentifizierung“. Im Gegensatz zur alten Umgebung können Sie Zugriffstoken oder ID-Token oder auch beide Optionen aktivieren.

-   „Durch diese API definierte Bereiche“ (oauth2Permissions) und „Autorisierte Clientanwendungen“ (preAuthorizedApplications) können auf der Seite „Eine API verfügbar machen“ konfiguriert werden. Weitere Informationen zum Konfigurieren einer App als Web-API und zum Verfügbarmachen von Berechtigungen/Bereichen finden Sie [in dieser Schnellstartanleitung](quickstart-configure-app-expose-web-apis.md).

-   Die Herausgeberdomäne (wird Benutzern bei der [Zustimmungsaufforderung der Anwendung angezeigt\'](application-consent-experience.md)) befindet sich auf der Seite „Branding“. Weitere Informationen zum Konfigurieren einer Herausgeberdomäne finden Sie [in dieser Anleitung](howto-configure-publisher-domain.md).

## <a name="limitations"></a>Einschränkungen

Für die neue Umgebung gelten die folgenden Einschränkungen:

-   Die neue Umgebung unterstützt noch keine App-Registrierungen für Azure AD B2C-Mandanten.

-   Die neue Umgebung unterstützt auch noch keine Live SDK-Apps, die mit persönlichen Microsoft-Konten erstellt werden.

-   Das Ändern des Werts für unterstützte Konten wird auf der Benutzeroberfläche nicht unterstützt. Sie müssen dazu das App-Manifest verwenden, sofern Sie nicht zwischen Azure AD mit einem Mandanten und mehreren Mandanten wechseln.

   > [!NOTE]
   > Wenn Sie im Azure AD-Mandanten ein persönliches Microsoft-Konto verwenden und der Mandantenadministrator den Zugriff auf das Azure-Portal beschränkt hat, erhalten Sie möglicherweise die Fehlermeldung „Zugriff verweigert“. Wenn Sie jedoch eine Verknüpfung verwenden, indem Sie App-Registrierungen in der Suchleiste eingeben oder diese anheften, können Sie auf die neue Umgebung zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit der neuen App-Registrierungsumgebung finden Sie unter [Schnellstart: Registrieren einer Anwendung bei der Microsoft Identity Platform](quickstart-register-app.md).
