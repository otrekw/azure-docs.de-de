---
title: Verknüpfen von CEF-Daten mit der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: Stellen Sie eine Verbindung mit einer externen Lösung her, die CEF-Nachrichten (Common Event Format) unter Verwendung eines Linux-Computers als Proxy an Azure Sentinel sendet.
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
ms.date: 10/01/2020
ms.author: yelevin
ms.openlocfilehash: d63893ab219854a270652da38c474e3ccad83abc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91630507"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Verbinden der externen Lösung mithilfe von Common Event Format

Wenn Sie eine Verbindung mit einer externen Lösung herstellen, die CEF-Nachrichten sendet, sind drei Schritte für die Verbindung mit Azure Sentinel erforderlich:

SCHRITT 1: [Stellen Sie eine Verbindung mit CEF her, indem Sie eine Syslog/CEF Forwarder-Instanz bereitstellen](connect-cef-agent.md) SCHRITT 2: [Ausführen lösungsspezifischer Schritte](connect-cef-solution-config.md) SCHRITT 3: [Überprüfen der Konnektivität](connect-cef-verify.md)

In diesem Artikel werden die Funktionsweise der Verbindung sowie die Voraussetzungen beschrieben und die Schritte zum Bereitstellen des Agents in Sicherheitslösungen erläutert, die zusätzlich zu Syslog CEF-Nachrichten senden. 

> [!NOTE] 
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

Damit Sie diese Verbindung herstellen können, müssen Sie einen Syslog Forwarder-Server bereitstellen, um die Kommunikation zwischen der Appliance und Azure Sentinel zu ermöglichen.  Der Server besteht aus einem dedizierten Linux-Computer (VM oder lokal), auf dem der Log Analytics-Agent für Linux installiert ist. 

Die folgende Abbildung beschreibt die Einrichtung einer Linux-VM in Azure:

 ![CEF in Azure](./media/connect-cef/cef-syslog-azure.png)

Alternativ gilt diese Einrichtung, wenn Sie eine VM in einer anderen Cloud oder auf einem lokalen Computer verwenden: 

 ![CEF lokal](./media/connect-cef/cef-syslog-onprem.png)

## <a name="security-considerations"></a>Sicherheitshinweise

Stellen Sie sicher, dass Sie die Sicherheit des Computers gemäß der Sicherheitsrichtlinie Ihrer Organisation konfigurieren. Beispielsweise können Sie das Netzwerk so konfigurieren, dass es der Sicherheitsrichtlinie für das Unternehmensnetzwerk entspricht, und die Ports und Protokolle im Daemon Ihren Anforderungen entsprechend ändern. Anhand der folgenden Anleitungen können Sie die Sicherheitskonfiguration Ihrer Computer verbessern:   [Sichern virtueller Computer in Azure](../virtual-machines/security-policy.md), [Bewährte Methoden für die Netzwerksicherheit](../security/fundamentals/network-best-practices.md).

Wenn Sie die TLS-Kommunikation zwischen der Syslog-Lösung und Syslog Forwarder verwenden möchten, müssen Sie den Syslog-Daemon (rsyslog oder syslog-ng) für die Kommunikation in TLS konfigurieren: [Verschlüsseln von Syslog-Datenverkehr mit TLS – rsyslog](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html), [Verschlüsseln von Protokollmeldungen mit TLS – syslog-ng](https://support.oneidentity.com/technical-documents/syslog-ng-open-source-edition/3.22/administration-guide/60#TOPIC-1209298).
 
## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass auf dem Linux-Computer, den Sie als Proxy verwenden, eines der folgenden Betriebssysteme ausgeführt wird:

- 64 Bit
  - CentOS 7 und Unterversionen sowie höhere Versionen (Version 6 ausgenommen)
  - Amazon Linux 2017.09
  - Oracle Linux 6 und 7
  - RHEL-Server 7 (Red Hat Enterprise Linux) und Unterversionen sowie höhere Versionen (Version 6 ausgenommen)
  - Debian GNU/Linux 8 und 9
  - Ubuntu Linux 14.04 LTS, 16.04 LTS und 18.04 LTS
  - SUSE Linux Enterprise Server 12
- 32 Bit
   - CentOS 7:
   - Oracle Linux 6
   - Red Hat Enterprise Linux Server 7
   - Debian GNU/Linux 8 und 9
   - Ubuntu Linux 14.04 LTS und 16.04 LTS
 
 - Daemonversionen
   - Syslog-ng: 2.1 bis 3.22.1
   - Rsyslog: v8
  
 - Unterstützte Syslog-RFCs
   - Syslog RFC 3164
   - Syslog RFC 5424
 
Stellen Sie sicher, dass Ihr Computer auch die folgenden Anforderungen erfüllt: 
- Berechtigungen
    - Sie müssen über erhöhte Berechtigungen (sudo) auf dem Computer verfügen. 
- Softwareanforderungen
    - Stellen Sie sicher, dass Python (Version 2.7 oder höher) auf dem Computer ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie CEF-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).

