---
title: Konfigurieren Ihrer Sicherheitslösung zum Verbinden von CEF-Daten mit der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Sicherheitslösung so konfigurieren, dass sie CEF-Daten mit Azure Sentinel verbindet.
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
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 1c25e48bd46f0d37330f693cb4d6538e7bc29c4b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85367239"
---
# <a name="step-2-configure-your-security-solution-to-send-cef-messages"></a>SCHRITT 2: Konfigurieren Ihrer Sicherheitslösung zum Senden von CEF-Nachrichten

In diesem Schritt führen Sie die erforderlichen Konfigurationsänderungen an Ihrer Sicherheitslösung durch, sodass Protokolle an den CEF-Agent gesendet werden.

## <a name="configure-a-solution-with-a-connector"></a>Konfigurieren einer Lösung mit einem Connector

Wenn Ihre Sicherheitslösung bereits über einen Connector verfügt, verwenden Sie die connectorspezifischen Anweisungen wie folgt:

- [AI Vectra Detect](connect-ai-vectra-detect.md)
- [Check Point](connect-checkpoint.md)
- [Cisco](connect-cisco.md)
- [ExtraHop Reveal(x)](connect-extrahop.md)
- [F5 ASM](connect-f5.md)  
- [Fortinet](connect-fortinet.md)
- [One Identity Safeguard](connect-one-identity.md)
- [Palo Alto Networks](connect-paloalto.md)
- [Trend Micro Deep Security](connect-trend-micro.md)
- [Zscaler](connect-zscaler.md)   

## <a name="configure-any-other-solution"></a>Konfigurieren einer beliebigen anderen Lösung

Wenn für Ihre spezifische Sicherheitslösung kein Connector vorhanden ist, verwenden Sie die folgenden allgemeinen Anweisungen für das Weiterleiten von Protokollen an den CEF-Agent.

1. Schritte zum Konfigurieren Ihrer Lösung für das Senden von CEF-Nachrichten finden Sie im Artikel zur entsprechenden Konfiguration. Wenn Ihre Lösung nicht aufgeführt ist, müssen Sie auf der Appliance folgende Werte festlegen, damit die erforderlichen Protokolle von der Appliance im erforderlichen Format an den Azure Sentinel-Syslog-Agent (der auf dem Log Analytics-Agent basiert) gesendet werden. Sie können diese Parameter in Ihrer Appliance ändern, sofern Sie diese im Syslog-Daemon auf dem Azure Sentinel-Agent ändern.
    - Protokoll = TCP
    - Port = 514
    - Format = CEF
    - IP-Adresse: Achten Sie darauf, die CEF-Nachrichten an die IP-Adresse des virtuellen Computers zu senden, den Sie für diesen Zweck reserviert haben.

   > [!NOTE]
   > Diese Lösung unterstützt Syslog RFC 3164 und RFC 5424.

1. Um das relevante Schema in Log Analytics für die CEF-Ereignisse zu verwenden, suchen Sie nach `CommonSecurityLog`.

1. Fahren Sie mit SCHRITT 3: [Überprüfen der Konnektivität](connect-cef-verify.md) fort.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie CEF-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
