---
title: Herstellen einer Verbindung von Azure Kubernetes Service-Diagnoseprotokollen (AKS) mit Azure Sentinel
description: Erfahren Sie, wie Sie mit Azure Policy eine Verbindung von Azure Kubernetes Service-Diagnoseprotokollen mit Azure Sentinel herstellen.
author: yelevin
manager: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: how-to
ms.date: 04/22/2021
ms.author: yelevin
ms.openlocfilehash: f1ef860f1b84de84c42996a7523af8ce174d5981
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107890796"
---
# <a name="connect-azure-kubernetes-service-diagnostics-logs"></a>Herstellen einer Verbindung von Azure Kubernetes Service-Diagnoseprotokollen

Azure Kubernetes Service (AKS) ist ein vollständig verwalteter Open-Source-Containerorchestrierungsdienst, mit dem Sie Docker-Container und containerbasierte Anwendungen in einer Clusterumgebung bereitstellen, skalieren und verwalten können.

Mit diesem Connector können Sie Azure Kubernetes Service-Diagnoseprotokolle (AKS) in Azure Sentinel streamen, sodass Sie die Aktivitäten in allen Instanzen kontinuierlich überwachen können. 

Erfahren Sie mehr über die [Überwachung von Azure Kubernetes Service](../azure-monitor/containers/container-insights-overview.md) und über [AKS-Diagnosetelemetrie](../aks/view-control-plane-logs.md).

## <a name="prerequisites"></a>Voraussetzungen

So erfassen Sie AKS-Protokolle in Azure Sentinel:

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Wenn Sie Azure Policy verwenden möchten, um eine Richtlinie zum Protokollstreaming in AKS-Ressourcen anzuwenden, müssen Sie die Rolle „Besitzer“ für den Zuweisungsbereich der Richtlinie besitzen.

## <a name="connect-to-azure-kubernetes-service"></a>Herstellen einer Verbindung mit Azure Kubernetes Service

Dieser Connector verwendet Azure Policy, um eine einzelne Konfiguration zum Azure Kubernetes Service-Protokollstreaming auf eine Sammlung von Ressourcen anzuwenden, die als Bereich definiert sind. Die in Azure Kubernetes Service erfassten Protokolltypen werden links auf der Connectorseite unter **Datentypen** angezeigt.

1. Klicken Sie im Azure Sentinel-Navigationsmenü auf **Data connectors** (Datenconnectors).

1. Wählen Sie im Katalog für Datenconnectors **Azure Kubernetes Service (AKS)** und dann im Vorschaubereich **Connectorseite öffnen** aus.

1. Erweitern Sie im Abschnitt **Konfiguration** der Connectorseite die Option **Streamdiagnoseprotokolle von Ihrem Azure Kubernetes Service (AKS) im großen Stil**.

1. Wählen Sie die Schaltfläche **Azure Policy-Zuweisungs-Assistent starten** aus.

    Der Richtlinienzuweisungs-Assistent wird geöffnet, und Sie können eine neue Richtlinie mit dem Namen **Bereitstellen: Diagnoseeinstellungen für Azure Kubernetes Service in Log Analytics-Arbeitsbereich konfigurieren** erstellen.

    1. Klicken Sie auf der Registerkarte **Grundlagen** unter **Bereich** auf die Schaltfläche mit den drei Punkten, um Ihr Abonnement (und optional eine Ressourcengruppe) auszuwählen. Sie können auch eine Beschreibung hinzufügen.

    1. Lassen Sie auf der Registerkarte **Parameter** die Felder **Auswirkung** und **Einstellungsname** unverändert. Wählen Sie in der Dropdownliste **Log Analytics-Arbeitsbereich** Ihren Azure Sentinel-Arbeitsbereich aus. Die übrigen Dropdownfelder stellen die verfügbaren Diagnoseprotokolltypen dar. Behalten Sie für alle Protokolltypen, die erfasst werden sollen, die Markierung „True“ bei.

    1. Die Richtlinie wird auf in Zukunft hinzugefügte Ressourcen angewendet. Wählen Sie zum Anwenden der Richtlinie auf Ihre vorhandenen Ressourcen die Registerkarte **Korrektur** aus, und aktivieren Sie das Kontrollkästchen **Korrekturtask erstellen**.

    1. Klicken Sie auf der Registerkarte **Überprüfen + erstellen** auf **Erstellen**. Damit ist die Richtlinie dem ausgewählten Bereich zugewiesen.

> [!NOTE]
>
> Mit diesem speziellen Datenconnector werden die Konnektivitätsstatusindikatoren (ein Farbstreifen im Datenconnectors-Katalog sowie Verbindungssymbole neben den Datentypnamen) nur dann als *verbunden* (grün) angezeigt, wenn Daten irgendwann innerhalb der letzten 14 Tage erfasst wurden. Wenn 14 Tage ohne Datenerfassung vergangen sind, wird der Connector als „getrennt“ angezeigt. In dem Moment, in dem weitere Daten den Connector passieren, wird der Status wieder als *verbunden* angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie mithilfe von Azure Policy eine Verbindung von Azure Kubernetes Service mit Azure Sentinel herstellen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
