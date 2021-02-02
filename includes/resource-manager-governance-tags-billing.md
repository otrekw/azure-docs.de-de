---
title: include file
description: include file
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 34388fe975df359c695b9358cafcd3ae711af0cf
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98900956"
---
Nachdem Sie Tags auf Ressourcen angewendet haben, können Sie die Kosten für Ressourcen mit diesen Tags anzeigen. Es dauert eine Weile, bis die Kostenanalyse die jüngste Nutzung anzeigt, daher werden möglicherweise noch keine Kosten angezeigt. Wenn die Kosten verfügbar sind, können Sie diese für Ressourcen in verschiedenen Ressourcengruppen Ihres Abonnements anzeigen. Benutzer müssen über [Zugriff auf Abrechnungsinformationen auf Abonnementebene](../articles/cost-management-billing/manage/manage-billing-access.md) verfügen, um die Kosten sehen zu können.

Um Kosten nach Tag im Portal anzuzeigen, wählen Sie Ihr Abonnement und danach **Kostenanalyse** aus.

![Kostenanalyse](./media/resource-manager-governance-tags-billing/select-cost-analysis.png)

Filtern Sie die Anzeige nach Tagwert, und wählen Sie **Anwenden** aus.

![Anzeigen von Kosten nach Tag](./media/resource-manager-governance-tags-billing/view-costs-by-tag.png)

Sie können auch die [Übersicht über die Azure-Nutzungs-API](../articles/cost-management-billing/manage/consumption-api-overview.md) verwenden, um Kosten programmgesteuert anzuzeigen.
