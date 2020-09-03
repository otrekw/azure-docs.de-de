---
title: include file
description: include file
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 08/24/2020
ms.openlocfilehash: d8ee7b61897a9718d7078fc16eddc177fefbff43
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2020
ms.locfileid: "89147257"
---
 Azure Machine Learning-Computecluster unterstützen auch [verwaltete Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview), um den Zugriff auf Azure-Ressourcen zu authentifizieren, ohne dass Ihr Code Anmeldeinformationen enthalten muss. Es gibt zwei Arten von verwalteten Identitäten:

* Eine **systemseitig zugewiesene verwaltete Identität** wird direkt im Azure Machine Learning-Computecluster aktiviert. Der Lebenszyklus einer systemseitig zugewiesenen Identität ist direkt an den Computecluster gebunden. Wenn der Computecluster gelöscht wird, bereinigt Azure automatisch die Anmeldeinformationen und die Identität in Azure AD.
* Eine **benutzerseitig zugewiesene verwaltete Identität** ist eine eigenständige Azure-Ressource, die über den Dienst für verwaltete Azure-Identitäten bereitgestellt wird. Sie können eine benutzerseitig zugewiesene Identität mehreren Ressourcen zuweisen, und sie wird für die gewünschte Dauer beibehalten.