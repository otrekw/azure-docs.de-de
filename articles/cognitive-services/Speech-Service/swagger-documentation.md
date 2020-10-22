---
title: Swagger-Dokumentation – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Die Swagger-Dokumentation kann zum automatischen Generieren von SDKs für verschiedene Programmiersprachen verwendet werden. Alle Vorgänge unseres Diensts werden von Swagger unterstützt.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: reference
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 6bb50e427fa85a170c5ad23a63d67c01e898a17d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91665683"
---
# <a name="swagger-documentation"></a>Swagger-Dokumentation

Der Speech-Dienst verfügt über eine Swagger-Spezifikation zum Interagieren mit einigen REST-APIs, die zum Importieren von Daten, Erstellen von Modellen, Testen der Modellgenauigkeit, Erstellen von benutzerdefinierten Endpunkten, Einreihen von Batchtranskriptionen in die Warteschlange und Verwalten von Abonnements verwendet werden. Die meisten Vorgänge, die über das Custom Speech-Portal verfügbar sind, können mit diesen APIs programmgesteuert durchgeführt werden.

> [!NOTE]
> Sowohl Spracherkennungs- als auch Sprachsynthesevorgänge werden unterstützt, und es sind REST-APIs vorhanden, die in der Swagger-Spezifikation dokumentiert sind.

## <a name="generating-code-from-the-swagger-specification"></a>Generieren von Code über die Swagger-Spezifikation

Die [Swagger-Spezifikation](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0) verfügt über Optionen, die Ihnen das schnelle Testen für verschiedene Pfade ermöglichen. Es kann aber auch der Fall sein, dass Code für alle Pfade generiert werden soll, um eine zentrale Bibliothek mit Aufrufen zu erstellen, die als Grundlage für zukünftige Lösungen dienen kann. Wir sehen uns nun den Prozess zum Generieren einer Python-Bibliothek an.

Sie müssen Swagger auf dieselbe Region wie Ihr Abonnement des Speech-Diensts festlegen. Sie können Ihre Region im Azure-Portal unter Ihrer Speech-Dienstressource überprüfen. Eine Liste mit allen unterstützten Regionen finden Sie unter [Regionen](regions.md).

1. Wechseln Sie in einem Browser zur Swagger-Spezifikation für Ihre Region:  
       `https://<your-region>.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0`
1. Klicken Sie auf dieser Seite auf **API-Definition** und anschließend auf **Swagger**. Kopieren Sie die URL der Seite, die angezeigt wird.
1. Navigieren Sie in einem neuen Browser zu https://editor.swagger.io.
1. Klicken Sie auf **Datei** und **Import URL** (URL importieren), kopieren Sie die URL, und klicken Sie dann auf **OK**.
1. Klicken Sie auf **Generate Client** (Client generieren), und wählen Sie **Python** aus. Die Clientbibliothek wird in einer `.zip`-Datei auf den Computer heruntergeladen.
1. Extrahieren Sie alles aus dem Download. Sie können `tar -xf` verwenden, um alles zu extrahieren.
1. Installieren Sie das extrahierte Modul in Ihrer Python-Umgebung:  
       `pip install path/to/package/python-client`
1. Das installierte Paket hat den Namen `swagger_client`. Überprüfen Sie, ob die Installation erfolgreich war:  
       `python -c "import swagger_client"`

Sie können die Python-Bibliothek verwenden, die Sie mit den [Speech-Dienstbeispielen auf GitHub](https://aka.ms/csspeech/samples) generiert haben.

## <a name="reference-docs"></a>Referenz

* [REST (Swagger): Batchtranskription und Anpassung](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0)
* [REST-API: Spracherkennung](rest-speech-to-text.md)
* [REST-API: Sprachsynthese](rest-text-to-speech.md)

## <a name="next-steps"></a>Nächste Schritte

* [Speech-Dienstbeispiele auf GitHub](https://aka.ms/csspeech/samples)
* [Kostenloses Testen des Speech-Diensts](overview.md#try-the-speech-service-for-free)
