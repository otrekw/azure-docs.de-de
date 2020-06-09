---
title: Benannte Entitäten für personenbezogene Informationen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: 57be24142a8504347f420e5780e9621cd2eac91d
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83778183"
---
## <a name="personal-information-entity-types"></a>Entitätstypen für personenbezogene Informationen:

### <a name="person"></a>Person
Erkennen von Personennamen in Text.

Sprachen:
* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                               | Erst ab dieser Modellversion verfügbar |
|--------------|-----------------------------------------------------------|----------------------------------------|
| –          | Erkannte Namen, z.B. `Bill Gates`, `Marie Curie` | `2020-02-01`                           |

### <a name="organization"></a>Organization  

Erkannte Organisationen, Unternehmen, Agenturen, Firmen, Vereine und andere Personengruppen.

Sprachen: 

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                                                                       | Erst ab dieser Modellversion verfügbar|
|--------------|---------------------------------------------------------------------------------------------------|--------------|
| –          | Organisationen, z. B. `Microsoft`, `NASA`, `National Oceanic and Atmospheric Administration` | `2020-02-01` |

### <a name="phone-number"></a>Rufnummer

Telefonnummern (nur US-Telefonnummern) 

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                    | Erst ab dieser Modellversion verfügbar |
|--------------|------------------------------------------------|----------------------------------------|
| –          | US-Telefonnummern, z. B. `(312) 555-0176` | `2020-02-01`                           |

### <a name="email"></a>Email

E-Mail-Adresse. 

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                      | Erst ab dieser Modellversion verfügbar |
|--------------|--------------------------------------------------|----------------------------------------|
| –          | E-Mail-Adresse, z. B. `support@contoso.com` | `2020-02-01`                           |

### <a name="url"></a>URL

Internet-URLs.

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                                          | Erst ab dieser Modellversion verfügbar |
|--------------|------------------------------------------------------|----------------------------------------|
| –          | URLs zu Websites, z. B. `https://www.bing.com`. | `2020-02-01`                           |

### <a name="ip-address"></a>IP-Adresse

Internetprotokolladresse

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                              | Erst ab dieser Modellversion verfügbar |
|--------------|------------------------------------------|----------------------------------------|
| –          | Netzwerkadresse, z. B. `10.0.0.101` | `2020-02-01`                           |

### <a name="quantity"></a>Menge 

Numerische Mengen

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                   | Erst ab dieser Modellversion verfügbar |
|--------------|-------------------------------|----------------------------------------|
| Age          | `90 days old`, `30 years old` | `2020-02-01`                           |

### <a name="datetime"></a>Datetime

Datums- und Uhrzeitentitäten

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                   | Erst ab dieser Modellversion verfügbar |
|--------------|-------------------------------|----------------------------------------|
| Date         | `May 2nd, 2017`, `05/02/2017` | `2020-02-01`                           |

### <a name="eu-gps-coordinates"></a>EU-GPS-Koordinaten

 GPS-Koordinaten für Standorte innerhalb der Europäischen Union. 

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps | BESCHREIBUNG                               | Erst ab dieser Modellversion verfügbar |
|--------------|-------------------------------------------|----------------------------------------|
| –          | GPS-Koordinaten innerhalb der Europäischen Union | `2019-10-01`                           |

### <a name="azure-information"></a>Azure-Informationen

Identifizierbare Azure-Informationen, Authentifizierungsinformationen und Verbindungszeichenfolgen eingeschlossen. 

* Erst ab der Modellversion `2019-10-01` verfügbar.

Sprachen:

* Öffentliche Vorschau: `English`

| Name des Untertyps                          | BESCHREIBUNG                                                                 |
|---------------------------------------|-----------------------------------------------------------------------------|
| Azure DocumentDB-Authentifizierungsschlüssel             | Autorisierungsschlüssel für einen Azure DocumentDB-Server.                           |
| Verbindungszeichenfolge für Azure-IaaS-Datenbank | Verbindungszeichenfolge für eine Azure-IaaS-Datenbank (Infrastructure-as-a-Service). |
| Azure SQL-Verbindungszeichenfolge           | Verbindungszeichenfolge für eine Azure SQL-Datenbank.                                |
| Azure IoT-Verbindungszeichenfolge           | Verbindungszeichenfolge für Azure IoT (Internet of Things).                        |
| Azure-Veröffentlichungseinstellung: Kennwort        | Kennwort für die Azure-Veröffentlichungseinstellungen.                                        |
| Azure Redis Cache-Verbindungszeichenfolge   | Verbindungszeichenfolge für eine Azure Redis Cache-Instanz.                             |
| Azure SaaS                             | Verbindungszeichenfolge für Azure SaaS (Software-as-a-Service).                     |
| Azure Service Bus-Verbindungszeichenfolge   | Verbindungszeichenfolge für eine Azure Service Bus-Instanz.                                |
| Azure Storage-Kontoschlüssel             | Schlüssel für ein Azure-Speicherkonto.                                   |
| Azure Storage-Kontoschlüssel (generisch)   | Generischer Schlüssel für ein Azure-Speicherkonto.                           |
| SQL Server-Verbindungszeichenfolge          | Verbindungszeichenfolge für eine SQL Server-Instanz.                                         |

