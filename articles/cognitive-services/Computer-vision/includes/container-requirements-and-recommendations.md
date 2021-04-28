---
title: Containeranforderungen und -empfehlungen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/09/2021
ms.author: aahi
ms.openlocfilehash: 5cbf07ab97206ae3509701a6d6b0f71e961b0bf8
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107308772"
---
> [!NOTE]
> Die Anforderungen und Empfehlungen basieren auf Benchmarks mit einer einzelnen Anforderung pro Sekunde, wobei ein 8 MB großes Bild eines gescannten Geschäftsbriefs mit 29 Zeilen und insgesamt 803 Zeichen verwendet wird.

In der folgenden Tabelle werden die minimale und empfohlene Zuordnung von Ressourcen für jeden OCR-Container für das Lesen beschrieben.

| Container | Minimum | Empfohlen |
|-----------|---------|-------------|
| Read 2.0-preview | 1 Kern, 8 GB Arbeitsspeicher |    8 Kerne, 16 GB Arbeitsspeicher |
| Read 3.2 | 8 Kerne, 16 GB Arbeitsspeicher | 8 Kerne, 24 GB Arbeitsspeicher |

* Jeder Kern muss eine Geschwindigkeit von mindestens 2,6 GHz aufweisen.

Kern und Arbeitsspeicher entsprechen den Einstellungen `--cpus` und `--memory`, die im Rahmen des Befehls `docker run` verwendet werden.
