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
ms.openlocfilehash: bcc55156ca1d03614a4ff9767d6cf3f2603c06ca
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "96025488"
---
Fügen Sie Ihrer bevorzugten Entwicklungsumgebung mithilfe der folgenden Methoden Unterstützung hinzu.

| Entwicklungsumgebung  | Anwendungstyp      | So fügen Sie Unterstützung hinzu |
|--------------------------|-----------------------|----------------|
| Visual Studio            | C#-Klassenbibliothek      | [Installieren des NuGet-Pakets](../articles/azure-functions/functions-bindings-register.md#vs) |
| Visual Studio Code       | Basierend auf [Kerntools](../articles/azure-functions/functions-run-local.md) | [Registrieren des Erweiterungspakets](../articles/azure-functions/functions-bindings-register.md#extension-bundles)<br><br>Die Installation der [Azure Tools-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) wird empfohlen. |
| Beliebiger anderer Editor/IDE     | Basierend auf [Kerntools](../articles/azure-functions/functions-run-local.md) | [Registrieren des Erweiterungspakets](../articles/azure-functions/functions-bindings-register.md#extension-bundles) |
| Azure-Portal             | Nur online im Portal | Wird beim Hinzufügen einer Bindung installiert.<br /><br /> Informationen zum Aktualisieren vorhandener Bindungserweiterungen, ohne Ihre Funktions-App erneut veröffentlichen zu müssen, finden Sie unter [Aktualisieren Ihrer Erweiterungen](../articles/azure-functions/functions-bindings-register.md). |