### <a name="identification"></a>Identifikation

* Erst ab der Modellversion `2019-10-01` verfügbar.

Sprachen:

* Öffentliche Vorschau: `English`

#### <a name="financial-account-identification"></a>Identifikation bei Finanzkonten

| Name des Untertyps               | BESCHREIBUNG                                                                |
|----------------------------|----------------------------------------------------------------------------|
| ABA-Nummern        | ABA-Nummern (American Bankers Association) für den Zahlungsverkehr mit US-amerikanischen Banken.                  |
| SWIFT-Code                 | SWIFT-Codes in Zahlungsanweisungen.                           |
| Kreditkarte                | Kreditkartennummern                                                       |
| IBAN-Code                  | IBAN-Codes in Zahlungsanweisungen.                            |

#### <a name="government-and-countryregion-specific-identification"></a>Behördliche und landes-/regionsspezifische Informationen zur Identifikation

Die nachstehenden Entitäten werden nach Land/Region gruppiert und aufgeführt:

Argentinien
* Nationale argentinische ID-Nummer (DNI)

Australien
* Steuernummer 
* Führerscheinnummer
* Reisepassnummer
* Krankenversicherungsnummer
* Kontonummern (beispielsweise Giro-, Spar- und Debitkonten)

Belgien
* Nationalregisternummer

Brasilien
* Nummer juristischer Personen (CNPJ)
* CPF-Nummer
* Nationale ID-Karte (RG)

Canada
* Reisepassnummer
* Führerscheinnummer
* Krankenversicherungsnummer
* Persönliche Krankenversicherungsnummer (Personal Health ID Number, PHIN)
* Sozialversicherungsnummer
* Kontonummern (beispielsweise Giro-, Spar- und Debitkonten)

Chile
* ID-Kartennummer 

China
* ID-Kartennummer
* Personalausweisnummer (PRC)

Kroatien
* ID-Kartennummer
* Persönliche Identifikationsnummer (OID)

Tschechische Republik
* Personalausweisnummer

Dänemark
* Persönliche Identifikationsnummer

Europäische Union (EU)
* Nationale Identifikationsnummer
* Reisepassnummer
* Führerscheinnummer
* Sozialversicherungsnummer (oder gleichwertig)
* EU-Steueridentifikationsnummer (TIN)
* EU-Debitkartennummer

Finnland
* Nationale Identifikationsnummer
* Reisepassnummer

Frankreich
* Personalausweisnummer (CNI)
* Sozialversicherungsnummer (INSEE)
* Reisepassnummer
* Führerscheinnummer

Deutschland
* Personalausweisnummer
* Reisepassnummer
* Führerscheinnummer

Griechenland 
* Personalausweisnummer

Hongkong
* Personalausweisnummer (HKID)

Indien
* PAN (Permanent Account Number)
* Eindeutige ID-Nummer (Aadhaar)

Indonesien
* ID-Kartennummer (KTP)

Irland
* Sozialversicherungsnummer (PPS)

Israel
* Nationale Identifikationsnummer
* Kontonummern (beispielsweise Giro-, Spar- und Debitkonten)

Italien
* Führerscheinnummer

Japan
* Melderegisternummer
* Personalausweisnummer
* Führerscheinnummer
* Sozialversicherungsnummer (SIN)
* Reisepassnummer
* Kontonummern (beispielsweise Giro-, Spar- und Debitkonten)

Malaysia
* ID-Kartennummer

Niederlande
* Persönliche Identifikationsnummer (BSN)

Neuseeland
* Verwaltungsnummer des Gesundheitsministeriums

Norwegen
* ID-Kartennummer

Philippinen
* Einheitliche Mehrzweck-ID-Nummer (Unified Multi-Purpose ID)

Polen
* Personalausweisnummer
* Nationale Identifikationsnummer (PESEL)
* Reisepassnummer

Portugal 
* ID-Kartennummer

Saudi-Arabien
* Nationale Identifikationsnummer

Singapur
* Nationale Registrierungs-ID-Kartennummer (NRIC)

Südafrika
* ID-Nummer
* Melderegisternummer

Südkorea
* Melderegisternummer

Spanien 
* US-Sozialversicherungsnummer (Social Security Number, SSN)

Schweden
* Nationale Identifikationsnummer
* Reisepassnummer

Taiwan 
* Nationale Identifikationsnummer
* Anwohnerzertifikatnummer (ARC/TARC)
* Reisepassnummer

Thailand
* Bürgeridentifikationscode

United Kingdom
* Reisepassnummer
* Führerscheinnummer
* Sozialversicherungsnummer (NINO)
* Gesundheitsdienstnummer (NHS)

USA
* US-Sozialversicherungsnummer (Social Security Number, SSN)
* Führerscheinnummer
* Reisepassnummer
* Wählerverzeichnisnummer
* Kennungsnummer für Steuerzahler (ITIN)
* DEA-Nummer
* Kontonummern (beispielsweise Giro-, Spar- und Debitkonten)
