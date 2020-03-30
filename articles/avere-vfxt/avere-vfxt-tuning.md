---
title: Avere vFXT-Clusteroptimierung – Azure
description: Übersicht über die benutzerdefinierten Einstellungen zur Leistungsoptimierung in Avere vFXT für Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: rohogue
ms.openlocfilehash: df20f050ff87fdb59a3e5cca373098240f8bfbb9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76152934"
---
# <a name="cluster-tuning"></a>Clusteroptimierung

Die meisten vFXT-Cluster können von benutzerdefinierten Leistungseinstellungen profitieren. Diese Einstellungen helfen dem Cluster, optimal mit Ihrem speziellen Workflow, Dataset und Ihren Tools zu arbeiten.

Diese Anpassung sollte mit Unterstützung durch einem Supportmitarbeiter ausgeführt werden, da sie die Konfiguration von Features umfassen kann, die nicht über die Avere-Systemsteuerung verfügbar sind.

In diesem Abschnitt werden einige der benutzerdefinierten Einstellungen beschrieben, die vorgenommen werden können.

## <a name="general-optimizations"></a>Allgemeine Optimierungen

Diese Änderungen können aufgrund von Datasetqualitäten oder auf Basis des Workflowstils empfohlen werden.

* Wenn der Workload sehr viele Schreibvorgänge umfasst, erhöhen Sie die standardmäßige Größe des Schreibcache von 20 %.
* Wenn das Dataset viele kleine Dateien umfasst, erhöhen Sie den Grenzwert für die Dateianzahl des Clustercache.
* Wenn die Arbeit das Kopieren oder Verschieben von Daten zwischen zwei Repositorys umfasst, passen Sie die Anzahl der Threads an, die zum Verschieben von Daten verwendet werden:
  * Um die Geschwindigkeit zu erhöhen, können Sie die Anzahl der verwendeten parallelen Threads erhöhen.
  * Wenn das Back-End-Speichervolume überlastet wird, müssen Sie die Anzahl der parallel verwendeten Threads möglicherweise verringern.
* Wenn der Cluster Daten für eine Kernspeichereinheit zwischenspeichert, die NFSv4 ACLs verwendet, aktivieren Sie das Zugriffsmoduscaching, um die Dateiautorisierung für bestimmte Clients zu optimieren.

## <a name="cloud-nas-or-cloud-gateway-optimizations"></a>Optimierungen für Cloud-NAS oder Cloudgateway

In einem Cloud-NAS- oder Gatewayszenario bietet der vFXT-Cluster Zugriff im NAS-Stil auf einen Cloudcontainer. Um die Vorteile höherer Datengeschwindigkeiten zwischen dem vFXT-Cluster und dem Cloudspeicher zu nutzen empfiehlt Ihr Berater möglicherweise, Einstellungen zu ändern, um Daten aggressiver aus dem Cache auf das Speichervolumen zu pushen. Beispiel:

* Erhöhen Sie die Anzahl der TCP-Verbindungen zwischen dem Cluster und dem Speichercontainer.

## <a name="cloud-bursting-or-hybrid-wan-optimizations"></a>Cloudbursting oder Hybrid-WAN-Optimierungen

In einem Cloudburstingszenario oder einem Szenario mit Hybrid-Speicher-WAN-Optimierung ermöglicht der vFXT-Cluster die Integration zwischen der Cloud und lokalem Hardwarespeicher. Die folgenden Änderungen können hilfreich sein:

* Erhöhen Sie die Anzahl der TCP-Verbindungen, die zwischen Cluster und Kernspeichereinheit erlaubt sind.
* Aktivieren Sie die Einstellung zur WAN-Optimierung für die Remotekernspeichereinheit (Diese Einstellung kann für eine On-Premises-Remotespeichereinheit oder eine Cloudkernspeichereinheit in einer anderen Azure-Region verwendet werden).
* Vergrößern Sie die TCP-Socketpuffergröße<sup>*</sup>
* Aktivieren Sie die Einstellung „immer weiterleiten“, um redundante Cachedateien zu verringern <sup>*</sup>

<sup>*</sup>Diese Anpassungen gelten möglicherweise nicht für alle Systeme, abhängig von der Workload und den Leistungsanforderungen.

## <a name="help-optimizing-your-avere-vfxt-for-azure"></a>Hilfe bei der Optimierung von Avere vFXT für Azure

Um sich mit Supportmitarbeitern bezüglich dieser Optimierungen in Verbindung zu setzen, verwenden Sie das unter [Abrufen von Hilfe zu Ihrem System](avere-vfxt-open-ticket.md) beschriebene Verfahren.
