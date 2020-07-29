---
title: Verbinden von Okta Single Sign-On-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Okta Single Sign-On-Daten mit Azure Sentinel verbinden.
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
ms.openlocfilehash: 37ade037b7f3c88f5ff33d7fc4640b19f366fe7a
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528093"
---
# <a name="connect-your-okta-single-sign-on-to-azure-sentinel-with-azure-function"></a>Verbinden von Okta Single Sign-On mit Azure Sentinel über Azure Functions

Mit dem Connector für Okta Single Sign-On (SSO) können Sie alle Protokolle Ihrer [Okta Single Sign-On (SSO)](https://www.okta.com/products/single-sign-on/)-Sicherheitslösung einfach mit Azure Sentinel verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Bei der Integration von Okta Single Sign-On und Azure Sentinel wird Azure Functions verwendet, um Protokolldaten per REST-API zu pullen.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-okta-single-sign-on"></a>Konfigurieren und Verbinden von Okta Single Sign-On

Azure Functions kann in Okta Single Sign-On integriert werden und Ereignisse und Protokolle direkt aus Okta Single Sign-On pullen und an Azure Sentinel weiterleiten.

1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**, und wählen Sie den Connector **Okta Single Sign-On** aus.

1. Wählen Sie **Connectorseite öffnen** aus.

1. Befolgen Sie die Anweisungen auf der Seite **Okta Single Sign-On**.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem Sie eine Verbindung hergestellt haben, werden die Daten in Log Analytics in der Tabelle **Okta_CL** aufgeführt.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie Okta Single Sign-On mithilfe von Azure-Funktions-Apps mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.

