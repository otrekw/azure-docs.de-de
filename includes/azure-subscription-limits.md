---
title: include file
description: include file
author: rothja
ms.service: azure-resource-manager
ms.topic: include
ms.date: 05/18/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 74f9c5304237ec77a629bc914d95c345882b784f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "95562107"
---
| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| vCPUs pro [Abonnement](https://azure.microsoft.com/pricing/)<sup>1</sup> |20 |10.000 |
| [Co-Admins](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) pro Abonnement |200 |200 |
| [Speicherkonten](../articles/storage/common/storage-account-create.md) pro Abonnement<sup>2</sup> |100 |100 |
| [Clouddienste](../articles/cloud-services/cloud-services-choose-me.md) pro Abonnement |20 |200 |
| [Lokale Netzwerke](/previous-versions/azure/reference/jj157100(v=azure.100)) pro Abonnement |10 |500 |
| DNS-Server pro Abonnement |9 |100 |
| Reservierte IPs pro Abonnement |20 |100 |
| [Affinitätsgruppen](/previous-versions/azure/virtual-network/virtual-networks-migrate-to-regional-vnet) pro Abonnement |256 |256 |
| Länge des Abonnementnamens (Anzahl der Zeichen) | 64 | 64 |

<sup>1</sup> Sehr kleine Instanzen zählen als ganze vCPU im Sinn des vCPU-Grenzwerts, auch wenn nur ein Teil eines CPU-Kerns verwendet wird.

<sup>2</sup> Das Speicherkontolimit umfasst sowohl Standard- als auch Premium-Speicherkonten.