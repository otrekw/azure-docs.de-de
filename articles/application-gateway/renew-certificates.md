---
title: Verlängern des Azure Application Gateway-Zertifikats
description: Es wird beschrieben, wie Sie ein Zertifikat verlängern, das einem Anwendungsgateway-Listener zugeordnet ist.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: how-to
ms.date: 01/20/2021
ms.author: victorh
ms.openlocfilehash: f0c06a94498f4d2481a6e953b959d766c60415fb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98622179"
---
# <a name="renew-application-gateway-certificates"></a>Verlängern von Application Gateway-Zertifikaten

Sie müssen Zertifikate nach einiger Zeit verlängern, falls Sie für Ihr Anwendungsgateway die TLS/SSL-Verschlüsselung konfiguriert haben.

Sie können ein Zertifikat, das einem Listener zugeordnet ist, verlängern, indem Sie entweder das Azure-Portal, Azure PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden:

## <a name="azure-portal"></a>Azure-Portal

Navigieren Sie zu Ihren Anwendungsgateway-Listenern, um ein Listenerzertifikat über das Portal zu verlängern. Wählen Sie den Listener aus, der über ein zu verlängerndes Zertifikat verfügt, und wählen Sie anschließend **Ausgewähltes Zertifikat erneuern oder bearbeiten** aus.

:::image type="content" source="media/renew-certificate/ssl-cert.png" alt-text="Verlängern eines Zertifikats":::

Laden Sie Ihr neues PFX-Zertifikat hoch, geben Sie ihm einen Namen, geben Sie das Kennwort ein, und wählen Sie dann **Speichern** aus.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Verwenden Sie das folgende Skript, um Ihr Zertifikat mit Azure PowerShell zu verlängern:

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-AzApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzApplicationGateway -ApplicationGateway $appgw
```
## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren der TLS-Abladung mit Azure Application Gateway finden Sie unter [Konfigurieren der TLS-Abladung](./create-ssl-portal.md).