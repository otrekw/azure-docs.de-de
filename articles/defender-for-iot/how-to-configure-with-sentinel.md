---
title: Konfigurieren von Azure Sentinel für Defender für IoT (Vorschau)
description: Erläutert die Konfiguration von Azure Sentinel zum Empfangen von Daten aus Ihrer Defender für IoT-Lösung.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2020
ms.author: mlottner
ms.openlocfilehash: 71147352c5b75195ed0dff2b05acc5315f3183cc
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931640"
---
# <a name="connect-your-data-from-defender-for-iot-to-azure-sentinel-preview"></a>Verbinden Ihrer Daten aus Defender für IoT mit Azure Sentinel (Vorschau)

Der Azure Security Center für IoT-Datenconnector in Azure Sentinel ist derzeit als Public Preview verfügbar. Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/).

In dieser Anleitung erfahren Sie, wie Sie Ihre Defender für IoT-Daten mit Azure Sentinel verbinden.

> [!div class="checklist"]
> * Voraussetzungen
> * Verbindungseinstellungen
> * Log Analytics-Warnungsansicht

Verbinden Sie Warnungen aus Defender für IoT, und streamen Sie sie direkt in Azure Sentinel.

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen über die Arbeitsbereichsberechtigungen **Lesen** und **Schreiben** verfügen.
- **Defender für IoT** muss für Ihre relevanten IoT Hub-Instanzen **aktiviert** sein.
- Sie müssen auf dem **Azure IoT Hub**, mit dem Sie eine Verbindung herstellen möchten, über die Berechtigungen **Lesen** und **Schreiben** verfügen.
- Außerdem müssen Sie über die Berechtigungen **Lesen** und **Schreiben** für die **Azure IoT Hub-Ressourcengruppe** verfügen.

> [!NOTE]
> In Ihrem Abonnement muss die Standard-Tarif-Lizenzierung von Azure Security Center ausgeführt werden, damit allgemeine Azure-Ressourcenwarnungen gesendet werden. Bei der für Defender für IoT erforderlichen Lizenzierung für den Free-Tarif werden nur Warnungen im Zusammenhang mit Defender für IoT an Azure Sentinel weitergeleitet.

## <a name="connect-to-defender-for-iot"></a>Verbinden mit Defender für IoT

1. Wählen Sie in Azure Sentinel **Datenconnectors** aus, und klicken Sie dann auf die Kachel **Defender für IoT**.
1. Klicken Sie unten im rechten Bereich auf **Connectorseite öffnen**.
1. Klicken Sie neben jedem IoT Hub-Abonnement, dessen Warnungen und Gerätewarnungen Sie in Azure Sentinel streamen möchten, auf **Verbinden**.
    - Wenn Defender für IoT auf diesem Hub nicht aktiviert wurde, wird eine Warnung vom Typ „Aktivieren“ angezeigt. Klicken Sie auf den Link **Aktivieren**, um den Dienst zu starten und zu aktivieren.
1. Sie können entscheiden, ob die Warnungen von Defender für IoT automatisch Incidents in Azure Sentinel generieren sollen. Wählen Sie unter **Create incidents** (Incidents erstellen) die Option **Aktivieren** aus, um die Regel zur automatischen Erstellung von Incidents aus den generierten Warnungen zu aktivieren.  Diese Regel kann unter **Analyse** > **Aktive Regeln** geändert oder bearbeitet werden.

> [!NOTE]
>Es kann 10 Sekunden oder länger dauern, bis die Hubliste aktualisiert wird, nachdem Verbindungsänderungen vorgenommen wurden.

## <a name="log-analytics-alert-display"></a>Log Analytics-Warnungsanzeige

So verwenden Sie das relevante Schema in Log Analytics, um die Defender für IoT-Benachrichtigungen anzuzeigen:

1. Öffnen Sie **Logs** > **SecurityInsights** > **SecurityAlert**, oder suchen Sie nach **SecurityAlert**.
1. Filtern Sie mithilfe des folgenden KQL-Filters, um nur die von Defender für IoT generierten Benachrichtigungen anzuzeigen:

```kusto
SecurityAlert | where ProductName == "Defender for IoT"
```

### <a name="service-notes"></a>Diensthinweise

Nachdem Sie eine Verbindung mit einem IoT Hub hergestellt haben, stehen die Hubdaten in Azure Sentinel ungefähr 15 Minuten später zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie Defender für IoT mit Azure Sentinel verbinden. Weitere Informationen zu Bedrohungserkennung und Zugriff auf Sicherheitsdaten finden Sie in folgenden Artikeln:

- Erfahren Sie, wie Sie mithilfe von Azure Sentinel [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](https://docs.microsoft.com/azure/sentinel/quickstart-get-visibility).

- Erfahren Sie, wie Sie auf Ihre [IoT-Sicherheitsdaten zugreifen](how-to-security-data-access.md).
