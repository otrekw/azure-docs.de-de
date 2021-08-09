---
title: 'Schnellstart: Azure Communication Services-API zur Anrufaufzeichnung'
titleSuffix: An Azure Communication Services quickstart document
description: Enthält ein Schnellstartbeispiel für die Anfrufaufzeichnungs-APIs.
author: ravithanneeru
manager: joseys
services: azure-communication-services
ms.author: joseys
ms.date: 06/30/2021
ms.topic: overview
ms.service: azure-communication-services
zone_pivot_groups: acs-csharp-java
ms.openlocfilehash: f4dfd0701c2eeeaa5ba3b2be20b9448ca2093601
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113111822"
---
# <a name="call-recording-api-quickstart"></a>Schnellstart: Anrufaufzeichnungs-API
In diesem Schnellstart erfahren Sie, wie Sie Sprach- und Videoanrufe aufzeichnen. Dabei wird davon ausgegangen, dass Sie bereits das [Anrufclient-SDK](get-started-with-video-calling.md) für die Einrichtung von Anruffunktionen für Endbenutzern verwendet haben. Mithilfe der **Anfrufserver-APIs und -SDKs** können Sie Aufzeichnungen aktivieren und verwalten. 

::: zone pivot="programming-language-csharp"
[!INCLUDE [Build Call Recording server sample with C#](./includes/call-recording-samples/recording-server-csharp.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Build Call Recording server sample with Java](./includes/call-recording-samples/recording-server-java.md)]
::: zone-end

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Communication Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind. Weitere Informationen zum Bereinigen von Ressourcen finden Sie [hier](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

- Sehen Sie sich unser [Hero-Beispiel für Anrufe](../../samples/calling-hero-sample.md) an.
- Informieren Sie sich über die [Funktionen des Calling SDK](./calling-client-samples.md).
- Informieren Sie sich über die [Funktionsweise von Anrufen](../../concepts/voice-video-calling/about-call-types.md).
