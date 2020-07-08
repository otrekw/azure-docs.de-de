---
title: Aktivieren der Ressourcenprotokollierung in Azure Traffic Manager
description: Es wird beschrieben, wie Sie die Ressourcenprotokollierung für Ihr Traffic Manager-Profil aktivieren und auf die Protokolldateien zugreifen, die sich bei dem Vorgang ergeben.
services: traffic-manager
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/25/2019
ms.author: rohink
ms.openlocfilehash: d84c4a9af366e9032604aef5531742eb790bb571
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84711679"
---
# <a name="enable-resource-logging-in-azure-traffic-manager"></a>Aktivieren der Ressourcenprotokollierung in Azure Traffic Manager

In diesem Artikel wird beschrieben, wie Sie die Erfassung von Diagnoseressourcenprotokollen aktivieren und auf die Protokolldaten für ein Traffic Manager-Profil zugreifen.

Azure Traffic Manager-Ressourcenprotokolle können Erkenntnisse zum Verhalten der Traffic Manager-Profilressource liefern. Beispielsweise können Sie die Protokolldaten des Profils verwenden, um zu ermitteln, warum für einzelne Tests eines Endpunkts ein Timeout aufgetreten ist.

## <a name="enable-resource-logging"></a>Aktivieren der Ressourcenprotokollierung

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Sie können die nachfolgenden Befehle in [Azure Cloud Shell](https://shell.azure.com/powershell) oder über PowerShell auf Ihrem Computer ausführen. Azure Cloud Shell ist eine kostenlose interaktive Shell. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Wenn Sie PowerShell auf Ihrem Computer ausführen, müssen Sie mindestens Version 1.0.0 des Azure PowerShell-Moduls ausführen. Sie können `Get-Module -ListAvailable Az` ausführen, um die installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-az-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzAccount` ausführen, um sich bei Azure anzumelden.

1. **Rufen Sie das Traffic Manager-Profil ab:**

    Zum Aktivieren der Ressourcenprotokollierung benötigen Sie die ID eines Traffic Manager-Profils. Rufen Sie das Traffic Manager-Profil ab, für das Sie die Ressourcenprotokollierung aktivieren möchten, indem Sie [Get-AzTrafficManagerProfile](/powershell/module/az.TrafficManager/Get-azTrafficManagerProfile) verwenden. Die Ausgabe enthält die Informationen zur ID des Traffic Manager-Profils.

    ```azurepowershell-interactive
    Get-AzTrafficManagerProfile -Name <TrafficManagerprofilename> -ResourceGroupName <resourcegroupname>
    ```

2. **Aktivieren der Ressourcenprotokollierung für das Traffic Manager-Profil:**

    Aktivieren Sie mit [Set-AzureDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting?view=latest) die Ressourcenprotokollierung für das Traffic Manager-Profil, indem Sie die ID verwenden, die Sie im vorherigen Schritt abgerufen haben. Mit dem folgenden Befehl werden ausführliche Protokolle für das Traffic Manager-Profil in einem angegebenen Azure Storage-Konto gespeichert. 

      ```azurepowershell-interactive
    Set-AzDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId> -StorageAccountId <storageAccountId> -Enabled $true
      ``` 
3. **Überprüfen von Diagnoseeinstellungen:**

      Überprüfen Sie die Diagnoseeinstellungen für das Traffic Manager-Profil, indem Sie [Get-AzureDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/get-azdiagnosticsetting?view=latest) verwenden. Mit dem folgenden Befehl werden die Kategorien angezeigt, die für eine Ressource protokolliert werden.

     ```azurepowershell-interactive
     Get-AzDiagnosticSetting -ResourceId <TrafficManagerprofileResourceId>
     ```  
      Stellen Sie sicher, dass für alle Protokollkategorien, die der Traffic Manager-Profilressource zugeordnet sind, „Aktiviert“ angezeigt wird. Stellen Sie außerdem sicher, dass das Speicherkonto richtig festgelegt wurde.

## <a name="access-log-files"></a>Zugreifen auf Protokolldateien
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Navigieren Sie im Portal zu Ihrem Azure Storage-Konto.
2. Wählen Sie auf der Seite **Übersicht** Ihres Azure-Speicherkontos unter **Dienste** die Option **Blobs**.
3. Wählen Sie unter **Container** die Option **insights-logs-probehealthstatusevents**, und navigieren Sie nach unten zur Datei „PT1H.json“. Klicken Sie anschließend auf **Herunterladen**, um eine Kopie dieser Protokolldatei herunterzuladen und zu speichern.

    ![Zugreifen auf Protokolldateien Ihres Traffic Manager-Profils über einen Blobspeicher](./media/traffic-manager-logs/traffic-manager-logs.png)


## <a name="traffic-manager-log-schema"></a>Traffic Manager-Protokollschema

Für alle Ressourcenprotokolle, die über Azure Monitor verfügbar sind, wird ein Schema der obersten Ebene gemeinsam genutzt. Auf diese Weise kann jeder Dienst für seine eigenen Ereignisse flexibel eindeutige Eigenschaften ausgeben. Informationen zum Ressourcenprotokollschema der obersten Ebene finden Sie unter [Unterstützte Dienste, Schemas und Kategorien für Azure-Ressourcenprotokolle](../azure-monitor/platform/tutorial-dashboards.md).

Die folgende Tabelle enthält das Protokollschema für die Azure Traffic Manager-Profilressource.

|||||
|----|----|---|---|
|**Feldname**|**Feldtyp**|**Definition**|**Beispiel**|
|EndpointName|String|Der Name des Traffic Manager-Endpunkts, dessen Integritätsstatus aufgezeichnet wird.|*myPrimaryEndpoint*|
|Status|String|Der Integritätsstatus des Traffic Manager-Endpunkts, der getestet wurde. Der Status kann entweder **Up** oder **Down** lauten.|**Up**|
|||||

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr zur [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md).

