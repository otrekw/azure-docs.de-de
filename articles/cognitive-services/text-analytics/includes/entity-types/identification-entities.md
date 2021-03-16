---
title: Identifikationsentitäten
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/17/2021
ms.author: aahi
ms.openlocfilehash: a376b050d79709885e3542d330bb6b1eea48d046
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750282"
---
### <a name="financial-account-identification"></a>Identifikation bei Finanzkonten

Diese Entitätskategorie umfasst Finanzinformationen und offizielle Formen der Identifikation.

#### <a name="category-aba-routing-number"></a>Kategorie: ABA-Nummer

Diese Kategorie enthält die folgende Entität:

:::row:::
    :::column span="":::
        **Entität**

        ABA-Nummer

    :::column-end:::
    :::column span="2":::
        **Details**

        ABA-Nummern (American Bankers Association) für den Zahlungsverkehr mit US-amerikanischen Banken.
      
    :::column-end:::
:::row-end:::

#### <a name="category-swift-code"></a>Kategorie: SWIFT-Code

Diese Kategorie enthält die folgende Entität:

:::row:::
    :::column span="":::
        **Entität**

        SWIFT-Code

    :::column-end:::
    :::column span="2":::
        **Details**

        SWIFT-Codes in Zahlungsanweisungen.
      
    :::column-end:::
:::row-end:::

#### <a name="category-credit-card"></a>Kategorie: Kreditkarte

Diese Kategorie enthält die folgende Entität:

:::row:::
    :::column span="":::
        **Entität**

        Kreditkarte

    :::column-end:::
    :::column span="2":::
        **Details**

        Kreditkartennummern 
      
    :::column-end:::
:::row-end:::

#### <a name="category-international-banking-account-number-iban"></a>Kategorie: IBAN (International Banking Account Number) 

Diese Kategorie enthält die folgende Entität:

:::row:::
    :::column span="":::
        **Entität**

        Kreditkarte

    :::column-end:::
    :::column span="2":::
        **Details**

        IBAN-Codes in Zahlungsanweisungen.
      
    :::column-end:::
:::row-end:::

### <a name="government-and-countryregion-specific-identification"></a>Behördliche und landes-/regionsspezifische Informationen zur Identifikation

> [!NOTE]
> Die folgenden finanziellen und länderspezifischen Entitäten werden mit dem Parameter `domain=phi` nicht zurückgegeben:
> * Reisepassnummern
> * Steuernummern

Die folgenden Entitäten sind nach Land/Region gruppiert und aufgelistet:

#### <a name="argentina"></a>Argentinien

:::row:::
    :::column span="":::
        **Entität**

        Nationale argentinische ID-Nummer (DNI)

    :::column-end:::
:::row-end:::


#### <a name="austria"></a>Österreich

:::row:::
    :::column span="":::
        **Entität**

        Österreichischer Personalausweis

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Österreichische Abgabenkontonummer

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Österreichische Umsatzsteuer-Identifikationsnummer

    :::column-end:::
:::row-end:::



#### <a name="australia"></a>Australien

:::row:::
    :::column span="":::
        **Entität**

        Australische Bankkontonummer

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Australische Geschäftsnummer

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Australische Unternehmensnummer

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Australischer Führerschein  

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Australische Krankenversicherungsnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Australische Reisepassnummer

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Australische Reisepassnummer

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Australische Steuernummer

    :::column-end:::

:::row-end:::


#### <a name="belgium"></a>Belgien

:::row:::
    :::column span="":::
        **Entität**

        Belgische Nationalregisternummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Belgische Umsatzsteuer-Identifikationsnummer

    :::column-end:::

:::row-end:::


#### <a name="brazil"></a>Brasilien 

:::row:::
    :::column span="":::
        **Entität**

        Brasilianische Nummer juristischer Personen (CNPJ)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Brasilianische CPF-Nummer

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Nationale brasilianische ID-Karte (RG)

    :::column-end:::
:::row-end:::

#### <a name="canada"></a>Canada

