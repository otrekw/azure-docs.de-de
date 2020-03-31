---
title: Verknüpfen von F5-Daten mit Azure Sentinel| Microsoft-Dokumentation
description: Erfahren Sie, wie Sie F5-Daten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 6f33cecca1c67f91d0f2be64ab156f45ee500521
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588262"
---
# <a name="connect-f5-to-azure-sentinel"></a>Verbinden von F5 mit Azure Sentinel

In diesem Artikel wird erläutert, wie Sie die F5 -Appliance mit Azure Sentinel verbinden. Der F5-Datenconnektor ermöglicht es Ihnen, Ihre F5-Protokolle auf einfache Weise mit Azure Sentinel zu verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und die Untersuchung von Daten zu verbessern. Die Verwendung von F5 für Azure Sentinel bietet Ihnen mehr Einblicke in die Internetnutzung Ihrer Organisation und erweitert die Funktionen für Sicherheitsvorgänge. 

## <a name="configure-your-f5-to-send-cef-messages"></a>Konfigurieren von F5 zum Senden von CEF-Nachrichten

1. Navigieren Sie zum F5-Artikel [Konfigurieren der Ereignisprotokollierung für die Anwendungssicherheit](https://techdocs.f5.com/kb/en-us/products/big-ip_asm/manuals/product/asm-implementations-11-5-0/12.html), und befolgen Sie die Anleitung zum Einrichten der Remoteprotokollierung anhand der folgenden Richtlinien:
   - Legen Sie **Remote storage type** auf **CEF** fest.
   - Legen Sie das **Protokoll** auf **TCP** fest.
   - Legen Sie **IP address** auf die IP-Adresse des Syslog-Servers fest.
   - Legen Sie **port number** auf **514** oder den Port fest, den Sie für Ihren Agent angegeben haben.
   - Sie können die maximale Größe von Abfragezeichenfolgen (**Maximum Query String Size**) auf die Größe festlegen, die Sie in Ihrem Agent angegeben haben.

1. Um das relevante Schema in Log Analytics für die CEF-Ereignisse zu verwenden, suchen Sie nach `CommonSecurityLog`.

1. Fahren Sie mit [SCHRITT 3 fort: Überprüfen der Konnektivität](connect-cef-verify.md) fort.


## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde beschrieben, wie Sie F5 mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.

