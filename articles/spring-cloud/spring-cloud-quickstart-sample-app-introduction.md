---
title: 'Schnellstart: Einführung in die Piggy Metrics-Beispiel-App – Azure Spring Cloud'
description: Hier wird die Piggy Metrics-Beispiel-App beschrieben, die für die Bereitstellung in Azure Spring Cloud verwendet wird.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 08/03/2020
ms.custom: devx-track-java
ms.openlocfilehash: d8eee715a3ac2256838b37c1c6722aa1307f6dd0
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89046781"
---
# <a name="introduction-to-piggy-metrics-sample-app"></a>Einführung in die Piggy Metrics-Beispiel-App

In dieser Schnellstartanleitung wird mit einem Beispiel für persönliche Finanzen mit dem Namen Piggy Metrics veranschaulicht, wie Sie eine App im Azure Spring Cloud-Dienst bereitstellen. Piggy Metrics veranschaulicht das Microservicearchitekturmuster sowie die Aufschlüsselung der Dienste. Sie erfahren, wie das Beispiel in Azure mit leistungsstarken Azure Spring Cloud-Funktionen wie Dienstermittlung, Konfigurationsserver, Protokolle, Metriken und verteilte Ablaufverfolgung bereitgestellt wird.

Für die Verwendung der Azure Spring Cloud-Bereitstellungsbeispiele benötigen Sie nur den Speicherort des Quellcodes. Dieser ist jeweils nach Bedarf angegeben.

## <a name="functional-services"></a>Funktionsdienste
Piggy Metrics wird in drei Hauptmicroservices unterteilt. Alle sind eigenständig bereitstellbare Anwendungen, die nach Geschäftsbereichen organisiert sind.

* **Kontodienst (für Bereitstellung vorgesehen)** : Enthält allgemeine Benutzereingabelogik und Validierung: Einkommen/Ausgaben, Einsparungen und Kontoeinstellungen.
* **Statistikdienst (in dieser Schnellstartanleitung nicht verwendet)** : Führt Berechnungen für wichtige Statistikparameter durch und erfasst Zeitreihen für jedes Konto. Der Datenpunkt enthält Werte, die auf die Basiswährung und den Zeitraum normalisiert sind. Diese Daten werden verwendet, um die Dynamik des Kapitalflusses während der Kontolebensdauer nachzuverfolgen.
* **Benachrichtigungsdienst (in dieser Schnellstartanleitung nicht verwendet)** : Speichert Kontaktinformationen und Benachrichtigungseinstellungen für Benutzer, z. B. Erinnerungs- und Sicherungshäufigkeit. Der geplante Worker sammelt erforderliche Informationen von anderen Diensten und sendet E-Mail-Nachrichten an abonnierte Kunden.

## <a name="infrastructure-services"></a>Infrastrukturdienste
Es gibt mehrere gängige Muster in verteilten Systemen, die zur einwandfreien Funktionsweise von Kerndiensten beitragen. Azure Spring Cloud bietet leistungsfähige Tools, die das Verhalten von Spring Boot-Anwendungen zur Implementierung dieser Muster verbessern: 

* **Konfigurationsdienst (von Azure Spring Cloud gehostet)** : Die Azure Spring Cloud-Konfiguration ist ein horizontal skalierbarer zentralisierter Konfigurationsdienst für verteilte Systeme. Er nutzt ein austauschbares Repository, das derzeit lokalen Speicher, Git und Subversion unterstützt.
* **Diensterkennung (von Azure Spring Cloud gehostet)** : Sie ermöglicht die automatische Erkennung von Netzwerkadressen für Dienstinstanzen, die aufgrund von automatischer Skalierung, Fehlern und Upgrades dynamisch zugewiesene Adressen aufweisen können.
* **Authentifizierungsdienst (für Bereitstellung vorgesehen)** : Autorisierungszuständigkeiten werden vollständig auf einen separaten Server extrahiert, der OAuth2-Token für die Back-End-Ressourcendienste gewährt. Der Authentifizierungsserver führt die Benutzerautorisierung aus und sorgt für die sichere Kommunikation zwischen Computern innerhalb eines bestimmten Umkreises.
* **API-Gateway (für Bereitstellung vorgesehen)** : Die drei Kerndienste machen eine externe API für den Client verfügbar. In realen Systemen kann die Anzahl von Funktionen mit der Systemkomplexität sehr schnell wachsen. Am Rendern einer einzigen komplexen Webseite können Hunderte von Diensten beteiligt sein. Das API-Gateway ist ein einzelner Einstiegspunkt in das System und wird zum Verarbeiten von Anforderungen und zum Weiterleiten dieser Anforderungen an den entsprechenden Back-End-Dienst oder zum Aufrufen mehrerer Back-End-Dienste verwendet. Die Ergebnisse werden dabei aggregiert. 

## <a name="sample-usage-of-piggy-metrics"></a>Beispielnutzung von Piggy Metrics
Details zur vollständigen Implementierung finden Sie unter [Piggy Metrics](https://github.com/Azure-Samples/piggymetrics). In den Beispielen wird nach Bedarf auf den Quellcode verwiesen.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Bereitstellen einer Azure Spring Cloud-Instanz](spring-cloud-quickstart-provision-service-instance.md)
