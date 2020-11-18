---
title: Identitätsmodell
titleSuffix: An Azure Communication Services concept
description: Weitere Informationen zu Identitäten und Zugriffstoken
author: tomaschladek
manager: nmurav
services: azure-communication-services
ms.author: tchladek
ms.date: 10/26/2020
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 996f138a14923319381738e7a55cd7ba4e8c4320
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2020
ms.locfileid: "94517766"
---
# <a name="identity-model"></a>Identitätsmodell

Azure Communication Services ist ein identitätsunabhängiger Dienst. Dieses Design bietet mehrere Vorteile:
- Wiederverwenden vorhandener Identitäten aus Ihrem Identitätsverwaltungssystem
- Flexibilität für Integrationsszenarien
- Hält Ihre Identitäten privat im Azure Communication Services

Anstatt vorhandene Informationen in Ihrem System zu duplizieren, behalten Sie die für Ihren Geschäftsfall spezifische Zuordnungsbeziehung bei. Beispielsweise die Zuordnung von Identitäten 1:1, 1:N, N:1, N:M. Externe Bezeichner (z. B. Telefonnummern, Benutzer, Geräte, Anwendungen, GUID) können nicht als Azure-Kommunikationsidentität verwendet werden. Für die Azure Communication Service-Identität generierte Zugriffstoken werden für den Zugriff auf Primitive wie Chat oder Anrufe verwendet. 

## <a name="identity"></a>Identity

Identitäten werden mit der Azure Communication Service-Verwaltungsbibliothek erstellt. Die Identität fungiert als Bezeichner in den Unterhaltungen und wird zum Erstellen von Zugriffstoken verwendet. Dieselbe Identität kann an mehreren gleichzeitigen Sitzungen auf mehreren Geräten teilnehmen. Die Identität kann gleichzeitig über mehrere aktive Zugriffstoken verfügen. Das Löschen von Identitäten, Ressourcen oder Abonnements bewirkt, dass alle zugehörigen Zugriffstoken ungültig werden sowie die Löschung aller Daten, die für diese Identität gespeichert sind. Eine gelöschte Identität kann weder neue Zugriffstoken ausstellen, noch auf zuvor gespeicherte Daten zugreifen (z. B. Chatnachrichten). 

Ihnen wird nicht die Anzahl der Identitäten in Rechnung gestellt, die Sie haben, sondern die Verwendung von Primitiven. Die Anzahl der Identitäten muss nicht einschränken, wie die Identitäten Ihrer Anwendung den Azure Communication Services-Identitäten zugeordnet werden. Mit der Freiheit der Zuordnung kommt gleichzeitig die Verantwortung hinsichtlich des Datenschutzes. Wenn der Benutzer Ihrer Anwendung aus Ihrem System gelöscht werden möchte, müssen Sie alle Identitäten löschen, die diesem Benutzer zugeordnet wurden.

Azure Communication Services bietet keine speziellen Identitäten für anonyme Benutzer. Es hält die Zuordnung zwischen den Benutzern und Identitäten nicht aufrecht, sodass es nicht erkennen könnte, ob eine Identität anonym ist. Sie können das Konzept so entwerfen, dass es Ihren Anforderungen entspricht. Unsere Empfehlung besteht darin, für den anonymen Benutzer jeder Anwendung eine neue Identität zu erstellen. Durch den Besitz des gültigen Zugriffstokens kann jeder Zugriff auf den nicht gelöschten Inhalt einer Identität erhalten. Zum Beispiel vom Benutzer gesendete Chatnachrichten. Der Zugriff ist nur auf Bereiche beschränkt, die Bestandteil des Zugriffstokens sind. Weitere Informationen zu Bereichen finden Sie im Abschnitt *Zugriffstoken*.

### <a name="mapping-of-identities"></a>Zuordnen von Identitäten

Azure Communication Services repliziert nicht die Funktionalität von IMS. Es bietet Kunden keine Möglichkeit, kundenspezifische Identitäten zu verwenden. Beispielsweise Telefonnummer oder E-Mail-Adresse. Stattdessen werden eindeutige Bezeichner bereitstellt, die Sie den Identitäten Ihrer Anwendung zuweisen können. Azure Communication Services speichert keine Informationen, die die tatsächliche Identität Ihrer Benutzer offenlegen können.

Anstatt den Entwurf zu duplizieren, möchten wir Sie ermuntern, selbst zu entwerfen, wie Benutzer aus Ihrer Identitätsdomäne den Azure Communication Service-Identitäten zugeordnet werden. Sie können dabei jede Art von Muster verwenden, z. B. 1:1, 1:N, N:1 oder M:N. Sie können entscheiden, ob ein einzelner Benutzer einer einzelnen Identität oder mehreren Identitäten zugeordnet wird. Wenn eine neue Identität erstellt wird, wird empfohlen, dass Sie die Zuordnung dieser Identität im Benutzer oder den Benutzern Ihrer Anwendung speichern. Da Identitäten die Zugriffstoken für die Verwendung der Primitiven benötigen, muss die Identität für den Benutzer oder die Benutzer Ihrer Anwendung bekannt sein.

