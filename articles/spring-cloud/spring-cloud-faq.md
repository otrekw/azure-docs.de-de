---
title: Häufig gestellte Fragen zu Azure Spring Cloud | Microsoft-Dokumentation
description: In diesem Artikel finden Sie häufig gestellte Fragen zu Azure Spring Cloud.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/07/2019
ms.author: brendm
ms.openlocfilehash: e4fbeef06ae49ffe24f84b1a12dbcdfe0a5f1fec
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2020
ms.locfileid: "76278571"
---
# <a name="azure-spring-cloud-faq"></a>Häufig gestellte Fragen zu Azure Spring Cloud

In diesem Artikel finden Sie häufig gestellte Fragen zu Azure Spring Cloud. 

## <a name="general"></a>Allgemein

### <a name="why-azure-spring-cloud"></a>Vorteile von Azure Spring Cloud

Azure Spring Cloud bietet Spring Cloud-Entwicklern eine PaaS-Lösung (Platform-as-a-Service). Azure Spring Cloud verwaltet Ihre Anwendungsinfrastruktur, damit Sie sich auf den Anwendungscode und die Geschäftslogik konzentrieren können. Zu den wichtigsten in Azure Spring Cloud integrierten Features gehören unter anderem Eureka, Konfigurationsserver, Dienstregistrierungsserver, Pivotal Build Service, Blau/Grün-Bereitstellungen und mehr. Mit diesem Dienst können Entwickler außerdem Ihre Anwendungen an andere Azure-Dienste wie Azure Cosmos DB, Azure Database for MySQL und Azure Cache for Redis binden.

Azure Spring Cloud verbessert durch die Integration von Azure Monitor, Application Insights und Log Analytics die Anwendungsdiagnose für Entwickler und Operatoren.

### <a name="what-service-plans-does-azure-spring-cloud-offer"></a>Welche Dienstpläne sind für Azure Spring Cloud verfügbar?

Während der Vorschauphase bietet Azure Spring Cloud nur einen Serviceplan.  Eine Spring Cloud-Bereitstellung umfasst 16 vCPU-Kerne und 32 GB Arbeitsspeicher.  Die obere Grenze für jede Microservice-Instanz innerhalb einer Bereitstellung beträgt 4 vCPU-Kerne mit 8 GB Arbeitsspeicher.

Resource | Amount
------- | -------
App-Instanzen pro Spring-Anwendung | 20
Gesamtanzahl der App-Instanzen pro Azure Spring Cloud-Dienstinstanz | 500
Azure Spring Cloud-Dienstinstanzen pro Region und Abonnement | 10
Persistente Volumes | 10 × 50 GB

