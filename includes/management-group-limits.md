---
title: include file
description: include file
author: tfitzmac
ms.service: governance
ms.topic: include
ms.date: 03/26/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 53e3f37d14153f3a2d7b5886a49b08ca9052b128
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107800062"
---
| Resource | Begrenzung |
| --- | --- |
| Verwaltungsgruppen pro Azure AD-Mandant | 10.000 |
| Abonnements pro Verwaltungsgruppe | Unbegrenzt. |
| Ebenen der Verwaltungsgruppenhierarchie | Stammebene plus 6 Ebenen<sup>1</sup> |
| Direkt übergeordnete Verwaltungsgruppe pro Verwaltungsgruppe | Eine |
| [Bereitstellungen auf Verwaltungsgruppenebene](../articles/azure-resource-manager/templates/deploy-to-management-group.md) pro Standort | 800<sup>2</sup> |

<sup>1</sup>Bei den 6 Ebenen ist die Abonnementebene nicht enthalten.

<sup>2</sup>Wenn der Grenzwert von 800 Bereitstellungen erreicht ist, löschen Sie nicht mehr benötigte Bereitstellungen aus dem Verlauf. Zum Löschen von Bereitstellungen auf Verwaltungsgruppenebene verwenden Sie [Remove-AzManagementGroupDeployment](/powershell/module/az.resources/Remove-AzManagementGroupDeployment) oder [az deployment mg delete](/cli/azure/deployment/mg#az_deployment_mg_delete).
