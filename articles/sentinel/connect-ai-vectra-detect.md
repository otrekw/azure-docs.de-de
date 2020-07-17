---
title: Verknüpfen von AI Vectra Detect-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie AI Vectra Detect-Daten mit Azure Sentinel verbinden.
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
ms.date: 06/21/2020
ms.author: yelevin
ms.openlocfilehash: 964bab2b81f6ce69e77fd69cab55f6d1894e4edb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85368404"
---
# <a name="connect-ai-vectra-detect-to-azure-sentinel"></a>Verbinden von AI Vectra Detect mit Azure Sentinel

In diesem Artikel wird erläutert, wie Sie die [AI Vectra Detect](https://www.vectra.ai/product/cognito-detect)-Appliance mit Azure Sentinel verbinden. Mit dem AI Vectra-Datenkonnektor lassen sich AI Vectra Detect-Daten problemlos in Azure Sentinel einbinden. Diese Daten können Sie dann in Arbeitsmappen anzeigen, als Vorlage für benutzerdefinierte Warnungen und zur Untersuchung von Problemen verwenden.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-your-ai-vectra-detect-appliance-to-send-cef-messages"></a>Konfigurieren der AI Vectra Detect-Appliance für das Senden von CEF-Nachrichten  

Konfigurieren Sie AI Vectra Detect für die Weiterleitung CEF-formatierter Syslog-Nachrichten an den Log Analytics-Arbeitsbereich. Verwenden Sie dazu die Syslog-Weiterleitung, den Sie in [SCHRITT 1: Bereitstellen der Protokollweiterleitung](connect-cef-agent.md) eingerichtet haben.

1. Navigieren Sie von der Vectra-Schnittstelle zu „Einstellungen > Benachrichtigungen“, und wählen Sie „Syslog-Konfiguration bearbeiten“ aus. Folgen Sie den unten stehenden Anweisungen, um die Verbindung herzustellen:

    - Neues Ziel hinzufügen (der Hostname von [Protokollweiterleitung](connect-cef-agent.md))
    - Port als **514** festlegen
    - Protokoll als **UDP** festlegen
    - Format als **CEF** festlegen
    - Protokolltypen festlegen (alle verfügbaren Protokolltypen auswählen)
    - Klicken Sie im Menü „Einstellungen“ auf **Speichern**

2. Sie können auf die Schaltfläche **Test** klicken, um das Senden einiger Testereignisse an die Protokollweiterleitung zu erzwingen.

3. Um das relevante Log Analytics-Schema für AI Vectra Detect-Ereignisse zu verwenden, suchen Sie nach **CommonSecurityLog**.

4. Fahren Sie mit [SCHRITT 3: Überprüfen der Konnektivität](connect-cef-verify.md) fort.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie gelernt, wie Sie AI Vectra Detect-Appliances mit Azure Sentinel verbinden. Um die Vorteile der in diesen Datenkonnektor integrierten Funktionen in vollem Umfang nutzen zu können, klicken Sie auf der Datenkonnektor-Seite auf die Registerkarte **Nächste Schritte**. Dort finden Sie einige vorgefertigte Beispielabfragen, die Ihnen die Suche nach nützlichen Informationen erleichtern.

Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
