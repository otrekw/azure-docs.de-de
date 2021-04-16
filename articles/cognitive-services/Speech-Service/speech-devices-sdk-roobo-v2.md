---
title: Speech-Geräte-SDK „Roobo Smart Audio Dev Kit v2“ – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Voraussetzungen und Anweisungen für den Einstieg in das Speech Devices SDK „Roobo Smart Audio Dev Kit v2“
services: cognitive-services
author: anushapatnala
manager: wellsi
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: v-anusp
ms.openlocfilehash: 2f325c80877068c53d690bd7ff74f768dab3a174
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "80371581"
---
# <a name="device-roobo-smart-audio-dev-kit-v2"></a>Gerät: Roobo Smart Audio Dev Kit v2

Dieser Artikel enthält gerätespezifische Informationen für das Roobo Smart Audio Dev Kit2.

## <a name="set-up-the-development-kit"></a>Einrichten des Development Kits

1. Das Development Kit hat zwei Micro-USB-Anschlüsse. Der linke Anschluss dient zur Stromversorgung des Development Kits und ist in der folgenden Abbildung mit „Power“ markiert. Der rechte dient zur Steuerung und ist in der Abbildung mit „Debug“ markiert. 
    ![Anschließen eines Development Kits](media/speech-devices-sdk/roobo-v2-connections.png)
1. Schalten Sie das Development Kit ein, nachdem Sie es mit einem Mikro-USB-Kabel an einen PC oder ein Netzteil angeschlossen haben. Eine grüne Stromanzeige leuchtet unter dem oberen Board auf.
1. Zum Steuern des Development Kit verbinden Sie den Debug-Anschluss mithilfe eines zweiten Micro-USB-Kabels mit einem Computer. Für eine zuverlässige Kommunikation ist es unerlässlich, ein hochwertiges Kabel zu verwenden.
1. Richten Sie Ihr Development Kit kreisförmig aus – Aufrecht, Mikrofone zur Decke gerichtet, wie oben gezeigt


## <a name="development-information"></a>Informationen zur Entwicklung

Weitere Informationen zur Entwicklung finden Sie im [Roobo-Entwicklungshandbuch](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

## <a name="audio-recordplay"></a>Audioaufnahme/-wiedergabe

DDK2-Audiooperationen können auf folgende Weise durchgeführt werden：
* Verwenden Sie ALSA-Open-Source-Bibliotheken und ihre Anwendungen.
* Verwenden Sie die appmainprog-Schnittstelle für die Anwendungsentwicklung. DDK2-Audio – Das zugehörige Softwareframework verwendet das ALSA-Standardframework. Sie können libasound verwenden. Um also Software direkt zu entwickeln. Daher können Sie „arecord“ und „aplay“ von ALSA direkt verwenden, um Audio aufzuzeichnen und wiederzugeben.
