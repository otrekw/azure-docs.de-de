---
title: Für den Import unterstützte URL-Typen – QnA Maker
description: Informationen zum Verwenden der Typen von URLs zum Importieren und Erstellen von Frage-Antwort-Paaren
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 01/02/2020
ms.openlocfilehash: 8bf50c1ea81cdf5246c47646d1a55926fe7d58d6
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91776696"
---
# <a name="urls-supported-for-importing-documents"></a>Unterstützte URLs zum Importieren von Dokumenten

Informationen zum Verwenden der Typen von URLs zum Importieren und Erstellen von Frage-Antwort-Paaren

## <a name="faq-urls"></a>FAQ-URLs

QnA Maker kann Webseiten mit häufig gestellten Fragen auf drei Arten unterstützen:

* Einfache FAQ-Seiten
* FAQ-Seiten mit Links
* FAQ-Seiten mit einer Themenstartseite

### <a name="plain-faq-pages"></a>Einfache FAQ-Seiten

Dies ist der am häufigsten verwendete Typ einer FAQ-Seite, bei der auf der gleichen Seite die Antworten unmittelbar auf die Fragen folgen.

Unten sehen Sie ein Beispiel für eine einfache FAQ-Seite:

![Beispiel für eine einfache FAQ-Seite für eine Wissensdatenbank](./media/qnamaker-concepts-datasources/plain-faq.png)


### <a name="faq-pages-with-links"></a>FAQ-Seiten mit Links

Bei diesem FAQ-Seitentyp sind Fragen zusammengefasst und mit Antworten verknüpft, die sich in verschiedenen Abschnitten auf derselben Seite oder auf verschiedenen Seiten befinden.

Unten sehen Sie ein Beispiel für eine FAQ-Seite mit Links in Abschnitten, die sich auf derselben Seite befinden:

 ![Beispiel für eine FAQ-Seite mit Abschnittsverknüpfung für eine Wissensdatenbank](./media/qnamaker-concepts-datasources/sectionlink-faq.png)


### <a name="parent-topics-page-links-to-child-answers-pages"></a>Übergeordnete Themenseite mit Links zu untergeordneten Antwortseiten

Diese Art von FAQ verfügt über eine Themenseite, auf der jedes Thema mit einem entsprechenden Satz von Fragen und Antworten auf einer anderen Seite verknüpft ist. QnA Maker durchsucht alle verknüpften Seiten, um die entsprechenden Fragen und Antworten zu extrahieren.

Unten sehen Sie ein Beispiel für eine Themenseite mit Links zu FAQ-Abschnitten auf verschiedenen Seiten.

 ![Beispiel für eine FAQ-Seite mit Deep-Link für eine Wissensdatenbank](./media/qnamaker-concepts-datasources/topics-faq.png)

## <a name="support-urls"></a>Support-URLs

QnA Maker kann semistrukturierte Supportwebseiten verarbeiten. Hierzu zählen beispielsweise Webartikel, in denen die Vorgehensweise zum Ausführen einer bestimmte Aufgabe, die Diagnostizierung oder Behebung eines bestimmten Problems oder die Verwendung bewährter Methoden für einen bestimmten Prozess beschrieben wird. Die Extraktion funktioniert am besten für Inhalte mit klarer Struktur und hierarchischen Überschriften.

> [!NOTE]
> Die Extraktion für Supportartikel ist ein neues Feature und befindet sich noch in einer frühen Phase. Sie funktioniert am besten für einfache, gut strukturierte Seiten ohne komplexe Kopf- und Fußzeilen.

![QnA Maker unterstützt die Extraktion von semistrukturierten Webseiten, die über eine klare Struktur mit hierarchischen Überschriften verfügen.](./media/qnamaker-concepts-datasources/support-web-pages-with-heirarchical-structure.png)