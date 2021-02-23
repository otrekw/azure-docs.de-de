---
title: Verbinden von Juniper Networks SRX-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Juniper SRX-Datenconnector Juniper SRX-Protokolle in Azure Sentinel pullen. Zeigen Sie Juniper SRX-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
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
ms.openlocfilehash: b10c47a31bf1be10c278d4d9e0dce633bc7bff6c
ms.sourcegitcommit: 7ec45b7325e36debadb960bae4cf33164176bc24
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530636"
---
# <a name="connect-your-juniper-srx-firewall-to-azure-sentinel"></a>Verbinden Ihrer Juniper SRX-Firewall mit Azure Sentinel

> [!IMPORTANT]
> Der Juniper SRX-Connector befindet sich derzeit in der **VORSCHAUVERSION**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

In diesem Artikel wird erläutert, wie Sie Ihre Juniper SRX-Firewall-Appliance mit Azure Sentinel verbinden. Mit dem Juniper SRX-Datenconnector lassen sich Ihre SRX-Protokolle problemlos mit Azure Sentinel verbinden, sodass Sie die Daten dann in Arbeitsmappen anzeigen, als Vorlage für benutzerdefinierte Warnungen und zur Verbesserung der Untersuchung von Problemen verwenden können. Für die Integration von Juniper SRX und Azure Sentinel wird Syslog genutzt.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Ihre Juniper SRX-Lösung muss so konfiguriert sein, dass Protokolle über Syslog exportiert werden.

## <a name="forward-juniper-srx-logs-to-the-syslog-agent"></a>Weiterleiten von Juniper SRX-Protokollen an den Syslog-Agent  

Konfigurieren Sie Juniper SRX für das Weiterleiten von Syslog-Nachrichten an Ihren Azure Sentinel-Arbeitsbereich über den Syslog-Agent.

1. Wählen Sie im Azure Sentinel-Navigationsmenü **Datenconnectors** aus.

1. Wählen Sie im **Datenconnectors**-Katalog den Connector **Juniper SRX (Vorschau)** und dann **Connectorseite öffnen** aus.

1. Befolgen Sie die Anweisungen auf der Connectorseite **Juniper SRX**:

    1. Installieren und Onboarding des Agents für Linux

        - Wählen Sie eine Azure Linux-VM oder einen Nicht-Azure-Linux-Computer (physisch oder virtuell) aus.

    1. Konfigurieren der zu erfassenden Protokolle

        - Wählen Sie die Einrichtungen und Schweregrade in der Konfiguration der Arbeitsbereichs-Agents aus.

    1. Konfigurieren und Verbinden von Juniper SRX

        - Befolgen Sie diese Anweisungen, um Juniper SRX für die Weiterleitung an syslog zu konfigurieren.
            - [Datenverkehrsprotokolle (Sicherheitsrichtlinienprotokolle)](https://kb.juniper.net/InfoCenter/index?page=content&id=KB16509&actp=METADATA)
            - [Systemprotokolle](https://kb.juniper.net/InfoCenter/index?page=content&id=kb16502)
        - Verwenden Sie für den Remoteserver die IP-Adresse des Linux-Computers, auf dem Sie den Linux-Agent installiert haben.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem Sie eine Verbindung hergestellt haben, werden die Daten in Log Analytics unter „Syslog“ angezeigt.

Einige hilfreiche Beispielabfragen finden Sie auf der Registerkarte **Nächste Schritte** auf der Connectorseite.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde beschrieben, wie Sie die Juniper SRX mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
