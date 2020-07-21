---
title: 'Überwachen von Anwendungen, die in Azure Functions mit Application Insights ausgeführt werden: Azure Monitor | Microsoft-Dokumentation'
description: Azure Monitor kann nahtlos in Ihre in Azure Functions ausgeführte Anwendung integriert werden und ermöglicht die Überwachung der Leistung sowie eine blitzschnelle Erkennung von Problemen mit Ihren Apps.
ms.topic: conceptual
author: MS-jgol
ms.author: jgol
ms.date: 06/26/2020
ms.openlocfilehash: 093448ad0280ada039f1d4e5abd0e83e4be19b03
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85482104"
---
# <a name="monitoring-azure-functions-with-azure-monitor-application-insights"></a>Überwachen von Azure Functions mit Azure Monitor Application Insights

[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) bietet standardmäßige Integration in Azure Application Insights zum Überwachen von Funktionen. 

Application Insights sammelt Protokoll-, Leistungs- und Fehlerdaten und erkennt automatisch Leistungsanomalien. Application Insights verfügt über leistungsstarke Analysetools, mit denen Sie Probleme untersuchen und nachvollziehen können, wie Ihre Funktionen verwendet werden. Wenn Sie Einblick in Ihre Anwendungsdaten haben, können Sie Leistung und Benutzerfreundlichkeit kontinuierlich verbessern. Sie können Application Insights auch während der lokalen Entwicklung an Funktions-App-Projekten verwenden. 

Die erforderliche Application Insights-Instrumentierung ist in Azure Functions integriert. Sie benötigen lediglich einen gültigen Instrumentierungsschlüssel, um Ihre Funktions-App mit einer Application Insights-Ressource zu verbinden. Der Instrumentierungsschlüssel sollte den Anwendungseinstellungen hinzugefügt werden, wenn ihre Funktions-App-Ressource in Azure erstellt wird. Wenn Ihre Funktions-App noch nicht über diesen Schlüssel verfügt, können Sie ihn manuell festlegen. Weitere Informationen finden Sie unter [Überwachen von Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-monitoring?tabs=cmd).

## <a name="distributed-tracing-for-java-applications-on-windows-public-preview"></a>Verteilte Ablaufverfolgung für Java-Anwendungen unter Windows (Public Preview)

> [!IMPORTANT]
> Dieses Feature befindet sich derzeit in der Public Preview für Azure Functions mit Java unter Windows. Die verteilte Ablaufverfolgung für Azure Functions mit Java unter Linux wird nicht unterstützt. Im Verbrauchstarif weist das Feature einen Kaltstart von 8 bis 9 Sekunden auf.

Wenn Ihre Anwendungen in Java geschrieben sind, können Sie umfassendere Daten aus ihren Funktionsanwendungen anzeigen, einschließlich Anforderungen, Abhängigkeiten, Protokolle und Metriken. Die zusätzlichen Daten ermöglichen Ihnen außerdem das Anzeigen und Diagnostizieren von End-to-End-Transaktionen sowie eine Anwendungsübersicht, die viele Transaktionen aggregiert, um eine topologische Ansicht der Interaktion der Systeme sowie der durchschnittlichen Leistung und Fehlerraten zu erhalten.

Die End-to-End-Diagnose und die Anwendungsübersicht ermöglichen Einblicke in eine einzelne Transaktion/Anforderung. Gemeinsam sind diese beiden Features sehr hilfreich, um die Grundursache von Zuverlässigkeitsproblemen und Leistungsengpässen für jede Anforderung einzeln zu ermitteln.

### <a name="how-to-enable-distributed-tracing-for-java-function-apps"></a>Aktivieren der verteilten Ablaufverfolgung für Java-Funktions-Apps

Navigieren Sie zum Übersichtsblatt für Funktions-Apps und dann zu den Konfigurationen. Klicken Sie unter „Anwendungseinstellungen“ auf „+ Neue Anwendungseinstellung“. Fügen Sie die beiden folgenden Anwendungseinstellungen mit den unten angegebenen Werten hinzu, und klicken Sie dann oben links auf „Speichern“. FERTIG!

```
XDT_MicrosoftApplicationInsights_Java -> 1
ApplicationInsightsAgent_EXTENSION_VERSION -> ~2
```

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich ausführlicher über die Überwachung unter [Überwachen von Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-monitoring).
* Verschaffen Sie sich einen Überblick über die [verteilte Ablaufverfolgung](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing).
* Informieren Sie sich über die Möglichkeiten der [Anwendungsübersicht](https://docs.microsoft.com/azure/azure-monitor/app/app-map?tabs=net) für Ihr Unternehmen.
* Erfahren Sie mehr über [Anforderungen und Abhängigkeiten für Java-Apps](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent).
* Informieren Sie sich ausführlicher über [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) und [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview).