---
title: Includedatei
description: Includedatei
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 08/24/2020
ms.openlocfilehash: d8ee7b61897a9718d7078fc16eddc177fefbff43
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89147257"
---
 Azure Machine Learning-Computecluster unterstützen auch [verwaltete Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview), um den Zugriff auf Azure-Ressourcen zu authentifizieren, ohne dass Ihr Code Anmeldeinformationen enthalten muss. Es gibt zwei Arten von verwalteten Identitäten:

* Eine **systemseitig zugewiesene verwaltete Identität** wird direkt im Azure Machine Learning-Computecluster aktiviert. Der Lebenszyklus einer systemseitig zugewiesenen Identität ist direkt an den Computecluster gebunden. Wenn der Computecluster gelöscht wird, bereinigt Azure automatisch die Anmeldeinformationen und die Identität in Azure AD.
* Eine **benutzerseitig zugewiesene verwaltete Identität** ist eine eigenständige Azure-Ressource, die über den Dienst für verwaltete Azure-Identitäten bereitgestellt wird. Sie können eine benutzerseitig zugewiesene Identität mehreren Ressourcen zuweisen, und sie wird für die gewünschte Dauer beibehalten.