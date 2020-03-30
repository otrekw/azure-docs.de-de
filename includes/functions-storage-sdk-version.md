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
ms.openlocfilehash: 6a37850eb6536c5399d63144e60ea210fbc194d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77198416"
---
#### <a name="azure-storage-sdk-version-in-functions-1x"></a>Azure Storage SDK-Version in Functions 1.x

In Functions 1.x verwenden die Storage-Trigger und -Bindungen Version 7.2.1 des Azure Storage SDK (NuGet-Paket [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1)). Wenn Sie auf eine andere Version des Storage SDK verweisen und in Ihrer Funktionssignatur an einen Storage SDK-Typ binden, meldet die Functions-Runtime möglicherweise, dass Bindungen an diesen Typ nicht möglich sind. Die Lösung besteht darin, sicherzustellen, dass Ihr Projekt auf [WindowsAzure.Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) verweist.
