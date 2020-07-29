---
title: Verbinden von Qualys Vulnerability Management-Daten mit Azure Sentinel| Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Qualys Vulnerability Management-Daten mit Azure Sentinel verbinden.
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
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: 44002a8f4ab3b644e3530ee2d2fc06a7af271fbe
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528064"
---
# <a name="connect-your-qualys-vm-to-azure-sentinel-with-azure-function"></a>Verbinden Ihrer Qualys VM-Instanz mithilfe von Azure Function mit Azure Sentinel

Mit dem Connector für Qualys Vulnerability Management (VM) können Sie alle Protokolle Ihrer [Qualys VM](https://www.qualys.com/apps/vulnerability-management/)-Sicherheitslösungen einfach mit Azure Sentinel verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Bei der Integration von Qualys VM und Azure Sentinel wird Azure Functions verwendet, um Protokolldaten per REST-API zu pullen.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-qualys-vm"></a>Konfigurieren und Verbinden von Qualys VM

Azure Functions kann in Qualys VM integriert werden und Ereignisse und Protokolle direkt aus Qualys VM pullen und an Azure Sentinel weiterleiten.

1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**, und wählen Sie den Connector **Qualys Vulnerability Management** aus.

1. Wählen Sie **Connectorseite öffnen** aus.

1. Befolgen Sie die Anweisungen auf der Seite **Qualys Vulnerability Management**.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem Sie eine Verbindung hergestellt haben, werden die Daten in Log Analytics in der Tabelle **QualysHostDetection_CL** aufgeführt.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie Qualys VM mithilfe von Azure-Funktions-Apps mit Azure Sentinel verknüpfen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.
