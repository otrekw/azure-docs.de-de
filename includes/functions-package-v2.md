---
title: include file
description: include file
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 01/28/2020
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 2eb82eb8b4a64cbbfef91539f46ba084aa73f207
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77205711"
---
Fügen Sie Ihrer bevorzugten Entwicklungsumgebung mithilfe der folgenden Methoden Unterstützung hinzu.

| Entwicklungsumgebung  | Anwendungstyp      | So fügen Sie Unterstützung hinzu |
|--------------------------|-----------------------|----------------|
| Visual Studio            | C#-Klassenbibliothek      | [Installieren des NuGet-Pakets](../articles/azure-functions/functions-bindings-register.md#vs) |
| Visual Studio Code       | Basierend auf [Kerntools](../articles/azure-functions/functions-run-local.md) | [Registrieren des Erweiterungspakets](../articles/azure-functions/functions-bindings-register.md#extension-bundles)<br><br>Die Installation der [Azure Tools-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) wird empfohlen. |
| Beliebiger anderer Editor/IDE     | Basierend auf [Kerntools](../articles/azure-functions/functions-run-local.md) | [Registrieren des Erweiterungspakets](../articles/azure-functions/functions-bindings-register.md#extension-bundles) |
| Azure-Portal             | Nur online im Portal | Wird beim Hinzufügen einer Bindung installiert.<br /><br /> Informationen zum Aktualisieren vorhandener Bindungserweiterungen, ohne Ihre Funktions-App erneut veröffentlichen zu müssen, finden Sie unter [Aktualisieren Ihrer Erweiterungen](../articles/azure-functions/install-update-binding-extensions-manual.md). |
