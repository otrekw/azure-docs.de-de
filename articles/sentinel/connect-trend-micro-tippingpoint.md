---
title: Verbinden von Trend Micro TippingPoint mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Trend Micro TippingPoint-Datenconnector die TippingPoint SMS-Protokolle in Azure Sentinel pullen. Zeigen Sie TippingPoint-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
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
ms.openlocfilehash: 5c7491a0e0ba2a3bf604988c613e1fd8937f277d
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98752172"
---
# <a name="connect-your-trend-micro-tippingpoint-solution-to-azure-sentinel"></a>Verbinden Ihrer Trend Micro TippingPoint-Lösung mit Azure Sentinel

> [!IMPORTANT]
> Der Trend Micro TippingPoint-Connector befindet sich derzeit in der **VORSCHAUVERSION**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

In diesem Artikel wird erläutert, wie Sie Ihre Trend Micro TippingPoint Threat Protection System-Lösung mit Azure Sentinel verbinden. Mit dem Trend Micro TippingPoint-Datenconnector lassen sich Ihre TippingPoint Security Management System (SMS)-Protokolle problemlos mit Azure Sentinel verbinden, sodass Sie die Daten dann in Arbeitsmappen anzeigen, als Vorlage für benutzerdefinierte Warnungen und zur Verbesserung der Untersuchung von Problemen verwenden können.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für Ihren Azure Sentinel-Arbeitsbereich.

- Sie müssen über Leseberechtigungen für freigegebene Schlüssel für den Arbeitsbereich verfügen.

## <a name="send-trend-micro-tippingpoint-logs-to-azure-sentinel"></a>Senden von Trend Micro TippingPoint-Protokollen an Azure Sentinel

Um die Protokolle Ihrer TippingPoint TPS-Lösung in Azure Sentinel zu erfassen, konfigurieren Sie die Lösung so, dass sie Syslog-Nachrichten im CEF-Format an einen Linux-basierten Protokollweiterleitungsserver (auf dem rsyslog oder syslog-ng ausgeführt wird) sendet. Auf diesem Server ist der Log Analytics-Agent installiert, der die Protokolle an Ihren Azure Sentinel-Arbeitsbereich weiterleitet. Der Connector verwendet eine Parserfunktion, um die empfangenen Daten in ein normalisiertes Schema zu konvertieren. 

1. Wählen Sie im Azure Sentinel-Navigationsmenü **Datenconnectors** aus.

1. Wählen Sie im **Datenconnectors**-Katalog den Eintrag **Trend Micro TippingPoint (Vorschau)** und dann **Connectorseite öffnen** aus.

1. Befolgen Sie die Anleitungen auf der Registerkarte **Anweisungen** unter **Konfiguration**:

    1. Klicken Sie unter **1. Linux-Syslog-Agent-Konfiguration**: Führen Sie diesen Schritt aus, wenn Sie noch keinen Protokollweiterleitungsserver ausführen, oder wenn Sie einen anderen benötigen. Detaillierte Anleitungen und Erläuterungen finden Sie unter [SCHRITT 1: Bereitstellen des Protokollweiterleitungsservers](connect-cef-agent.md) in der Azure Sentinel-Dokumentation.

    1. Wählen Sie unter **2. Weiterleiten von Trend Micro TippingPoint SMS-Protokollen an den Syslog-Agent**: Diese Konfiguration sollte die folgenden Elemente enthalten:
        - Protokollziel: Hostname und/oder IP-Adresse Ihres Protokollweiterleitungsservers.
        - Protokoll und Port: **TCP 514** (falls etwas anderes empfohlen wird, stellen Sie sicher, dass Sie die entsprechende Änderung parallel im syslog-Daemon und auf Ihrem Protokollweiterleitungsserver vornehmen).
        - Protokollformat: **ArcSight CEF-Format v4.2**
        - Protokolltypen: Alle verfügbaren.

    1. Klicken Sie unter **3. Überprüfen der Verbindung**: Überprüfen Sie die Datenerfassung, indem Sie den Befehl von der Connectorseite kopieren und auf Ihrem Protokollweiterleitungsserver ausführen. Detaillierte Anleitungen und Erläuterungen finden Sie unter [SCHRITT 3: Überprüfen der Verbindung](connect-cef-verify.md) in der Azure Sentinel-Dokumentation.

        Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine Verbindung hergestellt wurde, werden die Daten in der Tabelle *CommonSecurityLog* im Abschnitt **Azure Sentinel** in **Logs** angezeigt.

Um Trend Micro TippingPoint-Daten in Log Analytics zu erhalten, fragen Sie die Parserfunktion anstelle der Tabelle ab. Kopieren Sie Folgendes in das Abfragefenster, wobei Sie bei der Auswahl andere Filter anwenden:

```kusto
TrendMicroTippingPoint
| sort by TimeGenerated
```

Weitere hilfreiche Beispielabfragen finden Sie auf der Registerkarte **Nächste Schritte** auf der Connectorseite.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde erläutert, wie Sie Trend Micro TippingPoint mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
