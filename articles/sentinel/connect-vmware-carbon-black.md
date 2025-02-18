---
title: Verbinden von VMware Carbon Black Cloud Endpoint Standard-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie VMware Carbon Black Cloud Endpoint Standard-Daten mit Azure Sentinel verbinden.
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
ms.openlocfilehash: 362245b6aa3fa4ae943925242b8f306e43efd246
ms.sourcegitcommit: 05dd6452632e00645ec0716a5943c7ac6c9bec7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122351044"
---
# <a name="connect-your-vmware-carbon-black-cloud-endpoint-standard-to-azure-sentinel-with-azure-function"></a>Verbinden Ihres VMware Carbon Black Cloud Endpoint Standard mithilfe von Azure Functions mit Azure Sentinel

> [!IMPORTANT]
> Der VMware Carbon Black Cloud Endpoint Standard-Datenconnector in Azure Sentinel befindet sich derzeit in der öffentlichen Vorschauphase.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

Mit dem Connector für VMware Carbon Black Cloud Endpoint Standard können Sie alle Protokolle Ihrer [VMware Carbon Black Cloud Endpoint Standard](https://www.carbonblack.com/products/endpoint-standard/)-Sicherheitslösungen einfach mit Azure Sentinel verknüpfen, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Bei der Integration von VMware Carbon Black Cloud Endpoint Standard und Azure Sentinel wird Azure Functions verwendet, um Protokolldaten per REST-API zu pullen.


> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-vmware-carbon-black-cloud-endpoint-standard"></a>Konfigurieren und Verbinden von VMware Carbon Black Cloud Endpoint Standard

Azure Functions kann in VMware Carbon Black Cloud Endpoint Standard integriert werden und Ereignisse und Protokolle direkt aus VMware Carbon Black Cloud Endpoint Standard pullen und an Azure Sentinel weiterleiten.

1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**, und wählen Sie den Connector **VMware Carbon Black** aus.
2. Wählen Sie **Connectorseite öffnen** aus.
3. Befolgen Sie die Anweisungen auf der Seite **VMware Carbon Black**.


## <a name="find-your-data"></a>Suchen von Daten

Nachdem Sie erfolgreich eine Verbindung hergestellt haben, werden die Daten in Log Analytics in den Tabellen **CarbonBlackAuditLogs_CL**, **CarbonBlackNotifications_CL** und ****CarbonBlackEvents_CL**** aufgeführt.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität
Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 


## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie VMware Carbon Black Cloud Endpoint Standard mithilfe von Azure Functions-Apps mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](monitor-your-data.md), um Ihre Daten zu überwachen.