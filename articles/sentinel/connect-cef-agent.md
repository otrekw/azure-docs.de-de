---
title: Bereitstellen des Agents zum Verknüpfen von CEF-Daten mit Azure Sentinel (Vorschau) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Agent zum Verknüpfen von CEF-Daten mit Azure Sentinel bereitstellen.
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
ms.date: 11/26/2019
ms.author: yelevin
ms.openlocfilehash: b0c9335357cb793ea76e1dbe68575f716a50372a
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588466"
---
# <a name="step-1-deploy-the-agent"></a>Schritt 1: Bereitstellen des Agents


In diesem Schritt müssen Sie den Linux-Computer auswählen, der als Proxy zwischen Azure Sentinel und Ihrer Sicherheitslösung fungieren soll. Sie müssen auf dem Proxycomputer ein Skript ausführen, das die folgenden Aufgaben übernimmt:
- Installieren des Log Analytics-Agents und Konfigurieren nach Bedarf, um auf Syslog-Nachrichten zu lauschen
- Konfigurieren des Syslog-Daemons, um auf Syslog-Nachrichten über den TCP-Port 514 zu lauschen und dann Weiterleiten von nur CEF-Nachrichten an den Log Analytics-Agent über den TCP-Port 25226
- Festlegen des Syslog-Agents für das Erfassen von Daten und sicheres Senden der Daten an Azure Sentinel, um sie dort zu analysieren und anzureichern
 
## <a name="deploy-the-agent"></a>Bereitstellen des Agents
 
1. Klicken Sie im Azure Sentinel-Portal auf **Data connectors** (Datenconnectors), und wählen Sie **Common Event Format (CEF)** und dann **Open connector page** (Connectorseite öffnen) aus. 

1. Wählen Sie unter **Syslog-Agent installieren und konfigurieren** Ihren Computertyp aus: Azure, andere Cloud oder lokal. 
   > [!NOTE]
   > Da das Skript im nächsten Schritt den Log Analytics-Agent installiert und den Computer mit Ihrem Azure Sentinel-Arbeitsbereich verbindet, stellen Sie sicher, dass dieser Computer nicht mit einem anderen Arbeitsbereich verbunden ist.
1. Sie müssen über erhöhte Berechtigungen (sudo) auf dem Computer verfügen. Stellen Sie mithilfe des folgenden Befehls sicher, dass Sie auf dem Computer über Python verfügen: `python –version`

1. Führen Sie das folgende Skript auf dem Proxycomputer aus.
   `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]`
1. Wenn das Skript ausgeführt wird, stellen Sie sicher, dass keine Fehler- oder Warnmeldungen angezeigt werden.

Fahren Sie mit [SCHRITT 2 fort: Konfigurieren Ihrer Sicherheitslösung zum Weiterleiten von CEF-Nachrichten](connect-cef-solution-config.md) .


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie CEF-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).

