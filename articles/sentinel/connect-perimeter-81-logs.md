---
title: Herstellen einer Verbindung von Perimeter 81-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Verbindung von Perimeter 81-Daten mit Azure Sentinel herstellen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/21/2020
ms.author: yelevin
ms.openlocfilehash: 8c9a7a09d3085b1ec67bf29f142c6e7b205561ef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87021748"
---
# <a name="connect-your-perimeter-81-activity-logs-to-azure-sentinel"></a>Verbinden von Perimeter 81-Aktivitätsprotokollen mit Azure Sentinel

> [!IMPORTANT]
> Der Perimeter 81-Datenconnector in Azure Sentinel ist derzeit als öffentliche Vorschau verfügbar.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Artikel wird erläutert, wie Sie Ihre Appliance mit [Perimeter 81-Aktivitätsprotokollen](https://www.perimeter81.com/) mit Azure Sentinel verbinden. Mit dem Connector für Perimeter 81-Aktivitätsprotokolle lassen sich Perimeter 81-Daten problemlos in Azure Sentinel einbinden. Diese Daten können Sie dann in Arbeitsmappen anzeigen, als Vorlage für benutzerdefinierte Warnungen und zur Untersuchung von Problemen verwenden.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Sie müssen über Leseberechtigungen für freigegebene Schlüssel für den Arbeitsbereich verfügen.

## <a name="configure-and-connect-perimeter-81-activity-logs"></a>Konfigurieren und Verbinden von Perimeter 81-Aktivitätsprotokollen

Perimeter 81-Aktivitätsprotokolle können integriert werden und Protokolle dann direkt in Azure Sentinel exportieren.

1. Klicken Sie im Azure Sentinel-Portal im Navigationsmenü auf **Datenconnectors**.

1. Wählen Sie aus dem Katalog **Perimeter 81-Aktivitätsprotokolle** aus, und klicken Sie dann auf die Schaltfläche **Seite „Connector öffnen“** .

1. Kopieren Sie auf der Seite „Connector für Perimeter 81-Aktivitätsprotokolle“ die **Arbeitsbereichs-ID** und den **Primärschlüssel**, und fügen Sie diese Angaben [wie hier beschrieben](https://support.perimeter81.com/hc/en-us/articles/360012680780) in Perimeter 81 ein.

1. Nachdem Sie die Anweisungen ausgeführt haben, werden die verbundenen Datentypen auf der Azure Sentinel-Connectorseite angezeigt.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in **Logs** (Protokolle) unter **CustomLogs** - **Perimeter81_CL** angezeigt.

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie das Perimeter 81-Aktivitätsprotokoll mit Azure Sentinel verbinden. Um die in diesem Datenconnector integrierten Funktionen in vollem Umfang nutzen zu können, klicken Sie auf der Datenconnectorseite auf die Registerkarte **Nächste Schritte**. Dort finden Sie eine vorgefertigte Arbeitsmappe, damit Sie mit der Suche nach nützlichen Informationen beginnen können.

Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
