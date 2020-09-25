---
title: Einführung in Azure Spring Cloud
description: Lernen Sie die Features und Vorteile von Azure Spring Cloud zur Bereitstellung und Verwaltung von Java Spring-Anwendungen in Azure kennen.
author: bmitchell287
ms.service: spring-cloud
ms.topic: overview
ms.date: 09/08/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: f622042c9a2e8cc5bc11c8dc909d02afe38e5c78
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90908368"
---
# <a name="what-is-azure-spring-cloud"></a>Was ist Azure Spring Cloud?

Azure Spring Cloud vereinfacht das Bereitstellen von Spring Boot-basierten Microserviceanwendungen ohne Codeänderungen in Azure.  Azure Spring Cloud verwaltet die Infrastruktur von Spring Cloud-Anwendungen und ermöglicht es Entwicklern dadurch, sich auf ihren Code zu konzentrieren.  Spring Cloud bietet Lebenszyklusverwaltung durch umfassende Überwachung und Diagnose, Konfigurationsverwaltung, Dienstermittlung, CI/CD-Integration, Blaugrün-Bereitstellungen und mehr.

Azure Spring Cloud unterstützt sowohl Java [Spring Boot](https://spring.io/projects/spring-boot)- als auch ASP.NET Core-[Steeltoe](https://steeltoe.io/)-Apps. Steeltoe-Unterstützung wird zurzeit als öffentliche Vorschauversion angeboten. Angebote der Public Preview ermöglichen Ihnen das Experimentieren mit neuen Funktionen vor der offiziellen Veröffentlichung.  Funktionen und Dienste in der Public Preview sind nicht zur Verwendung in der Produktion bestimmt.  Weitere Informationen finden Sie in den [häufig gestellten Fragen](https://azure.microsoft.com/support/faq/). Sie können auch eine [Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) einreichen.

Als Teil des Azure-Ökosystems ermöglicht Azure Spring Cloud eine einfache Anbindung an andere Azure-Dienste wie Speicher, Datenbanken, Überwachung und mehr.

In dieser Einführung werden die folgenden Funktionen von Azure Spring Cloud beschrieben:

* Konfigurationsserver
* Blau/Grün-Bereitstellungen
* Anwendungsskalierung
* Integration mit Azure DevOps
* Anwendungsüberwachung

## <a name="spring-cloud-config-server"></a>Spring Cloud-Konfigurationsserver

Der Azure Spring Cloud-Konfigurationsserver bietet externalisierte Konfiguration in einem verteilten System mit server- und clientseitiger Unterstützung.  Der Azure Spring Cloud-Konfigurationsserver bietet einen zentralen Ort zum Verwalten von Anwendungseigenschaften in allen Umgebungen. Weitere Informationen finden Sie in der [Referenz zum Spring Cloud-Konfigurationsserver](https://spring.io/projects/spring-cloud-config.md). 

## <a name="bluegreen-deployments"></a>Blau/Grün-Bereitstellungen

Azure Spring Cloud unterstützt Blau/Grün-Bereitstellungen zur Freigabe und Aktualisierung von Code für Produktionsumgebungen.  Dieses Change Management-Muster ermöglicht es Entwicklern, Funktionen und Codeänderungen mit der Sicherheit eines sofortigen Fallbacks bei Bedarf zu implementieren.  Entwicklern können sich auf das Schreiben von Code mit mehreren Produktionsumgebungen konzentrieren, um Codeänderungen ohne Unterbrechung der Anwendung zu aktualisieren oder rückgängig zu machen.  Weitere Informationen zu Stagingumgebungen und Blau/Grün-Bereitstellungen finden Sie in diesem [Anleitungsartikel](spring-cloud-howto-staging-environment.md).

## <a name="cicd-pipeline-automation"></a>CI/CD-Pipelineautomatisierung

Azure Spring Cloud ermöglicht die Integration mit Azure DevOps mithilfe der Azure CLI.  Mithilfe von Azure DevOps können Sie die Integration und Bereitstellung von Code für Ihre Spring-Anwendung automatisieren.  Weitere Informationen finden Sie in diesem [Artikel](spring-cloud-howto-cicd.md).

## <a name="application-scaling"></a>Anwendungsskalierung

Azure Spring Cloud ermöglicht Ihnen das einfache Skalieren der Microservices in Ihrem Azure Spring Cloud-Dashboard.  Sowohl die Anzahl der vCPUs als auch die Speichermenge, die für Ihre Microservices zur Verfügung steht, können je nach Bedarf erhöht oder verringert werden.  Die Skalierung wird in Sekunden wirksam und erfordert weder Codeänderungen noch die erneute Bereitstellung.  Führen Sie dieses [Tutorial](spring-cloud-tutorial-scale-manual.md) aus, um mehr zu erfahren.

## <a name="application-monitoring"></a>Anwendungsüberwachung

Die Tools für die verteilte Ablaufverfolgung von Spring Cloud ermöglichen Entwicklern das Debuggen und Überwachen der komplexen Verbindungen zwischen Microservices in einer Anwendung.  Durch die Integration von [Spring Cloud Sleuth](https://spring.io/projects/spring-cloud-sleuth) in Azure [Application Insights](../azure-monitor/insights/insights-overview.md) stellt Azure leistungsstarke Funktionen für die verteilte Ablaufverfolgung direkt über das Azure-Portal bereit.  Führen Sie dieses [Tutorial](spring-cloud-tutorial-distributed-tracing.md) aus, um mehr zu erfahren.

## <a name="next-steps"></a>Nächste Schritte

Arbeiten Sie zum Einstieg die [Spring Cloud-Schnellstartanleitung](spring-cloud-quickstart.md) durch.

Beispiele finden Sie auf GitHub: [Azure Spring Cloud-Beispiele](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/).