\* _Zum Erhöhen des Grenzwerts müssen Sie ein [Supportticket](https://azure.microsoft.com/support/faq/) erstellen._

Weitere Informationen finden Sie unter [Häufig gestellte Fragen zum Azure-Support](https://azure.microsoft.com/support/faq/).

### <a name="how-secure-is-azure-spring-cloud"></a>Wie sicher ist Azure Spring Cloud?

Sicherheit und Datenschutz gehören zu den wichtigsten Prioritäten für Azure- und Azure Spring Cloud-Kunden. Azure gewährleistet durch eine sichere Datenverschlüsselung, dass nur Kunden Zugriff auf Anwendungsdaten, Protokolle oder Konfigurationen haben. In Azure Spring Cloud sind alle Dienstinstanzen voneinander isoliert.

Azure Spring Cloud bietet eine umfassende SSL- und Zertifikatverwaltung.

Wichtige Sicherheitspatches für OpenJDK- und Spring Cloud-Runtimes werden so bald wie möglich auf Azure Spring Cloud angewendet.

### <a name="in-which-regions-is-azure-spring-cloud-available"></a>In welchen Regionen ist Azure Spring Cloud verfügbar?

„USA, Osten“, „USA, Westen 2“, „Europa, Westen“ und „Asien, Südosten“.

### <a name="what-are-the-known-limitations-of-azure-spring-cloud"></a>Welche Einschränkungen sind für Azure Spring Cloud bekannt?

Während dem Vorschaurelease wies Azure Spring Cloud die folgenden Einschränkungen auf:

* `spring.application.name` wird durch den Anwendungsnamen überschrieben, der zum Erstellen der einzelnen Anwendungen verwendet wird.
* `server.port` ist in der Konfigurationsdatei aus dem Git-Repository nicht zulässig. Wenn diese Eigenschaft zur Konfigurationsdatei hinzugefügt wird, führt das wahrscheinlich dazu, dass Ihre Anwendung für andere Anwendungen und für das Internet nicht erreichbar ist.
* Das Hochladen von Anwendungspaketen wird vom Azure-Portal und von Azure Resource Manager-Vorlagen nicht unterstützt. Sie können Anwendungspakete nur hochladen, indem Sie die Anwendung über die Azure CLI bereitstellen.
* Weitere Informationen zu Kontingentbeschränkungen finden Sie unter [Welche Dienstpläne sind für Azure Spring Cloud verfügbar?](#what-service-plans-does-azure-spring-cloud-offer).

### <a name="how-can-i-provide-feedback-and-report-issues"></a>Wie kann ich Feedback geben und Probleme melden?

Wenn bei der Azure Spring Cloud Probleme auftreten, erstellen Sie eine [Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Besuchen Sie [Azure-Feedback](https://feedback.azure.com/forums/34192--general-feedback), um eine Featureanforderung oder Feedback einzureichen.

## <a name="development"></a>Entwicklung

### <a name="i-am-a-spring-cloud-developer-but-new-to-azure-what-is-the-quickest-way-for-me-to-learn-how-to-develop-an-azure-spring-cloud-application"></a>Ich bin Spring Cloud-Entwickler, habe aber noch nicht mit Azure gearbeitet. Wie lerne ich am schnellsten, wie ich eine Azure Spring Cloud-Anwendung entwickle?

Für den schnellsten Einstieg mit Azure Spring Cloud führen Sie die Anweisungen im [Schnellstart: Starten einer Azure Spring Cloud-Anwendung über das Azure-Portal](spring-cloud-quickstart-launch-app-portal.md).

### <a name="what-java-runtime-does-azure-spring-cloud-support"></a>Welche Java-Runtime unterstützt Azure Spring Cloud?

Azure Spring Cloud unterstützt Java 8 und 11.

### <a name="where-can-i-view-my-spring-cloud-application-logs-and-metrics"></a>Wo kann ich meine Spring Cloud-Anwendungsprotokolle und -metriken einsehen?

Metriken finden Sie auf der Registerkarte „App Overview“ (App-Übersicht) sowie der Registerkarte [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics#interacting-with-azure-monitor-metrics).

Azure Spring Cloud unterstützt das Exportieren von Spring Cloud-Anwendungsprotokollen und -metriken in Azure Storage, EventHub und [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs#log-queries). Der Tabellenname im Log Analytics lautet *AppPlatformLogsforSpring*. Informationen zum Aktivieren dieser Protokollierung finden Sie unter [Diagnosedienste](diagnostic-services.md).

### <a name="does-azure-spring-cloud-support-distributed-tracing"></a>Unterstützt Azure Spring Cloud die verteilte Ablaufverfolgung?

Ja. Weitere Informationen finden Sie im [Tutorial: Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud](spring-cloud-tutorial-distributed-tracing.md).

### <a name="what-resource-types-does-service-binding-support"></a>Welche Ressourcentypen werden von der Dienstbindung unterstützt?

Derzeit werden drei Dienste unterstützt: Azure Cosmos DB, Azure Database for MySQL und Azure Cache for Redis.

### <a name="can-i-view-add-or-move-persistent-volumes-from-inside-my-applications"></a>Kann ich persistente Volumes innerhalb meiner Anwendungen anzeigen, hinzufügen oder verschieben?

Ja.

## <a name="deployment"></a>Bereitstellung

### <a name="does-azure-spring-cloud-support-blue-green-deployment"></a>Werden Blau/Grün-Bereitstellungen von Azure Spring Cloud unterstützt?
Ja. Weitere Informationen finden Sie unter [Einrichten einer Stagingumgebung](spring-cloud-howto-staging-environment.md).

### <a name="can-i-access-kubernetes-to-manipulate-my-application-containers"></a>Kann ich auf Kubernetes zugreifen, um meine Anwendungscontainer zu bearbeiten?

Nein.  Azure Spring Cloud trennt den Entwickler von der zugrunde liegenden Architektur, sodass Sie sich auf den Anwendungscode und die Geschäftslogik konzentrieren können.

### <a name="does-azure-spring-cloud-support-building-containers-from-source"></a>Unterstützt Azure Spring Cloud das Erstellen von Containern aus einer Quelle?

Ja. Weitere Informationen finden Sie unter [Starten Ihrer Spring Cloud-Anwendung über den Quellcode](spring-cloud-launch-from-source.md).

### <a name="does-azure-spring-cloud-support-autoscaling-in-app-instances"></a>Unterstützt Azure Spring Cloud die automatische Skalierung in App-Instanzen?

Nein.

### <a name="what-are-the-best-practices-for-migrating-existing-spring-cloud-microservices-to-azure-spring-cloud"></a>Wie lauten die bewährten Methoden für die Migration vorhandener Spring Cloud-Microservices zu Azure Spring Cloud?

Beim Migrieren vorhandener Spring Cloud-Microservices zu Azure Spring Cloud empfiehlt es sich, die folgenden bewährten Methoden zu befolgen:
* Alle Anwendungsabhängigkeiten müssen aufgelöst werden.
* Bereiten Sie Ihre Konfigurationseinträge, Umgebungsvariablen und JVM-Parameter so vor, dass Sie sie mit der Bereitstellung in Azure Spring Cloud vergleichen können.
* Wenn Sie die Dienstbindung verwenden möchten, überprüfen Sie Ihre Azure-Dienste, und stellen Sie sicher, dass Sie die entsprechenden Zugriffsberechtigungen festgelegt haben.
* Es wird empfohlen, dass Sie jegliche eingebettete Dienste, die möglicherweise mit von Azure Spring Cloud verwalteten Diensten wie dem Ermittlungsdienst, dem Konfigurationsserver usw. im Konflikt stehen können, entfernen oder deaktivieren.
* Es wird empfohlen, dass Sie offizielle, stabile Pivotal Spring-Bibliotheken verwenden. Inoffizielle oder verzweigte Versionen und Betaversionen von Pivotal Spring-Bibliotheken haben keine SLA-Unterstützung (Vereinbarung zum Servicelevel).

Überwachen Sie nach der Migration Ihre CPU-/RAM-Metriken und den Netzwerkdatenverkehr, um sicherzustellen, dass die Anwendungsinstanzen entsprechend skaliert werden.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Fragen haben, finden Sie weitere Informationen im [Leitfaden zur Problembehandlung bei Azure Spring Cloud](spring-cloud-troubleshoot.md).
