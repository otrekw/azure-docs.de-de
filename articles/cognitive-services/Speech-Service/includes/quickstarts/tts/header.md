---
title: 'Schnellstart: Synthetisieren von Sprache – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Sprach-SDK Sprache synthetisieren.
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 09/20/2019
ms.author: yulili
ms.openlocfilehash: d19f779f67cc0dea8cc7f06aa275885d75c3092e
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818353"
---
In dieser Schnellstartanleitung wird Text unter Verwendung des [Sprach-SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) in synthetisierte Sprache umgewandelt. Nachdem Sie einige Voraussetzungen erfüllt haben, erfordert das Rendern synthetisierter Sprache über die Standardlautsprecher nur vier Schritte:
> [!div class="checklist"]
> * Erstellen Sie ein Objekt vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region.
> * Erstellen Sie ein Objekt vom Typ ````SpeechSynthesizer```` unter Verwendung des obigen ````SpeechConfig````-Objekts.
> * Verwenden Sie das ````SpeechSynthesizer````-Objekt, um den Text zu sprechen.
> * Überprüfen Sie das zurückgegebene ````SpeechSynthesisResult````-Objekt auf Fehler.
