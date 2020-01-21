---
title: include file
description: include file
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/05/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a27d44a7684b4ad3d39d7fba50ca52e08e932971
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75769766"
---
Mit der Azure Functions-Projektvorlage in Visual Studio wird ein Projekt erstellt, das in einer Funktions-App in Azure veröffentlicht werden kann. Sie können mit einer Funktions-App Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung, Skalierung und Freigabe von Ressourcen.

1. Wählen Sie in Visual Studio im Menü **Datei** die Optionen **Neu** > **Projekt** aus.

1. Suchen Sie im Dialogfeld **Neues Projekt erstellen** nach `functions`, und wählen Sie die Vorlage **Azure Functions** und dann **Weiter** aus.

1. Geben Sie einen Namen für Ihr Projekt ein, und wählen Sie **Erstellen** aus. Der Name der Funktions-App muss als C#-Namespace gültig sein, verwenden Sie daher keine Unterstriche, Bindestriche oder andere nicht alphanumerische Zeichen.

1. Verwenden Sie unter **Neue Azure Functions-Anwendung erstellen** die folgenden Optionen:

    + **Azure Functions v2 (.NET Core)**
    + **HTTP-Trigger**
    + **Storage Account** (Speicherkonto): **Speicheremulator**
    + **Autorisierungsstufe:** **Anonymous** 

    | Option      | Vorgeschlagener Wert  | Beschreibung                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Functions-Runtime** | **Azure Functions 2.x <br />(.NET Core)** | Mit dieser Einstellung wird ein Funktionsprojekt mit Version 2.x der Runtime von Azure Functions erstellt, die .NET Core unterstützt. Azure Functions 1.x unterstützt .NET Framework. Weitere Informationen finden Sie unter [Einstellen von Runtimeversionen von Azure Functions als Ziel](../articles/azure-functions/functions-versions.md).   |
    | **Funktionsvorlage** | **HTTP-Trigger** | Mit dieser Einstellung wird eine Funktion erstellt, die mit einer HTTP-Anforderung ausgelöst wird. |
    | **Speicherkonto**  | **Speicheremulator** | Für einen HTTP-Trigger wird die Azure Storage-Kontoverbindung nicht verwendet. Für alle anderen Triggertypen ist eine gültige Speicherkonto-Verbindungszeichenfolge erforderlich. Da für Functions ein Speicherkonto erforderlich ist, wird ein Konto zugewiesen oder erstellt, wenn Sie Ihr Projekt in Azure veröffentlichen. |
    | **Autorisierungsstufe** | **Anonym** | Die erstellte Funktion kann von jedem Client ausgelöst werden, ohne dass ein Schlüssel angegeben werden muss. Diese Autorisierungseinstellung erleichtert Ihnen das Testen Ihrer neuen Funktion. Weitere Informationen zu Schlüsseln und zur Autorisierung finden Sie unter [Autorisierungsschlüssel](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) im Artikel [HTTP- und Webhookbindungen in Azure Functions](../articles/azure-functions/functions-bindings-http-webhook.md). |
    
    > [!NOTE]
    > Stellen Sie sicher, dass **Autorisierungsstufe** auf `Anonymous` festgelegt ist. Bei Auswahl der Standardebene `Function` müssen Sie in Anforderungen für den Zugriff auf den Funktionsendpunkt den [Funktionsschlüssel](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) angeben.
    
4. Wählen Sie **Erstellen** aus, um das Funktionsprojekt und die per HTTP ausgelöste Funktion zu erstellen.
