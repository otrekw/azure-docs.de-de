---
title: Informationen zu API-Connectors in Azure AD B2C
description: Verwenden Sie die API-Connectors von Azure Active Directory (Azure AD), um Ihre Flows zur Benutzerregistrierung mithilfe von Web-APIs anzupassen und zu erweitern.
services: active-directory-b2c
ms.service: active-directory
ms.subservice: B2C
ms.topic: how-to
ms.date: 09/30/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.openlocfilehash: 195101a432d16c2236ea2d164416e75df33b12e4
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91828225"
---
# <a name="use-api-connectors-to-customize-and-extend-sign-up-user-flows"></a>Verwenden von API-Connectors zum Anpassen und Erweitern von Flows zur Benutzerregistrierung

## <a name="overview"></a>Übersicht 
Als Entwickler oder IT-Administrator können Sie API-Connectors verwenden, um Ihre Flows zur Benutzerregistrierung mit Web-APIs zu integrieren und die Registrierungsumgebung anzupassen. API-Connectors bieten folgende Möglichkeiten:

- **Durchführen der Identitätsüberprüfung**. Verwenden Sie einen Identitätsüberprüfungsdienst, um Entscheidungen bei der Kontoerstellung eine zusätzliche Sicherheitsstufe hinzuzufügen.
- **Überprüfen von Benutzereingabedaten**. Überprüfen Sie auf fehlerhafte oder ungültige Benutzerdaten. Beispielsweise können Sie vom Benutzer bereitgestellte Daten im Vergleich mit vorhandenen Daten in einem externen Datenspeicher oder einer Liste zulässiger Werte überprüfen. Falls ungültig, können Sie einen Benutzer auffordern, gültige Daten anzugeben, oder den Benutzer daran hindern, den Registrierungsflow fortzusetzen.
- **Integrieren in einen benutzerdefinierten Genehmigungsworkflow**. Verbinden mit einem benutzerdefinierten Genehmigungssystem zum Verwalten und Einschränken der Kontoerstellung.
- **Überschreiben von Benutzerattributen**. Sie können ein vom Benutzer erfasstes Attribut neu formatieren oder ihm einen Wert zuweisen. Wenn z.B. ein Benutzer den Vornamen vollständig in Kleinbuchstaben oder Großbuchstaben eingibt, können Sie den Namen so formatieren, dass nur der erste Buchstabe groß geschrieben wird. 
- **Ausführen von benutzerdefinierter Geschäftslogik**. Sie können nachgelagerte Ereignisse in Ihren Cloudsystemen auslösen, um Pushbenachrichtigungen zu senden, Unternehmensdatenbanken zu aktualisieren, Berechtigungen zu verwalten, Datenbanken zu überwachen und andere benutzerdefinierte Aktionen auszuführen.

Ein API-Connector stellt Azure Active Directory die erforderlichen Informationen zum Aufrufen einer API bereit, einschließlich Endpunkt-URL und Authentifizierung. Sobald Sie einen API-Connector konfiguriert haben, können Sie ihn für einen bestimmten Schritt in einem Benutzerflow aktivieren. Wenn ein Benutzer diesen Schritt im Registrierungsflow erreicht, wird der API-Connector aufgerufen und als HTTP-POST-Anforderung an Ihre API materialisiert, wobei Benutzerinformationen („Ansprüche“) als Schlüssel-Wert-Paare in einem JSON-Text gesendet werden. Die API-Antwort kann die Ausführung des Benutzerflows beeinträchtigen. Beispielsweise kann die API-Antwort einen Benutzer an der Registrierung hindern, den Benutzer zur erneuten Eingabe von Informationen auffordern oder Benutzerattribute überschreiben und anfügen.

## <a name="where-you-can-enable-an-api-connector-in-a-user-flow"></a>Wann ein API-Connector in einem Benutzerflow aktiviert werden kann

Es gibt zwei Punkte in einem Benutzerflow, an denen Sie einen API-Connector aktivieren können:

- Nach Anmeldung bei einem Identitätsanbieter
- Vor dem Erstellen des Benutzers

> [!IMPORTANT]
> In beiden Fällen werden die API-Connectors bei der **Registrierung** und nicht bei der Anmeldung eines Benutzer aufgerufen.

### <a name="after-signing-in-with-an-identity-provider"></a>Nach Anmeldung bei einem Identitätsanbieter

Ein API-Connector in diesem Schritt des Registrierungsprozesses wird unmittelbar nach der Authentifizierung des Benutzers mit einem Identitätsanbieter (z. B. Google, Facebook und Azure AD) aufgerufen. Dieser Schritt geht der ***Seite zur Attributsammlung***  voraus, die dem Benutzer zum Sammeln von Benutzerattributen angezeigt wird. Dieser Schritt wird nicht aufgerufen, wenn sich ein Benutzer mit einem lokalen Konto registriert. Im Folgenden finden Sie Beispiele für Szenarien mit API-Connectors, die Sie in diesem Schritt aktivieren können:

- Verwenden Sie die E-Mail- oder Verbundidentität, die der Benutzer angegeben hat, um Ansprüche in einem bestehenden System nachzuschlagen. Geben Sie diese Ansprüche aus dem bestehenden System zurück, füllen Sie die Seite zur Attributsammlung vorab aus, und stellen Sie sie zur Rückgabe im Token zur Verfügung.
- Implementieren Sie anhand der Social Media-Identität eine Zulassungs- oder Sperrliste.

### <a name="before-creating-the-user"></a>Vor dem Erstellen des Benutzers

Ein API-Connector in diesem Schritt des Registrierungsprozesses wird nach der Seite zur Attributsammlung aufgerufen, sofern vorhanden. Dieser Schritt wird immer aufgerufen, bevor ein Benutzerkonto erstellt wird. Es folgen Beispiele für Szenarien, die Sie an dieser Stelle während der Registrierung aktivieren könnten:

- Überprüfen eingegebener Benutzerdaten und Aufforderung an einen Benutzer, Daten erneut zu übermitteln
- Sperren einer Benutzerregistrierung auf Grundlage der vom Benutzer eingegebenen Daten
- Durchführen der Identitätsüberprüfung
- Abfragen externer Systeme nach vorhandenen Daten zum Benutzer, um diese im Anwendungstoken zurückzugeben oder in Azure AD zu speichern


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [einen API-Connector zu einem Benutzerflow hinzufügen](add-api-connector.md).
<!-- - Learn how to [add a custom approval system to self-service sign-up](add-approvals.md) -->