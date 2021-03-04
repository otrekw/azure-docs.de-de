---
title: 'ITSM-Connector – Secure Export in Azure Monitor: Konfiguration mit BMC'
description: In diesem Artikel erfahren Sie, wie Sie Ihre ITSM-Produkte/-Dienste mit BMC für Secure Export in Azure Monitor verbinden.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/31/2020
ms.openlocfilehash: 7c50d533f95e1131e26ddd808c77023ae4591a26
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102041704"
---
# <a name="connect-bmc-helix-to-azure-monitor"></a>Verbinden von BMC Helix mit Azure Monitor

In den folgenden Abschnitten finden Sie Einzelheiten dazu, wie Sie Ihr BMC Helix-Produkt und Secure Export in Azure verbinden.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Azure AD wurde registriert.
* Sie verfügen über die unterstützte Version von BMC Helix Multi-Cloud Service Management (Version 19.08 oder höher).

## <a name="configure-the-bmc-helix-connection"></a>Konfigurieren der BMC Helix-Verbindung

1. Führen Sie in der BMC Helix-Umgebung die folgenden Schritte aus, um den URI für den sicheren Export abzurufen:

   1. Melden Sie sich bei Integration Studio an.
   1. Suchen Sie nach dem Flow zum **Erstellen von Incidents anhand von Azure-Warnungen**.
   1. Kopieren Sie die Webhook-URL.
   
   ![Screenshot der Webhook-URL in Integration Studio.](media/itsmc-secure-webhook-connections-bmc/bmc-url.png)
   
2. Gehen Sie gemäß der versionsspezifischen Anleitung vor:
   * [Aktivieren der vordefinierten Integration in Azure Monitor für Version 20.02](https://docs.bmc.com/docs/multicloud/enabling-prebuilt-integration-with-azure-monitor-879728195.html)
   * [Aktivieren der vordefinierten Integration in Azure Monitor für Version 19.11](https://docs.bmc.com/docs/multicloudprevious/enabling-prebuilt-integration-with-azure-monitor-904157623.html)

3. Wechseln Sie bei der Konfiguration der Verbindung in BMC Helix zu Ihrer BMC-Integrationsinstanz, und führen Sie folgende Schritte aus:

   1. Wählen Sie einen **Katalog** aus.
   2. Wählen Sie **Azure-Warnungen** aus.
   3. Wählen Sie **Connectors** aus.
   4. Wählen Sie eine **Konfiguration** aus.
   5. Wählen Sie die Konfiguration **Neue Verbindung hinzufügen** aus.
   6. Geben Sie im Abschnitt zur Konfiguration die folgenden Informationen ein:
      - **Name**: Legen Sie einen eigenen Namen fest.
      - **Autorisierungstyp**: **NONE**
      - **Beschreibung**: Geben Sie eine eigene Beschreibung ein.
      - **Site**: **Cloud**
      - **Anzahl von Instanzen**: **2** (Standardwert).
      - **Aktivieren**: Standardmäßig ausgewählt, um die Verwendung zu ermöglichen.
      - Die Azure-Mandanten-ID und die Azure-Anwendungs-ID stammen von der Anwendung, die Sie zuvor definiert haben.

![Screenshot einer BMC-Konfiguration.](media/itsmc-secure-webhook-connections-bmc/bmc-configuration.png)
