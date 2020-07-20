---
title: 'Informationen zu API-Connectors in Self-Service-Registrierungsflows: Azure AD'
description: Verwenden Sie die API-Connectors von Azure Active Directory (Azure AD), um Ihre benutzerdefinierten Self-Service-Flows zur Benutzerregistrierung mithilfe von Web-APIs anzupassen und zu erweitern.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1c5e546c6eac77c4952a0d32d360f49d4251d49d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84905176"
---
# <a name="use-api-connectors-to-customize-and-extend-self-service-sign-up"></a>Verwenden von API-Connectors zum Anpassen und Erweitern der Self-Service-Registrierung 

## <a name="overview"></a>Übersicht 
Als Entwickler oder IT-Administrator können Sie API-Connectors verwenden, um Ihre [ Self-Service-Flows zur Benutzerregistrierung](self-service-sign-up-overview.md) bei externen Systemen durch Nutzung von Web-APIs zu integrieren. Beispielsweise können Sie mit API-Connectors folgende Aktionen ausführen:

- [**Integrieren in einen benutzerdefinierten Genehmigungsworkflow**](self-service-sign-up-add-approvals.md). Stellen Sie eine Verbindung mit einem benutzerdefinierten Genehmigungssystem zur Verwaltung der Kontoerstellung her.
- [**Durchführen der Identitätsüberprüfung**](code-samples-self-service-sign-up.md#identity-verification). Verwenden Sie einen Identitätsüberprüfungsdienst, um Entscheidungen bei der Kontoerstellung eine zusätzliche Sicherheitsstufe hinzuzufügen.
- **Überprüfen von Benutzereingabedaten**. Überprüfen Sie auf fehlerhafte oder ungültige Benutzerdaten. Beispielsweise können Sie vom Benutzer bereitgestellte Daten im Vergleich mit vorhandenen Daten in einem externen Datenspeicher oder einer Liste zulässiger Werte überprüfen. Falls ungültig, können Sie einen Benutzer auffordern, gültige Daten anzugeben, oder den Benutzer daran hindern, den Registrierungsflow fortzusetzen.
- **Überschreiben von Benutzerattributen**. Sie können ein vom Benutzer erfasstes Attribut neu formatieren oder ihm einen Wert zuweisen. Wenn z.B. ein Benutzer den Vornamen vollständig in Kleinbuchstaben oder Großbuchstaben eingibt, können Sie den Namen so formatieren, dass nur der erste Buchstabe groß geschrieben wird. 
<!-- - **Enrich user data**. Integrate with your external cloud systems that store user information to integrate them with the sign-up flow. For example, your API can receive the user's email address, query a CRM system, and return the user's loyalty number. Returned claims can be used to pre-fill form fields or return additional data in the application token.  -->
- **Ausführen von benutzerdefinierter Geschäftslogik**. Sie können nachgelagerte Ereignisse in Ihren Cloudsystemen auslösen, um Pushbenachrichtigungen zu senden, Unternehmensdatenbanken zu aktualisieren, Berechtigungen zu verwalten, Datenbanken zu überwachen und andere benutzerdefinierte Aktionen auszuführen.

Ein API-Connector stellt einen Vertrag zwischen Azure AD und einem API-Endpunkt dar, indem der HTTP-Endpunkt, die Authentifizierung, die Anforderung und die erwartete Antwort definiert werden. Sobald Sie einen API-Connector konfiguriert haben, können Sie ihn für einen bestimmten Schritt in einem Benutzerflow aktivieren. Wenn ein Benutzer diesen Schritt im Registrierungsflow erreicht, wird der API-Connector aufgerufen und als HTTP-POST-Anforderung materialisiert, wobei ausgewählte Ansprüche als Schlüssel-Wert-Paare in einem JSON-Text gesendet werden. Die API-Antwort kann die Ausführung des Benutzerflows beeinträchtigen. Beispielsweise kann die API-Antwort einen Benutzer an der Registrierung hindern, den Benutzer zur erneuten Eingabe von Informationen auffordern oder Benutzerattribute überschreiben und anfügen.

## <a name="where-you-can-enable-an-api-connector-in-a-user-flow"></a>Wann ein API-Connector in einem Benutzerflow aktiviert werden kann

Es gibt zwei Punkte in einem Benutzerflow, an denen Sie einen API-Connector aktivieren können:

- Nach Anmeldung bei einem Identitätsanbieter
- Vor dem Erstellen des Benutzers

In beiden Fällen werden die API-Connectors während der Registrierung und nicht bei der Anmeldung aufgerufen.

### <a name="after-signing-in-with-an-identity-provider"></a>Nach Anmeldung bei einem Identitätsanbieter

Ein API-Connector in diesem Schritt des Registrierungsprozesses wird unmittelbar nach der Authentifizierung des Benutzers bei einem Identitätsanbieter (Google, Facebook, Azure AD) aufgerufen. Dieser Schritt geht der ***Seite zur Attributsammlung***  voraus, die dem Benutzer zum Sammeln von Benutzerattributen angezeigt wird. Im Folgenden finden Sie Beispiele für Szenarien mit API-Connectors, die Sie in diesem Schritt aktivieren können:

- Verwenden Sie die E-Mail- oder Verbundidentität, die der Benutzer angegeben hat, um Ansprüche in einem bestehenden System nachzuschlagen. Geben Sie diese Ansprüche aus dem bestehenden System zurück, füllen Sie die Seite zur Attributsammlung vorab aus, und stellen Sie sie zur Rückgabe im Token zur Verfügung.
- Überprüfen Sie, ob der Benutzer in einer Zulassungs- oder Verweigerungsliste enthalten ist, und bestimmen Sie, ob er mit dem Registrierungsflow fortfahren kann.

### <a name="before-creating-the-user"></a>Vor dem Erstellen des Benutzers

Ein API-Connector in diesem Schritt des Registrierungsprozesses wird nach der Seite zur Attributsammlung aufgerufen, sofern vorhanden. Dieser Schritt wird stets aufgerufen, ehe ein Benutzerkonto in Azure AD erstellt wird. Es folgen Beispiele für Szenarien, die Sie an dieser Stelle während der Registrierung aktivieren könnten:

- Überprüfen eingegebener Benutzerdaten und Aufforderung an einen Benutzer, Daten erneut zu übermitteln
- Sperren einer Benutzerregistrierung auf Grundlage der vom Benutzer eingegebenen Daten
- Durchführen der Identitätsüberprüfung
- Abfragen externer Systeme nach vorhandenen Daten zum Benutzer, um diese im Anwendungstoken zurückzugeben oder in Azure AD zu speichern

<!-- > [!IMPORTANT]
> If an invalid response is returned or another error occurs (for example, a network error), the user will be redirected to the app with the error re -->

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [einen API-Connector zu einem Benutzerflow hinzufügen](self-service-sign-up-add-api-connector.md).
- Erfahren Sie, wie Sie [ein benutzerdefiniertes Genehmigungssystem zur Self-Service-Registrierung hinzufügen](self-service-sign-up-add-approvals.md).