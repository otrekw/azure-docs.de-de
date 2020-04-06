---
title: Aktualisieren des Tarifs eines Azure Event Grid-Themas oder einer -Domäne
description: In diesem Artikel wird beschrieben, wie Sie den Tarif eines Azure Event Grid-Themas oder einer -Domäne (Basic in Premium, Premium in Basic) mithilfe des Azure-Portals, der Azure CLI und Azure PowerShell aktualisieren.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: spelluru
ms.openlocfilehash: 1e92a9f0b9ba007b289c634995b04f4f1575310a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79300593"
---
# <a name="update-pricing-tier"></a>Aktualisieren des Tarifs 
In diesem Artikel wird gezeigt, wie Sie den Tarif eines Azure Event Grid-Themas oder einer -Domäne mithilfe des Azure-Portals, der Azure CLI und Azure PowerShell aktualisieren. 

## <a name="use-azure-portal"></a>Verwenden des Azure-Portals
In diesem Abschnitt wird gezeigt, wie der Tarif eines Themas oder einer Domäne im Azure-Portal geändert wird. 

### <a name="overview-page"></a>Seite „Übersicht“
Sie können den Tarif eines Themas oder einer Domäne auf der Seite **Übersicht** ändern. Im folgenden Beispiel wird gezeigt, wie ein Thema aus dem Tarif „Basic“ in den Tarif „Premium“ aktualisiert wird. Die Schritte beim Herabstufen aus dem Tarif „Premium“ in „Basic“ sind ähnlich.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Seite Ihres Themas oder Ihrer Domäne. 
2. Wählen Sie auf der Seite **Übersicht** den aktuellen Tarif aus (im folgenden Beispiel ist dies **Basic**).
    
    ![Auswählen des aktuellen Tarifs](./media/update-tier/select-tier.png)
3. Ändern Sie auf der Seite **Tarif** den Tarif, und wählen Sie dann **OK** aus. 

    ![Aktualisieren des Tarifs](./media/update-tier/change-tier.png)
4. Überprüfen Sie den Status des Vorgangs auf der Seite **Benachrichtigungen**.

    ![Aktualisierungsstatus](./media/update-tier/status.png)    
5. Vergewissern Sie sich, dass auf der Seite **Übersicht** der aktualisierte Tarif angezeigt wird. 

    ![Aktualisierungsstatus](./media/update-tier/changed-tier.png)

### <a name="networking-page"></a>Seite „Netzwerk“
Auf der Seite **Netzwerk** können Sie ein **Upgrade** aus dem Basic-Tarif in den Premium-Tarif ausführen. Eine Herabstufung aus dem Premium-Tarif in den Basic-Tarif ist auf dieser Seite jedoch nicht möglich. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Seite Ihres Themas oder Ihrer Domäne. 
2. Wechseln Sie auf der Seite **Netzwerk** zur Registerkarte **Private Endpunktverbindungen (Vorschau)** . 
3. Wenn der aktuelle Tarif **Basic** ist, wird die folgende Meldung angezeigt. Wählen Sie diese aus. 

    ![Aktualisieren des Tarifs auf der Seite „Private Endpunktverbindungen“.](./media/update-tier/private-endpoint-connections-page.png)
4. Wählen Sie auf der Seite **Update auf Premium-Tarif** die Option **Ja** aus. 
    
    ![Bestätigen des Upgrades](./media/update-tier/message-private-endpoint-connection.png)
5. Überprüfen Sie den Status des Vorgangs auf der Seite **Benachrichtigungen**.

    ![Aktualisierungsstatus](./media/update-tier/status.png)



## <a name="use-azure-cli"></a>Mithilfe der Azure-Befehlszeilenschnittstelle
In diesem Abschnitt wird gezeigt, wie der Tarif eines Themas oder einer Domäne mit Azure CLI-Befehlen geändert wird. 

### <a name="upgrade-a-topic-from-basic-to-premium"></a>Upgrade eines Themas aus Basic in Premium

```azurecli-interactive
az rest --method patch \
        --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" \
        --body "{\""sku\"": {\""name\"": \""premium\""}}"
```

### <a name="downgrade-a-topic-from-premium-to-basic"></a>Herabstufen eines Themas aus Premium in Basic

```azurecli-interactive
az rest --method patch \
        --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" \
        --body "{\""sku\"": {\""name\"": \""basic\""}}"
```

### <a name="upgrade-a-domain-from-basic-to-premium"></a>Upgrade einer Domäne aus Basic in Premium

```azurecli-interactive
az rest --method patch \
        --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" \
        --body "{\""sku\"": {\""name\"": \""premium\""}}"
```

### <a name="downgrade-a-domain-from-premium-to-basic"></a>Herabstufen einer Domäne aus Premium in Basic

```azurecli-interactive
az rest --method patch \
        --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" \
        --body "{\""sku\"": {\""name\"": \""basic\""}}"
```



## <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell
In diesem Abschnitt wird gezeigt, wie der Tarif eines Themas oder einer Domäne mit PowerShell geändert wird. 

### <a name="prepare-token-and-headers-for-rest-api-calls"></a>Vorbereiten von Token und Headern für REST-API-Aufrufe 
Führen Sie die folgenden erforderlichen Befehle aus, um ein Authentifizierungstoken zur Verwendung mit REST-API-Aufrufen und Autorisierungs- und andere Headerinformationen abzurufen. 

```azurepowershell-interactive
# replace <CLIENT ID> and <CLIENT SECRET>
$body = "grant_type=client_credentials&client_id=<CLIENT ID>&client_secret=<CLIENT SECRET>&resource=https://management.core.windows.net"

# get the authentication token. Replace <TENANT ID>
$Token = Invoke-RestMethod -Method Post `
    -Uri https://login.microsoftonline.com/<TENANT ID>/oauth2/token `
    -Body $body `
    -ContentType 'application/x-www-form-urlencoded'

# set authorization and content-type headers
$Headers = @{}
$Headers.Add("Authorization","$($Token.token_type) "+ " " + "$($Token.access_token)")
$Headers.Add("Content-Type","application/json")
```

### <a name="upgrade-a-topic-from-basic-to-premium"></a>Upgrade eines Themas aus Basic in Premium

```azurepowershell-interactive
$body = @{"sku"=@{"name"="premium"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="downgrade-a-topic-from-premium-to-basic"></a>Herabstufen eines Themas aus Premium in Basic

```azurepowershell-interactive
$body = @{"sku"=@{"name"="basic"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="upgrade-a-domain-from-basic-to-premium"></a>Upgrade einer Domäne aus Basic in Premium

```azurepowershell-interactive
$body = @{"sku"=@{"name"="premium"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="downgrade-a-domain-from-premium-to-basic"></a>Herabstufen einer Domäne aus Premium in Basic

```azurepowershell-interactive
$body = @{"sku"=@{"name"="basic"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/domains/<EVENT GRID DOMAIN NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

## <a name="next-steps"></a>Nächste Schritte
Für Themen und Domänen des Premium-Tarifs können Sie private Endpunkte konfigurieren, um den Zugriff auf ausgewählte virtuelle Netzwerke zu beschränken. Schrittweise Anleitungen dazu finden Sie unter [Konfigurieren privater Endpunkte](configure-private-endpoints.md).
