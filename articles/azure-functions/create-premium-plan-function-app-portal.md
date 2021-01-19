---
title: Erstellen eines Azure Functions-Premium-Plans im Portal
description: Erfahren Sie, wie Sie im Azure-Portal eine Funktions-App erstellen, die im Premium-Plan ausgeführt wird.
ms.topic: how-to
ms.date: 10/30/2020
ms.openlocfilehash: 20921423247dda3cbb39b58dcc805dac6d367390
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2021
ms.locfileid: "97937520"
---
# <a name="create-a-premium-plan-function-app-in-the-azure-portal"></a>Erstellen einer Funktions-App für den Premium-Plan im Azure-Portal

Azure Functions bietet einen skalierbaren Premium-Plan, der Konnektivität im virtuellen Netzwerk, Features zum Vermeiden von Kaltstarts und Premium-Hardware umfasst. Weitere Informationen finden Sie unter [Premium-Plan (Premium-Tarif) für Azure Functions](functions-premium-plan.md). 

In diesem Artikel erfahren Sie, wie Sie das Azure-Portal verwenden, um eine Funktions-App in einem Premium-Plan zu erstellen. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Sie müssen über eine Funktionen-App verfügen, die die Ausführung Ihrer Funktionen in Azure hostet. Sie können mit einer Funktions-App Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung, Skalierung und Freigabe von Ressourcen.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

An diesem Punkt können Sie Funktionen in der neuen Funktions-App erstellen. Diese Funktionen profitieren von den Vorteilen des [Premium-Plans](functions-premium-plan.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer durch HTTP ausgelösten Funktion](functions-create-first-azure-function.md#create-function)
