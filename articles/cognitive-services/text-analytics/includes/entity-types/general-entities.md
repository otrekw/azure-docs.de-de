---
title: Allgemeine benannte Entitäten
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: ba516a548fc8385ca86526a7f2dd082b27e53208
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77211421"
---
## <a name="general-entity-types"></a>Allgemeine Entitätstypen:

### <a name="person"></a>Person

Erkennen von Personennamen in Text.

Sprachen:
* Öffentliche Vorschau: `Arabic`, `Czech`, `Chinese-Simplified`, `Danish`, `Dutch`, `English`, `Finnish`, `French`, `German`, `Hungarian`, `Italian`, `Japanese`, `Korean`, `Norwegian (Bokmål)`, `Polish`, `Portuguese (Portugal)`, `Portuguese (Brazil)`, `Russian`, `Spanish`, `Swedish` und `Turkish`

| Name des Untertyps | BESCHREIBUNG                                                      | Erst ab dieser Modellversion verfügbar |
|--------------|------------------------------------------------------------------|---------------------------------------|
| –          | Erkannte Personennamen, beispielsweise `Bill Gates`, `Marie Curie` | `2019-10-01`                          | 

### <a name="persontype"></a>PersonType
Von einer Person eingenommene Position oder Rolle.

Sprachen:
* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                                                                | Erst ab dieser Modellversion verfügbar |
|--------------|--------------------------------------------------------------------------------------------|----------------------------------------|
| –          | Berufe, z. B. `civil engineer`, `salesperson`, `chef`, `librarian`, `nursing aide` | `2020-02-01`                           |

### <a name="location"></a>Location

Natürliche und von Menschen hergestellte Wahrzeichen, Strukturen, geografische Merkmale und geopolitische Entitäten.

Sprachen:

* Öffentliche Vorschau: `Arabic`, `Czech`, `Chinese-Simplified`, `Danish`, `Dutch`, `English`, `Finnish`, `French`, `German`, `Hungarian`, `Italian`, `Japanese`, `Korean`, `Norwegian (Bokmål)`, `Polish`, `Portuguese (Portugal)`, `Portuguese (Brazil)`, `Russian`, `Spanish`, `Swedish` und `Turkish`

| Name des Untertyps              | BESCHREIBUNG                                                                              | Erst ab dieser Modellversion verfügbar |
|---------------------------|------------------------------------------------------------------------------------------|----------------------------------------|
| –                       | Orte, z.B. `Atlantic Ocean`, `library`, `Eiffel Tower`, `Statue of Liberty`  | `2019-10-01`                           |
| Geopolitische Entität (GPE) – nur Englisch| Städte, Länder, Staaten, beispielsweise `Seattle`, `Pennsylvania`, `South Africa`, `Tokyo` | `2020-02-01`                           |

### <a name="organization"></a>Organization  

Erkannte Organisationen, Unternehmen, Agenturen und andere Personengruppen. Beispiel: Firmen, politische Gruppen, Musikgruppen, Sportvereine, Regierungsstellen und öffentliche Organisationen. Nationalitäten und Religionen werden in diesem Entitätstyp nicht berücksichtigt. 

Sprachen: 

* Öffentliche Vorschau: `Arabic`, `Czech`, `Chinese-Simplified`, `Danish`, `Dutch`, `English`, `Finnish`, `French`, `German`, `Hungarian`, `Italian`, `Japanese`, `Korean`, `Norwegian (Bokmål)`, `Polish`, `Portuguese (Portugal)`, `Portuguese (Brazil)`, `Russian`, `Spanish`, `Swedish` und `Turkish`

| Name des Untertyps | BESCHREIBUNG                                                                                             | Erst ab dieser Modellversion verfügbar |
|--------------|---------------------------------------------------------------------------------------------------------|----------------------------------------|
| –          | Organisationen, z. B. `Microsoft`, `NASA`, `National Oceanic and Atmospheric Administration`, `VOA` | `2019-10-01`                           |

### <a name="event"></a>Ereignis  

