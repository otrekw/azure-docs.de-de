---
title: 'Schnellstart: Clientbibliothek oder REST-API für die Formularerkennung'
titleSuffix: Azure Applied AI Services
description: Verwenden Sie die Clientbibliothek oder die REST-API für die Formularerkennung, um eine Formularverarbeitungs-App zu erstellen, die Schlüssel-Wert-Paare und Tabellendaten aus Ihren benutzerdefinierten Dokumenten extrahiert.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 04/14/2021
ms.author: lajanuar
zone_pivot_groups: programming-languages-set-formre
ms.custom: devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
keywords: Formularverarbeitung, automatisierte Datenverarbeitung
ms.openlocfilehash: 34b2e591c8d050a168e82bb40c5167823c136e52
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110374157"
---
# <a name="quickstart-use-the-form-recognizer-client-library-or-rest-api"></a>Schnellstart: Verwenden der Clientbibliothek oder REST-API für die Formularerkennung

Hier finden Sie Informationen zu den ersten Schritten mit der Formularerkennung in der Entwicklungssprache Ihrer Wahl. Die Azure-Formularerkennung ist ein kognitiver Dienst, mit dem Sie unter Verwendung von Technologie für maschinelles Lernen Software für die automatisierte Datenverarbeitung entwickeln können. Identifizieren und extrahieren Sie Schlüssel-Wert-Paare, Auswahlmarkierungen, Tabellendaten und Ähnliches aus Formulardokumenten. Der Dienst gibt strukturierte Daten aus, die auch die Beziehungen in der ursprünglichen Datei umfassen. Die Formularerkennung kann per REST-API oder per SDK verwendet werden. Sie sollten den kostenlosen Dienst nutzen, wenn Sie die Technologie erlernen. Bedenken Sie, dass die Anzahl der kostenlosen Seiten auf 500 pro Monat beschränkt ist.

Verwenden Sie die folgenden APIs zum Extrahieren strukturierter Daten aus Formularen und Dokumenten:

|Name |BESCHREIBUNG |
|---|---|
| **[Analysieren des Layouts](#analyze-layout)** | Analysieren Sie ein als Stream übergebenes Dokument, um Text, Auswahlmarkierungen, Tabellen und die Struktur aus dem Dokument zu extrahieren. |
| **[Analysieren von Belegen](#analyze-receipts)** | Analysieren Sie ein Belegdokument, um zentrale Informationen und anderen Belegtext zu extrahieren.|
| **[Analysieren von Visitenkarten](#analyze-business-cards)** | Analysieren Sie eine Visitenkarte, um wichtige Informationen und Text zu extrahieren.|
| **[Analysieren von Rechnungen](#analyze-invoices)** | Analysieren Sie eine Rechnung, um zentrale Informationen, Tabellen und anderen Rechnungstext zu extrahieren.|
| **[Analysieren von Ausweisdokumenten](#analyze-identity-documents)** | Analysieren Sie Ausweisdokumente, um wichtige Informationen und anderen Ausweistext zu extrahieren.|
| **[Trainieren eines benutzerdefinierten Modells](#train-a-custom-model)**| Trainieren eines neuen Modells zur Analyse Ihrer Formulare mit fünf Formularen gleichen Typs. Legen Sie den Parameter _useLabelFile_ auf `true` fest, um mit manuell beschrifteten Daten zu trainieren. |
| **[Analysieren von Formularen mit einem benutzerdefinierten Modell](#analyze-forms-with-a-custom-model)**|Analysieren Sie ein als Stream übergebenes Formular, um Text, Schlüssel-Wert-Paare und Tabellen mit Ihrem benutzerdefinierten Modell aus dem Formular zu extrahieren.  |
|**[Verwalten benutzerdefinierter Modelle](#manage-custom-models)**| Sie können die Anzahl der benutzerdefinierten Modelle in Ihrem Formularerkennungskonto überprüfen, ein bestimmtes Modell anhand seiner ID abrufen und ein Modell aus Ihrem Konto löschen.|

::: zone pivot="programming-language-csharp"

[!INCLUDE [C# SDK quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Java SDK quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [NodeJS SDK quickstart](../includes/quickstarts/javascript-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Python SDK quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="programming-language-rest-api"

[!INCLUDE [REST API quickstart](../includes/quickstarts/rest-api.md)]

::: zone-end
