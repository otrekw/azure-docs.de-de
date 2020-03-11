---
title: 'Migrieren von Wissensdatenbanken: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Die Migration einer Wissensdatenbank erfordert den Export aus einer Wissensdatenbank und den Import in eine andere.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/28/2020
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 942932c229ace82a0bf66da7a5421f936b028088
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302558"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migrieren einer Knowledge Base durch Exportieren und Importieren

Die Migration einer Wissensdatenbank erfordert den Export aus einer Wissensdatenbank und den Import in eine andere.

## <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.
* Richten Sie einen neuen [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md) ein.

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Migrieren einer Wissensdatenbank aus QnA Maker
1. Melden Sie sich beim [QnA Maker-Portal](https://qnamaker.ai) an.
1. Wählen Sie die ursprüngliche Wissensdatenbank aus, die Sie migrieren möchten.

1. Wählen Sie auf der Seite **Settings** (Einstellungen) die Option **Wissensdatenbank exportieren** (Export knowledge base), um eine TSV-Datei mit dem Inhalt der ursprünglichen Wissensdatenbank herunterzuladen, d. h. Fragen, Antworten, Metadaten, Folgeaufforderungen und die Namen der Datenquellen, aus denen sie extrahiert wurden.

1. Wählen Sie im oberen Menü die Option **Create a knowledge base** (Wissensdatenbank erstellen) aus, um eine _leere_ Wissensdatenbank zu erstellen. Sie ist leer, da Sie bei ihrer Erstellung keine URLs oder Dateien hinzufügen werden. Diese werden nach der Erstellung während des Importschritts hinzugefügt.

    Konfigurieren Sie die Wissensdatenbank. Legen Sie nur den neuen Namen der Wissensdatenbank fest. Doppelte Namen werden ebenso wie Sonderzeichen unterstützt.

    Wählen Sie keine Werte aus Schritt 4 aus, da diese Werte beim Importieren der Datei überschrieben werden.

1. Wählen Sie in Schritt 5 die Option **Erstellen** aus.

1. Öffnen Sie in dieser neuen Wissensdatenbank die Registerkarte **Einstellungen**, und klicken Sie auf **Import knowledge base** (Wissensdatenbank importieren). Dadurch werden die Fragen, Antworten, Metadaten und Folgeaufforderungen importiert sowie die Namen der Datenquellen beibehalten, aus denen sie extrahiert wurden.

   > [!div class="mx-imgBorder"]
   > [![Importieren der Wissensdatenbank](../media/qnamaker-how-to-migrate-kb/Import.png)](../media/qnamaker-how-to-migrate-kb/Import.png#lightbox)

1. **Testen** Sie die neue Knowledge Base im Testbereich. Erfahren Sie mehr über das [Testen Ihrer Knowledge Base](../How-To/test-knowledge-base.md).

1. **Veröffentlichen** Sie die Wissensdatenbank, und erstellen Sie einen Chatbot. Erfahren Sie mehr über das [Veröffentlichen Ihrer Knowledge Base](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).

## <a name="chat-logs-and-alterations"></a>Chatprotokolle und Varianten
Varianten (Synonyme) ohne Berücksichtigung von Groß-/Kleinbuchstaben werden nicht automatisch importiert. Verwenden Sie die [V4-APIs](https://go.microsoft.com/fwlink/?linkid=2092179), um die Änderungen in die neue Wissensdatenbank zu verschieben.

Die Migration von Chatprotokollen ist nicht möglich, da die neue Wissensdatenbank Application Insights zum Speichern von Chatprotokollen verwendet.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bearbeiten einer Knowledge Base](../How-To/edit-knowledge-base.md)
