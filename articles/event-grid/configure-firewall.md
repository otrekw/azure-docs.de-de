---
title: Konfigurieren der IP-Firewall für Azure Event Grid-Themen oder -Domänen (Vorschau)
description: In diesem Artikel wird beschrieben, wie Firewalleinstellungen für Azure Event Grid-Themen oder -Domänen konfiguriert werden.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: spelluru
ms.openlocfilehash: 0db6c2c346a6eb6ef016340fcfc2974c85958e6c
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858107"
---
# <a name="configure-ip-firewall-for-azure-event-grid-topics-or-domains-preview"></a>Konfigurieren der IP-Firewall für Azure Event Grid-Themen oder -Domänen (Vorschau)
Standardmäßig kann auf Themen und Domänen über das Internet zugegriffen werden, solange die Anforderung eine gültige Authentifizierung und Autorisierung aufweist. Mit der IP-Firewall können Sie den Zugriff auf eine Gruppe von IPv4-Adressen oder IPv4-Adressbereichen in [CIDR-Notation (Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) weiter einschränken. Herausgeber, die von einer anderen IP-Adresse stammen, werden abgelehnt und erhalten eine 403-Antwort (Verboten). Weitere Informationen zu Netzwerksicherheitsfunktionen, die von Event Grid unterstützt werden, finden Sie unter [Netzwerksicherheit für Event Grid](network-security.md).

In diesem Artikel wird beschrieben, wie IP-Firewalleinstellungen für Azure Event Grid-Themen oder -Domänen konfiguriert werden.

## <a name="use-azure-portal"></a>Verwenden des Azure-Portals
In diesem Abschnitt erfahren Sie, wie Sie mit dem Azure-Portal eingehende IP-Firewallregeln erstellen. Die in diesem Abschnitt beschriebenen Schritte beziehen sich auf Themen. Sie können ähnliche Schritte verwenden, um Regeln für eingehende IP-Adressen für **Domänen** zu erstellen. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Event Grid-Thema oder Ihrer -Domäne, und wechseln Sie zur Registerkarte **Netzwerk**.
2. Wählen Sie **Öffentliche Netzwerke** aus, um den Zugriff auf die Ressource durch das gesamte Netzwerk (einschließlich Internet) zuzulassen. 

    Sie können den Datenverkehr mithilfe von IP-basierten Firewallregeln einschränken. Geben Sie eine einzelne IPv4-Adresse oder einen Bereich von IP-Adressen in CIDR-Notation (Classless Inter-Domain Routing) an. 

    ![Seite „Öffentliche Netzwerke“](./media/configure-firewall/public-networks-page.png)
3. Wählen Sie **Nur private Endpunkte** aus, damit nur private Endpunktverbindungen auf diese Ressource zugreifen können. Verwenden Sie die Registerkarte **Private Endpunktverbindungen** auf dieser Seite, um Verbindungen zu verwalten. 

    ![Seite „Öffentliche Netzwerke“](./media/configure-firewall/private-endpoints-page.png)
4. Wählen Sie auf der Symbolleiste **Speichern** aus. 



## <a name="use-azure-cli"></a>Mithilfe der Azure-Befehlszeilenschnittstelle
In diesem Abschnitt wird gezeigt, wie Sie Azure CLI-Befehle verwenden, um Themen mit eingehenden IP-Regeln zu erstellen. Die in diesem Abschnitt beschriebenen Schritte beziehen sich auf Themen. Sie können ähnliche Schritte verwenden, um Regeln für eingehende IP-Adressen für **Domänen** zu erstellen. 


### <a name="prerequisites"></a>Voraussetzungen
Aktualisieren Sie die Azure Event Grid-Erweiterung für die CLI, indem Sie den folgenden Befehl ausführen: 

```azurecli-interactive
az extension update -n eventgrid
```

Wenn die Erweiterung nicht installiert ist, führen Sie den folgenden Befehl aus, um sie zu installieren: 

```azurecli-interactive
az extension add -n eventgrid
```

### <a name="enable-or-disable-public-network-access"></a>Aktivieren oder Deaktivieren des Zugriffs auf das öffentliche Netzwerk
Standardmäßig ist der öffentliche Netzwerkzugriff für Themen und Domänen aktiviert. Sie können ihn auch explizit aktivieren oder deaktivieren. Sie können den Datenverkehr einschränken, indem Sie eingehende IP-Firewallregeln konfigurieren. 

#### <a name="enable-public-network-access-while-creating-a-topic"></a>Aktivieren des öffentlichen Netzwerkzugriffs beim Erstellen eines Themas

```azurecli-interactive
az eventgrid topic create \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --public-network-access enabled
```


#### <a name="disable-public-network-access-while-creating-a-topic"></a>Deaktivieren des öffentlichen Netzwerkzugriffs beim Erstellen eines Themas

```azurecli-interactive
az eventgrid topic create \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --public-network-access disabled
```

> [!NOTE]
> Wenn öffentlicher Netzwerkzugriff für ein Thema oder eine Domäne deaktiviert ist, ist Datenverkehr über das öffentliche Internet unzulässig. Nur private Endpunktverbindungen dürfen auf diese Ressourcen zugreifen. 


#### <a name="enable-public-network-access-for-an-existing-topic"></a>Aktivieren öffentlichen Netzwerkzugriffs für ein vorhandenes Thema

```azurecli-interactive
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --public-network-access enabled 
```

#### <a name="disable-public-network-access-for-an-existing-topic"></a>Deaktivieren öffentlichen Netzwerkzugriffs für ein vorhandenes Thema 

```azurecli-interactive
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --public-network-access disabled
```

### <a name="create-a-topic-with-single-inbound-ip-rule"></a>Erstellen eines Themas mit einer einzelnen eingehenden IP-Regel
Der folgende CLI-Beispielbefehl erstellt ein Event Grid-Thema mit eingehenden IP-Regeln. 

```azurecli-interactive
az eventgrid topic create \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --public-network-access enabled \
    --inbound-ip-rules <IP ADDR or CIDR MASK> allow 
```

### <a name="create-a-topic-with-multiple-inbound-ip-rules"></a>Erstellen eines Themas mit mehreren eingehenden IP-Regeln

Der folgende CLI-Beispielbefehl erstellt ein Event Grid-Thema mit zwei eingehenden IP-Regeln in einem Schritt: 

```azurecli-interactive
az eventgrid topic create \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location \
    --public-network-access enabled \
    --inbound-ip-rules <IP ADDR 1 or CIDR MASK 1> allow \
    --inbound-ip-rules <IP ADDR 2 or CIDR MASK 2> allow
```

### <a name="update-an-existing-topic-to-add-inbound-ip-rules"></a>Aktualisieren eines vorhandenen Themas zum Hinzufügen eingehender IP-Regeln
In diesem Beispiel wird zunächst ein Event Grid-Thema erstellt, dann werden eingehende IP-Regeln für das Thema in einem separaten Befehl hinzugefügt. Außerdem werden die eingehenden IP-Regeln aktualisiert, die im zweiten Befehl festgelegt wurden. 

```azurecli-interactive

# create the event grid topic first
az eventgrid topic create \
    --resource-group $resourceGroupName \
    --name $topicName \
    --location $location

# add inbound IP rules to an existing topic
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --public-network-access enabled \
    --inbound-ip-rules <IP ADDR or CIDR MASK> allow

# later, update topic with additional ip rules
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --public-network-access enabled \
    --inbound-ip-rules <IP ADDR 1 or CIDR MASK 1> allow \
    --inbound-ip-rules <IP ADDR 2 or CIDR MASK 2> allow
```

### <a name="remove-an-inbound-ip-rule"></a>Entfernen eingehender IP-Regeln
Der folgende Befehl entfernt die zweite Regel, die Sie im vorherigen Schritt erstellt haben, indem Sie beim Aktualisieren der Einstellung nur die erste Regel angeben. 

```azurecli-interactive
az eventgrid topic update \
    --resource-group $resourceGroupName \
    --name $topicName \
    --public-network-access enabled \
    --inbound-ip-rules <IP ADDR 1 or CIDR MASK 1> allow
```


## <a name="use-powershell"></a>Verwenden von PowerShell
In diesem Abschnitt wird gezeigt, wie Sie Azure PowerShell-Befehle verwenden, um Azure Event Grid-Themen mit eingehenden IP-Firewallregeln zu erstellen. Die in diesem Abschnitt beschriebenen Schritte beziehen sich auf Themen. Sie können ähnliche Schritte verwenden, um Regeln für eingehende IP-Adressen für **Domänen** zu erstellen. 

### <a name="prerequisites"></a>Voraussetzungen
Befolgen Sie die Anweisungen unter [Vorgehensweise: Verwenden des Portals zum Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals, der auf Ressourcen zugreifen kann](../active-directory/develop/howto-create-service-principal-portal.md), um eine Azure Active Directory-Anwendung zu erstellen, und notieren Sie sich die folgenden Werte:

- Verzeichnis-ID (Mandant)
- Anwendungs-ID (Client)
- Anwendungsgeheimnis (Client)

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

### <a name="enable-public-network-access-for-an-existing-topic"></a>Aktivieren öffentlichen Netzwerkzugriffs für ein vorhandenes Thema
Standardmäßig ist der öffentliche Netzwerkzugriff für Themen und Domänen aktiviert. Sie können den Datenverkehr einschränken, indem Sie eingehende IP-Firewallregeln konfigurieren. 

```azurepowershell-interactive
$body = @{"properties"=@{"publicNetworkAccess"="enabled"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="disable-public-network-access-for-an-existing-topic"></a>Deaktivieren öffentlichen Netzwerkzugriffs für ein vorhandenes Thema
Wenn öffentlicher Netzwerkzugriff für ein Thema oder eine Domäne deaktiviert ist, ist Datenverkehr über das öffentliche Internet unzulässig. Nur private Endpunktverbindungen dürfen auf diese Ressourcen zugreifen. 

```azurepowershell-interactive
$body = @{"properties"=@{"publicNetworkAccess"="disabled"}} | ConvertTo-Json -Depth 5

Invoke-RestMethod -Method 'Patch' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body `
    | ConvertTo-Json -Depth 5
```

### <a name="create-an-event-grid-topic-with-inbound-rules-in-one-step"></a>Erstellen eines Event Grid-Themas mit eingehenden Regeln in einem Schritt

```azurepowershell-interactive

# prepare the body for the REST PUT method. Notice that inbound IP rules are included. 
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="basic"}; "properties"=@{"publicNetworkAccess"="enabled"; "inboundIpRules"=@(@{"ipmask"="<IP ADDR or CIDR MASK>";"action"="allow"})}} | ConvertTo-Json -Depth 5

# create the event grid topic with inbound IP rules
Invoke-RestMethod -Method 'Put' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body

# verify that the topic was created
Invoke-RestMethod -Method 'Get' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    | ConvertTo-Json -Depth 5
```


### <a name="create-event-grid-topic-first-and-then-add-inbound-ip-rules"></a>Erstellen des Event Grid-Themas und anschließendes Hinzufügen eingehender IP-Regeln

```azurepowershell-interactive

# prepare the body for the REST PUT method. Notice that no inbound IP rules are specified. 
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="basic"}; "properties"=@{"publicNetworkAccess"="enabled";}} | ConvertTo-Json -Depth 5

# create the Event Grid topic
Invoke-RestMethod -Method 'Put' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body`

# verify that the topic was created
Invoke-RestMethod -Method 'Get' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    | ConvertTo-Json -Depth 5

# prepare the body for REST PUT method. Notice that it includes inbound IP rules now. This feature is available in both basic and premium tiers.
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="basic"}; "properties"=@{"publicNetworkAccess"="enabled"; "inboundIpRules"=@(@{"ipmask"="<IP ADDR or CIDR MASK>";"action"="allow"}, @{"ipmask"="<IP ADDR or CIDR MASK>";"action"="allow"})}} | ConvertTo-Json -Depth 5

# update the topic with inbound IP rules
Invoke-RestMethod -Method 'Put' `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    -Body $body

# verify that the topic was updated
Invoke-RestMethod -Method 'Get' 
    -Uri "https://management.azure.com/subscriptions/<AzURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" `
    -Headers $Headers `
    | ConvertTo-Json -Depth 5

```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Überwachung von Ereignisübermittlungen finden Sie unter [Überwachen der Event Grid-Nachrichtenübermittlung](monitor-event-delivery.md).
* Weitere Informationen zum Authentifizierungsschlüssel finden Sie unter [Event Grid – Sicherheit und Authentifizierung](security-authentication.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).
