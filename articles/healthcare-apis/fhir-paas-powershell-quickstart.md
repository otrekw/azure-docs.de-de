---
title: 'Schnellstart: Bereitstellen von Azure API for FHIR mithilfe von PowerShell'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Azure API for FHIR mithilfe von PowerShell bereitstellen.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 10/15/2019
ms.author: mihansen
ms.openlocfilehash: d7156543a66cdf50d7cfddec27e685429324f9e6
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "84819997"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-powershell"></a>Schnellstart: Bereitstellen von Azure API for FHIR mithilfe von PowerShell

In dieser Schnellstartanleitung erfahren Sie, wie Sie Azure API for FHIR mithilfe von PowerShell bereitstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="register-the-azure-api-for-fhir-resource-provider"></a>Registrieren des Azure API for FHIR-Ressourcenanbieters

Falls der Ressourcenanbieter `Microsoft.HealthcareApis` nicht bereits für Ihr Abonnement registriert ist, können Sie ihn wie folgt registrieren:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.HealthcareApis
```

## <a name="create-azure-resource-group"></a>Erstellen einer Azure-Ressourcengruppe

```azurepowershell-interactive
New-AzResourceGroup -Name "myResourceGroupName" -Location westus2
```

## <a name="deploy-azure-api-for-fhir"></a>Bereitstellen von Azure API for FHIR

```azurepowershell-interactive
New-AzHealthcareApisService -Name nameoffhirservice -ResourceGroupName myResourceGroupName -Location westus2 -Kind fhir-R4
```

> [!NOTE]
> Abhängig von der von Ihnen installierten Version des `Az`-PowerShell-Moduls kann der bereitgestellte FHIR-Server für die Verwendung der [lokalen RBAC](configure-local-rbac.md) konfiguriert werden, und in der Liste der zulässigen Identitätsobjekt-IDs für den bereitgestellten FHIR-Dienst kann der derzeit angemeldete PowerShell-Benutzer festgelegt sein. Es wird empfohlen, künftig zum Zuweisen von Datenebenenrollen die [Azure RBAC zu verwenden](configure-azure-rbac.md). Unter Umständen müssen Sie diese Benutzerobjekt-ID nach der Bereitstellung löschen, um den Azure RBAC-Modus zu aktivieren.


## <a name="fetch-capability-statement"></a>Abrufen der Funktionsbestätigung

Sie können überprüfen, ob das Azure API for FHIR-Konto ausgeführt wird, indem Sie eine FHIR-Funktionsbestätigung abrufen:

```azurepowershell-interactive
$metadata = Invoke-WebRequest -Uri "https://nameoffhirservice.azurehealthcareapis.com/metadata"
$metadata.RawContent
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht mehr benötigen, löschen Sie die Ressourcengruppe wie folgt:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Azure API for FHIR in Ihrem Abonnement bereitgestellt. Wenn Sie zusätzliche Einstellungen in Azure API for FHIR festlegen möchten, fahren Sie mit der Schrittanleitung für zusätzliche Einstellungen fort.

>[!div class="nextstepaction"]
>[Zusätzliche Einstellungen in Azure API for FHIR](azure-api-for-fhir-additional-settings.md)
