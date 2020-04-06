---
title: Konfigurieren der IP-Firewall für Azure Event Grid-Themen oder -Domänen (Vorschau)
description: In diesem Artikel wird beschrieben, wie Firewalleinstellungen für Azure Event Grid-Themen oder -Domänen konfiguriert werden.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: spelluru
ms.openlocfilehash: b195872ca1002970fa96ae133d5eb47a9267796d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79300509"
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


### <a name="enable-public-network-access-for-an-existing-topic"></a>Aktivieren öffentlichen Netzwerkzugriffs für ein vorhandenes Thema
Standardmäßig ist der öffentliche Netzwerkzugriff für Themen und Domänen aktiviert. Sie können den Datenverkehr einschränken, indem Sie eingehende IP-Firewallregeln konfigurieren. 

```azurecli-interactive
az rest --method patch --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" --body "{\""properties\"": {\""publicNetworkAccess\"": \""Enabled\""}}"
```

### <a name="disable-public-network-access-for-an-existing-topic"></a>Deaktivieren öffentlichen Netzwerkzugriffs für ein vorhandenes Thema
Wenn öffentlicher Netzwerkzugriff für ein Thema oder eine Domäne deaktiviert ist, ist Datenverkehr über das öffentliche Internet unzulässig. Nur private Endpunktverbindungen dürfen auf diese Ressourcen zugreifen. 

```azurecli-interactive
az rest --method patch --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" --body "{\""properties\"": {\""publicNetworkAccess\"": \""Disabled\""}}"
```

### <a name="create-topic-with-inbound-ip-rules"></a>Erstellen eines Themas mit eingehenden IP-Regeln
Der folgende CLI-Beispielbefehl erstellt ein Event Grid-Thema mit eingehenden IP-Regeln in einem Schritt. 

```azurecli-interactive
az rest --method put \
    --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" \
    --body {\""location\"":\""<LOCATION>\", \""properties\"" :{\""publicNetworkAccess\"":\""enabled\"",\""InboundIpRules\"": [ {\""ipMask\"": \""<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>\"", \""action\"": \""allow\""} ]}}
```

### <a name="create-topic-first-and-then-add-inbound-ip-rules"></a>Erstellen des Themas und anschließendes Hinzufügen eingehender IP-Regeln
In diesem Beispiel wird zunächst ein Event Grid-Thema erstellt, dann werden eingehende IP-Regeln für das Thema in einem separaten Befehl hinzugefügt. Außerdem werden die eingehenden IP-Regeln aktualisiert, die im zweiten Befehl festgelegt wurden. 

```azurecli-interactive

# create the event grid topic first
az rest --method put \
    --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" \
    --body {\""location\"":\""<LOCATION>\""}

# add inbound IP rules
az rest --method put \
    --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" 
    --body {\""location\"":\""<LOCATION>\", \""properties\"" :{\""publicNetworkAccess\"":\""enabled\"", \""InboundIpRules\"": [ {\""ipMask\"": \""<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>\"", \""action\"": \""allow\""} ]}}

# later, update topic with additional ip rules or remove them. 
az rest --method put \
    --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview" 
    --body {\""location\"":\""<LOCATION>\", \""properties\"" :{\""publicNetworkAccess\"":\""enabled\"", \""InboundIpRules\"": [ {\""ipMask\"": \""<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>\"", \""action\"": \""allow\""}, {\""ipMask\"": \""<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>\"", \""action\"": \""allow\""} ]}}
```


## <a name="use-powershell"></a>Verwenden von PowerShell
In diesem Abschnitt wird gezeigt, wie Sie Azure PowerShell-Befehle verwenden, um Azure Event Grid-Themen mit eingehenden IP-Firewallregeln zu erstellen. Die in diesem Abschnitt beschriebenen Schritte beziehen sich auf Themen. Sie können ähnliche Schritte verwenden, um Regeln für eingehende IP-Adressen für **Domänen** zu erstellen. 

### <a name="prerequisite"></a>Voraussetzung
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
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="basic"}; "properties"=@{"publicNetworkAccess"="enabled"; "inboundIpRules"=@(@{"ipmask"="<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>";"action"="allow"})}} | ConvertTo-Json -Depth 5

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

# prepare the body for REST PUT method. Notice that it includes inbound IP rules now. This feature available in both basic and premium tiers.
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="basic"}; "properties"=@{"publicNetworkAccess"="enabled"; "inboundIpRules"=@(@{"ipmask"="<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>";"action"="allow"}, @{"ipmask"="<IP ADDRESS or IP ADDRESS RANGE in CIDR notation>";"action"="allow"})}} | ConvertTo-Json -Depth 5

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
