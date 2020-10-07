---
title: Übersicht über Azure Media Player
description: Weitere Informationen zu Azure Media Player
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 6f3c173c61e9f6d82a51fcaf7e11cda47eb47c1c
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "87011705"
---
# <a name="azure-media-player-overview"></a>Übersicht über Azure Media Player #

Azure Media Player ist ein Webvideoplayer für die Wiedergabe von Medieninhalten aus [Microsoft Azure Media Services](https://azure.microsoft.com/services/media-services/) über eine Vielzahl von Browsern und Geräten. Der Azure Media Player nutzt Industriestandards wie HTML5, MSE (Media Source Extensions) und EME (Encrypted Media Extensions) für ein funktionsreiches adaptives Streaming.  Wenn diese Standards auf einem Gerät oder in einem Browser nicht verfügbar sind, verwendet Azure Media Player Flash und Silverlight als Fallback-Technologien. Unabhängig von der verwendeten Wiedergabetechnologie müssen Entwickler eine einheitliche Schnittstelle für JavaScript für den Zugriff auf APIs verwenden.  Dies ermöglicht die Inhaltswiedergabe von Azure Media Services über eine breite Palette von Geräten und Browsern ohne jeglichen Zusatzaufwand.

Mit Microsoft Azure Media Services können Inhalte im DASH-, Smooth Streaming- und HLS-Format wiedergegeben werden. Der Azure Media Player erkennt diese unterschiedlichen Formate und übernimmt automatisch die beste Verknüpfung basierend auf den Funktionen von Plattform und Browser. Microsoft Azure Media Services ermöglicht auch die dynamische Verschlüsselung von Medienobjekten mit Common Encryption (PlayReady oder Widevine) oder AES-128-Bit-Umschlagverschlüsselung. Der Azure Media Player ermöglicht bei entsprechender Konfiguration die Entschlüsselung von PlayReady- und AES-128-Bit-verschlüsselten Inhalten.  Informationen zu dieser Konfiguration finden Sie im Abschnitt [Geschützter Inhalt](azure-media-player-protected-content.md).

Bringen Sie sich mit Anfragen nach neuen Features, Ideen oder Feedback auf der [UserVoice-Seite für Azure Media Player](https://aka.ms/ampuservoice) ein. Wenn Sie ein spezielles Problem oder eine Frage haben oder einen Fehler finden, wenden Sie sich unter ampinfo@microsoft.com an uns.

[Registrieren Sie sich](https://aka.ms/ampsignup), um kein Release mehr zu verpassen und stets über alle Neuigkeiten in Azure Media Player informiert zu sein.

> [!NOTE]
> Beachten Sie, dass Azure Media Player nur Mediendatenströme von Azure Media Services unterstützt.

## <a name="license"></a>Lizenz ##

Azure Media Player ist lizenziert und unterliegt den Bestimmungen der Microsoft-Software-Lizenzbedingungen für Azure Media Player. Die vollständigen Bestimmungen finden Sie in der [Lizenzdatei](/legal/azure-media-player/azure-media-player-license). Weitere Informationen finden Sie in den [Datenschutzbestimmungen](https://www.microsoft.com/en-us/privacystatement/default.aspx).

Copyright 2015 Microsoft Corporation

## <a name="next-steps"></a>Nächste Schritte ##

- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)
