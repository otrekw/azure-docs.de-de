---
title: Anzeigen oder Löschen von Daten – Custom Vision Service
titleSuffix: Azure Cognitive Services
description: Sie behalten die volle Kontrolle über Ihre Daten. In diesem Artikel wird erläutert, wie Sie Ihre Daten im Custom Vision Service anzeigen, exportieren oder löschen können.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: pafarley
ms.openlocfilehash: fe17fa4349085be47201974b418493fe8d912ece
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "90527393"
---
# <a name="view-or-delete-user-data-in-custom-vision"></a>Anzeigen oder Löschen von Benutzerdaten aus Custom Vision

Custom Vision sammelt Benutzerdaten, um den Dienst zu betreiben. Die Kunden haben jedoch über die [Training-API](https://go.microsoft.com/fwlink/?linkid=865446) von Custom Vision vollständige Kontrolle über die Anzeige und das Löschen ihrer Daten.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Weitere Informationen zum Anzeigen und Löschen von Benutzerdaten aus Custom Vision finden Sie in der folgenden Tabelle.

| Data | Anzeigevorgang | Löschvorgang |
| ---- | ---------------- | ---------------- |
| Kontoinformationen (Abonnementschlüssel) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Löschvorgänge werden über das Azure-Portal (Azure-Abonnements) durchgeführt. Alternativ kann auch die Schaltfläche „Ihr Konto löschen“ auf der Einstellungsseite für „CustomVision.ai“ (Microsoft-Kontoabonnements) verwendet werden. | 
| Details zur Iteration | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Details zur Iterationsleistung | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Liste der Iterationen | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Projekte und Projektdetails | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) und [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Imagetags | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) und [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Bilder | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (stellt URI für den Bilddownload bereit) und [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (stellt URI für Bilddownload bereit) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Exportierte Iterationen | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Wird bei Kontolöschung gelöscht. |
