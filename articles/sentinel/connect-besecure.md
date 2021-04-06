---
title: Verbinden von Beyond Security beSECURE-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Beyond Security beSECURE-Datenconnector beSECURE-Protokolle in Azure Sentinel pullen. Zeigen Sie beSECURE-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
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
ms.openlocfilehash: 313f201aeabd470850b27d979dc5253f80e82a55
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98541160"
---
# <a name="connect-your-beyond-security-besecure-to-azure-sentinel"></a>Verbinden Ihrer Beyond Security beSECURE-Instanz mit Azure Sentinel

> [!IMPORTANT]
> Der Beyond Security beSECURE-Connector befindet sich derzeit in der **Vorschau**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

Mit dem Connector für beSECURE von Beyond Security können Sie alle Protokolle Ihrer beSECURE-Sicherheitslösungen einfach mit Azure Sentinel verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Die Integration von beSECURE und Azure Sentinel basiert auf der REST-API.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-besecure"></a>Konfigurieren und Verbinden von beSECURE

beSECURE kann in Azure Sentinel integriert werden und Protokolle dann direkt in Azure Sentinel exportieren.

1. Wählen Sie im Azure Sentinel-Navigationsmenü **Datenconnectors** aus.

1. Wählen Sie im Katalog **Datenconnectors** den Eintrag **Beyond Security beSECURE (Vorschau)** und dann **Connectorseite öffnen** aus.

1. Führen Sie die folgenden Schritte aus, um Ihre beSECURE-Lösung zum Senden von Scanergebnissen, Scanstatus und Überwachungsprotokollen an Azure Sentinel zu konfigurieren.

    **Zugreifen auf das Menü „Integration“:**
    1. Klicken Sie auf die Menüoption „More“ (Weitere).

    1. Server auswählen

    1. Wählen Sie „Integration“ aus.

    1. Aktivieren von Azure Sentinel

    **Bereitstellen von Azure Sentinel-Einstellungen in beSECURE:**

      Kopieren Sie die Werte für *Arbeitsbereichs-ID* und *Primärschlüssel* von der Connectorseite in Azure Sentinel, fügen Sie sie in die Konfiguration von beSECURE ein, und klicken Sie auf **Modify** (Ändern).
      
      :::image type="content" source="media/connectors/workspace-id-primary-key.png" alt-text="{Arbeitsbereichs-ID und Primärschlüssel}":::

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten im Abschnitt **CustomLogs** in **Logs** in einer der folgenden Tabellen angezeigt:
  - `beSECURE_ScanResults_CL`
  - `beSECURE_ScanEvents_CL`
  - `beSECURE_Audit_CL`

Wenn Sie die beSECURE-Protokolle in Analyseregeln, Hunting-Abfragen, Untersuchungen oder an anderer Stelle in Azure Sentinel abfragen möchten, geben Sie am oberen Rand des Abfragefensters einen der obigen Tabellennamen ein.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität
Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde beschrieben, wie Sie beSECURE mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
