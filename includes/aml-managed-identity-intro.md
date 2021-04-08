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
ms.openlocfilehash: 70b636b7bb508b71475a7464983b091d5d10e0e1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "96026425"
---
 Azure Machine Learning-Computecluster unterstützen auch [verwaltete Identitäten](../articles/active-directory/managed-identities-azure-resources/overview.md), um den Zugriff auf Azure-Ressourcen zu authentifizieren, ohne dass Ihr Code Anmeldeinformationen enthalten muss. Es gibt zwei Arten von verwalteten Identitäten:

* Eine **systemseitig zugewiesene verwaltete Identität** wird direkt im Azure Machine Learning-Computecluster aktiviert. Der Lebenszyklus einer systemseitig zugewiesenen Identität ist direkt an den Computecluster gebunden. Wenn der Computecluster gelöscht wird, bereinigt Azure automatisch die Anmeldeinformationen und die Identität in Azure AD.
* Eine **benutzerseitig zugewiesene verwaltete Identität** ist eine eigenständige Azure-Ressource, die über den Dienst für verwaltete Azure-Identitäten bereitgestellt wird. Sie können eine benutzerseitig zugewiesene Identität mehreren Ressourcen zuweisen, und sie wird für die gewünschte Dauer beibehalten.