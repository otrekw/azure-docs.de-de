---
title: 'Azure CLI: Aktivieren des privaten Zugriffs mit Private Link (Vorschau)'
titleSuffix: Azure Digital Twins
description: In diesem Artikel erfahren Sie, wie Sie den privaten Zugriff für Azure Digital Twins-Lösungen mit Private Link über die Azure CLI aktivieren.
author: baanders
ms.author: baanders
ms.date: 02/09/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: c38db60c89b02e932c0a381daff1013b17008f54
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108208831"
---
# <a name="enable-private-access-with-private-link-preview-azure-cli"></a>Aktivieren des privaten Zugriffs mit Private Link (Vorschau): Azure CLI

[!INCLUDE [digital-twins-private-link-selector.md](../../includes/digital-twins-private-link-selector.md)]

In diesem Artikel werden verschiedene Methoden zum [Aktivieren von Private Link mit einem privaten Endpunkt für eine Azure Digital Twins-Instanz](concepts-security.md#private-network-access-with-azure-private-link-preview) beschrieben (befindet sich derzeit in der Vorschauphase). Das Konfigurieren eines privaten Endpunkts für Ihre Azure Digital Twins-Instanz ermöglicht es Ihnen, Ihre Azure Digital Twins-Instanz zu schützen und eine öffentliche Offenlegung zu vermeiden. Außerdem wird auch die Datenexfiltration aus Ihrer [Azure Virtual Network-Instanz (virtuelles Netzwerk)](../virtual-network/virtual-networks-overview.md) verhindert.

In diesem Artikel wird die Vorgehensweise mit der [Azure CLI](/cli/azure/what-is-azure-cli) beschrieben.

Die folgenden Schritte werden in diesem Artikel behandelt: 
1. Aktivieren Sie Private Link, und konfigurieren Sie einen privaten Endpunkt für eine Azure Digital Twins-Instanz.
1. Deaktivieren oder aktivieren Sie Zugriffsflags für das öffentliche Netzwerkzugriff, um den API-Zugriff ausschließlich auf Private Link-Verbindungen einzuschränken.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie einen privaten Endpunkt einrichten können, benötigen Sie eine [Azure Virtual Network-Instanz (virtuelles Netzwerk)](../virtual-network/virtual-networks-overview.md) , in dem der Endpunkt bereitgestellt werden kann. Wenn Sie noch nicht über ein virtuelles Netzwerk verfügen, können Sie einen [Schnellstart](../virtual-network/quick-create-portal.md) für Azure Virtual Network durchlaufen, um eines einzurichten.

## <a name="manage-private-endpoints-for-an-azure-digital-twins-instance"></a>Verwalten von privaten Endpunkten für eine Azure Digital Twins-Instanz 

Wenn Sie die [Azure CLI](/cli/azure/what-is-azure-cli) verwenden, können Sie private Endpunkte mit Private Link für eine bereits vorhandene Azure Digital Twins-Instanz einrichten (sie können nicht im Rahmen der Instanzerstellung hinzugefügt werden). Diese können Sie anschließend über zusätzliche CLI-Befehle anzeigen und verwalten. 

>[!TIP]
> Sie können einen Private Link-Endpunkt auch über den Private Link-Dienst einrichten, anstatt über Ihre Azure Digital Twins-Instanz. Dies bietet Ihnen dieselben Konfigurationsoptionen und dasselbe Endergebnis.
>
> Weitere Informationen zum Einrichten von Private Link-Ressourcen finden Sie in der Private Link-Dokumentation zum [Azure-Portal](../private-link/create-private-endpoint-portal.md), zur [Azure CLI](../private-link/create-private-endpoint-cli.md), zu [ARM-Vorlagen](../private-link/create-private-endpoint-template.md) oder [PowerShell](../private-link/create-private-endpoint-powershell.md).

### <a name="add-a-private-endpoint-to-an-existing-instance"></a>Hinzufügen eines privaten Endpunkts zu einer vorhandenen Instanz

Verwenden Sie den Befehl [az network private-endpoint create](/cli/azure/network/private-endpoint#az_network_private_endpoint_create), um einen privaten Endpunkt zu erstellen und diesen mit einer Azure Digital Twins-Instanz zu verknüpfen. Identifizieren Sie die Azure Digital Twins-Instanz mithilfe ihrer vollqualifizierten ID im Parameter `--private-connection-resource-id`.

Im folgenden Beispiel wird der Befehl zum Erstellen eines privaten Endpunkts mit nur den erforderlichen Parametern verwendet.

```azurecli-interactive
az network private-endpoint create --connection-name {private_link_service_connection} -n {name_for_private_endpoint} -g {resource_group} --subnet {subnet_ID} --private-connection-resource-id "/subscriptions/{subscription_ID}/resourceGroups/{resource_group}/providers/Microsoft.DigitalTwins/digitalTwinsInstances/{Azure_Digital_Twins_instance_name}" 
```

Eine vollständige Liste der erforderlichen und optionalen Parameter sowie weitere Beispiele zur Erstellung privater Endpunkte finden Sie in der Referenzdokumentation zu [az network private-endpoint create](/cli/azure/network/private-endpoint#az_network_private_endpoint_create).

### <a name="manage-private-endpoint-connections-on-the-instance"></a>Verwalten privater Endpunktverbindungen für die Instanz

Sobald ein privater Endpunkt für Ihre Azure Digital Twins-Instanz erstellt wurde, können Sie den Befehl [az dt network private-endpoint connection](/cli/azure/dt/network/private-endpoint/connection) verwenden, um mit der Verwaltung **privater Endpunktverbindungen** in Bezug auf die Instanz fortzufahren. Folgende Vorgänge sind möglich:
* Anzeigen einer privaten Endpunktverbindung
* Festlegen des Zustands der privaten Endpunktverbindung
* Löschen der privaten Endpunktverbindung
* Auflisten aller privaten Endpunktverbindungen für eine Instanz

Weitere Informationen und Beispiele finden Sie in der Referenzdokumentation zu [az dt network private-endpoint](/cli/azure/dt/network/private-endpoint).

### <a name="manage-other-private-link-information-on-an-azure-digital-twins-instance"></a>Verwalten anderer Private Link-Informationen zu einer Azure Digital Twins-Instanz

Sie können weitere Informationen zum Private Link-Status Ihrer Instanz mithilfe des Befehls [az dt network private-link](/cli/azure/dt/network/private-link) abrufen. Folgende Vorgänge sind möglich:
* Auflisten privater Verbindungen einer Azure Digital Twins-Instanz
* Anzeigen einer privaten Verbindung der Instanz

Weitere Informationen und Beispiele finden Sie in der Referenzdokumentation zu [az dt network private-link](/cli/azure/dt/network/private-link).

## <a name="disable--enable-public-network-access-flags"></a>Deaktivieren und Aktivieren von Zugriffsflags für das öffentliche Netzwerk

Sie können Ihre Azure Digital Twins-Instanz so konfigurieren, dass alle öffentlichen Verbindungen verweigert und nur Verbindungen über private Endpunkte zugelassen werden, um die Netzwerksicherheit zu erhöhen. Diese Aktion erfolgt mithilfe eines **Zugriffsflags für das öffentliche Netzwerk**. 

Mithilfe dieser Richtlinie können Sie den API-Zugriff auf Private Link-Verbindungen einschränken. Wenn das Zugriffsflag für das öffentliche Netzwerk auf *deaktiviert* festgelegt wird, geben alle REST-API-Aufrufe der Azure Digital Twins-Instanzdatenebene über die öffentliche Cloud `403, Unauthorized` zurück. Wenn die Richtlinie auf *deaktiviert* festgelegt ist und eine Anforderung über einen privaten Endpunkt erfolgt, ist der API-Aufruf erfolgreich.

In diesem Artikel wird gezeigt, wie der Wert des Netzwerkflags mithilfe der [Azure CLI](/cli/azure/) oder des [ARMClient-Befehlstools](https://github.com/projectkudu/ARMClient) geändert wird. Anweisungen zur Durchführung dieses Verfahrens über das Azure-Portal finden Sie in der [Version zum Azure-Portal](how-to-enable-private-link-portal.md) dieses Artikels.

### <a name="use-the-azure-cli"></a>Verwenden der Azure-CLI

In der Azure CLI können Sie den öffentlichen Netzwerkzugriff deaktivieren oder aktivieren, indem Sie einen `--public-network-access`-Parameter zum `az dt create`-Befehl hinzufügen. Obwohl dieser Befehl auch zum Erstellen einer neuen Instanz verwendet werden kann, können Sie ihn zum Bearbeiten der Eigenschaften einer vorhandenen Instanz verwenden, indem Sie den Namen einer bereits vorhandenen Instanz angeben. (Weitere Informationen zu diesem Befehl finden Sie in der zugehörigen [Referenzdokumentation](/cli/azure/dt#az_dt_create) oder in den [allgemeinen Anweisungen zum Einrichten einer Azure Digital Twins-Instanz](how-to-set-up-instance-cli.md#create-the-azure-digital-twins-instance).)

Verwenden Sie den Parameter `--public-network-access` wie folgt, um den öffentlichen Netzwerkzugriff für eine Azure Digital Twins-Instanz zu **deaktivieren**:

```azurecli-interactive
az dt create -n {name_of_existing_instance} -g {resource_group} --public-network-access Disabled
```

Verwenden Sie den folgenden ähnlichen Befehl, um den öffentlichen Netzwerkzugriff für eine Instanz zu **aktivieren**, für die er derzeit deaktiviert ist:

```azurecli-interactive
az dt create -n {name_of_existing_instance} -g {resource_group} --public-network-access Enabled
```

### <a name="usethe-armclientcommand-tool"></a>Verwenden des ARMClient-Befehlstools 

Mit dem [ARMClient-Befehlstool](https://github.com/projectkudu/ARMClient) können Sie den öffentlichen Netzwerkzugriff mithilfe der folgenden Befehle aktivieren oder deaktivieren. 

So **deaktivieren** Sie den öffentlichen Netzwerkzugriff:
  
```cmd/sh
armclient login 

armclient PATCH /subscriptions/<your-Azure-subscription-ID>/resourceGroups/<your-resource-group>/providers/Microsoft.DigitalTwins/digitalTwinsInstances/<your-Azure-Digital-Twins-instance>?api-version=2020-12-01 "{ 'properties': { 'publicNetworkAccess': 'disabled' } }"  
```

So **aktivieren** Sie den öffentlichen Netzwerkzugriff:  
  
```cmd/sh
armclient login 

armclient PATCH /subscriptions/<your-Azure-subscription-ID>/resourceGroups/<your-resource-group>/providers/Microsoft.DigitalTwins/digitalTwinsInstances/<your-Azure-Digital-Twins-instance>?api-version=2020-12-01 "{ 'properties': { 'publicNetworkAccess': 'enabled' } }"  
``` 

## <a name="next-steps"></a>Nächste Schritte

Hier erfahren Sie mehr über Private Link für Azure: 
* [Was ist der Azure Private Link-Dienst?](../private-link/private-link-service-overview.md)