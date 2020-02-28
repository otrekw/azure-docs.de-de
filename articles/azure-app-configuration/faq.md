---
title: Häufig gestellte Fragen zu Azure App Configuration
description: Häufig gestellte Fragen zu Azure App Configuration
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: lcozzens
ms.openlocfilehash: 60ba0a7723861d6e642a23418dda6a1daa57f14e
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77523491"
---
# <a name="azure-app-configuration-faq"></a>Häufig gestellte Fragen zu Azure App Configuration

In diesem Artikel werden häufig gestellte Fragen zu Azure App Configuration beantwortet.

## <a name="how-is-app-configuration-different-from-azure-key-vault"></a>Wie unterscheidet sich App Configuration von Azure Key Vault?

App Configuration hilft Entwicklern dabei, Anwendungseinstellungen zu verwalten und die Verfügbarkeit von Funktionen zu steuern. Es zielt darauf ab, viele der Aufgaben, aus denen die Arbeit mit komplexen Konfigurationsdaten besteht, zu vereinfachen.

App Configuration unterstützt Folgendes:

- Hierarchische Namespaces
- Bezeichnungen
- Umfangreiche Abfragen
- Batchabruf
- Spezielle Verwaltungsvorgänge
- Eine Benutzeroberfläche zur Featureverwaltung

App Configuration ergänzt Key Vault, und die beiden sollten in den meisten Anwendungsbereitstellungen nebeneinander verwendet werden.

## <a name="should-i-store-secrets-in-app-configuration"></a>Sollte ich Geheimnisse in App Configuration speichern?

App Configuration bietet zwar verstärkte Sicherheit, aber Key Vault ist weiterhin der beste Ort zum Speichern von Anwendungsgeheimnissen. Key Vault bietet Verschlüsselung auf Hardwareebene, granulare Zugriffsrichtlinien und Verwaltungsvorgänge wie die Zertifikatsrotation.

Sie können App Configuration-Werte erstellen, die auf in Key Vault gespeicherte Geheimnisse verweisen. Weitere Informationen finden Sie unter [Verwenden von Key Vault-Verweisen in einer ASP.NET Core-App](./use-key-vault-references-dotnet-core.md).

## <a name="does-app-configuration-encrypt-my-data"></a>Verschlüsselt App Configuration meine Daten?

Ja. App Configuration verschlüsselt alle Schlüsselwerte, die der Dienst enthält, sowie die Netzwerkkommunikation. Schlüsselnamen und Bezeichnungen werden als Indizes zum Abrufen von Konfigurationsdaten verwendet und nicht verschlüsselt.

## <a name="how-is-app-configuration-different-from-azure-app-service-settings"></a>Wie unterscheidet sich App Configuration von Azure App Service-Einstellungen?

