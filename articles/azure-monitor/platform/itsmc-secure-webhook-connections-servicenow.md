---
title: 'ITSM-Connector – Secure Export in Azure Monitor: Konfiguration mit ServiceNow'
description: In diesem Artikel erfahren Sie, wie Sie Ihre ITSM-Produkte/-Dienste mit ServiceNow für Secure Export in Azure Monitor verbinden.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/31/2020
ms.openlocfilehash: fc384ffbc246f5ce9fa84b8161cbc4a5226fa5c8
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2020
ms.locfileid: "97836402"
---
# <a name="connect-servicenow-to-azure-monitor"></a>Herstellen einer Verbindung von ServiceNow mit Azure Monitor

In den folgenden Abschnitten finden Sie Einzelheiten dazu, wie Sie Ihr ServiceNow-Produkt und den sicheren Export in Azure verbinden.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Azure AD wurde registriert.
* Sie verfügen über eine unterstützte Version von The ServiceNow Event Management – ITOM (Version „Orlando“ oder höher).

## <a name="configure-the-servicenow-connection"></a>Konfigurieren der ServiceNow-Verbindung

1. Verwenden Sie diesen Link als URI für die Definition des sicheren Exports: https://(instance name).service-now.com/api/sn_em_connector/em/inbound_event?source=azuremonitor.

2. Gehen Sie gemäß der versionsspezifischen Anleitung vor:
   * [Paris](https://docs.servicenow.com/bundle/paris-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
   * [Orlando](https://docs.servicenow.com/bundle/orlando-it-operations-management/page/product/event-management/task/azure-events-authentication.html)
   * [New York](https://docs.servicenow.com/bundle/newyork-it-operations-management/page/product/event-management/task/azure-events-authentication.html)