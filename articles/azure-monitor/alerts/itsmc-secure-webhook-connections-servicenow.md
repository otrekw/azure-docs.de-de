---
title: 'ITSM-Connector – Secure Export in Azure Monitor: Konfiguration mit ServiceNow'
description: In diesem Artikel erfahren Sie, wie Sie Ihre ITSM-Produkte/-Dienste mit ServiceNow für Secure Export in Azure Monitor verbinden.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/31/2020
ms.openlocfilehash: cf19911bd8126bfb73f848c086aa817a7d7adb00
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103563690"
---
# <a name="connect-servicenow-to-azure-monitor"></a>Herstellen einer Verbindung von ServiceNow mit Azure Monitor

In den folgenden Abschnitten finden Sie Einzelheiten dazu, wie Sie Ihr ServiceNow-Produkt und den sicheren Export in Azure verbinden.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Azure AD wurde registriert.
* Sie verfügen über eine unterstützte Version von The ServiceNow Event Management – ITOM (ab Version „New York“).
* In ServiceNow-Instanz installierte [Anwendung](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ac4c9c57dbb1d090561b186c1396191a/1.3.1?referer=%2Fstore%2Fsearch%3Flistingtype%3Dallintegrations%25253Bancillary_app%25253Bcertified_apps%25253Bcontent%25253Bindustry_solution%25253Boem%25253Butility%26q%3DEvent%2520Management%2520Connectors&sl=sh).

## <a name="configure-the-servicenow-connection"></a>Konfigurieren der ServiceNow-Verbindung

1. Verwenden Sie diesen Link als URI für die Definition des sicheren Exports: https://(instance name).service-now.com/api/sn_em_connector/em/inbound_event?source=azuremonitor.

2. Gehen Sie gemäß der versionsspezifischen Anleitung vor:
   * [Paris](https://docs.servicenow.com/bundle/paris-it-operations-management/page/product/event-management/concept/azure-integration.html)
   * [Orlando](https://docs.servicenow.com/bundle/orlando-it-operations-management/page/product/event-management/concept/azure-integration.html)
   * [New York](https://docs.servicenow.com/bundle/newyork-it-operations-management/page/product/event-management/concept/azure-integration.html)
