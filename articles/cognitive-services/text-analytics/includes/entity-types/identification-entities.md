---
title: Identifikationsentitäten
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/29/2020
ms.author: aahi
ms.openlocfilehash: f07b71bf8996612798b87d32a21a15ec72db0b32
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140923"
---
Diese Entitätskategorie umfasst Finanzinformationen und offizielle Formen der Identifikation. Erst ab der Modellversion `2019-10-01` verfügbar. Die Untertypen sind im Folgenden aufgeführt. 

### <a name="financial-account-identification"></a>Identifikation bei Finanzkonten

| Name des Untertyps               | BESCHREIBUNG                                                                |
|----------------------------|----------------------------------------------------------------------------|
| ABA-Nummern        | ABA-Nummern (American Bankers Association) für den Zahlungsverkehr mit US-amerikanischen Banken.                  |
| SWIFT-Code                 | SWIFT-Codes in Zahlungsanweisungen.                           |
| Kreditkarte                | Kreditkartennummern                                                       |
| IBAN (International Banking Account Number)                  | IBAN-Codes in Zahlungsanweisungen.                            |


### <a name="government-and-countryregion-specific-identification"></a>Behördliche und landes-/regionsspezifische Informationen zur Identifikation

> [!NOTE]
> Die folgenden finanziellen und länderspezifischen Entitäten werden mit dem Parameter `domain=phi` nicht zurückgegeben:
> * Reisepassnummern
> * Steuernummern

Die nachstehenden Entitäten werden nach Land/Region gruppiert und aufgeführt:

Argentinien
* Nationale argentinische ID-Nummer (DNI)

Australien
* Australische Reisepassnummer
* Australische Steuernummer
* Australische Führerscheinnummer
* Australische Krankenversicherungsnummer
* Australische Bankkontonummer

Belgien
* Nationale belgische Nummer

Brasilien 
* Brasilianische Nummer juristischer Personen (CNPJ)
* Brasilianische CPF-Nummer
* Nationale brasilianische ID-Karte (RG)

Canada
* Kanadische Sozialversicherungsnummer
* Kanadische Führerscheinnummer
* Kanadische Bankkontonummer
* Kanadische Reisepassnummer
* Persönliche kanadische Krankenversicherungsnummer (PHIN, Personal Health Identification Number)
* Nummer des kanadischen Gesundheitsdiensts

Chile
* ID-Kartennummer 

China
* Chinesische Anwohner-ID-Kartennummer (VRC)

Kroatien
* Kroatische ID-Kartennummer
* Kroatische persönliche ID-Nummer (OIB)

Tschechische Republik
* Tschechische persönliche Identifikationsnummer

Dänemark
* Dänische persönliche ID-Nummer

Europäische Union (EU)
* EU-Personalausweisnummer
* EU-Reisepassnummer
* EU-Führerscheinnummer
* EU-Sozialversicherungsnummer (SSN) oder entsprechende ID
* EU-Steueridentifikationsnummer (TIN)
* EU-Debitkartennummer

Finnland
* Nationale finnische ID-Nummer
* Finnische Reisepassnummer

Frankreich
* Französischer Personalausweis (Carte Nationale d'Identité, CNI)
* Französische Sozialversicherungsnummer (NIRPP)
* Französische Reisepassnummer
* Französische Führerscheinnummer

Deutschland
* Deutsche Personalausweisnummer
* Deutsche Reisepassnummer
* Deutsche Führerscheinnummer

Griechenland 
* Griechische Personalausweisnummer

Hongkong
* ID-Kartennummer aus Hongkong (HKID)

Indien
* Indische PAN (Permanent Account Number)
* Indische eindeutige ID-Nummer (Aadhaar)

Indonesien
* Indonesische ID-Kartennummer (KTP)

Irland
* Irische Sozialversicherungsnummer (PPS, Personal Public Service)

Israel
* Nationale israelische ID-Nummer
* Israelische Bankkontonummer

Italien
* Italienische Führerscheinnummer

Japan
* Japanische Melderegisternummer
* Japanische Aufenthaltskartennummer
* Japanische Führerscheinnummer
* Sozialversicherungsnummer (SIN)
* Japanische Reisepassnummer
* Japanische Bankkontonummer

Malaysia
* Malaysische ID-Kartennummer

Niederlande
* Niederländische persönliche Identifikationsnummer (BSN)

Neuseeland
* Neuseeländische Nummer des Gesundheitsministeriums

Norwegen
* Norwegische ID-Nummer

Philippinen
* Philippinische einheitliche Mehrzweck-ID-Nummer (Unified Multi-Purpose ID)

Polen
* Polnische Identitätskarte
* Nationale polnische ID-Nummer (PESEL)
* Polnische Reisepassnummer

Portugal 
* Portugiesische ID-Kartennummer (Citizen Card)

Saudi-Arabien
* Nationale saudi-arabische ID-Nummer

Singapur
* Nationale Registrierungs-ID-Kartennummer (NRIC) für Singapur

Südafrika
* Südafrikanische ID-Nummer

Südkorea
* Südkoreanische Einwohnerregistrierungsnummer (Resident Registration Number)

Spanien 
* Spanische Sozialversicherungsnummer (SSN)

Schweden
* Nationale schwedische ID-Nummer
* Schwedische Reisepassnummer

Taiwan 
* Nationale taiwanesische ID-Nummer
* Taiwanesisches Einwohnerzertifikat (ARC/TARC, Resident Certificate)
* Taiwanesische Reisepassnummer

Thailand
* Thailändische Personenidentifizierungscode

United Kingdom
* Reisepassnummer
* UK- Führerscheinnummer
* UK- Sozialversicherungsnummer (NINO)
* UK- Gesundheitsdienstnummer
* UK- Wählerverzeichnisnummer
* US-/UK- Reisepassnummer

USA
* USA US-Sozialversicherungsnummer (Social Security Number, SSN)
* USA Führerscheinnummer
* US-/UK- Reisepassnummer
* USA Steuernummer (Individual Taxpayer Identification Number, ITIN)
* DEA-Nummer
* US-Bankkontonummer
