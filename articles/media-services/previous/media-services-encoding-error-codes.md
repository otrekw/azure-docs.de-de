---
title: Fehlercodes bei der Azure Media Services-Codierung | Microsoft-Dokumentation
description: Dieses Thema enthält Fehlercodes, die zurückgegeben werden können, wenn bei der Ausführung einer Verschlüsselungsaufgabe ein Fehler auftritt.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: 7e6848fb49dd63fa67a639d09754a28dd5953a32
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103013481"
---
# <a name="encoding-error-codes"></a>Verschlüsselungsfehlercodes

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Die folgende Tabelle enthält die Fehlercodes, die zurückgegeben werden können, wenn bei der Ausführung einer Codieraufgabe ein Fehler auftritt.  Verwenden Sie zum Abrufen von Fehlerdetails im .NET-Code die [ErrorDetails](/previous-versions/azure/jj126075(v=azure.100)) -Klasse. Verwenden Sie zum Abrufen von Fehlerdetails im REST-Code die [ErrorDetail](/rest/api/media/operations/errordetail) -REST-API.

| ErrorDetail.Code | Mögliche Ursachen des Fehlers |
| --- | --- |
| Unknown |Unbekannter Fehler beim Ausführen der Aufgabe |
| ErrorDownloadingInputAssetMalformedContent |Kategorie von Fehlern, die Fehler beim Herunterladen von Eingabe-Assets abdeckt, z. B. fehlerhafte Dateinamen, Dateien mit Nulllänge, falsche Formate usw. |
| ErrorDownloadingInputAssetServiceFailure |Kategorie von Fehlern, die Probleme auf der Dienstseite abdeckt, z. B. Netzwerk- oder Speicherfehler beim Herunterladen. |
| ErrorParsingConfiguration |Kategorie von Fehlern, bei denen die Aufgabe \<see cref="MediaTask.PrivateData"/> (Konfiguration) nicht gültig ist, z.B. wenn die Konfiguration keine gültige Systemvoreinstellung ist oder ungültige XML-Daten enthält. |
| ErrorExecutingTaskMalformedContent |Kategorie von Fehlern während der Ausführung der Aufgabe, wenn Probleme in den Eingabemediendateien zu Fehlern führen. |
| ErrorExecutingTaskUnsupportedFormat |Kategorie von Fehlern, bei denen die bereitgestellten Dateien mit dem Medienprozessor nicht verarbeitet werden können: keine Unterstützung des Medienformats oder fehlende Übereinstimmung mit der Konfiguration. Beispiel: Der Versuch, eine Audioausgabe für ein Asset zu erstellen, die nur Video ermöglicht. |
| ErrorProcessingTask |Kategorie anderer Fehler, die für den Medienprozessor beim Verarbeiten der Aufgabe auftreten und sich nicht auf den Inhalt beziehen. |
| ErrorUploadingOutputAsset |Kategorie von Fehlern beim Hochladen des Ausgabe-Assets. |
| ErrorCancelingTask |Kategorie von Fehlern, die auftreten, wenn versucht wird, die Aufgabe abzubrechen. |
| TransientError |Kategorie von Fehlern, mit der vorübergehende Probleme abgedeckt werden (z.B. temporäre Netzwerkprobleme mit Azure Storage) |

Öffnen Sie ein **Supportticket** , um Hilfe vom [Media Services](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)-Team anzufordern.

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwandte Artikel
* [Ausführen von Aufgaben für die erweiterte Codierung durch Anpassen der Media Encoder Standard-Voreinstellungen](media-services-custom-mes-presets-with-dotnet.md)
* [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
