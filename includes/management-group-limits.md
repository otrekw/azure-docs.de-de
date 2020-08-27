---
title: include file
description: include file
author: tfitzmac
ms.service: governance
ms.topic: include
ms.date: 03/26/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 204ca121045d1bd53fd507acc7ea9691bfeac12e
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88748848"
---
| Resource | Begrenzung |
| --- | --- |
| Verwaltungsgruppen pro Azure AD-Mandant | 10.000 |
| Abonnements pro Verwaltungsgruppe | Unbegrenzt. |
| Ebenen der Verwaltungsgruppenhierarchie | Stammebene plus 6 Ebenen<sup>1</sup> |
| Direkt übergeordnete Verwaltungsgruppe pro Verwaltungsgruppe | Eine |
| [Bereitstellungen auf Verwaltungsgruppenebene](../articles/azure-resource-manager/templates/deploy-to-management-group.md) pro Standort | 800<sup>2</sup> |

<sup>1</sup>Bei den 6 Ebenen ist die Abonnementebene nicht enthalten.

<sup>2</sup>Wenn der Grenzwert von 800 Bereitstellungen erreicht ist, löschen Sie nicht mehr benötigte Bereitstellungen aus dem Verlauf. Zum Löschen von Bereitstellungen auf Verwaltungsgruppenebene verwenden Sie [Remove-AzManagementGroupDeployment](/powershell/module/az.resources/Remove-AzManagementGroupDeployment) oder [az deployment mg delete](/cli/azure/deployment/mg?view=azure-cli-latest#az-deployment-mg-delete).