Azure App Service gestattet es Ihnen, App-Einstellungen für jede App Service-Instanz zu definieren. Diese Einstellungen werden als Umgebungsvariablen an den Anwendungscode übergeben. Wenn gewünscht, können Sie einem bestimmten Bereitstellungsslot eine Einstellung zuordnen. Weitere Informationen finden Sie unter [Konfigurieren von App-Einstellungen](/azure/app-service/configure-common#configure-app-settings).

Im Gegensatz dazu gestattet Ihnen Azure App Configuration das Definieren von Einstellungen, die von mehreren Apps gemeinsam genutzt werden können. Dies gilt auch für Apps, die sowohl in App Service als auch auf anderen Plattformen ausgeführt werden. Ihr Anwendungscode greift über die Konfigurationsanbieter für .NET und Java, über das Azure SDK oder direkt über REST-APIs auf diese Einstellungen zu.

Sie können Einstellungen auch zwischen App Service und App Configuration importieren und exportieren. Diese Funktion ermöglicht Ihnen, schnell einen neuen App Configuration-Speicher auf Basis vorhandener App Service-Einstellungen einzurichten. Sie können die Konfiguration auch für eine vorhandene App freigeben, die auf App Service-Einstellungen basiert.

## <a name="are-there-any-size-limitations-on-keys-and-values-stored-in-app-configuration"></a>Gibt es Größenbeschränkungen für Schlüssel und Werte, die in App Configuration gespeichert sind?

Für ein einzelnes Schlüssel-Wert-Element gilt eine Beschränkung von 10 KB.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>Wie sollte ich Konfigurationen für mehrere Umgebungen (Test, Staging, Produktion usw.) speichern?

Sie kontrollieren auf Speicherebene (also pro Speicher), wer Zugriff auf App Configuration hat. Verwenden Sie für jede Umgebung, die andere Berechtigungen erfordert, einen separaten Speicher. Dieser Ansatz bietet Ihnen die beste Sicherheitsisolierung.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>Welche Methoden werden für die Verwendung von App Configuration empfohlen?

Siehe unter [Bewährte Methoden](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>Wie viel kostet App Configuration?

Es gibt zwei Tarife: 

- Free-Tarif
- Standard-Tarif.

Wenn Sie vor der Einführung des Standard-Tarifs einen Speicher erstellt haben, wird er bei allgemeiner Verfügbarkeit automatisch in den Free-Tarif verschoben. Sie können ein Upgrade auf den Standard-Tarif durchführen, oder im Free-Tarif verbleiben.

Ein Downgrade vom Standard-Tarif in den Free-Tarif ist nicht möglich. Sie können einen neuen Speicher im Free-Tarif erstellen und dann Konfigurationsdaten in diesen Speicher importieren.

## <a name="which-app-configuration-tier-should-i-use"></a>Welchen App Configuration-Tarif sollte ich verwenden?

Beide App Configuration-Tarife bieten Kernfunktionen inklusive Konfigurationseinstellungen, Featureflags, Key Vault-Verweisen, grundlegender Verwaltungsvorgänge, Metriken und Protokolle.

Nachfolgend sind verschiedene Aspekte aufgeführt, die Ihnen bei der Auswahl eines Tarifs helfen können.

- **Ressourcen pro Abonnement**: Eine Ressource besteht aus einem einzelnen Konfigurationsspeicher. Jedes Abonnement ist auf einen Konfigurationsspeicher im Free-Tarif beschränkt. Abonnements können über eine unbegrenzte Anzahl von Konfigurationsspeichern im Standard-Tarif verfügen.
- **Speicher pro Ressource**: Im Free-Tarif ist jeder Konfigurationsspeicher auf 10 MB Speicherplatz beschränkt. Im Standard-Tarif kann jeder Konfigurationsspeicher bis zu 1 GB Speicherplatz verwenden.
- **Schlüsselverlauf**: App Configuration speichert den Verlauf aller an den Schlüsseln vorgenommenen Änderungen. Im Free-Tarif wird dieser Verlauf sieben Tage lang gespeichert. Im Standard-Tarif wird dieser Verlauf 30 Tage lang gespeichert.
- **Anforderungen pro Tag**: Speicher im Free-Tarif sind auf 1.000 Anforderungen pro Tag beschränkt. Sobald ein Speicher 1.000 Anforderungen erreicht, wird bis Mitternacht (UTC) für alle Anforderungen der HTTP-Statuscode 429 zurückgegeben.

    Für Speicher im Standard-Tarif sind die ersten 200.000 Anforderungen täglich in der täglichen Gebühr enthalten. Zusätzliche Anforderungen werden als Überschreitung abgerechnet.

- **Vereinbarung zum Servicelevel**: Die SLA des Standard-Tarifs sichert eine Verfügbarkeit von 99,9 % zu. Für den Free-Tarif gibt es keine SLA.
- **Sicherheitsfeatures**: Beide Tarife beinhalten grundlegende Sicherheitsfunktionen einschließlich Verschlüsselung mit von Microsoft verwalteten Schlüsseln, Authentifizierung über HMAC oder Azure Active Directory, RBAC-Unterstützung und verwaltete Identität. Der Standard-Tarif bietet fortgeschrittenere Sicherheitsfunktionen einschließlich Private Link-Unterstützung und Verschlüsselung mit vom Kunden verwalteten Schlüsseln.
- **Kosten**: Für Speicher im Standard-Tarif fällt eine tägliche Nutzungsgebühr an. Es gibt auch eine Überschreitungsgebühr für Anforderungen, die über die tägliche Zuordnung hinausgehen. Es fallen keine Kosten für die Nutzung eines Speichers im Free-Tarif an.

## <a name="can-i-upgrade-a-store-from-the-free-tier-to-the-standard-tier-can-i-downgrade-a-store-from-the-standard-tier-to-the-free-tier"></a>Kann ich einen Speicher vom Free-Tarif in den Standard-Tarif upgraden? Kann ich einen Speicher vom Standard-Tarif in den Free-Tarif herabstufen?

Ein Upgrade vom Free-Tarif in den Standard-Tarif ist jederzeit möglich.

Ein Downgrade vom Standard-Tarif in den Free-Tarif ist nicht möglich. Sie können einen neuen Speicher im Free-Tarif erstellen und dann [Konfigurationsdaten in diesen Speicher importieren](howto-import-export-data.md).

## <a name="how-can-i-receive-announcements-on-new-releases-and-other-information-related-to-app-configuration"></a>Wie kann ich Ankündigungen zu neuen Releases und andere Informationen im Zusammenhang mit App Configuration erhalten?

Abonnieren Sie das [GitHub-Ankündigungsrepository](https://github.com/Azure/AppConfiguration-Announcements).

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>Wie kann ich ein Problem melden oder einen Vorschlag machen?

Sie erreichen uns direkt auf [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zu Azure App Configuration](./overview.md)
