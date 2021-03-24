---
title: Verbinden von Cisco Meraki-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Cisco Meraki-Datenconnector entsprechende Cisco Meraki-Protokolle in Azure Sentinel pullen. Zeigen Sie Cisco Meraki-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
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
ms.date: 02/28/2021
ms.author: yelevin
ms.openlocfilehash: dc2d2a0724f18a02a5b667eb1004963a326ec360
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743513"
---
# <a name="connect-your-cisco-meraki-to-azure-sentinel"></a>Verbinden von Cisco Meraki mit Azure Sentinel

> [!IMPORTANT]
> Der Cisco Meraki-Connector befindet sich derzeit in der **VORSCHAUVERSION**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

In diesem Artikel wird erläutert, wie Sie Cisco Meraki-Appliances (MX/MS/MR) mit Azure Sentinel verbinden. Der Cisco Meraki-Datenconnektor ermöglicht es Ihnen, Ihre Cisco Meraki-Protokolle auf einfache Weise mit Azure Sentinel zu verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und die Untersuchung von Daten zu verbessern. Bei der Integration von Cisco Meraki und Azure Sentinel wird ein Syslog-Server verwendet, auf dem der Log Analytics-Agent installiert ist. Außerdem wird ein benutzerdefinierter Protokollparser verwendet, der auf einer Kusto-Funktion basiert.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Ihre Cisco Meraki-Lösung muss so konfiguriert sein, dass Protokolle über Syslog exportiert werden.

## <a name="send-cisco-meraki-logs-to-azure-sentinel-via-the-syslog-agent"></a>Senden von Cisco Meraki-Protokollen an Azure Sentinel über den Syslog-Agent  

Konfigurieren Sie Cisco Meraki für das Weiterleiten von Syslog-Nachrichten an Ihren Azure Sentinel-Arbeitsbereich über den Syslog-Agent.

1. Wählen Sie im Azure Sentinel-Navigationsmenü **Datenconnectors** aus.

1. Wählen Sie im **Datenconnectors**-Katalog den Connector **Cisco Meraki (Vorschau)** und dann **Connectorseite öffnen** aus.

1. Befolgen Sie die Anweisungen auf der **Cisco Meraki**-Connectorseite:

    1. Installieren und Onboarding des Agents für Linux

        - Wählen Sie eine Azure Linux-VM oder einen Nicht-Azure-Linux-Computer (physisch oder virtuell) aus.

    1. Konfigurieren der zu erfassenden Protokolle

        - Wählen Sie die Einrichtungen und Schweregrade in der **Konfiguration der Arbeitsbereichs-Agents** aus.

    1. Konfigurieren und Verbinden von Cisco Meraki-Geräten

        - Befolgen Sie [diese Anweisungen](https://documentation.meraki.com/General_Administration/Monitoring_and_Reporting/Meraki_Device_Reporting_-_Syslog%2C_SNMP_and_API), um die Cisco Meraki-Geräte für die Weiterleitung an syslog zu konfigurieren. Verwenden Sie für den Remoteserver die IP-Adresse des Linux-Computers, auf dem Sie den Linux-Agent installiert haben.

## <a name="validate-connectivity-and-find-your-data"></a>Überprüfen der Konnektivität und Suchen Ihrer Daten

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Azure Sentinel angezeigt werden. 

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in **Protokolle** unter *LogManagement* in der Tabelle *Syslog* angezeigt.

Dieser Datenconnector ist von einem Parser abhängig, der auf einer Kusto-Funktion basiert und erwartungsgemäß funktionieren muss. [Befolgen Sie diese Schritte](https://aka.ms/sentinel-ciscomeraki-parser), um den **CiscoMeraki** Kusto-Funktionsalias zu erstellen. Anschließend können Sie `CiscoMeraki` im Abfragefenster eingeben, um die Daten abzufragen.

Einige hilfreiche Beispielabfragen finden Sie auf der Registerkarte **Nächste Schritte** auf der Connectorseite.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde beschrieben, wie Sie Cisco Meraki mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
