---
title: 'Schnellstart: Einführung in die Beispiel-App – Azure Spring Cloud'
description: Hier wird die Beispiel-App beschrieben, die in dieser Reihe von Schnellstartanleitungen für die Bereitstellung in Azure Spring Cloud verwendet wird.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 09/08/2020
ms.custom: devx-track-java
zone_pivot_groups: programming-languages-spring-cloud
ms.openlocfilehash: 5c3bc83d19eddc448992e2d3fab7287e8d84e43a
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108132717"
---
# <a name="introduction-to-the-sample-app"></a>Einführung in die Beispiel-App

::: zone pivot="programming-language-csharp"
In dieser Reihe von Schnellstartanleitungen wird mithilfe einer Beispiel-App, die aus zwei Microservices besteht, veranschaulicht, wie Sie eine .NET Core-Steeltoe-App im Azure Spring Cloud-Dienst bereitstellen. Sie verwenden Azure Spring Cloud-Funktionen wie Dienstermittlung, Konfigurationsserver, Protokolle, Metriken und verteilte Ablaufverfolgung.

## <a name="functional-services"></a>Funktionsdienste

Die Beispiel-App besteht aus zwei Microservices:

* Der Dienst `planet-weather-provider` gibt Wettertext als Antwort auf eine HTTP-Anforderung zurück, die den Namen des Planeten angibt. Beispielsweise kann für den Planeten Merkur „sehr warm“ zurückgegeben werden. Die Wetterdaten werden vom Konfigurationsserver abgerufen. Der Konfigurationsserver ruft die Wetterdaten aus einer YAML-Datei in einem Git-Repository ab, z. B.:

  ```yaml
  MercuryWeather: very warm
  VenusWeather: quite unpleasant
  MarsWeather: very cool
  SaturnWeather: a little bit sandy
  ```

* Der Dienst `solar-system-weather` gibt Daten für vier Planeten als Antwort auf eine HTTP-Anforderung zurück. Er ruft die Daten ab, indem vier HTTP-Anforderungen an `planet-weather-provider` gesendet werden. Der Dienst nutzt den Eureka Server-Ermittlungsdienst, um `planet-weather-provider` aufzurufen. JSON-Code wird zurückgegeben, z. B.:

  ```json
  [{
      "Key": "Mercury",
      "Value": "very warm"
  }, {
      "Key": "Venus",
      "Value": "quite unpleasant"
  }, {
      "Key": "Mars",
      "Value": "very cool"
  }, {
      "Key": "Saturn",
      "Value": "a little bit sandy"
  }]
  ```

Die Architektur der Beispiel-App wird anhand des folgenden Diagramms veranschaulicht:

:::image type="content" source="media/spring-cloud-quickstart-sample-app-introduction/sample-app-diagram.png" alt-text="Beispiel-App-Diagramm":::

## <a name="code-repository"></a>Coderepository

Die Beispiel-App befindet sich im Ordner [steeltoe-sample](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/steeltoe-sample) des Repositorys „Azure-Samples/Azure-Spring-Cloud-Samples“ auf GitHub.

In den Anweisungen der folgenden Schnellstartanleitungen wird bei Bedarf auf den Quellcode verwiesen.

::: zone-end

::: zone pivot="programming-language-java"
In dieser Schnellstartanleitung wird mit einem Beispiel für persönliche Finanzen mit dem Namen PiggyMetrics veranschaulicht, wie Sie eine App im Azure Spring Cloud-Dienst bereitstellen. PiggyMetrics veranschaulicht das Microservicearchitekturmuster sowie die Aufschlüsselung der Dienste. Sie erfahren, wie das Beispiel in Azure mit leistungsstarken Azure Spring Cloud-Funktionen wie Dienstermittlung, Konfigurationsserver, Protokolle, Metriken und verteilte Ablaufverfolgung bereitgestellt wird.

