---
title: include file
description: include file
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 12/12/2019
ms.custom: include file
ms.openlocfilehash: 6fa21d78d766ee8e541d6d60f64628b55a84fe41
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75467935"
---
> [!NOTE]
> Mehrteilige Anforderungen umfassen in der Regel drei Informationselemente:
> * Einen Header vom Typ **Content-Type**:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * Eine **Content-Disposition**:
>   * `form-data; name="metadata"`
> * Der hochzuladende Dateiinhalt
>
> **Content-Type** und **Content-Disposition** variieren je nach Verwendungsszenario.

Mehrteilige Anforderungen können programmgesteuert vorgenommen werden (über C#), über einen REST-Client oder Tools wie z.B. [Postman](https://docs.microsoft.com/azure/digital-twins/how-to-configure-postman#make-a-multipart-post-request). REST-Clienttools weisen möglicherweise verschiedene Ebenen der Unterstützung für komplexe mehrteiligen Anforderungen auf. Konfigurationseinstellungen können von Tool zu Tool leicht variieren. Überprüfen Sie, welches Tool am besten für Ihre Anforderungen geeignet ist.

> [!IMPORTANT]
> Mehrteilige Anforderungen an Verwaltungs-APIs von Azure Digital Twins bestehen normalerweise aus zwei Teilen:
> * Blobmetadaten (z.B. ein zugeordneter MIME-Typ), der von **Content-Type** und/oder **Content-Disposition** deklariert wird.
> * Blobinhalt einschließlich der unstrukturierten Inhalte einer hochzuladenden Datei
>
> Bei **PATCH**-Anforderungen wird keiner der beiden Teile benötigt. Bei **POST**- oder Erstellungsvorgängen werden dagegen beide Teile benötigt.

Der [Occupancy Quickstart-Quellcode](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/api/update.cs) enthält vollständige C#-Beispiele, die zeigen, wie mehrteilige Anforderungen an die Verwaltungs-APIs von digitalen Zwillingen in Azure gerichtet werden.