---
title: Allgemeine benannte Entitäten
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 05/13/2020
ms.author: aahi
ms.openlocfilehash: 630f04bf2cc9e7de6331f9d25754a20fe2327d76
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91779174"
---
Die folgenden Entitätskategorien werden beim Senden von Anforderungen an den `/entities/recognition/general`-Endpunkt zurückgegeben.

| Category   | Unterkategorie | BESCHREIBUNG                          | Ab Modellversion                                                    | Notizen |
|------------|-------------|--------------------------------------|-------------------------------------------------------------|--------------------------------------|
| Person     | –         | Namen von Personen  | `2019-10-01`  | Wird auch von NER v2.1 zurückgegeben |
| PersonType | –         | Von einer Person eingenommene Position oder Rolle | `2020-02-01` | |
|Standort    | –         | Natürliche und von Menschen hergestellte Wahrzeichen, Gebäude, geografische Merkmale und geopolitische Entitäten     |  `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
|Standort     | Geopolitische Entität (GPE)        | Städte, Länder/Regionen, Staaten      | `2020-02-01` | |
|Standort     | Strukturell                       | Von Menschen geschaffene Strukturen | `2020-04-01` | |
|Standort     | Geografisch       | Geografische und natürliche Merkmale wie Flüsse, Ozeane und Wüsten |  `2020-04-01` | |
|Organization  | – | Firmen, politische Gruppen, Musikgruppen, Sportvereine, Regierungsstellen und öffentliche Organisationen  | `2019-10-01` | Nationalitäten und Religionen werden in diesem Entitätstyp nicht berücksichtigt. Wird auch von NER v2.1 zurückgegeben |
|Organization | Medizin | Medizinische Unternehmen und Gruppen | `2020-04-01` |  |
|Organization | Börse | Börsengruppen | `2020-04-01` | |
| Organization | Sport | Sportbezogene Organisationen | `2020-04-01` |  |
| Ereignis  | – | Historische Ereignisse, gesellschaftliche Ereignisse und Naturereignisse | `2020-02-01` |  |
| Ereignis  | Kultur | Kulturveranstaltungen und Feiertage | `2020-04-01` | |
| Ereignis  | Natural | Naturereignisse | `2020-04-01` |  |
| Ereignis  | Sport | Sportveranstaltungen  | `2020-04-01` | |
| Produkt | – | Physische Objekte verschiedener Kategorien. | `2020-02-01` | |
| Produkt | Rechenergebnisse | Rechenergebnisse |  `2020-02-01 ` | |
| Skill | – | Eine Funktion, Kenntnis oder ein Fachwissen | `2020-02-01` |  |
| Adresse | – | Vollständige Postanschrift  | `2020-04-01` |  |
| PhoneNumber | – | Telefonnummern (nur US- und EU-Telefonnummern) | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Email | – | E-Mail-Adressen | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| URL | – | URLs zu Websites | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben  |
| IP | – | Netzwerk-IP-Adressen | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Datetime | – | Datums- und Uhrzeitangaben | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben | 
| Datetime | Date | Kalenderdatumsangaben | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Datetime | Time | Tageszeiten | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Datetime | DateRange | Datumsbereiche | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Datetime | TimeRange | Uhrzeitbereiche | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Datetime | Duration | Zeitspannen | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Datetime | Set | Festgelegte Wiederholungszeiten |  `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Menge | – | Zahlen und numerische Mengen. | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben  |
| Menge | Number | Zahlen. | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Menge | Prozentwert | Prozentangaben| `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Menge | Ordinal | Ordinalzahlen | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Menge | Age | Altersangaben | `2019-10-01` |  Wird auch von NER v2.1 zurückgegeben |
| Menge | Währung | Währungen | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Menge | Dimension | Abmessungen und Maße | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
| Menge | Temperatur | Temperaturen | `2019-10-01` | Wird auch von NER v2.1 zurückgegeben |