Für die Verwendung der Azure Spring Cloud-Bereitstellungsbeispiele benötigen Sie nur den Speicherort des Quellcodes. Dieser ist jeweils nach Bedarf angegeben.

## <a name="functional-services"></a>Funktionsdienste

PiggyMetrics wird in drei Hauptmicroservices unterteilt. Alle sind eigenständig bereitstellbare Anwendungen, die nach Geschäftsbereichen organisiert sind.

* **Kontodienst (für Bereitstellung vorgesehen)** : Enthält allgemeine Benutzereingabelogik und Validierung: Einkommen/Ausgaben, Einsparungen und Kontoeinstellungen.
* **Statistikdienst (in dieser Schnellstartanleitung nicht verwendet)** : Führt Berechnungen für wichtige Statistikparameter durch und erfasst Zeitreihen für jedes Konto. Der Datenpunkt enthält Werte, die auf die Basiswährung und den Zeitraum normalisiert sind. Diese Daten werden verwendet, um die Dynamik des Kapitalflusses während der Kontolebensdauer nachzuverfolgen.
* **Benachrichtigungsdienst (in dieser Schnellstartanleitung nicht verwendet)** : Speichert Kontaktinformationen und Benachrichtigungseinstellungen für Benutzer, z. B. Erinnerungs- und Sicherungshäufigkeit. Der geplante Worker sammelt erforderliche Informationen von anderen Diensten und sendet E-Mail-Nachrichten an abonnierte Kunden.

## <a name="infrastructure-services"></a>Infrastrukturdienste

Es gibt mehrere gängige Muster in verteilten Systemen, die zur einwandfreien Funktionsweise von Kerndiensten beitragen. Azure Spring Cloud bietet leistungsfähige Tools, die das Verhalten von Spring Boot-Anwendungen zur Implementierung dieser Muster verbessern: 

* **Konfigurationsdienst (von Azure Spring Cloud gehostet)** : Die Azure Spring Cloud-Konfiguration ist ein horizontal skalierbarer zentralisierter Konfigurationsdienst für verteilte Systeme. Er nutzt ein austauschbares Repository, das derzeit lokalen Speicher, Git und Subversion unterstützt.
* **Diensterkennung (von Azure Spring Cloud gehostet)** : Sie ermöglicht die automatische Erkennung von Netzwerkadressen für Dienstinstanzen, die aufgrund von automatischer Skalierung, Fehlern und Upgrades dynamisch zugewiesene Adressen aufweisen können.
* **Authentifizierungsdienst (für Bereitstellung vorgesehen)** : Autorisierungszuständigkeiten werden vollständig auf einen separaten Server extrahiert, der OAuth2-Token für die Back-End-Ressourcendienste gewährt. Der Authentifizierungsserver führt die Benutzerautorisierung aus und sorgt für die sichere Kommunikation zwischen Computern innerhalb eines bestimmten Umkreises.
* **API-Gateway (für Bereitstellung vorgesehen)** : Die drei Kerndienste machen eine externe API für den Client verfügbar. In realen Systemen kann die Anzahl von Funktionen mit der Systemkomplexität sehr schnell wachsen. Am Rendern einer einzigen komplexen Webseite können Hunderte von Diensten beteiligt sein. Das API-Gateway ist ein einzelner Einstiegspunkt in das System und wird zum Verarbeiten von Anforderungen und zum Weiterleiten dieser Anforderungen an den entsprechenden Back-End-Dienst oder zum Aufrufen mehrerer Back-End-Dienste verwendet. Die Ergebnisse werden dabei aggregiert. 

## <a name="sample-usage-of-piggymetrics"></a>Beispielnutzung von PiggyMetrics

Details zur vollständigen Implementierung finden Sie unter [PiggyMetrics](https://github.com/Azure-Samples/piggymetrics). In den Beispielen wird nach Bedarf auf den Quellcode verwiesen.
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bereitstellen einer Azure Spring Cloud-Instanz](./quickstart-provision-service-instance.md)