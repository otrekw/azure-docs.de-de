---
title: Überwachen von lokal ausgeführten Java-Anwendungen – Azure Monitor Application Insights
description: Überwachen der Anwendungsleistung für lokal ausgeführte Java-Anwendungen, ohne die App zu instrumentieren. Verteilte Ablaufverfolgung und Anwendungszuordnung.
ms.topic: conceptual
author: MS-jgol
ms.author: jgol
ms.date: 04/16/2020
ms.openlocfilehash: ea604a4e0f11db7146e6e3c11b47d5687abf571e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81733627"
---
# <a name="java-codeless-application-monitoring-on-premises---azure-monitor-application-insights---public-preview"></a>Lokale Java-Anwendungsüberwachung ohne Code mit Azure Monitor Application Insights – öffentliche Vorschau

Bei der Java-Anwendungsüberwachung ohne Code geht es um Einfachheit. Es gibt keine Codeänderungen, und der Java-Agent kann durch nur wenige Konfigurationsänderungen aktiviert werden.

## <a name="overview"></a>Übersicht

Nach der Aktivierung sammelt der Java-Agent automatisch eine Vielzahl von Anforderungen, Abhängigkeiten, Protokollen und Metriken aus den am häufigsten genutzten Bibliotheken und Frameworks.

Sehen Sie sich die [detaillierten Anweisungen](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent) für die verschiedenen Umgebungen an, darunter auch lokale.

 ## <a name="next-steps"></a>Nächste Schritte

* [Anweisungen zum Herunterladen des Java-Agents](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent)
* [Konfigurieren der JVM-Argumente](https://github.com/microsoft/ApplicationInsights-Java/wiki/3.0-Preview:-Tips-for-updating-your-JVM-args)
* [Anpassen der Konfiguration](https://github.com/microsoft/ApplicationInsights-Java/wiki/3.0-Preview:-Configuration-Options)