---
title: 'Tutorial: Migrieren von Google Maps zu Azure Maps | Microsoft Azure Maps'
description: Hier finden Sie ein Tutorial zum Migrieren von Google Maps zu Microsoft Azure Maps. In diesem Leitfaden wird die Umstellung auf Azure Maps-APIs und SDKs erläutert.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 1f6f282406c6813b2b126c300f21bda21e8f9464
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76988973"
---
# <a name="migrate-from-google-maps-to-azure-maps"></a>Migrieren von Google Maps zu Azure Maps

Dieses Tutorial bietet Einblicke in die Migration von Web-, Mobil- und serverbasierten Anwendungen von Google Maps zur Microsoft Azure Maps-Plattform. Dieses Tutorial enthält vergleichende Codebeispiele, Migrationsvorschläge und Best Practices für die Migration zu Azure Maps.

## <a name="azure-maps-platform-overview"></a>Übersicht über die Azure Maps-Plattform

Azure Maps stellt Entwicklern aus allen Branchen leistungsstarke Geofunktionen mit regelmäßig aktualisierten Kartendaten zur Verfügung, um geografischen Kontext für Web- und Mobilanwendungen bereitzustellen. Azure Maps verfügt über verschiedene, mit der Azure One-API konforme REST-APIs. Diese Rest-APIs bieten Karten, Suche, Routenplanung, Verkehrsinfo, Zeitzonen, Geolocation, Geofencing, Kartendaten, Mobilität, Wetter und räumliche Operationen. Für Operationen stehen Web- und Android-SDKs zur Verfügung, um eine einfache, flexible und portable Entwicklung für verschiedene Plattformen zu ermöglichen.

## <a name="high-level-platform-comparison"></a>Allgemeiner Plattformvergleich

Die folgende Tabelle enthält eine allgemeine Liste von Azure Maps-Features mit entsprechenden Google Maps-Features. Hierbei handelt es sich nicht um eine vollständige Liste aller Azure Maps-Features. Azure Maps bietet noch weitere Features wie etwa Barrierefreiheit, Geofencing-APIs, Isochrone, räumliche Operationen, direkten Zugriff auf Kartenkacheln, Batchdienste und Vergleiche zur Datenabdeckung (Aufnahmenabdeckung).

| Google Maps-Feature         | Azure Maps-Unterstützung                     |
|-----------------------------|:--------------------------------------:|
| Web-SDK                     | ✓                                      |
| Android-SDK                 | ✓                                      |
| iOS SDK                     | Geplant                                |
| REST-Dienst-APIs           | ✓                                      |
| Richtungen (Routing)        | ✓                                      |
| Entfernungsmatrix             | ✓                                      |
| Elevation                   | Geplant                                |
| Geocodierung (vorwärts/rückwärts) | ✓                                      |
| Geolocation                 | –                                    |
| Suche nach Orten               | ✓                                      |
| Details zu Orten              | N/A – Website und Telefonnummer verfügbar |
| Fotos von Orten               | –                                    |
| AutoVervollständigen von Orten          | ✓                                      |
| Statische Karten                 | ✓                                      |
| Statische Straßenansicht          | –                                    |
| Zeitzone                   | ✓                                      |
| In API eingebettete Karten           | –                                    |
| Karten-URLs                    | –                                    |

Google Maps bietet eine grundlegende schlüsselbasierte Authentifizierung. Azure Maps bietet sowohl eine grundlegende schlüsselbasierte Authentifizierung als auch eine sehr sichere Azure Active Directory-Authentifizierung.

## <a name="licensing-considerations"></a>Lizenzierungsaspekte

Bei der Migration von Google Maps zu Azure Maps sollten folgende Punkte bei der Lizenzierung beachtet werden.

