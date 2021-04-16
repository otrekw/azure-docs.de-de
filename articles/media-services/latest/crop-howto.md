---
title: 'Zuschneiden von Videodateien mit Media Services: .NET | Microsoft Docs'
description: Zuschneiden ist der Prozess der Auswahl eines rechteckigen Fensters innerhalb des Videoframes und die ausschließliche Codierung der Pixel innerhalb dieses Fensters. In diesem Thema wird gezeigt, wie Videodateien mit Media Services mithilfe von .NET zugeschnitten werden.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: how-to
ms.date: 03/24/2021
ms.author: inhenkel
ms.openlocfilehash: 43d0e1e3b8c0eaa37a3b09557b333c3abafe8b63
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105572226"
---
# <a name="how-to-crop-video-files-with-media-services---net"></a>Zuschneiden von Videodateien mit Media Services: .NET

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Mit Media Services können Sie ein Eingabevideo zuschneiden. Zuschneiden ist der Prozess der Auswahl eines rechteckigen Fensters innerhalb des Videoframes und die ausschließliche Codierung der Pixel innerhalb dieses Fensters. Das folgende Diagramm veranschaulicht diesen Prozess.

## <a name="pre-processing-stage"></a>Vorab verarbeitetes Skript

Das Zuschneiden ist eine Vorverarbeitungsphase, weshalb die *Zuschneideparameter* in der Codierungsvoreinstellung für das ursprüngliche *Eingabevideo* gelten. Die Codierung ist eine nachfolgende Phase, weshalb die Einstellungen für Breite und Höhe für das *vorverarbeitete Video* und nicht für das Originalvideo gelten. Gehen Sie beim Entwerfen der Voreinstellung folgendermaßen vor:

1. Wählen Sie die Zuschneideparameter basierend auf dem ursprünglichen Eingabevideo aus.
1. Wählen Sie die Codiereinstellungen basierend auf dem zugeschnittenen Video aus.

> [!WARNING]
> Wenn Ihre Codierungseinstellungen nicht mit dem zugeschnittenen Video übereinstimmen, ist die Ausgabe nicht wie erwartet.

Angenommen Sie, Ihr Eingabevideo hat eine Auflösung von 1920 x 1080 Pixel (Seitenverhältnis 16:9), aber links und rechts schwarze Balken (sog. Pillarboxes), weshalb nur ein Fenster mit dem Seitenverhältnis 4:3 oder 1440 x 1080 Pixeln ein aktives Video enthält. Sie können die schwarzen Balken wegschneiden und den 1440 x 1080-Bereich kodieren.

## <a name="transform-code"></a>Code transformieren

Der folgende Codeausschnitt veranschaulicht, wie Sie eine Transformation in .NET schreiben können, um Videos zu erstellen.  Der Code setzt voraus, dass Sie über eine lokale Datei verfügen, mit der Sie arbeiten können.

- Links ist die linke Position des Zuschnitts.
- Oben ist die oberste Position des Zuschnitts.
- Breite ist die endgültige Breite des Zuschnitts.
- Height ist die endgültige Höhe des Zuschnitts.

```dotnet
var preset = new StandardEncoderPreset

    {

        Filters = new Filters

        {                   

            Crop = new Rectangle

            {

                Left = "200",

                Top = "200",

                Width = "1280",

                Height = "720"

            }

        },

        Codecs =

        {

            new AacAudio(),

            new H264Video()

            {

                Layers =

                {                           

                    new H264Layer

                    {

                        Bitrate = 1000000,

                        Width = "1280",

                        Height = "720"

                    }

                }

            }

        },

        Formats =

        {

            new Mp4Format

            {

                FilenamePattern = "{Basename}_{Bitrate}{Extension}"

            }

        }

    }

```
