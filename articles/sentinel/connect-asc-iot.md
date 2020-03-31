---
title: Verbinden von Azure Security Center für IoT mit Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Daten aus Azure Security Center für IoT mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: yelevin
ms.openlocfilehash: 3af51110a4c4604444573f62be65077c786db606
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588636"
---
# <a name="connect-your-data-from-azure-security-center-for-iot-to-azure-sentinel"></a>Verknüpfen Ihrer Daten aus Azure Security Center für IoT mit Azure Sentinel 


> [!IMPORTANT]
> Der Azure Security Center für IoT-Datenconnector befindet sich derzeit in der Public Preview-Phase. Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit dem Azure Security Center für IoT-Datenconnector können Sie alle Ihre Ereignisse aus Azure Security Center für IoT an Azure Sentinel streamen. 

## <a name="prerequisites"></a>Voraussetzungen

- **Lese-** und **Schreibberechtigungen** für den Arbeitsbereich, in dem Azure Sentinel bereitgestellt wird.
- **Azure Security Center für IoT** muss für Ihre relevanten IoT Hub-Instanzen **aktiviert** sein.
- **Lese-** und **Schreibberechtigungen** für die Instanz von **Azure IoT Hub**, für die Sie eine Verbindung herstellen möchten.
- **Lese-** und **Schreibberechtigungen** für **Azure IoT Hub-Ressourcengruppe**.

> [!NOTE]
> Zum Streamen von IoT-Ressourcenwarnungen an Azure Sentinel muss für Ihr Abonnement die Azure Security Center-Lizenz für den Tarif **Standard** aktiviert werden. Um Warnungen für Azure Security Center für IoT in Azure Sentinel anzuzeigen, muss für Ihr Abonnement dagegen nur die Azure Security Center-Lizenz für den Tarif **Free** aktiviert werden. 

## <a name="connect-to-azure-security-center-for-iot"></a>Herstellen einer Verbindung mit Azure Security Center für IoT

1. Wählen Sie in Azure Sentinel die Option **Data connectors** (Datenconnectors) aus, und klicken Sie dann auf die Kachel **Azure Security Center für IoT**.
1. Klicken Sie im Bereich unten rechts auf **Connectorseite öffnen**. 
1. Klicken Sie neben jedem IoT Hub-Abonnement, dessen Warnungen und Gerätewarnungen Sie in Azure Sentinel streamen möchten, auf **Verbinden**. 
    - Wenn Azure Security Center für IoT auf diesem Hub nicht aktiviert wurde, wird eine Warnung vom Typ **Aktivieren** angezeigt. Klicken Sie auf den Link **Aktivieren**, um den Dienst zu starten. 
1. Sie können entscheiden, ob die Warnungen von Azure Security Center für IoT automatisch Incidents in Azure Sentinel generieren sollen. Wählen Sie unter **Create incidents** (Incidents erstellen) die Option **Aktivieren** aus, um die Standardanalyseregel zu aktivieren, dass aus den im verbundenen Sicherheitsdienst generierten Warnungen automatisch Incidents erstellt werden sollen. Diese Regel kann unter **Analytics** > **Aktive Regeln** geändert oder bearbeitet werden.

> [!NOTE]
> Es kann etwas dauern, bis die Hubliste aktualisiert wird, nachdem Verbindungsänderungen vorgenommen wurden. 

## <a name="log-analytics-alert-display"></a>Log Analytics-Warnungsanzeige

So verwenden Sie das relevante Schema in Log Analytics, um die Azure Security Center für IoT-Warnungen anzuzeigen:

1. Öffnen Sie **Logs** > **SecurityInsights** > **SecurityAlert**, oder suchen Sie nach **SecurityAlert**. 
2. Filtern Sie mit dem folgenden KQL-Filter, damit nur die von Azure Security Center für IoT generierten Warnungen angezeigt werden:

```kusto
SecurityAlert | where ProductName == "Azure Security Center for IoT"
``` 

### <a name="service-notes"></a>Diensthinweise

Nachdem Sie eine Verbindung mit einem IoT Hub hergestellt haben, stehen die Hubdaten in Azure Sentinel ungefähr 15 Minuten später zur Verfügung.


## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie Daten aus Azure Security Center für IoT mit Azure Sentinel verknüpfen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
