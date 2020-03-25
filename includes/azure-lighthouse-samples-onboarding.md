---
title: include file
description: include file
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 12/19/2019
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: e32b55ed655b1e47f85640eb7f494a89f3274667
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2020
ms.locfileid: "75456749"
---
Wir stellen verschiedene Vorlagen bereit, um bestimmte Onboardingszenarien zu behandeln. Wählen Sie die am besten geeignete Option aus, und achten Sie darauf, die Parameterdatei an Ihre Umgebung anzupassen. Weitere Informationen zur Verwendung dieser Dateien in der Bereitstellung finden Sie unter [Onboarding eines Kunden für delegierte Azure-Ressourcenverwaltung durchführen](../articles/lighthouse/how-to/onboard-customer.md).

| **Vorlage** | **Beschreibung** |
|---------|---------|
| [delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management) | Führt das Onboarding des Abonnements eines Kunden der delegierten Azure-Ressourcenverwaltung durch. Für jedes Abonnement muss eine separate Bereitstellung durchgeführt werden. |
| [rg-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management) | Führt das Onboarding einer oder mehrerer Ressourcengruppen eines Kunden für die delegierte Azure-Ressourcenverwaltung durch. Verwenden Sie **rgDelegatedResourceManagement**  für eine einzelne Ressourcengruppe oder **multipleRgDelegatedResourceManagement**, um das Onboarding für mehrere Ressourcengruppen im selben Abonnement durchzuführen. |
| [marketplace-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management) | Wenn Sie [ein Angebot für verwaltete Dienste im Azure Marketplace](../articles/lighthouse/how-to/publish-managed-services-offers.md) veröffentlicht haben, können Sie diese Vorlage optional zum Onboarding von Ressourcen für Kunden verwenden, die das Angebot angenommen haben. Die Marketplace-Werte in der Parameterdatei müssen mit den Werten identisch sein, die Sie beim Veröffentlichen Ihres Angebots verwendet haben. |

In der Regel ist für jedes Abonnement, für das ein Onboarding durchgeführt wird, eine separate Bereitstellung erforderlich. Sie können jedoch auch Vorlagen für mehrere Abonnements bereitstellen.

| **Vorlage** | **Beschreibung** |
|---------|---------|
| [cross-subscription-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/cross-subscription-deployment) | Stellt Azure Resource Manager-Vorlagen in mehreren Abonnements bereit. |
