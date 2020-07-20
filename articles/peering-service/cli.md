---
title: Registrieren einer Verbindung mit der Peering Service-Vorschauversion mithilfe der Azure CLI
description: Anleitung zum Registrieren einer Peering Service-Verbindung mithilfe der Azure CLI
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/2/2020
ms.author: derekol
ms.openlocfilehash: fbb9a6237aafe1c1b7c94d9dd23ad4c3665c0c73
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871460"
---
# <a name="register-a-peering-service-connection-by-using-the-azure-cli"></a>Registrieren einer Peering Service-Verbindung mithilfe der Azure CLI

Azure Peering Service ist ein Netzwerkdienst zur Verbesserung der Kundenkonnektivität mit Microsoft Cloud Services wie Office 365, Dynamics 365, SaaS-Diensten (Software-as-a-Service), Azure und anderen Microsoft-Diensten, auf die über das öffentliche Internet zugegriffen werden kann. In diesem Artikel erfahren Sie, wie Sie eine Peering Service-Verbindung mithilfe der Azure CLI registrieren.

Sollten Sie über kein Azure-Abonnement verfügen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, erfordert dieser Artikel mindestens die Azure CLI-Version 2.0.28. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Voraussetzungen 

Sie benötigen Folgendes:

### <a name="azure-account"></a>Azure-Konto

Sie müssen über ein gültiges und aktives Microsoft Azure-Konto verfügen. Dieses Konto wird zum Einrichten der Peering Service-Verbindung benötigt. Peering Service ist eine Ressource innerhalb von Azure-Abonnements.

### <a name="connectivity-provider"></a>Konnektivitätsanbieter

Sie können mit einem Internetdienst- oder Internet Exchange-Anbieter zusammenarbeiten, um Peering Service zu erhalten und eine Verbindung zwischen Ihrem Netzwerk und dem Microsoft-Netzwerk herzustellen.

Achten Sie darauf, dass es sich bei den Konnektivitätsanbietern um Microsoft-Partner handelt.

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.

Um mit der Konfiguration zu beginnen, melden Sie sich bei Ihrem Azure-Konto an. Wenn Sie das Cloud Shell-Testmodul verwenden, werden Sie automatisch angemeldet **.** Verwenden Sie die folgenden Beispiele, um eine Verbindung herzustellen.

```azurecli-interactive
az login
```

Überprüfen Sie die Abonnements für das Konto.

```azurecli-interactive
az account list
```

Wählen Sie das Abonnement aus, für das Sie die Peering Service-Verbindung registrieren möchten.

```azurecli-interactive
az account set --subscription "<subscription ID>"
```

Wenn Sie noch keine Ressourcengruppe besitzen, müssen Sie zuerst eine erstellen, bevor Sie Ihre Peering Service-Verbindung erstellen. Sie können mithilfe des folgenden Befehls eine Ressourcengruppe erstellen:

```azurecli-interactive
az group create -n MyResourceGroup -l "West US"
```

### <a name="2-register-your-subscription-with-the-resource-provider-and-feature-flag"></a>2. Registrieren des Abonnements mit dem Ressourcenanbieter und Featureflag

Registrieren Sie zunächst mithilfe der Azure CLI Ihr Abonnement mit dem Ressourcenanbieter und Featureflag, bevor Sie mit der Registrierung der Peering Service-Verbindung mithilfe der Azure CLI beginnen. Die Azure CLI-Befehle finden Sie hier:

```azurecli-interactive

az feature register --namespace Microsoft.Peering --name AllowPeeringService

```

### <a name="3-register-the-peering-service-connection"></a>3. Registrieren der Peering Service-Verbindung

Registrieren Sie die Peering Service-Verbindung mithilfe der folgenden Befehle über die Azure CLI. In diesem Beispiel wird eine Peering Service-Verbindung namens „myPeeringService“ registriert.

```azurecli-interactive
az peering service create : Create peering service\
  --location -l \
  --name myPeeringService\
  --resource-group -g MyResourceGroup\
  --peering-service-location\
  --peering-service-provider\
  --tags
```

### <a name="4-register-the-prefix"></a>4. Präfix registrieren

Führen Sie die folgenden Befehle über die Azure CLI aus, um das vom Konnektivitätsanbieter bereitgestellte Präfix zu registrieren. In diesem Beispiel wird das Präfix „myPrefix“ registriert.

```azurecli-interactive
az peering service prefix create \
  --name  myPrefix\
  --peering-service-name myPeeringService\
  --resource-group  -g myResourceGroup\
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Peering Service-Verbindung finden Sie unter [Peering Service-Verbindung](connection.md).
- Informationen zur Peering Service-Verbindungstelemetrie finden Sie unter [Peering Service-Verbindungstelemetrie](connection-telemetry.md).
- Informationen zum Messen der Telemetrie finden Sie unter [Messen der Verbindungstelemetrie](measure-connection-telemetry.md).
- Informationen zum Registrieren der Verbindung mithilfe von Azure PowerShell finden Sie unter [Registrieren einer Peering Service-Verbindung – Azure PowerShell](powershell.md).
- Informationen zum Registrieren der Verbindung über das Azure-Portal finden Sie unter [Registrieren einer Peering Service-Verbindung – Azure-Portal](azure-portal.md).
