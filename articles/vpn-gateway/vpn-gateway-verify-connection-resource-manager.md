---
title: 'Azure VPN Gateway: Überprüfen einer Gatewayverbindung'
description: Dieser Artikel zeigt, wie Sie eine Verbindung eines virtuellen Netzwerks mit einem VPN Gateway überprüfen.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/19/2020
ms.author: cherylmc
ms.custom: devx-track-azurecli
ms.openlocfilehash: b59294d07ef64875cb6fbd3e3a49dec61d8b8135
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "94659402"
---
# <a name="verify-a-vpn-gateway-connection"></a>Überprüfen einer VPN Gateway-Verbindung

In diesem Artikel erfahren Sie, wie Sie eine VPN Gateway-Verbindung für das klassische und das Resource Manager-Bereitstellungsmodell überprüfen.

## <a name="azure-portal"></a>Azure-Portal

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

Installieren Sie die aktuelle Version der [Azure Resource Manager-PowerShell-Cmdlets](/powershell/azure/), um eine VPN Gateway-Verbindung für das Resource Manager-Bereitstellungsmodell mit PowerShell zu überprüfen.

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

Installieren Sie die aktuelle Version der [CLI-Befehle](/cli/azure/install-azure-cli) (2.0 oder höher), um eine VPN Gateway-Verbindung für das Resource Manager-Bereitstellungsmodell über die Azure-Befehlszeilenschnittstelle zu überprüfen.

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <a name="azure-portal-classic"></a>Azure-Portal (klassisch)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (klassisch)

Installieren Sie die aktuelle Version der Azure PowerShell-Cmdlets, um Ihre VPN Gateway-Verbindung für das klassische Bereitstellungsmodell mit PowerShell zu überprüfen. Laden Sie das Modul [Dienstverwaltung](/powershell/azure/servicemanagement/install-azure-ps?#azure-service-management-cmdlets) herunter, und installieren Sie es. Melden Sie sich mit „Add-AzureAccount“ beim klassischen Bereitstellungsmodell an.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Nächste Schritte

* Sie können Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Für diese Schritte finden Sie Informationen unter [Erstellen eines virtuellen Computers](../virtual-machines/windows/quick-create-portal.md) .