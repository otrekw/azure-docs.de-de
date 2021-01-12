---
title: Übersicht über den ITSM-Connector
description: In diesem Artikel finden Sie eine Übersicht über den ITSM-Connector (IT Service Management).
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/16/2020
ms.custom: references_regions
ms.openlocfilehash: 93b6160b495b609e23e10b3f709d130792067423
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/29/2020
ms.locfileid: "97803781"
---
# <a name="it-service-management-connector-overview"></a>Übersicht über den ITSM-Connector

:::image type="icon" source="media/itsmc-overview/itsmc-symbol.png":::

Der ITSM-Connector (IT-Service-Management) ermöglicht Ihnen, eine Verbindung zwischen Azure und einem unterstützten ITSM-Produkt oder -Dienst herzustellen.

Azure-Dienste wie Azure Log Analytics und Azure Monitor stellen Tools zur Verfügung, mit denen Sie Probleme mit Ihren Azure- und Nicht-Azure-Ressourcen erkennen, analysieren und beheben können. Die Arbeitselemente, die sich auf ein Problem beziehen, befinden sich jedoch typischerweise in einem ITSM-Produkt oder -Dienst. Der ITSM-Connector bietet eine bidirektionale Verbindung zwischen Azure und ITSM-Tools, sodass Sie Probleme schneller lösen können.

## <a name="configuration-steps"></a>Konfigurationsschritte

Der ITSM-Connector unterstützt Verbindungen mit den folgenden ITSM-Tools:

-   ServiceNow
-   System Center Service Manager
-   Provance
-   Cherwell

   >[!NOTE]
> Unseren Cherwell- und Provance-Kunden schlagen wir die Verwendung der [Webhookaktion](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#webhook) für Cherwell- und Provance-Endpunkte als weitere Lösung für die Integration vor.

Mit dem ITSMC können Sie folgende Aktionen ausführen:

-  Sie können Arbeitselemente im ITSM-Tool basierend auf Ihren Azure-Warnungen (Metrik-, Aktivitätsprotokoll- und Log Analytics-Warnungen) erstellen.
-  Optional können Sie Incident- und Änderungsanforderungsdaten aus Ihrem ITSM-Tool mit einem Azure Log Analytics-Arbeitsbereich synchronisieren.

Informationen zu rechtlichen und Datenschutzbestimmungen finden Sie in der [Datenschutzerklärung von Microsoft](https://go.microsoft.com/fwLink/?LinkID=522330&clcid=0x9).

Damit Sie den ITSM-Connector verwenden können, führen Sie vorab die folgenden Schritte aus:

1. [Richten Sie Ihre ITSM-Umgebung ein, um Warnungen von Azure zu akzeptieren.](./itsmc-connections.md)
1. [Konfigurieren Sie die ITSM-Lösung von Azure.](./itsmc-definition.md#add-it-service-management-connector)
1. [Konfigurieren Sie den ITSM-Connector von Azure für Ihre ITSM-Umgebung.](./itsmc-definition.md#create-an-itsm-connection)
1. [Konfigurieren Sie die Aktionsgruppe für die Verwendung des ITSM-Connectors.](./itsmc-definition.md#use-itsmc)

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung in ITSM-Connector](./itsmc-resync-servicenow.md)