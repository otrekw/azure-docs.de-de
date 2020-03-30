---
title: include file
description: include file
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 02/20/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 3778ec2fac13aee29ce361402a535ca70fd56c33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "75750715"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Aktivieren der Protokollierung mit Diagnoseeinstellungen

[!INCLUDE [updated-for-az](./updated-for-az.md)]

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem IoT Hub.

2. Wählen Sie **Diagnoseeinstellungen** aus.

3. Wählen Sie **Diagnose aktivieren**.

   ![Aktivieren der Diagnose](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. Benennen Sie die Diagnoseeinstellungen.

5. Wählen Sie aus, wohin die Protokolle gesendet werden sollen. Sie können eine beliebige Kombination der drei Optionen auswählen:

   * In einem Speicherkonto archivieren
   * An einen Event Hub streamen
   * An Log Analytics senden

6. Wählen Sie aus, welche Vorgänge Sie überwachen möchten, und aktivieren Sie die Protokolle für diese Vorgänge. Die Vorgänge, für die über die Diagnoseeinstellungen Berichte erstellt werden können, sind die folgenden:

   * Verbindungen
   * Gerätetelemetrie
   * C2D-Nachrichten
   * Geräte-Identitätsvorgänge
   * Dateiuploads
   * Nachrichtenweiterleitung
   * Cloud-zu-Gerät-Zwillingsvorgänge
   * Gerät-zu-Cloud-Zwillingsvorgänge
   * Zwillingsvorgänge
   * Auftragsvorgänge
   * Direkte Methoden  
   * Verteilte Ablaufverfolgung (Vorschauversion)
   * Configurations
   * Gerätestreams
   * Gerätemetriken

6. Speichern Sie die neuen Einstellungen. 

Wenn Sie Diagnoseeinstellungen mit PowerShell aktivieren möchten, verwenden Sie den folgenden Code:

```azurepowershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Neue Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel auf dem Blatt **Diagnoseeinstellungen** angezeigt. Weitere Informationen zur Konfiguration von Diagnosen finden Sie unter [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](../articles/azure-monitor/platform/platform-logs-overview.md).
