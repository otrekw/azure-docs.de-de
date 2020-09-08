---
title: Einführung in Azure Spring Cloud
description: Lernen Sie die Features und Vorteile von Azure Spring Cloud zur Bereitstellung und Verwaltung von Java Spring-Anwendungen in Azure kennen.
author: bmitchell287
ms.service: spring-cloud
ms.topic: overview
ms.date: 11/4/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: aa3f1032301224701f5bfc08807e89194f263da6
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89255238"
---
# <a name="what-is-azure-spring-cloud"></a>Was ist Azure Spring Cloud?

Azure Spring Cloud vereinfacht das Bereitstellen von Spring Boot-basierten Microserviceanwendungen ohne Codeänderungen in Azure.  Azure Spring Cloud verwaltet die Infrastruktur von Spring Cloud-Anwendungen und ermöglicht es Entwicklern dadurch, sich auf ihren Code zu konzentrieren.  Spring Cloud bietet Lebenszyklusverwaltung durch umfassende Überwachung und Diagnose, Konfigurationsverwaltung, Dienstermittlung, CI/CD-Integration, Blaugrün-Bereitstellungen und mehr.

Als Teil des Azure-Ökosystems ermöglicht Azure Spring Cloud eine einfache Anbindung an andere Azure-Dienste wie Speicher, Datenbanken, Überwachung und mehr.

In dieser Einführung erfahren Sie mehr über den Azure Spring Cloud-Konfigurationsserver. Darüber hinaus wird erläutert, wie Sie Blau/Grün-Bereitstellungen aktivieren, Apps skalieren und die App-Leistung überwachen.

## <a name="spring-cloud-config-server"></a>Spring Cloud-Konfigurationsserver

Der Azure Spring Cloud-Konfigurationsserver bietet externalisierte Konfiguration in einem verteilten System mit server- und clientseitiger Unterstützung.  Der Azure Spring Cloud-Konfigurationsserver bietet einen zentralen Ort zum Verwalten von Anwendungseigenschaften in allen Umgebungen. Weitere Informationen finden Sie in der [Referenz zum Spring Cloud-Konfigurationsserver](https://spring.io/projects/spring-cloud-config.md). 

## <a name="enable-bluegreen-deployments"></a>Blau/Grün-Bereitstellungen aktivieren

Azure Spring Cloud unterstützt Blau/Grün-Bereitstellungen zur Freigabe und Aktualisierung von Code für Produktionsumgebungen.  Dieses Change Management-Muster ermöglicht es Entwicklern, Funktionen und Codeänderungen mit der Sicherheit eines sofortigen Fallbacks bei Bedarf zu implementieren.  Entwicklern können sich auf das Schreiben von Code mit mehreren Produktionsumgebungen konzentrieren, um Codeänderungen ohne Unterbrechung der Anwendung zu aktualisieren oder rückgängig zu machen.  Weitere Informationen zu Stagingumgebungen und Blau/Grün-Bereitstellungen finden Sie in diesem [Anleitungsartikel](spring-cloud-howto-staging-environment.md).

## <a name="automate-cicd-pipelines"></a>Automatisieren von CI/CD-Pipelines

Azure Spring Cloud ermöglicht die Integration mit Azure DevOps mithilfe der Azure CLI.  Mithilfe von Azure DevOps können Sie die Integration und Bereitstellung von Code für Ihre Spring-Anwendung automatisieren.  Weitere Informationen finden Sie in diesem [Artikel](spring-cloud-howto-cicd.md).

## <a name="scale-your-application"></a>Skalieren Ihrer Anwendung

Azure Spring Cloud ermöglicht Ihnen das einfache Skalieren der Microservices in Ihrem Azure Spring Cloud-Dashboard.  Sowohl die Anzahl der vCPUs als auch die Speichermenge, die für Ihre Microservices zur Verfügung steht, können je nach Bedarf erhöht oder verringert werden.  Die Skalierung wird in Sekunden wirksam und erfordert weder Codeänderungen noch die erneute Bereitstellung.  Führen Sie dieses [Tutorial](spring-cloud-tutorial-scale-manual.md) aus, um mehr zu erfahren.

## <a name="application-monitoring"></a>Anwendungsüberwachung

### <a name="monitor-your-application-using-distributed-tracing-and-azure-app-insights"></a>Überwachen Ihrer Anwendung mit verteilter Ablaufverfolgung und Azure App Insights

Die Tools für die verteilte Ablaufverfolgung von Spring Cloud ermöglichen Entwicklern das Debuggen und Überwachen der komplexen Verbindungen zwischen Microservices in einer Anwendung.  Durch die Integration von [Spring Cloud Sleuth](https://spring.io/projects/spring-cloud-sleuth) in Azure [Application Insights](../azure-monitor/insights/insights-overview.md) stellt Azure leistungsstarke Funktionen für die verteilte Ablaufverfolgung direkt über das Azure-Portal bereit.  Führen Sie dieses [Tutorial](spring-cloud-tutorial-distributed-tracing.md) aus, um mehr zu erfahren.

## <a name="next-steps"></a>Nächste Schritte
Arbeiten Sie zum Einstieg die Spring Cloud-Schnellstartanleitung durch:
> [!div class="nextstepaction"]
> [Schnellstart: Bereitstellen Ihrer ersten Azure Spring Cloud-Anwendung](spring-cloud-quickstart.md)

Weitere Beispiele finden Sie auf GitHub: [Azure Spring Cloud-Beispiele](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