- Bei Azure Maps wird die Nutzung interaktiver Karten (basierend auf der Anzahl geladener Kartenkacheln) abgerechnet. Bei Google Maps werden die Gebühren dagegen für das Laden des Kartensteuerelements berechnet. In den interaktiven Azure Maps SDKs werden Karten Kacheln automatisch zwischengespeichert, um die Kosten für den Entwickler zu reduzieren. Für jeweils 15 geladene Kartenkacheln wird eine Azure Maps-Transaktion generiert. Die interaktiven Azure Maps SDKs verwenden 512-Pixel-Kacheln und generieren im Durchschnitt mindestens eine Transaktion pro Seitenansicht.
- Es ist oft wesentlich kostengünstiger, statische Kartenbilder von Google Maps-Webdiensten durch das Azure Maps Web SDK zu ersetzen. Das Azure Maps Web SDK verwendet Kartenkacheln, und sofern der Benutzer in der Karte nicht schwenkt und zoomt, wird oftmals nur ein Bruchteil einer Transaktion pro Kartenladevorgang generiert. Das Azure Maps Web SDK verfügt über Optionen zum Deaktivieren von Schwenken und Zoomen. Außerdem bietet das Azure Maps Web SDK wesentlich mehr Optionen für die Datenvisualisierung als ein statischer Kartenwebdienst.
- Azure Maps ermöglicht es, Daten von seiner Plattform in Azure zu speichern. Sie können auch gemäß den [Nutzungsbedingungen](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46) an anderer Stelle für bis zu sechs Monate zwischengespeichert werden.

Nachfolgend finden Sie einige zugehörige Ressourcen für Azure Maps:

- [Preisübersicht für Azure Maps](https://azure.microsoft.com/pricing/details/azure-maps/)
- [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=azure-maps)
- [Nutzungsbedingungen für Azure Maps](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46) (in den Microsoft-Bedingungen für Onlinedienste inbegriffen)
- [Auswählen des richtigen Tarifs in Azure Maps](https://docs.microsoft.com/azure/azure-maps/choose-pricing-tier)

## <a name="suggested-migration-plan"></a>Empfohlener Migrationsplan

Nachfolgend finden Sie einen allgemeinen Migrationsplan.

1. Stellen Sie fest, welche SDKs und Dienste von Google Maps von Ihrer Anwendung verwendet werden, und überprüfen Sie, ob Azure Maps alternative SDKs und Dienste anbietet, zu denen Sie migrieren können.
2. Erstellen Sie auf [https://azure.com](https://azure.com) ein Azure-Abonnement, falls Sie noch keins besitzen.
3. Erstellen Sie ein Azure Maps-Konto ([Dokumentation](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys)) und einen Authentifizierungsschlüssel oder Azure Active Directory ([Dokumentation](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication)).
4. Migrieren Sie den Anwendungscode.
5. Testen Sie Ihre migrierte Anwendung.
6. Stellen Sie die migrierte Anwendung in der Produktionsumgebung bereit.

## <a name="azure-maps-technical-resources"></a>Technische Azure Maps-Ressourcen

Nachfolgend finden Sie eine Liste nützlicher technischer Ressourcen für Azure Maps.

- Übersicht: [https://azure.com/maps](https://azure.com/maps)
- Dokumentation: [https://aka.ms/AzureMapsDocs](https://aka.ms/AzureMapsDocs)
- Web SDK-Codebeispiele:[https://aka.ms/AzureMapsSamples](https://aka.ms/AzureMapsSamples)
- Entwicklerforen: [https://aka.ms/AzureMapsForums](https://aka.ms/AzureMapsForums)
- Videos: [https://aka.ms/AzureMapsVideos](https://aka.ms/AzureMapsVideos)
- Blog: [https://aka.ms/AzureMapsBlog](https://aka.ms/AzureMapsBlog)
- Azure Maps-Feedback (UserVoice): [https://aka.ms/AzureMapsFeedback](https://aka.ms/AzureMapsFeedback)

## <a name="migration-support"></a>Migrationsunterstützung

Entwickler können über die [Foren](https://aka.ms/AzureMapsForums) oder über eine der vielen Azure-Supportoptionen Unterstützung bei der Migration erhalten: [https://azure.microsoft.com/support/options](https://azure.microsoft.com/support/options)

## <a name="next-steps"></a>Nächste Schritte

In diesen Artikeln erfahren Sie, wie Sie Ihre Google Maps-Anwendungen migrieren:

> [!div class="nextstepaction"]
> [Migrieren einer Web-App](migrate-from-google-maps-web-app.md)

> [!div class="nextstepaction"]
> [Migrieren einer Android-App](migrate-from-google-maps-android-app.md)

> [!div class="nextstepaction"]
> [Migrieren eines Webdiensts](migrate-from-google-maps-web-services.md)
