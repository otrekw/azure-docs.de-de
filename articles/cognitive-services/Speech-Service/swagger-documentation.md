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
ms.openlocfilehash: fcc43caf895dadfaf832a47c3254f9b828bcb71a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77430804"
---
# <a name="swagger-documentation"></a>Swagger-Dokumentation

Der Speech-Dienst verfügt über eine Swagger-Spezifikation zum Interagieren mit einigen REST-APIs, die zum Importieren von Daten, Erstellen von Modellen, Testen der Modellgenauigkeit, Erstellen von benutzerdefinierten Endpunkten, Einreihen von Batchtranskriptionen in die Warteschlange und Verwalten von Abonnements verwendet werden. Die meisten Vorgänge, die über das Custom Speech-Portal verfügbar sind, können mit diesen APIs programmgesteuert durchgeführt werden.

> [!NOTE]
> Sowohl Spracherkennungs- als auch Sprachsynthesevorgänge werden unterstützt, und es sind REST-APIs vorhanden, die in der Swagger-Spezifikation dokumentiert sind.

## <a name="generating-code-from-the-swagger-specification"></a>Generieren von Code über die Swagger-Spezifikation

Die [Swagger-Spezifikation](https://cris.ai/swagger/ui/index) verfügt über Optionen, die Ihnen das schnelle Testen für verschiedene Pfade ermöglichen. Es kann aber auch der Fall sein, dass Code für alle Pfade generiert werden soll, um eine zentrale Bibliothek mit Aufrufen zu erstellen, die als Grundlage für zukünftige Lösungen dienen kann. Wir sehen uns nun den Prozess zum Generieren einer Python-Bibliothek an.

Sie müssen Swagger auf dieselbe Region wie Ihr Abonnement des Speech-Diensts festlegen. Sie können Ihre Region im Azure-Portal unter Ihrer Speech-Dienstressource überprüfen. Eine Liste mit allen unterstützten Regionen finden Sie unter [Regionen](regions.md).

1. Besuchen Sie https://editor.swagger.io.
2. Klicken Sie auf **Datei** und dann auf **Importieren**.
3. Geben Sie die Swagger-URL ein, einschließlich der Region Ihres Abonnements für den Speech-Dienst: `https://<your-region>.cris.ai/docs/v2.0/swagger`.
4. Klicken Sie auf **Generate Client** (Client generieren), und wählen Sie „Python“.
5. Speichern der Clientbibliothek

Sie können die Python-Bibliothek verwenden, die Sie mit den [Speech-Dienstbeispielen auf GitHub](https://aka.ms/csspeech/samples) generiert haben.

## <a name="reference-docs"></a>Referenz

* [REST (Swagger): Batchtranskription und Anpassung](https://westus.cris.ai/swagger/ui/index)
* [REST-API: Spracherkennung](rest-speech-to-text.md)
* [REST-API: Sprachsynthese](rest-text-to-speech.md)

## <a name="next-steps"></a>Nächste Schritte

* [Speech-Dienstbeispiele auf GitHub](https://aka.ms/csspeech/samples)
* [Kostenloses Testen des Speech-Diensts](get-started.md)
