---
title: Verbinden von Azure Defender für IoT mit Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Daten aus Azure Defender (vormals Azure Security Center) für IoT mit Azure Sentinel verknüpfen.
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
ms.date: 09/07/2020
ms.author: yelevin
ms.openlocfilehash: 822d0c742bbd54b5bab0c69e82652743584a0696
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89659608"
---
# <a name="connect-your-data-from-azure-defender-formerly-azure-security-center-for-iot-to-azure-sentinel"></a>Verknüpfen Ihrer Daten aus Azure Defender (vormals Azure Security Center) für IoT mit Azure Sentinel 


> [!IMPORTANT]
> Der Azure Defender für IoT-Datenconnector befindet sich derzeit in der Public Preview-Phase. Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit dem Azure Defender für IoT-Datenconnector können Sie alle Ihre Ereignisse aus Azure Defender für IoT an Azure Sentinel streamen. 

## <a name="prerequisites"></a>Voraussetzungen

- **Lese-** und **Schreibberechtigungen** für den Arbeitsbereich, in dem Azure Sentinel bereitgestellt wird.
- **Azure Defender für IoT** muss für Ihre relevanten IoT Hub-Instanzen **aktiviert** sein.
- **Lese-** und **Schreibberechtigungen** für die Instanz von **Azure IoT Hub**, für die Sie eine Verbindung herstellen möchten.
- **Lese-** und **Schreibberechtigungen** für **Azure IoT Hub-Ressourcengruppe**.

## <a name="connect-to-azure-defender-for-iot"></a>Verbinden mit Azure Defender für IoT

1. Wählen Sie in Azure Sentinel die Option **Datenconnectors** und dann **Azure Defender für IoT** (möglicherweise heißt es noch „Azure Security Center für IoT“) aus dem Katalog aus.
1. Klicken Sie im Bereich unten rechts auf **Connectorseite öffnen**. 
1. Klicken Sie neben jedem IoT Hub-Abonnement, dessen Warnungen und Gerätewarnungen Sie in Azure Sentinel streamen möchten, auf **Verbinden**. 
    - Wenn Azure Defender für IoT auf diesem Hub nicht aktiviert wurde, wird eine Warnung vom Typ **Aktivieren** angezeigt. Klicken Sie auf den Link **Aktivieren**, um den Dienst zu starten. 
1. Sie können entscheiden, ob die Warnungen von Azure Defender für IoT automatisch Incidents in Azure Sentinel generieren sollen. Wählen Sie unter **Create incidents** (Incidents erstellen) die Option **Aktivieren** aus, um die Standardanalyseregel zu aktivieren, dass aus den im verbundenen Sicherheitsdienst generierten Warnungen automatisch Incidents erstellt werden sollen. Diese Regel kann unter **Analytics** > **Aktive Regeln** geändert oder bearbeitet werden.

> [!NOTE]
> Es kann etwas dauern, bis die Hubliste aktualisiert wird, nachdem Verbindungsänderungen vorgenommen wurden. 

## <a name="log-analytics-alert-display"></a>Log Analytics-Warnungsanzeige

So verwenden Sie das relevante Schema in Log Analytics, um die Azure Defender für IoT-Benachrichtigungen anzuzeigen:

1. Öffnen Sie **Logs** > **SecurityInsights** > **SecurityAlert**, oder suchen Sie nach **SecurityAlert**. 
2. Filtern Sie mithilfe des folgenden KQL-Filters, um nur die von Azure Defender für IoT generierten Benachrichtigungen anzuzeigen:

```kusto
SecurityAlert | where ProductName == "Azure Defender for IoT"
``` 

### <a name="service-notes"></a>Diensthinweise

Nachdem Sie eine Verbindung mit einem IoT Hub hergestellt haben, stehen die Hubdaten in Azure Sentinel ungefähr 15 Minuten später zur Verfügung.


## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie Daten aus Azure Defender für IoT mit Azure Sentinel verknüpfen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
