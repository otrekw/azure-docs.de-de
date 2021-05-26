---
title: 'Sprachunterstützung: Formularerkennung'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie mehr über die menschlichen Sprachen, die in der Formularerkennung verfügbar sind.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 05/10/2021
ms.author: lajanuar
ms.openlocfilehash: eee8c7d595ff73bf14ac996c030ac938523d1a09
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110374421"
---
# <a name="language-support-for-form-recognizer"></a>Sprachunterstützung für die Formularerkennung

 In der folgenden Tabelle sind die Schriftsprachen aufgeführt, die vom Formularerkennungsdienst unterstützt werden:

<!-- markdownlint-disable MD001 -->
<!-- markdownlint-disable MD024 -->

## <a name="layout-and-custom-model"></a>Layout und benutzerdefiniertes Modell

### <a name="v21"></a>[v2.1](#tab/v2-1)

|Sprache| Sprachcode |
|:-----|:----:|
|Afrikaans|`af`|
|Albanisch |`sq`|
|Asturisch |`ast`|
|Baskisch  |`eu`|
|Bislama   |`bi`|
|Bretonisch    |`br`|
|Katalanisch    |`ca`|
|Cebuano    |`ceb`|
|Chamorro  |`ch`|
|Chinesisch (vereinfacht) | `zh-Hans`|
|Chinesisch (traditionell) | `zh-Hant`|
|Kornisch     |`kw`|
|Korsisch      |`co`|
|Krimtatarisch (Lateinisch)  |`crh`|
|Tschechisch | `cs` |
|Dänisch | `da` |
|Niederländisch | `nl` |
|Englisch (gedruckt und handschriftlich) | `en` |
|Estnisch  |`et`|
|Fidschi |`fj`|
|Filipino  |`fil`|
|Finnisch | `fi` |
|Französisch | `fr` |
|Friaulisch  | `fur` |
|Galizisch   | `gl` |
|Deutsch | `de` |
|Gilbertese    | `gil` |
|Grönländisch   | `kl` |
|Haitianisches Kreolisch  | `ht` |
|Hani  | `hni` |
|Hmong Daw (Lateinisch) | `mww` |
|Ungarisch | `hu` |
|Indonesisch   | `id` |
|Interlingual  | `ia` |
|Inuktitut (Lateinisch)  | `iu`  |
|Irisch    | `ga` |
|Italienisch | `it` |
|Japanisch | `ja` |
|Javanisch | `jv` |
|K’iche’  | `quc` |
|Kabuverdianu | `kea` |
|Kachin (Lateinisch) | `kac` |
|Kara-Kalpak | `kaa` |
|Kaschubisch | `csb` |
|Khasi  | `kha` |
|Koreanisch | `ko` |
|Kurdisch (Lateinisch) | `kur` |
|Luxemburgisch  | `lb` |
|Malaiisch (Lateinisch)  | `ms` |
|Manx  | `gv` |
|Neapolitanisch   | `nap` |
|Norwegisch | `no` |
|Okzitanisch | `oc` |
|Polnisch | `pl` |
|Portugiesisch | `pt` |
|Rätoromanisch  | `rm` |
|Schotten  | `sco` |
|Schottisch-Gälisch  | `gd` |
|Slowenisch  | `slv` |
|Spanisch | `es` |
|Suaheli (Lateinisch)  | `sw` |
|Schwedisch | `sv` | 
|Tatarisch (Lateinisch)  | `tat` |
|Tetum    | `tet` |
|Türkisch | `tr` |
|Obersorbisch  | `hsb` |
|Usbekisch (Lateinisch)     | `uz` |
|Volapük   | `vo` |
|Walser    | `wae` |
|Westfriesisch | `fy` |
|Yukatekisches Maya | `yua` |
|Zhuang | `za` |
|Zulu  | `zu` |

### <a name="v20"></a>[v2.0](#tab/v2-0)

|Sprache| Sprachcode |
|:-----|:----:|
Chinesisch (vereinfacht) | `zh-Hans`|
|Niederländisch | `nl` |
|Englisch (gedruckt und handschriftlich) | `en` |
|Französisch | `fr` |
|Deutsch | `de` |
|Italienisch | `it` |
|Japanisch | `ja` |
|Portugiesisch | `pt` |
|Spanisch | `es` |

-----

## <a name="prebuilt-receipt-and-business-card"></a>Vordefinierter Beleg und Visitenkarte

>[!NOTE]
 > Sie müssen kein Gebietsschema angeben. Dies ist ein optionaler Parameter, und die Deep Learning-Technologie der Formularerkennung erkennt automatisch die Sprache des Texts in Ihrem Bild.

### <a name="v21"></a>[v2.1](#tab/v2-1)

Vordefinierte Belege und Visitenkarten unterstützen alle englischen Belege und Visitenkarten mit den folgenden Gebietsschemas:

|Sprache| Code des Gebietsschemas |
|:-----|:----:|
|Englisch (Australien)|`en-au`|
|Englisch (Kanada)|`en-ca`|
|Walisisch (Großbritannien)|`en-gb`|
|Englisch (Indien)|`en-in`|
|Englisch (USA)| `en-us`|

### <a name="v20"></a>[v2.0](#tab/v2-0)

> [!IMPORTANT]
> Dieses Feature ist in der ausgewählten API-Version nicht verfügbar.

-----

## <a name="prebuilt-invoice"></a>Vordefinierte Rechnung

### <a name="v21"></a>[v2.1](#tab/v2-1)

Sprache| Code des Gebietsschemas |
|:-----|:----:|
|Englisch (USA)|de-de|

### <a name="v20"></a>[v2.0](#tab/v2-0)

> [!IMPORTANT]
> Dieses Feature ist in der ausgewählten API-Version nicht verfügbar.

-----

## <a name="prebuilt-identity-documents"></a>Vordefinierte Identitätsdokumente

### <a name="v21"></a>[v2.1](#tab/v2-1)

> [!NOTE]
> Diese Technologie ist derzeit für US-Führerscheine und die biografische Seite von internationalen Reisepässen (ohne Visa und andere Reisedokumente) verfügbar.

### <a name="v20"></a>[v2.0](#tab/v2-0)

> [!IMPORTANT]
> Dieses Feature ist in der ausgewählten API-Version nicht verfügbar.

-----

> [!div class="nextstepaction"]
> [Formularerkennung ausprobieren](https://aka.ms/fott-2.1-ga)