Wenn Sie eine relationale Datenbank für die Speicherung von Benutzern verwenden, kann sich die Implementierung je nach Ihrem Zuordnungsszenario unterscheiden. Für Szenarien mit der Zuordnung 1:1 oder N:1 können Sie der Tabelle eine Spalte *CommunicationServicesId* hinzufügen, um Ihre Azure Communication Services-Identität zu speichern. In Szenarien mit der Beziehung 1: N oder N:M sollten Sie in Erwägung ziehen, eine gesonderte Tabelle in der relationalen Datenbank zu erstellen.

## <a name="access-token"></a>Zugriffstoken

Das Zugriffstoken ist ein JWT-Token, das verwendet werden kann, um Zugriff auf Azure Communication Service-Primitive zu erhalten. Das ausgestellte Zugriffstoken besitzt Integritätsschutz, seine Ansprüche können also nach der Ausstellung nicht mehr geändert werden. Das heißt, dass das Zugriffstoken durch die manuelle Änderung von Eigenschaften wie Identität, Ablauf oder Bereichen ungültig wird. Die Verwendung von Primitiven mit ungültigen Token führt dazu, dass der Zugriff auf den Primitiv verweigert wird. 

Die Eigenschaften des Zugriffstokens lauten *identity, expiration* (Identität, Ablauf) und *scopes* (Bereiche). Ein Zugriffstoken ist immer 24 Stunden lang gültig. Nach Ablauf dieser Zeit wird das Zugriffstoken ungültig und kann nicht mehr für den Zugriff auf Primitive verwendet werden. Die Identität muss über eine Möglichkeit verfügen, ein neues Zugriffstoken vom serverseitigen Dienst anzufordern. Der Parameter *scope* (Bereich) definiert einen nicht leeren Satz von Primitiven, die verwendet werden können. Von Azure Communication Services werden die folgenden Bereiche für Zugriffstoken unterstützt:

|Name|BESCHREIBUNG|
|---|---|
|Chat|  Ermöglicht die Teilnahme an einem Chat|
|VoIP|  Ermöglicht das Anrufen von Identitäten und Telefonnummern|


Wenn Sie das Zugriffstoken vor dessen Ablauf widerrufen möchten, können Sie hierfür die Azure Communication Service-Verwaltungsbibliothek verwenden. Die Sperrung des Tokens erfolgt nicht sofort, und ihre Weitergabe kann bis zu 15 Minuten dauern. Das Entfernen von Identitäten, Ressourcen oder Abonnements führt zur Sperrung aller Zugriffstoken. Wenn Sie verhindern möchten, dass ein Benutzer auf bestimmte Funktionen zugreifen kann, widerrufen Sie alle Zugriffstoken. Stellen Sie dann ein neues Zugriffstoken mit einem eingeschränkteren Satz von Bereichen aus.
Die Rotation von Zugriffsschlüsseln von Azure Communication Service führt zur Sperrung aller aktiven Zugriffstoken, die mit dem vorherigen Zugriffsschlüssel erstellt wurden. Alle Identitäten verlieren den Zugriff auf Azure Communication Service und müssen neue Zugriffstoken ausstellen. 

Es wird empfohlen, Zugriffstoken in Ihrem serverseitigen Dienst und nicht in der Anwendung des Clients auszustellen. Der Grund dafür ist, dass das Ausstellen einen Zugriffsschlüssel oder eine verwaltete Identität erfordert. Aus Sicherheitsgründen wird davon abgeraten, die Zugriffsschlüssel mit der Anwendung des Clients zu teilen. Die Clientanwendung sollte einen vertrauenswürdigen Dienstendpunkt verwenden, der Ihre Clients authentifizieren und in deren Auftrag Zugriffstoken ausstellen kann. Weitere Informationen zur Architektur finden Sie [hier](./client-and-server-architecture.md).

Wenn Sie Zugriffstoken in einen Sicherungsspeicher zwischenspeichern, empfehlen wir die Verwendung von Verschlüsselung. Zugriffstoken sind sensible Daten und können für bösartige Aktivitäten verwendet werden, wenn sie nicht geschützt werden. Wer sich im Besitz des Zugriffstokens befindet, kann das SDK initialisieren und Zugriff auf die API erhalten. Die API, auf die zugegriffen werden kann, ist nur auf der Grundlage von Bereichen eingeschränkt, die zum Zugriffstoken gehören. Es wird empfohlen, Zugriffstoken nur mit Bereichen auszustellen, die erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in die Verwaltung von Zugriffstoken finden Sie unter [Erstellen und Verwalten von Zugriffstoken](https://docs.microsoft.com/azure/communication-services/quickstarts/access-tokens).
* Eine Einführung in die Authentifizierung finden Sie unter [Authentifizierung bei Azure Communication Services](https://docs.microsoft.com/azure/communication-services/concepts/authentication).
* Eine Einführung in die Datenresidenz und den Datenschutz finden Sie unter [Regionale Verfügbarkeit und Datenresidenz](https://docs.microsoft.com/azure/communication-services/concepts/privacy).