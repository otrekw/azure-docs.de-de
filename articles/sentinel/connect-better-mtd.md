---
title: Verbinden von BETTER Mobile Threat Defense (MTD) mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem BETTER Mobile Threat Defense (MTD)-Datenconnector MTD-Protokolle in Azure Sentinel pullen. Zeigen Sie MTD-Daten in Arbeitsmappen an, Erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2021
ms.author: yelevin
ms.openlocfilehash: 4828e31b9d15f101740c158ee62c90c95673c9a7
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98541511"
---
# <a name="connect-your-better-mobile-threat-defense-mtd-to-azure-sentinel"></a>Verbinden Ihrer BETTER Mobile Threat Defense (MTD) mit Azure Sentinel

> [!IMPORTANT]
> Der BETTER Mobile Threat Defense (MTD)-Connector befindet sich derzeit in der **VORSCHAUVERSION**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

Mit dem BETTER Mobile Threat Defense (MTD)-Connector können Sie alle Protokolle Ihrer BETTER MTD-Sicherheitslösung einfach mit Azure Sentinel verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Für die Integration von BETTER Mobile Threat Defense und Azure Sentinel wird die REST-API genutzt.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-better-mobile-threat-defense"></a>Konfigurieren und Verbinden von BETTER Mobile Threat Defense

BETTER MTD kann in Azure Sentinel integriert werden und Protokolle dann direkt in Azure Sentinel exportieren.

1. Wählen Sie im Azure Sentinel-Navigationsmenü **Datenconnectors** aus.

1. Wählen Sie im **Datenconnectors**-Katalog den Eintrag **BETTER Mobile Threat Defense (MTD) (Vorschau)** und dann **Connectorseite öffnen** aus.

1. Führen Sie die Schritte auf der Connectorseite und auf [dieser Seite der BETTER MTD-Dokumentation](https://mtd-docs.bmobi.net/integrations/azure-sentinel/setup-integration#mtd-integration-configuration) aus, um die Integration in die BETTER MTD-Konsole abzuschließen.

    Wenn Sie zur Eingabe der Werte für **Arbeitsbereichs-ID** und **Primärschlüssel** aufgefordert werden, kopieren Sie diese von der Connectorseite in Azure Sentinel, und fügen Sie sie in die Konfiguration von BETTER MTD ein.

    :::image type="content" source="media/connectors/workspace-id-primary-key.png" alt-text="{Arbeitsbereichs-ID und Primärschlüssel}":::

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten im Abschnitt **CustomLogs** in **Logs** in einer der folgenden Tabellen angezeigt:
- `BetterMTDDeviceLog_CL`
- `BetterMTDIncidentLog_CL`
- `BetterMTDAppLog_CL`
- `BetterMTDNetflowLog_CL`

Wenn Sie die BETTER MTD-Protokolle in Analyseregeln, Hunting-Abfragen oder an anderer Stelle in Azure Sentinel abfragen möchten, geben Sie am oberen Rand des Abfragefensters einen der obigen Tabellennamen ein.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie BETTER Mobile Threat Defense (MTD) mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
