---
title: include file
description: include file
services: azure-resource-manager
author: tfitzmac
ms.service: cost-management-billing
ms.topic: include
ms.date: 02/10/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: b7db49d09d2292c08ce33ce86b1b7f427ef75fbc
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618478"
---
| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Abonnements pro Azure Active Directory-Mandant | Unbegrenzt. | Unbegrenzt. |
| [Co-Admins](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) pro Abonnement |Unbegrenzt. |Unbegrenzt. |
| [Ressourcengruppen](../articles/azure-resource-manager/management/overview.md) pro Abonnement |980 |980 |
| Anforderungsgröße für Azure Resource Manager-API |4\.194.304 Byte |4\.194.304 Byte |
| Tags pro Abonnement<sup>1</sup> |Unbegrenzt. |Unbegrenzt. |
| Eindeutige Tag-Berechnungen pro Abonnement<sup>1</sup> | 10.000 | 10.000 |
| [Bereitstellungen auf Abonnementebene](../articles/azure-resource-manager/templates/deploy-to-subscription.md) pro Standort | 800<sup>2</sup> | 800 |

<sup>1</sup>Sie können eine unbegrenzte Anzahl von Tags pro Abonnement anwenden. Die Anzahl von Tags pro Ressource oder Ressourcengruppe ist auf 50 beschränkt. Resource Manager gibt nur eine [Liste mit eindeutigen Tagnamen und Werten](/rest/api/resources/tags) im Abonnement zurück, wenn die Anzahl der Tags 10.000 oder weniger beträgt. Sie können eine Ressource jedoch auch anhand des Tags finden, wenn die Anzahl 10.000 überschreitet.  

<sup>2</sup>Wenn der Grenzwert von 800 Bereitstellungen erreicht ist, löschen Sie nicht mehr benötigte Bereitstellungen aus dem Verlauf. Um Bereitstellungen auf Abonnementebene zu löschen, verwenden Sie [Remove-AzDeployment](/powershell/module/az.resources/Remove-AzDeployment) oder [az deployment delete](/cli/azure/deployment?view=azure-cli-latest#az-deployment-delete).
