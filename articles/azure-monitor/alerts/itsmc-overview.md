---
title: Übersicht über den ITSM-Connector
description: In diesem Artikel finden Sie eine Übersicht über den ITSM-Connector (IT Service Management).
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/16/2020
ms.custom: references_regions
ms.openlocfilehash: d22bb05ad6db3630e9b0242e098fd81f65e34b05
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100601616"
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
> Ab dem 1. Oktober 2020 werden ITSM-Integrationen von Cherwell und Provance in Azure-Warnungen für neue Kunden nicht mehr aktiviert. Neue ITSM-Verbindungen werden nicht unterstützt.
> Bestehende ITSM-Verbindungen werden unterstützt.

Mit dem ITSMC können Sie folgende Aktionen ausführen:

-  Sie können Arbeitselemente im ITSM-Tool basierend auf Ihren Azure-Warnungen (Metrik-, Aktivitätsprotokoll- und Log Analytics-Warnungen) erstellen.
-  Optional können Sie Incident- und Änderungsanforderungsdaten aus Ihrem ITSM-Tool mit einem Azure Log Analytics-Arbeitsbereich synchronisieren.

Informationen zu rechtlichen und Datenschutzbestimmungen finden Sie in der [Datenschutzerklärung von Microsoft](https://go.microsoft.com/fwLink/?LinkID=522330&clcid=0x9).

Damit Sie den ITSM-Connector verwenden können, führen Sie vorab die folgenden Schritte aus:

1. [Richten Sie Ihre ITSM-Umgebung ein, um Warnungen von Azure zu akzeptieren.](./itsmc-connections.md)
1. [Konfigurieren Sie die ITSM-Lösung von Azure.](./itsmc-definition.md#add-it-service-management-connector)
1. [Konfigurieren Sie den ITSM-Connector von Azure für Ihre ITSM-Umgebung.](./itsmc-definition.md#create-an-itsm-connection)
1. [Konfigurieren Sie die Aktionsgruppe für die Verwendung des ITSM-Connectors.](./itsmc-definition.md#define-a-template)

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung in ITSM-Connector](./itsmc-resync-servicenow.md)
