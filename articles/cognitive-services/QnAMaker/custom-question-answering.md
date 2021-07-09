---
title: QnA Maker Managed wird jetzt in benutzerdefinierte Fragen und Antworten umbenannt
titleSuffix: Azure Cognitive Services
description: Dieser Artikel enthält Neuigkeiten zu den geänderten Features von QnA Maker.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/11/2021
ms.custom: references_regions
ms.openlocfilehash: 6e8f9a26836be14d4952ba309933511635ff673a
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111982644"
---
# <a name="qna-maker-managed-is-now-renamed-to-custom-question-answering"></a>QnA Maker Managed wird jetzt in benutzerdefinierte Fragen und Antworten umbenannt

[QnA Maker Managed (Vorschauversion)](https://techcommunity.microsoft.com/t5/azure-ai/introducing-qna-maker-managed-now-in-public-preview/ba-p/1845575) wurde im November 2020 als kostenloses öffentliches Vorschauangebot gestartet. Es wurden mehrere neue Features eingeführt, einschließlich verbesserter Relevanz mithilfe eines Deep Learning-Bewerters, präziser Antworten und End-to-End-Unterstützung für Regionen. 

Im Rahmen unserer Bemühungen, die Sprachangebote von Cognitive Services zu konsolidieren, ist QnA Maker Managed jetzt ein Feature in der Textanalyse und wurde in benutzerdefinierte Fragen und Antworten umbenannt.  

## <a name="creating-a-new-custom-question-answering-service"></a>Erstellen eines neuen benutzerdefinierten Fragen und Antworten-Diensts

[Erstellen Sie eine Textanalyse-Ressource](https://ms.portal.azure.com/?quickstart=true#create/Microsoft.CognitiveServicesTextAnalytics), um die Beantwortung von Fragen und andere Features wie Entitätserkennung, Stimmungsanalyse usw. zu verwenden.  

Wenn Sie nun eine neue Textanalyse erstellen, können Sie Features auswählen, die Sie enthalten möchten. Wählen Sie **benutzerdefinierte Fragen und Antworten (Vorschau) aus** und fahren Sie mit der Erstellung Ihrer Ressource fort.  

> [!div class="mx-imgBorder"]
> [ ![Screenshot: Benutzeroberflächenmenü der Textanalyse-Ressource mit ausgewähltem benutzerdefinierter Fragen und Antworten-Feature ]( ./media/select-feature.png)]( ./media/select-feature.png#lightbox)


Sie können eine QnA Maker Managed-Ressource nicht mehr aus dem QnA Maker Erstellen-Flow erstellen. Stattdessen werden Sie an den Textanalyse-Dienst umgeleitet. Es gibt keine Änderung an der stabilen QnA Maker-Version. 

> [!div class="mx-imgBorder"]
> [ ![Screenshot: Menü der Ressourcengruppenerstellung]( ./media/create-resource.png) ]( ./media/create-resource.png#lightbox)

## <a name="details"></a>Details

- Alle vorhandenen QnA Maker Managed-Ressourcen (Vorschau) funktionieren weiterhin wie zuvor. Für diese Ressourcen ist derzeit keine Aktion erforderlich.
- Der Erstellungsablauf für die benutzerdefinierte Fragen und Antworten (Vorschau) ist die primäre Änderung. Der Dienst, das Portal, die Endpunkte, SDK usw. bleiben wie zuvor.
- Benutzerdefinierte Fragen und Antworten (Vorschauversion) werden weiterhin als kostenlose öffentliche Vorschau angeboten. Dieses Feature ist nur als Teil der Standardressourcen der Textanalyse verfügbar. Ändern Sie nicht Ihren Tarif für Textanalyseressourcen in „Kostenlos“.
- Benutzerdefinierte Fragen und Antworten (Vorschauversion) sind in den folgenden Regionen verfügbar:
    - USA Süd Mitte
    - Nordeuropa
    - Australien, Osten.

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit der QnA Maker-Clientbibliothek](./quickstarts/quickstart-sdk.md)
* [Erste Schritte mit dem QnA Maker-Portal](./quickstarts/create-publish-knowledge-base.md)