:::row:::
    :::column span="":::
        **Entität**

        Kanadische Bankkontonummer

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Kanadische Führerscheinnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Nummer des kanadischen Gesundheitsdiensts

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Kanadische Reisepassnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Persönliche kanadische Krankenversicherungsnummer (PHIN, Personal Health Identification Number)

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Kanadische Sozialversicherungsnummer

    :::column-end:::
:::row-end:::

#### <a name="chile"></a>Chile 

:::row:::
    :::column span="":::
        **Entität**

        Chilenische Personalausweisnummer

    :::column-end:::
:::row-end:::

#### <a name="china"></a>China

:::row:::
    :::column span="":::
        **Entität**

        Personalausweisnummer für Personen mit Wohnsitz in China (VRC)

    :::column-end:::
:::row-end:::


#### <a name="european-union-eu"></a>Europäische Union (EU)

:::row:::
    :::column span="":::
        **Entität**

        EU-Debitkartennummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        EU-Führerscheinnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        EU-Personalausweisnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        EU-Reisepassnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        EU-Sozialversicherungsnummer (SSN) oder entsprechende ID

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        EU-Steueridentifikationsnummer (TIN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        EU-GPS-Koordinaten

    :::column-end:::
:::row-end:::

#### <a name="france"></a>Frankreich

:::row:::
    :::column span="":::
        **Entität**

        Französische Führerscheinnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Französische Krankenversicherungsnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Französischer Personalausweis (Carte Nationale d'Identité, CNI)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Französische Reisepassnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Französische Sozialversicherungsnummer (NIRPP)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Französische Steueridentifikationsnummer (SPI)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Französische Umsatzsteuer-Identifikationsnummer

    :::column-end:::
:::row-end:::

#### <a name="germany"></a>Deutschland

:::row:::
    :::column span="":::
        **Entität**

        Deutsche Führerscheinnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Deutsche Personalausweisnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Deutsche Reisepassnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Deutsche Steueridentifikationsnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Deutsche Umsatzsteuer-Identifikationsnummer

    :::column-end:::
:::row-end:::

#### <a name="hong-kong"></a>Hongkong

:::row:::
    :::column span="":::
        **Entität**

        ID-Kartennummer aus Hongkong (HKID)

    :::column-end:::
:::row-end:::

#### <a name="hungary"></a>Ungarn

:::row:::
    :::column span="":::
        **Entität**

        Ungarische persönliche ID-Nummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Ungarische Steueridentifikationsnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Ungarische Umsatzsteuer-Identifikationsnummer

    :::column-end:::
:::row-end:::

#### <a name="india"></a>Indien

:::row:::
    :::column span="":::
        **Entität**

        Indische PAN (Permanent Account Number)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Indische eindeutige ID-Nummer (Aadhaar)

    :::column-end:::
:::row-end:::


#### <a name="indonesia"></a>Indonesien

:::row:::
    :::column span="":::
        **Entität**

        Indonesische ID-Kartennummer (KTP)

    :::column-end:::
:::row-end:::

#### <a name="ireland"></a>Irland

:::row:::
    :::column span="":::
        **Entität**

        Irische Sozialversicherungsnummer (PPS, Personal Public Service)

    :::column-end:::
:::row-end:::

#### <a name="israel"></a>Israel

:::row:::
    :::column span="":::
        **Entität**

        Nationale israelische ID-Nummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Israelische Bankkontonummer

    :::column-end:::
:::row-end:::

#### <a name="italy"></a>Italien

:::row:::
    :::column span="":::
        **Entität**

        Italienische Führerscheinnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Italienische Steuernummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Italienische Umsatzsteuer-Identifikationsnummer

    :::column-end:::
:::row-end:::


#### <a name="japan"></a>Japan

:::row:::
    :::column span="":::
        **Entität**

        Japanische Bankkontonummer

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Japanische Führerscheinnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japanische „Meine Nummer“ (persönlich)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japanische „Meine Nummer“ (geschäftlich)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japanische Melderegisternummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japanische Personalausweisnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japanische Sozialversicherungsnummer (SIN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japanische Reisepassnummer

    :::column-end:::
:::row-end:::

#### <a name="luxembourg"></a>Luxemburg

:::row:::
    :::column span="":::
        **Entität**

        Luxemburgische nationale Identifikationsnummer (natürliche Personen)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Luxemburgische nationale Identifikationsnummer (juristische Personen)

    :::column-end:::
:::row-end:::

#### <a name="malta"></a>Malta

:::row:::
    :::column span="":::
        **Entität**

        Maltesische ID-Kartennummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Maltesische Steueridentifikationsnummer

    :::column-end:::
:::row-end:::


#### <a name="new-zealand"></a>Neuseeland

:::row:::
    :::column span="":::
        **Entität**

        Neuseeländische Bankkontonummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Neuseeländische Führerscheinnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Neuseeländische Steuernummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Neuseeländische Nummer des Gesundheitsministeriums

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Neuseeländische Sozialversicherungsnummer

    :::column-end:::
:::row-end:::


#### <a name="philippines"></a>Philippinen

:::row:::
    :::column span="":::
        **Entität**

        Philippinische einheitliche Mehrzweck-ID-Nummer (Unified Multi-Purpose ID)

    :::column-end:::
:::row-end:::

#### <a name="portugal"></a>Portugal 

:::row:::
    :::column span="":::
        **Entität**

        Portugiesische ID-Kartennummer (Citizen Card)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Portugiesische Steueridentifikationsnummer

    :::column-end:::
:::row-end:::

#### <a name="singapore"></a>Singapur

:::row:::
    :::column span="":::
        **Entität**

        Singapur: Nationale Registrierungs-ID-Kartennummer (NRIC)

    :::column-end:::
:::row-end:::


#### <a name="south-africa"></a>Südafrika

:::row:::
    :::column span="":::
        **Entität**

        Südafrikanische ID-Nummer

    :::column-end:::
:::row-end:::


#### <a name="south-korea"></a>Südkorea

:::row:::
    :::column span="":::
        **Entität**

        Südkoreanische Einwohnerregistrierungsnummer (Resident Registration Number)

    :::column-end:::
:::row-end:::

#### <a name="spain"></a>Spanien

:::row:::
    :::column span="":::
        **Entität**

        Spanischer Personalausweis (DNI)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Spanische Sozialversicherungsnummer (SSN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Spanische Steueridentifikationsnummer

    :::column-end:::
:::row-end:::
 
#### <a name="switzerland"></a>Schweiz

:::row:::
    :::column span="":::
        **Entität**

        Schweizerische Sozialversicherungsnummer (AHV)

    :::column-end:::
:::row-end:::


#### <a name="taiwan"></a>Taiwan 

:::row:::
    :::column span="":::
        **Entität**

        Nationale taiwanesische ID-Nummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Taiwanesisches Einwohnerzertifikat (ARC/TARC, Resident Certificate)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Taiwanesische Reisepassnummer

    :::column-end:::
:::row-end:::

#### <a name="united-kingdom"></a>United Kingdom

:::row:::
    :::column span="":::
        **Entität**

        UK- Führerscheinnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       UK- Wählerverzeichnisnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       UK- National Health Service-Nummer (NHS)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       UK- Sozialversicherungsnummer (NINO)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       UK- oder US- Reisepassnummer

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

       Vereinigtes Königreich: Eindeutige Steuerzahlerreferenznummer

    :::column-end:::

:::row-end:::


#### <a name="united-states"></a>USA

:::row:::
    :::column span="":::
        **Entität**

        USA US-Sozialversicherungsnummer (Social Security Number, SSN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       USA Führerscheinnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       US- oder UK- Reisepassnummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       USA Steuernummer (Individual Taxpayer Identification Number, ITIN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       USA DEA-Nummer

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       USA Bankkontonummer

    :::column-end:::
:::row-end:::
