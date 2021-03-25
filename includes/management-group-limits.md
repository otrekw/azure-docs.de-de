---
title: include file
description: include file
author: tfitzmac
ms.service: governance
ms.topic: include
ms.date: 03/26/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: cdcf6215973755444da9e513761de7ac71e479d4
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98738641"
---
| Resource | Begrenzung |
| --- | --- |
| Verwaltungsgruppen pro Azure AD-Mandant | 10.000 |
| Abonnements pro Verwaltungsgruppe | Unbegrenzt. |
| Ebenen der Verwaltungsgruppenhierarchie | Stammebene plus 6 Ebenen<sup>1</sup> |
| Direkt übergeordnete Verwaltungsgruppe pro Verwaltungsgruppe | Eine |
| [Bereitstellungen auf Verwaltungsgruppenebene](../articles/azure-resource-manager/templates/deploy-to-management-group.md) pro Standort | 800<sup>2</sup> |

<sup>1</sup>Bei den 6 Ebenen ist die Abonnementebene nicht enthalten.

<sup>2</sup>Wenn der Grenzwert von 800 Bereitstellungen erreicht ist, löschen Sie nicht mehr benötigte Bereitstellungen aus dem Verlauf. Zum Löschen von Bereitstellungen auf Verwaltungsgruppenebene verwenden Sie [Remove-AzManagementGroupDeployment](/powershell/module/az.resources/Remove-AzManagementGroupDeployment) oder [az deployment mg delete](/cli/azure/deployment/mg#az-deployment-mg-delete).
