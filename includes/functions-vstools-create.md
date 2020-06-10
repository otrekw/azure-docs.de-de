---
title: include file
description: include file
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/26/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 4e56063891a92ef06e3e14651e26f0b73d280e79
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84231469"
---
Mit der Azure Functions-Projektvorlage in Visual Studio wird ein Projekt erstellt, das Sie in einer Funktions-App in Azure veröffentlichen können. Sie können mit einer Funktions-App Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung, Skalierung und Freigabe von Ressourcen.

1. Wählen Sie im Visual Studio-Menü **Datei** > **Neu** > **Projekt** aus.

1. Geben Sie unter **Neues Projekt erstellen** den Suchbegriff *functions* in das Suchfeld ein, und wählen Sie die Vorlage **Azure Functions** und anschließend **Weiter** aus.

1. Geben Sie unter **Neues Projekt konfigurieren** im Feld **Projektname** einen Namen für Ihr Projekt ein, und wählen Sie anschließend **Erstellen** aus. Der Name der Funktions-App muss als C#-Namespace gültig sein, verwenden Sie daher keine Unterstriche, Bindestriche oder andere nicht alphanumerische Zeichen.

1. Verwenden Sie für die Einstellungen zum **Erstellen einer neuen Azure-Funktionsanwendung** die Werte in der folgenden Tabelle:

    | Einstellung      | Wert  | BESCHREIBUNG                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Functions-Runtime** | **Azure Functions v3 <br />(.NET Core)** | Durch diesen Wert wird ein Funktionsprojekt mit Version 3.x der Runtime von Azure Functions erstellt, die .NET Core 3.x unterstützt. Azure Functions 1.x unterstützt .NET Framework. Weitere Informationen finden Sie unter [Einstellen von Runtimeversionen von Azure Functions als Ziel](../articles/azure-functions/functions-versions.md).   |
    | **Funktionsvorlage** | **HTTP-Trigger** | Mit diesem Wert wird eine Funktion erstellt, die durch eine HTTP-Anforderung ausgelöst wird. |
    | **Speicherkonto**  | **Speicheremulator** | Da für eine Azure-Funktion ein Speicherkonto erforderlich ist, wird ein Speicherkonto zugewiesen oder erstellt, wenn Sie Ihr Projekt in Azure veröffentlichen. Von HTTP-Triggern wird keine Azure Storage-Kontoverbindungszeichenfolge verwendet. Für alle anderen Triggertypen ist dagegen eine gültige Azure Storage-Kontoverbindungszeichenfolge erforderlich.  |
    | **Zugriffsrechte** | **Anonym** | Die erstellte Funktion kann von jedem Client ausgelöst werden, ohne dass ein Schlüssel angegeben werden muss. Diese Autorisierungseinstellung erleichtert Ihnen das Testen Ihrer neuen Funktion. Weitere Informationen zu Schlüsseln und zur Autorisierung finden Sie unter [Autorisierungsschlüssel](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) sowie in der [Übersicht über HTTP-Trigger und -Bindungen in Azure Functions](../articles/azure-functions/functions-bindings-http-webhook.md). |
    
    
    ![Azure Functions-Projekteinstellungen](./media/functions-vs-tools-create/functions-project-settings.png)

    Legen Sie die **Zugriffsrechte** auf **Anonym** fest. Bei Verwendung der Standardebene **Funktion** muss in Anforderungen für den Zugriff auf den Funktionsendpunkt der [Funktionsschlüssel](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) angegeben werden.

1. Wählen Sie **Erstellen** aus, um das Funktionsprojekt und die HTTP-Triggerfunktion zu erstellen.
