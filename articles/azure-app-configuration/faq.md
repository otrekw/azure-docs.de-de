---
title: Häufig gestellte Fragen zu Azure App Configuration | Microsoft-Dokumentation
description: Häufig gestellte Fragen zu Azure App Configuration
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: 8d286cbab33a1fb6a2d2a2cb70caed11b21af735
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904089"
---
# <a name="azure-app-configuration-faq"></a>Häufig gestellte Fragen zu Azure App Configuration

In diesem Artikel werden häufig gestellte Fragen zu Azure App Configuration beantwortet.

## <a name="how-is-app-configuration-different-from-azure-key-vault"></a>Wie unterscheidet sich App Configuration von Azure Key Vault?

App Configuration ist auf einen bestimmten Satz von Anwendungsfällen ausgelegt: Es hilft Entwicklern dabei, Anwendungseinstellungen zu verwalten und die Verfügbarkeit von Funktionen zu steuern. Es zielt darauf ab, viele der Aufgaben, aus denen die Arbeit mit komplexen Konfigurationsdaten besteht, zu vereinfachen.

App Configuration unterstützt Folgendes:

- Hierarchische Namespaces
- Bezeichnungen
- Umfangreiche Abfragen
- Batchabruf
- Spezielle Verwaltungsvorgänge
- Eine Benutzeroberfläche zur Featureverwaltung

App Configuration stellt eine Ergänzung zu Key Vault dar, und die beiden sollten in den meisten Anwendungsbereitstellungen nebeneinander verwendet werden.

## <a name="should-i-store-secrets-in-app-configuration"></a>Sollte ich Geheimnisse in App Configuration speichern?

App Configuration bietet zwar verstärkte Sicherheit, aber Key Vault ist weiterhin der beste Ort zum Speichern von Anwendungsgeheimnissen. Key Vault bietet Verschlüsselung auf Hardwareebene, granulare Zugriffsrichtlinien und Verwaltungsvorgänge wie die Zertifikatsrotation.

Sie können App Configuration-Werte erstellen, die auf in Key Vault gespeicherte Geheimnisse verweisen. Weitere Informationen finden Sie unter [Verwenden von Key Vault-Verweisen in einer ASP.NET Core-App](./use-key-vault-references-dotnet-core.md).

## <a name="does-app-configuration-encrypt-my-data"></a>Verschlüsselt App Configuration meine Daten?

Ja. App Configuration verschlüsselt alle Schlüsselwerte, die der Dienst enthält, sowie die Netzwerkkommunikation. Schlüsselnamen werden als Indizes zum Abrufen von Konfigurationsdaten verwendet und nicht verschlüsselt.

## <a name="how-is-app-configuration-different-from-azure-app-service-settings"></a>Wie unterscheidet sich App Configuration von Azure App Service-Einstellungen?

Azure App Service gestattet es Ihnen, App-Einstellungen für jede App Service-Instanz zu definieren. Diese Einstellungen werden als Umgebungsvariablen an den Anwendungscode übergeben. Wenn gewünscht, können Sie einem bestimmten Bereitstellungsslot eine Einstellung zuordnen. Weitere Informationen finden Sie unter [Konfigurieren von App-Einstellungen](/azure/app-service/configure-common#configure-app-settings).

Im Gegensatz dazu gestattet Ihnen Azure App Configuration das Definieren von Einstellungen, die von mehreren Apps gemeinsam genutzt werden können, einschließlich Apps, die in App Service ausgeführt werden. Auf diese Einstellungen kann im Anwendungscode über die Konfigurationsanbieter für .NET und Java, über das Azure SDK oder direkt über REST-APIs zugegriffen werden.

Sie können Einstellungen auch zwischen App Service und App Configuration importieren und exportieren. Auf diese Weise können Sie schnell einen neuen App Configuration-Speicher auf Grundlage vorhandener App Service Einstellungen einrichten oder die Konfiguration problemlos mit einer vorhandenen App teilen, die auf App Service-Einstellungen basiert.

## <a name="are-there-any-size-limitations-on-keys-and-values-stored-in-app-configuration"></a>Gibt es Größenbeschränkungen für Schlüssel und Werte, die in App Configuration gespeichert sind?

Für ein einzelnes Schlüssel-Wert-Element ist eine Beschränkung von 10 KB vorhanden.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>Wie sollte ich Konfigurationen für mehrere Umgebungen (Test, Staging, Produktion usw.) speichern?

Zurzeit können Sie auf Speicherebene (also pro Speicher) kontrollieren, wer Zugriff auf App Configuration hat. Verwenden Sie für jede Umgebung, die andere Berechtigungen erfordert, einen separaten Speicher. Dieser Ansatz bietet Ihnen die beste Sicherheitsisolierung.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>Welche Methoden werden für die Verwendung von App Configuration empfohlen?

Siehe unter [Bewährte Methoden](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>Wie viel kostet App Configuration?

Der Dienst ist während der öffentlichen Vorschauphase kostenlos.

## <a name="how-can-i-receive-announcements-on-new-releases-and-other-information-related-to-app-configuration"></a>Wie kann ich Ankündigungen zu neuen Releases und andere Informationen im Zusammenhang mit App Configuration erhalten?

Abonnieren Sie das [GitHub-Ankündigungsrepository](https://github.com/Azure/AppConfiguration-Announcements).

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>Wie kann ich ein Problem melden oder einen Vorschlag machen?

Sie erreichen uns direkt auf [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zu Azure App Configuration](./overview.md)
