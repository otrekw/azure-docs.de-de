---
title: Verknüpfen von Salesforce Service Cloud-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Datenconnector für Salesforce Service Cloud zum Pullen von Salesforce-Protokollen in Azure Sentinel verwenden. Zeigen Sie Salesforce-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
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
ms.date: 01/17/2021
ms.author: yelevin
ms.openlocfilehash: 1efd91d92bac1bc1f39d82aaa0cc71daa0275f8e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100570536"
---
# <a name="connect-your-salesforce-service-cloud-to-azure-sentinel"></a>Verbinden Ihrer Salesforce Service Cloud mit Azure Sentinel

> [!IMPORTANT]
> Der Salesforce Service Cloud-Connector befindet sich derzeit in der **Vorschauphase**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

In diesem Artikel wird erläutert, wie Sie Ihre Salesforce Service Cloud-Lösung mit Azure Sentinel verbinden. Mit dem Salesforce Service Cloud-Datenconnector lassen sich Ihre Salesforce-Daten problemlos mit Azure Sentinel verbinden, sodass Sie diese dann in Arbeitsmappen anzeigen und als Vorlage für benutzerdefinierte Warnungen sowie zur Verbesserung der Untersuchung von Problemen verwenden können. Die Integration von Salesforce Service Cloud und Azure Sentinel stützt sich auf die REST-API.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Sie müssen über Leseberechtigungen für freigegebene Schlüssel für den Arbeitsbereich verfügen. [Weitere Informationen zu Arbeitsbereichsschlüsseln](../azure-monitor/agents/log-analytics-agent.md#workspace-id-and-key)

- Sie müssen über Lese- und Schreibberechtigungen für Azure Functions verfügen, um eine Funktions-App erstellen zu können. [Informieren Sie sich ausführlicher über Azure Functions](../azure-functions/index.yml).

- Sie benötigen die folgenden Anmeldeinformationen für die Salesforce-REST-API: **Salesforce-API-Benutzername**, **Salesforce-API-Kennwort**, **Salesforce-Sicherheitstoken**, **Salesforce-Consumerschlüssel**, **Salesforce-Consumergeheimnis**. [Weitere Informationen zur Salesforce-REST-API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/quickstart.htm)

## <a name="configure-and-connect-salesforce-service-cloud"></a>Konfigurieren und Verbinden der Salesforce Service Cloud

Salesforce Service Cloud kann in Azure Sentinel integriert werden, und Protokolle können dann direkt in Azure Sentinel exportiert werden.

1. Wählen Sie im Azure Sentinel-Navigationsmenü **Datenconnectors** aus.

1. Wählen Sie im Katalog **Datenconnectors** den Eintrag **Salesforce Service Cloud (Vorschau)** und dann **Connectorseite öffnen** aus.

1. Führen Sie die im Abschnitt **Konfiguration** auf der Connectorseite beschriebenen Schritte aus.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in der Tabelle `SalesforceServiceCloud_CL` im Abschnitt **CustomLogs** unter **Protokolle** angezeigt.

Einige hilfreiche Beispielabfragen finden Sie auf der Registerkarte **Nächste Schritte** auf der Connectorseite.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie Salesforce Service Cloud mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.