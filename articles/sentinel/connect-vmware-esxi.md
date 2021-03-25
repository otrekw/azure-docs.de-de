---
title: Verbinden von VMware ESXi-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem VMware ESXi-Datenconnector die ESXi-Protokolle in Azure Sentinel pullen. Zeigen Sie ESXi-Daten in Arbeitsmappen an, Erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
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
ms.date: 03/01/2021
ms.author: yelevin
ms.openlocfilehash: 3d478a9ac3cf91f3f6815859b8534efff88f07b1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743292"
---
# <a name="connect-your-vmware-esxi-to-azure-sentinel"></a>Verbinden von VMware ESXi mit Azure Sentinel

> [!IMPORTANT]
> Der VMware ESXi-Connector befindet sich derzeit in der **VORSCHAUVERSION**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

In diesem Artikel wird erläutert, wie Sie Ihre VMware ESXi-Appliance mit Azure Sentinel verbinden. Mit dem VMware ESXi-Datenconnector können Sie Ihre VMware ESXi-Protokolle problemlos in Azure Sentinel erfassen, um Ihnen einen besseren Einblick in die ESXi-Aktivitäten Ihres Unternehmens zu geben und die Sicherheitsfunktionen zu verbessern. Bei der Integration von VMware ESXi und Azure Sentinel wird ein Syslog-Server verwendet, auf dem der Log Analytics-Agent installiert ist. Außerdem wird ein benutzerdefinierter Protokollparser verwendet, der auf einer Kusto-Funktion basiert.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Ihre VMware ESXi-Lösung muss so konfiguriert sein, dass Protokolle über Syslog exportiert werden.

## <a name="send-vmware-esxi-logs-to-the-syslog-agent"></a>Senden von VMware ESXi-Protokollen an den Syslog-Agent  

Konfigurieren Sie VMware ESXi für das Weiterleiten von Syslog-Nachrichten an Ihren Azure Sentinel-Arbeitsbereich über den Syslog-Agent.
3. Folgen Sie den Anweisungen auf der **VMware ESXi**-Seite.


1. Wählen Sie im Azure Sentinel-Navigationsmenü **Datenconnectors** aus.

1. Wählen Sie im **Datenconnectors**-Katalog den Connector **VMware ESXi (Vorschau)** und dann **Connectorseite öffnen** aus.

1. Folgen Sie den Anweisungen auf der **VMware ESXi**-Connectorseite:

    1. Installieren und Onboarding des Agents für Linux

        - Wählen Sie eine Azure Linux-VM oder einen Nicht-Azure-Linux-Computer (physisch oder virtuell) aus.

    1. Konfigurieren der zu erfassenden Protokolle

        - Wählen Sie die Einrichtungen und Schweregrade in der **Konfiguration der Arbeitsbereichs-Agents** aus.

    1. Konfigurieren und Verbinden von VMware ESXi

        - Befolgen Sie diese Anweisungen, um VMware ESXi für die Weiterleitung an syslog zu konfigurieren:
            - [VMware ESXi 3.5 und 4.x](https://kb.vmware.com/s/article/1016621)
            - [VMware ESXi 5.0 und höher](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.monitoring.doc/GUID-9F67DB52-F469-451F-B6C8-DAE8D95976E7.html)

            Verwenden Sie für den Remoteserver die IP-Adresse des Linux-Computers, auf dem Sie den Linux-Agent installiert haben.

## <a name="validate-connectivity-and-find-your-data"></a>Überprüfen der Konnektivität und Suchen Ihrer Daten

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Azure Sentinel angezeigt werden. 

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in **Protokolle** unter *LogManagement* in der Tabelle *Syslog* angezeigt.

Dieser Datenconnector ist von einem Parser abhängig, der auf einer Kusto-Funktion basiert und erwartungsgemäß funktionieren muss. [Befolgen Sie diese Schritte](https://aka.ms/sentinel-vmwareesxi-parser), um den **VMwareESXi** Kusto-Funktionsalias zu erstellen. Anschließend können Sie `VMwareESXi` in ein beliebiges Abfragefenster in Azure Sentinel eingeben, um die Daten abzufragen.

Einige hilfreiche Beispielabfragen finden Sie auf der Registerkarte **Nächste Schritte** auf der Connectorseite.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde beschrieben, wie Sie die VMware ESXi mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
