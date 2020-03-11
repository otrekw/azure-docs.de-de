---
title: Azure Security Center für IoT-Anleitung für die Konfiguration mit Azure Sentinel (Vorschau) | Microsoft-Dokumentation
description: Erläutert die Konfiguration von Azure Sentinel zum Empfangen von Daten aus Ihrer Azure Security Center für IoT-Lösung.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: mlottner
ms.openlocfilehash: 082b33332051fee9da2aebe63b0c41edb300afaf
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303544"
---
> [!IMPORTANT]
> Der Azure Security Center für IoT-Datenconnector in Azure Sentinel ist derzeit als Public Preview verfügbar.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

# <a name="connect-your-data-from-azure-security-center-for-iot-to-azure-sentinel-preview"></a>Verbinden Ihrer Daten aus Azure Security Center für IoT mit Azure Sentinel (Vorschau) 

In dieser Anleitung erfahren Sie, wie Sie Ihre Azure Security Center für IoT-Daten mit Azure Sentinel verbinden.  

> [!div class="checklist"]
> * Voraussetzungen 
> * Verbindungseinstellungen
> * Log Analytics-Warnungsansicht 

Verbinden Sie Warnungen aus Azure Security Center für IoT, und streamen Sie sie direkt in Azure Sentinel.

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen über die Arbeitsbereichsberechtigungen **Lesen** und **Schreiben** verfügen.
- **Azure Security Center für IoT** muss auf Ihrem/Ihren relevanten IoT Hub(s) **aktiviert** sein.
- Sie müssen auf dem **Azure IoT Hub**, mit dem Sie eine Verbindung herstellen möchten, über die Berechtigungen **Lesen** und **Schreiben** verfügen.
- Außerdem müssen Sie über die Berechtigungen **Lesen** und **Schreiben** für die **Azure IoT Hub-Ressourcengruppe** verfügen.

> [!NOTE]
> In Ihrem Abonnement muss die Standard-Tarif-Lizenzierung von Azure Security Center ausgeführt werden, damit allgemeine Azure-Ressourcenwarnungen gesendet werden. Bei der für Azure Security Center für IoT erforderlichen Free-Tarif-Lizenzierung werden nur Warnungen im Zusammenhang mit Azure Security Center für IoT an Azure Sentinel weitergeleitet. 

## <a name="connect-to-azure-security-center-for-iot"></a>Herstellen einer Verbindung mit Azure Security Center für IoT

1. Wählen Sie in Azure Sentinel die Option **Data connectors** (Datenconnectors) aus, und klicken Sie dann auf die Kachel **Azure Security Center für IoT**.
1. Klicken Sie unten im rechten Bereich auf **Connectorseite öffnen**. 
1. Klicken Sie neben jedem IoT Hub-Abonnement, dessen Warnungen und Gerätewarnungen Sie in Azure Sentinel streamen möchten, auf **Verbinden**. 
    - Wenn Azure Security Center für IoT auf diesem Hub nicht aktiviert wurde, wird eine Aktivierungswarnung angezeigt. Klicken Sie auf den Link **Aktivieren**, um den Dienst zu starten und zu aktivieren. 
1. Sie können entscheiden, ob die Warnungen von Azure Security Center für IoT automatisch Incidents in Azure Sentinel generieren sollen. Wählen Sie unter **Create incidents** (Incidents erstellen) die Option **Aktivieren** aus, um die Regel zur automatischen Erstellung von Incidents aus den generierten Warnungen zu aktivieren.  Diese Regel kann unter **Analyse** > **Aktive Regeln** geändert oder bearbeitet werden.

> [!NOTE]
>Es kann 10 Sekunden oder länger dauern, bis die Hubliste aktualisiert wird, nachdem Verbindungsänderungen vorgenommen wurden. 

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

In diesem Dokument haben Sie erfahren, wie Sie Azure Security Center für IoT mit Azure Sentinel verbinden. Weitere Informationen zu Bedrohungserkennung und Zugriff auf Sicherheitsdaten finden Sie in folgenden Artikeln:

- Erfahren Sie, wie Sie mithilfe von Azure Sentinel [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](https://docs.microsoft.com/azure/sentinel/quickstart-get-visibility).

- Erfahren Sie, wie Sie auf Ihre [IoT-Sicherheitsdaten zugreifen](how-to-security-data-access.md).