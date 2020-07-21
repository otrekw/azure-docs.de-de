---
title: Verknüpfen von Palo Alto Networks-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Palo Alto Networks-Datenconnector verwenden, um einfach eine Verbindung mit Ihren Palo Alto Networks-Protokollen mit Azure Sentinel herzustellen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: a4b21d67-1a72-40ec-bfce-d79a8707b4e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 245db436fc3216fe5c8c8f51c50c0ac03190f9eb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564563"
---
# <a name="connect-palo-alto-networks-to-azure-sentinel"></a>Verbinden von Palo Alto Networks mit Azure Sentinel



In diesem Artikel wird erläutert, wie Sie die Palo Alto Networks-Appliance mit Azure Sentinel verbinden. Der Palo Alto Networks-Datenconnector ermöglicht es Ihnen, Ihre Palo Alto Networks-Protokolle auf einfache Weise mit Azure Sentinel zu verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und die Untersuchung von Daten zu verbessern. Die Verwendung von Palo Alto Networks für Azure Sentinel bietet Ihnen mehr Einblicke in die Internetnutzung Ihrer Organisation und erweitert die Funktionen für Sicherheitsvorgänge. 


## <a name="forward-palo-alto-networks-logs-to-the-syslog-agent"></a>Weiterleiten der Palo Alto Networks-Protokolle an den Syslog-Agent

Konfigurieren Sie Palo Alto Networks, um Syslog-Nachrichten im CEF-Format über den Syslog-Agent an Ihren Azure-Arbeitsbereich weiterzuleiten.
1.  Wechseln Sie zu [Common Event Format (CEF) Configuration Guides](https://docs.paloaltonetworks.com/resources/cef), und laden Sie die PDF-Datei für den Typ Ihrer Appliance herunter. Führen Sie alle Anweisungen in der Anleitung aus, um Ihre Palo Alto Networks-Appliance so einzurichten, dass sie CEF-Ereignisse erfasst. 

1.  Wechseln Sie zu [Configure Syslog monitoring](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/use-syslog-for-monitoring/configure-syslog-monitoring) (Konfigurieren der Syslog-Überwachung), und führen Sie die Schritte 2 und 3 aus, um CEF-Ereignisweiterleitung von Ihrer Palo Alto Networks-Appliance an Azure Sentinel zu konfigurieren.

    1. Legen Sie die Option **Syslog server format** (Format des Syslog-Servers) auf **BSD** fest.

       > [!NOTE]
       > Bei den Kopieren/Einfügen-Vorgängen aus der PDF-Datei kann es vorkommen, dass Text geändert wird und zufällige Zeichen eingefügt werden. Um dies zu vermeiden, kopieren Sie den Text in einen Editor, und entfernen Sie, bevor Sie den Text einfügen, alle Zeichen, die das Protokollformat beeinträchtigen könnten, wie dies im folgenden Beispiel zu sehen ist.
 
        ![Problem beim Kopieren von CEF-Text](./media/connect-cef/paloalto-text-prob1.png)

1. Um das relevante Schema in Log Analytics für die Palo Alto Networks-Ereignisse zu verwenden, suchen Sie nach **CommonSecurityLog**.

1. Fahren Sie mit [SCHRITT 3 fort: Überprüfen der Konnektivität](connect-cef-verify.md) fort.




## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Palo Alto Networks-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.