Historische Ereignisse, gesellschaftliche Ereignisse und Naturereignisse.  

Sprachen: 

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                                            | Erst ab dieser Modellversion verfügbar |
|--------------|------------------------------------------------------------------------|----------------------------------------|
| –          | Ereignisse wie `wedding`, `hurricane`, `car accident`, `solar eclipse`, `American Revolution` | `2020-02-01`                           |

### <a name="product"></a>Produkt  

Physische Objekte verschiedener Kategorien.  

Sprachen: 

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                                                        | Erst ab dieser Modellversion verfügbar |
|--------------|------------------------------------------------------------------------------------|----------------------------------------|
| –          | Beispiel: `Microsoft Surface laptop`, `sunglasses`, `motorcycle`, `bag`, `Xbox`. | `2020-02-01`                           |
| Computing    | `Azure Cosmos DB`, `Azure Kubernetes Service`                                     | `2020-02-01`                           |

### <a name="skill"></a>Skill  

Eine Entität, die eine Funktion oder ein Fachwissen beschreibt.  

Sprachen: 

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                                                 | Erst ab dieser Modellversion verfügbar |
|--------------|-----------------------------------------------------------------------------|----------------------------------------|
| –          | `nursing`, `data mining`, `linguistics`, `critical thinking`, `photography` | `2020-02-01`                           |

### <a name="phone-number"></a>Rufnummer

Telefonnummern (nur US-Telefonnummern) 

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                    | Erst ab dieser Modellversion verfügbar |
|--------------|------------------------------------------------|----------------------------------------|
| –          | US-Telefonnummern, z. B. `(312) 555-0176` | `2019-10-01`                           |

### <a name="email"></a>Email

E-Mail-Adresse. 

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                      | Erst ab dieser Modellversion verfügbar |
|--------------|--------------------------------------------------|----------------------------------------|
| –          | E-Mail-Adresse, z. B. `support@contoso.com` | `2019-10-01`                           |

### <a name="url"></a>URL

Internet-URLs.

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                          | Erst ab dieser Modellversion verfügbar |
|--------------|------------------------------------------------------|----------------------------------------|
| –          | URLs zu Websites, z. B. `https://www.bing.com`. | `2019-10-01`                           |

### <a name="ip-address"></a>IP-Adresse

Internetprotokolladresse

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                              | Erst ab dieser Modellversion verfügbar |
|--------------|------------------------------------------|----------------------------------------|
| –          | Netzwerkadresse, z. B. `10.0.0.101` | `2019-10-01`                           |

###  <a name="datetime"></a>Datetime

Datums- und Uhrzeitentitäten 

* Erst ab dieser Modellversion verfügbar `2019-10-01`

Sprachen:

* Öffentliche Vorschau: `Chinese-Simplified`, `English`, `French`, `German` und `Spanish`

| Name des Untertyps    | Beispiele                     |
|-------------|------------------------------|
| –         | `6:30PM February 4, 2012`, `4/1/2011 2:45`                   |
| Date  | `May 2nd, 2017`, `05/02/2017`       |
| Time     | `8:15`, `6AM`              |
| DateRange    | `August 2nd to August 5th`         |
| TimeRange   | `4-6PM`, `10:00AM to Noon`          |
| Duration | `2.5 minutes`, `one and a half hours`         |
| Set | `every Saturday`         |

###  <a name="quantity"></a>Menge

Zahlen und numerische Mengen. 

* Erst ab dieser Modellversion verfügbar `2019-10-01`

Sprachen:

* Öffentliche Vorschau: `Chinese-Simplified`, `English`, `French`, `German` und `Spanish`

| Name des Untertyps    | Beispiele                     |
|-------------|------------------------------|
| Number         | `6`, `six`                   |
| Prozentwert  | `50%`, `fifty percent`       |
| Ordinal     | `2nd`, `second`              |
| Age         | `90 day old`, `30 years old` |
| Währung    | `$10.99`, `€30.00`           |
| Dimension   | `10 miles`, `40 cm`          |
| Temperatur | `32 degrees`, `10°C`         |
