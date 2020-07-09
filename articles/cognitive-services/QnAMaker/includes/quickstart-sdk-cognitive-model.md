---
ms.openlocfilehash: c87b9dc22ecd937abb27417aeddc1e30c0d724e7
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2020
ms.locfileid: "85114529"
---

## <a name="using-this-example-knowledge-base"></a>Verwenden dieser Beispiel-Wissensdatenbank

Die Wissensdatenbank in diesem Schnellstart beginnt mit zwei QnA-Unterhaltungspaaren. Dies wurde absichtlich so implementiert, um das Beispiel zu vereinfachen und hochgradig vorhersagbare IDs für die Update-Methode zu erhalten, die Folgeaufforderungen mit Fragen zu neuen Paaren verknüpft. Dies wurde für diese Schnellstartanleitung in einer bestimmten Reihenfolge geplant und implementiert.

Wenn Sie beabsichtigen, Ihre Wissensdatenbank im Lauf der Zeit mit Folgeaufforderungen zu entwickeln, die von vorhandenen QnA-Paaren abhängen, bieten sich Ihnen die folgenden Möglichkeiten:
* Bei größeren Wissensdatenbanken Verwaltung der Wissensdatenbank in einem Text-Editor oder einem TSV-Tool, das Automatisierung unterstützt. Die Wissensdatenbank wird dann in einem Durchgang vollständig durch ein Update ersetzt.
* Bei kleineren Wissensdatenbanken ausschließliches Verwalten der Nachfolgeaufforderungen im QnA Maker-Portal.

Details zu den QnA-Paaren, die in dieser Schnellstartanleitung verwendet werden:
* Typen von QnA-Paaren: Nach dem Update gibt es zwei Typen von QnA-Paaren in dieser Wissensdatenbank: Geplauder und domänenspezifische Informationen. Dies ist typisch, wenn Ihre Wissensdatenbank an eine Unterhaltungsanwendung gebunden ist, wie etwa einen Chatbot.
* Die Antworten aus Ihrer Wissensdatenbank lassen sich zwar anhand von Metadaten oder Folgeaufforderungen filtern, dies wird in dieser Schnellstartanleitung aber nicht gezeigt. Suchen Sie [hier](../Quickstarts/get-answer-from-knowledge-base-using-url-tool.md) nach diesen sprachunabhängigen generateAnswer-Beispielen.
* Der Antworttext ist Markdown und kann eine [große Bandbreite von Markdown](../reference-markdown-format.md) enthalten, z. B. Bilder (öffentlich verfügbare, internetbasierte Bilder), Links (zu öffentlich verfügbaren URLs) und Aufzählungspunkte. Diese Unterart spielt in diesem Schnellstart keine Rolle.
