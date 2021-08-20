---
title: Oxford COVID-19 Government Response Tracker
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie das Dataset „Oxford COVID-19 Government Response Tracker“ in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: a39c8915027cddeb168f5bb0c63f915492ece398
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114444612"
---
# <a name="oxford-covid-19-government-response-tracker"></a>Oxford COVID-19 Government Response Tracker

Das [Dataset des Oxford-Trackers für Regierungsmaßnahmen angesichts COVID-19](https://github.com/OxCGRT/covid-policy-tracker/) (Oxford Covid-19 Government Response Tracker, OxCGRT) enthält systematische Informationen dazu, welche Regierungen wann welche Maßnahmen ergriffen haben.

Diese Informationen können Entscheidungsträgern und Bürgern helfen, Regierungsmaßnahmen konsistent zu interpretieren und die Anstrengungen im Kampf gegen die Pandemie unterstützen. Der OxCGRT erfasst systematisch Informationen zu verschiedenen gängigen Regierungsmaßnahmen, zeichnet diese im entsprechenden Umfang auf, um die Dimension der Regierungsmaßnahmen abzubilden, und fasst diese Bewertungen in einer Reihe von politischen Indizes zusammen.


[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="datasets"></a>Datasets

Geänderte Versionen des Datasets sind im CSV-, JSON-, JSONL- und Parquet-Format verfügbar. Sie werden täglich aktualisiert:
- https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_policy_tracker/latest/covid_policy_tracker.csv
- https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_policy_tracker/latest/covid_policy_tracker.json
- https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_policy_tracker/latest/covid_policy_tracker.jsonl
- https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_policy_tracker/latest/covid_policy_tracker.parquet

Allen geänderten Versionen wurden ISO-Ländercodes und Ladezeiten hinzugefügt. Außerdem wurden die Spaltennamen in Kleinbuchstaben umformatiert und mit Unterstrichen als Trennzeichen versehen.

Rohdaten: https://pandemicdatalake.blob.core.windows.net/public/raw/covid-19/covid_policy_tracker/latest/CovidPolicyTracker.csv

Vorgängerversionen der geänderten Daten und Rohdaten: https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_policy_tracker/ https://pandemicdatalake.blob.core.windows.net/public/raw/covid-19/covid_policy_tracker/

## <a name="data-volume"></a>Datenvolumen

Nach Stand des 8. Juni 2020 enthalten diese 27.919 Zeilen (CSV: 4,9 MB, JSON: 20,9 MB, JSONL: 20,8 MB, Parquet: 133,0 KB).

## <a name="data-source"></a>Datenquellen-

Die Daten stammen von: Thomas Hale, Sam Webster, Anna Petherick, Toby Phillips und Beatriz Kira. (2020). Oxford COVID-19 Government Response Tracker. [Blavatnik School of Government](https://www.bsg.ox.ac.uk/). Rohdaten werden täglich aus der [neuesten OxCGRT-CSV-Datei](https://github.com/OxCGRT/covid-policy-tracker/blob/master/data/OxCGRT_latest.csv) übernommen. Weitere Informationen zu diesem Dataset, einschließlich der Art der Datensammlung, finden Sie auf der [Antwortseite zum Government-Tracker](https://www.bsg.ox.ac.uk/research/research-projects/covid-19-government-response-tracker).

## <a name="data-quality"></a>Datenqualität
Der OxCGRT übernimmt keine Garantie für die Genauigkeit oder Aktualität der Daten. Weitere Informationen finden Sie in den [Hinweisen zur Datenqualität](https://github.com/OxCGRT/covid-policy-tracker#data-quality).

## <a name="license-and-use-rights-attribution"></a>Anerkennung der Lizenzbedingungen und Nutzungsrechte

Diese Daten sind unter der [Creative Commons Attribution 4.0 International License](https://github.com/OxCGRT/covid-policy-tracker/blob/master/LICENSE.txt) lizenziert.

Bitte wie folgt zitieren: Thomas Hale, Sam Webster, Anna Petherick, Toby Phillips und Beatriz Kira. (2020). Oxford COVID-19 Government Response Tracker. Blavatnik School of Government.

## <a name="contact"></a>Contact

Wenn Sie Fragen oder Feedback zu diesem oder anderen Datasets im COVID-19-Data Lake haben, wenden Sie sich an askcovid19dl@microsoft.com.

## <a name="columns"></a>Spalten

| Name                                  | Datentyp | Eindeutig | Beispielwerte            | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|---------------------------------------|-----------|--------|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| c1_flag                               | boolean   | 3      | True                       | Binärflag für die geografische Reichweite 0: gezielt 1: allgemeingültig Leer: keine Daten                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| c1_school_closing                     | double    | 5      | 3,0 2,0                    | Erfasst Schließungen von Schulen und Universitäten. 0: keine Maßnahmen 1: Schließung empfohlen 2: Schließung angeordnet (nur für bestimmte Einrichtungstypen, z. B. weiterführende oder ausschließlich öffentliche Schulen) 3: Schließung für alle Bildungseinrichtungen angeordnet Leer: keine Daten                                                                                                                                                                                                                                                                                                 |
| c2_flag                               | boolean   | 3      | True                       | Binärflag für die geografische Reichweite 0: gezielt 1: allgemeingültig Leer: keine Daten                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| c2_workplace_closing                  | double    | 5      | 2,0 1,0                    | Erfasst Arbeitsplatzschließungen. 0: keine Maßnahmen 1: Schließung (oder Arbeit im Homeoffice) empfohlen 2: Schließung (oder Arbeit im Homeoffice) für bestimmte Sektoren oder Arbeitnehmer angeordnet 3: Schließung (oder Arbeit im Homeoffice) aller nicht systemrelevanten Arbeitsplätze (ausgenommen sind z. B. Lebensmittelgeschäfte und Arztpraxen) angeordnet Leer: keine Daten                                                                                                                                                                                                                          |
| c3_cancel_public_events               | double    | 4      | 2,0 1,0                    | Erfasst abgesagte öffentliche Veranstaltungen 0: keine Maßnahmen 1: Absage empfohlen 2: Absage angeordnet Leer: keine Daten                                                                                                                                                                                                                                                                                                                                                                                                                    |
| c3_flag                               | boolean   | 3      | True                       | Binärflag für die geografische Reichweite 0: gezielt 1: allgemeingültig Leer: keine Daten                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| c4_flag                               | boolean   | 3      | True                       | Binärflag für die geografische Reichweite 0: gezielt 1: allgemeingültig Leer: keine Daten                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| c4_restrictions_on_gatherings         | double    | 6      | 4,0 3,0                    | Erfasst Beschränkungen für private Versammlungen. 0: keine Beschränkungen 1: Beschränkungen für Großversammlungen (mehr als 1.000 Personen) 2: Beschränkungen für Versammlungen mit 101 bis 1.000 Personen 3: Beschränkungen für Versammlungen mit 11 bis 100 Personen 4 : Beschränkungen für Versammlungen mit 10 oder weniger Personen Leer: keine Daten                                                                                                                                                                                                                 |
| c5_close_public_transport             | double    | 4      | 1,0 2,0                    | Erfasst Einstellungen des öffentlichen Nahverkehrs 0: keine Maßnahmen 1: Einstellung empfohlen (oder deutlich geringere Taktung/weniger Routen/weniger Transportmittel) 2: Einstellung angeordnet (oder Nutzung des öffentlichen Nahverkehrs für die meisten Personen verboten) Leer: keine Daten                                                                                                                                                                                                                                                                                                          |
| c5_flag                               | boolean   | 3      | True                       | Binärflag für die geografische Reichweite 0: gezielt 1: allgemeingültig Leer: keine Daten                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| c6_flag                               | boolean   | 3      | True                       | Binärflag für die geografische Reichweite 0: gezielt 1: allgemeingültig Leer: keine Daten                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| c6_stay_at_home_requirements          | double    | 5      | 1,0 2,0                    | Erfasst Anordnungen zur Selbstquarantäne und allgemein zum Zuhausebleiben 0: keine Maßnahmen 1: Zuhausebleiben empfohlen 2: Zuhausebleiben angeordnet, außer für Sport, Lebensmitteleinkäufe und notwendige Reisen 3: Zuhausebleiben mit minimalen Ausnahmen (z. B. einmal die Woche, jeweils eine Person darf den Haushalt verlassen usw.) angeordnet Leer: keine Daten                                                                                                                                                |
| c7_flag                               | boolean   | 3      | True                       | Binärflag für die geografische Reichweite 0: gezielt 1: allgemeingültig Leer: keine Daten                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| c7_restrictions_on_internal_movement  | double    | 4      | 2,0 1,0                    | Erfasst Beschränkungen für landesinterne Reisen in andere Städte/Regionen 0: keine Maßnahmen 1: von Reisen in andere Städte/Regionen wird abgeraten 2: Beschränkungen für landesinterne Reisen in Kraft Leer: keine Daten                                                                                                                                                                                                                                                                                                                                      |
| c8_international_travel_controls      | double    | 6      | 3,0 4,0                    | Erfasst Einschränkungen des internationalen Reiseverkehrs. Hinweis: Dieses Dataset erfasst die politischen Maßnahmen für ausländische Reisende, nicht für Staatsbürger. 0: keine Einschränkungen 1: Untersuchung bei der Einreise 2: Quarantäne nach der Einreise für bestimmte oder alle Ursprungsregionen 3: Einreise aus bestimmten Regionen verboten 4: Einreise aus allen Regionen verboten oder vollständige Grenzschließung Leer: keine Daten                                                                                                                                                                                                                           |
| confirmedcases                        | SMALLINT  | 18.238 | 1 2                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| confirmeddeaths                       | SMALLINT  | 14.906 | 1 2                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| countrycode                           | Zeichenfolge    | 186    | USA BRA                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| countryname                           | Zeichenfolge    | 186    | USA, Brasilien       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| date                                  | date      | 478    | 2020-08-25 2021-03-30      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| e1_flag                               | boolean   | 3      | True                       | Binärflag für den sektoralen Anwendungsbereich 0: formeller Sektor, nur Arbeitnehmer 1: Transfers zum informellen Sektor, auch Arbeitnehmer Leer: keine Daten                                                                                                                                                                                                                                                                                                                                                                                                          |
| e1_income_support                     | double    | 4      | 1,0 2,0                    | Erfasst, ob Regierungsbehörden die finanzielle Unterstützung für Personen direkt ausbezahlen, die ihre Stelle verlieren oder nicht arbeiten können/dürfen. Hinweis: Zahlungen an Unternehmen werden nur aufgenommen, wenn diese explizit an eine Gehaltsliste/Gehälter geknüpft sind. 0: keine Einkommensunterstützung 1: die Regierung stockt weniger als 50 % des verlorenen Gehalts auf (oder bei einem Pauschalbetrag weniger als 50 % des durchschnittlichen Gehalts) 2: die Regierung stockt 50 % oder mehr des verlorenen Gehalts auf (oder bei einem Pauschalbetrag mehr als 50 % des durchschnittlichen Gehalts) Leer: keine Daten                                                                        |
| e2_debt/contract_relief               | double    | 4      | 1,0 2,0                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| e3_fiscal_measures                    | double    | 819    | –0,01 3,0                  | Angekündigte Investitionen in Konjunkturmaßnahmen Hinweis: Nur zusätzlich zu bereits angekündigten Ausgaben zugesagte Geldmittel werden erfasst (Geldbeträge der Konjunkturmittel in USD, inklusive aller Ausgaben oder Steuersenkungen, die NICHT in E4, H4 oder H5 enthalten sind). 0: keine neuen Ausgaben am betreffenden Tag Leer: keine Daten                                                                                                                                                                                                                                                               |
| e4_international_support              | double    | 113    | –0,02 5000000,0            | Ankündigung von angebotenen, COVID-19-bezogenen Hilfszahlungen an andere Länder Hinweis: Nur zusätzlich zu den bereits angekündigten Ausgaben zugesagte Geldmittel werden erfasst (Geldbeträge in USD). 0: keine neuen Ausgaben am betreffenden Tag Leer: keine Daten                                                                                                                                                                                                                                                                                                                 |
| h1_flag                               | boolean   | 3      | True                       | Binärflag für die geografische Reichweite 0: gezielt 1: allgemeingültig Leer: keine Daten                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| h1_public_information_campaigns       | double    | 4      | 2,0 1,0                    | Erfasst öffentliche Informationskampagnen 0: keine öffentlichen Informationskampagnen zu COVID-19 1: Behörden mahnen aufgrund von COVID-19 zur Vorsicht 2: koordinierte öffentliche Informationskampagnen (z. B. in traditionellen und sozialen Medien) Leer: keine Daten                                                                                                                                                                                                                                                                                         |
| h2_testing_policy                     | double    | 5      | 2,0 1,0                    | Erfasst die politische Strategie der Regierungsbehörden im Hinblick auf den Zugang zu Tests Hinweis: Hier werden politische Strategien für Tests auf eine vorliegende Infektion (PCR-Tests) erfasst, nicht jedoch für Immunitätstests (Antikörpertests) 0: keine Teststrategie 1: nur für Personen, die (a) Symptome zeigen und (b) bestimmte Kriterien erfüllen (z. B. Schlüsselkraft, ins Krankenhaus eingewiesen, in Kontakt mit einem nachweislich Infizierten gekommen, aus Übersee zurückgekehrt) 2: Tests für alle Personen mit COVID-19-Symptomen 3: frei zugängliche öffentliche Tests (z. B. „Drive-through-Tests“, die auch Personen ohne Symptome in Anspruch nehmen können) Leer: keine Daten |
| h3_contact_tracing                    | double    | 4      | 2,0 1,0                    | Erfasst die politische Strategie der Regierungsbehörden für die Kontaktverfolgung nach einer positiven Diagnose Hinweis: Hiermit sind politischen Maßnahmen gemeint, mit denen sich alle Personen identifizieren lassen, die potenziell mit COVID-19 infiziert sind. Freiwillige Bluetooth-Apps erfüllen diesen Zweck höchstwahrscheinlich nicht. 0: keine Kontaktverfolgung 1: eingeschränkte Kontaktverfolgung, nicht für alle Fälle 2: umfassende Kontaktverfolgung, für alle identifizierten Fälle                                                                                                                                                          |
| h4_emergency_investment_in_healthcare | double    | 462    | 35,0 562,0                 | Ankündigung von kurzfristigen Investitionen in das Gesundheitssystem, z. B. für Krankenhäuser oder in Gesichtsmasken. Hinweis: Nur zusätzlich zu den bereits angekündigten Ausgaben zugesagte Geldmittel werden erfasst (Geldbeträge in USD). 0: keine neuen Ausgaben am betreffenden Tag Leer: keine Daten                                                                                                                                                                                                                                                                                                         |
| h5_investment_in_vaccines             | double    | 133    | 1,0 191,0                  | Angekündigte öffentliche Investitionen in die Entwicklung einer COVID-19-Impfung Hinweis: Nur zusätzlich zu den bereits angekündigten Ausgaben zugesagte Geldmittel werden erfasst (Geldbeträge in USD). 0: keine neuen Ausgaben am betreffenden Tag Leer: keine Daten                                                                                                                                                                                                                                                                                                                            |
| iso_country                           | Zeichenfolge    | 186    | US BR                      | Landes- oder Regionscode gemäß ISO 3166                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| load_date                             | timestamp | 1      | 2021-04-26 00:06:25.157000 | Die Datums- und Uhrzeitangaben wurden aus einer externen Quelle geladen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| stringencyindex                       | double    | 188    | 11,11 60,19                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| stringencyindexfordisplay             | double    | 188    | 11,11 60,19                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

## <a name="preview"></a>Vorschau

| countryname | countrycode | date       | c1_school_closing | c2_workplace_closing | c3_cancel_public_events | c4_restrictions_on_gatherings | c5_close_public_transport | c6_stay_at_home_requirements | c7_restrictions_on_internal_movement | c8_international_travel_controls | e1_income_support | e2_debt/contract_relief | e3_fiscal_measures | e4_international_support | h1_public_information_campaigns | h2_testing_policy | h3_contact_tracing | h4_emergency_investment_in_healthcare | h5_investment_in_vaccines | m1_wildcard | stringencyindex | stringencyindexfordisplay | iso_country | load_date             |
|-------------|-------------|------------|-------------------|----------------------|-------------------------|-------------------------------|---------------------------|------------------------------|--------------------------------------|----------------------------------|-------------------|-------------------------|--------------------|--------------------------|---------------------------------|-------------------|--------------------|---------------------------------------|---------------------------|-------------|-----------------|---------------------------|-------------|-----------------------|
| Aruba       | ABW         | 2020-01-01 | 0                 | 0                    | 0                       | 0                             | 0                         | 0                            | 0                                    | 0                                | 0                 | 0                       | 0                  | 0                        | 0                               | 0                 | 0                  | 0                                     | 0                         | NULL        | 0               | 0                         | AW          | 4/26/2021 12:06:25 AM |
| Aruba       | ABW         | 2020-01-02 | 0                 | 0                    | 0                       | 0                             | 0                         | 0                            | 0                                    | 0                                | 0                 | 0                       | 0                  | 0                        | 0                               | 0                 | 0                  | 0                                     | 0                         | NULL        | 0               | 0                         | AW          | 4/26/2021 12:06:25 AM |
| Aruba       | ABW         | 2020-01-03 | 0                 | 0                    | 0                       | 0                             | 0                         | 0                            | 0                                    | 0                                | 0                 | 0                       | 0                  | 0                        | 0                               | 0                 | 0                  | 0                                     | 0                         | NULL        | 0               | 0                         | AW          | 4/26/2021 12:06:25 AM |
| Aruba       | ABW         | 2020-01-04 | 0                 | 0                    | 0                       | 0                             | 0                         | 0                            | 0                                    | 0                                | 0                 | 0                       | 0                  | 0                        | 0                               | 0                 | 0                  | 0                                     | 0                         | NULL        | 0               | 0                         | AW          | 4/26/2021 12:06:25 AM |
| Aruba       | ABW         | 2020-01-05 | 0                 | 0                    | 0                       | 0                             | 0                         | 0                            | 0                                    | 0                                | 0                 | 0                       | 0                  | 0                        | 0                               | 0                 | 0                  | 0                                     | 0                         | NULL        | 0               | 0                         | AW          | 4/26/2021 12:06:25 AM |
| Aruba       | ABW         | 2020-01-06 | 0                 | 0                    | 0                       | 0                             | 0                         | 0                            | 0                                    | 0                                | 0                 | 0                       | 0                  | 0                        | 0                               | 0                 | 0                  | 0                                     | 0                         | NULL        | 0               | 0                         | AW          | 4/26/2021 12:06:25 AM |
| Aruba       | ABW         | 01.07.2020 | 0                 | 0                    | 0                       | 0                             | 0                         | 0                            | 0                                    | 0                                | 0                 | 0                       | 0                  | 0                        | 0                               | 0                 | 0                  | 0                                     | 0                         | NULL        | 0               | 0                         | AW          | 4/26/2021 12:06:25 AM |
| Aruba       | ABW         | 2020-01-08 | 0                 | 0                    | 0                       | 0                             | 0                         | 0                            | 0                                    | 0                                | 0                 | 0                       | 0                  | 0                        | 0                               | 0                 | 0                  | 0                                     | 0                         | NULL        | 0               | 0                         | AW          | 4/26/2021 12:06:25 AM |
| Aruba       | ABW         | 09.01.2020 | 0                 | 0                    | 0                       | 0                             | 0                         | 0                            | 0                                    | 0                                | 0                 | 0                       | 0                  | 0                        | 0                               | 0                 | 0                  | 0                                     | 0                         | NULL        | 0               | 0                         | AW          | 4/26/2021 12:06:25 AM |
| Aruba       | ABW         | 2020-01-10 | 0                 | 0                    | 0                       | 0                             | 0                         | 0                            | 0                                    | 0                                | 0                 | 0                       | 0                  | 0                        | 0                               | 0                 | 0                  | 0                                     | 0                         | NULL        | 0               | 0                         | AW          | 4/26/2021 12:06:25 AM |

## <a name="data-access"></a>Datenzugriff

### <a name="azure-notebooks"></a>Azure Notebooks

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=oxford-covid-19-government-response-tracker -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureNotebooks&package=azure-storage&registryId=oxford-covid-19-government-response-tracker)** .

## <a name="this-notebook-documents-the-urls-and-sample-code-to-access-the-oxford-covid-19-government-response-tracker-oxcgrt-dataset"></a>Dieses Notebook dokumentiert die URLs und den Beispielcode für den Zugriff auf das Dataset „Oxford COVID-19 Government Response Tracker“ (OxCGRT).

URLs verschiedener Dateiformate, die in Azure Blob Storage gehostet werden:

CSV: https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_policy_tracker/latest/covid_policy_tracker.csv 

JSON: https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_policy_tracker/latest/covid_policy_tracker.json

JSONL: https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_policy_tracker/latest/covid_policy_tracker.jsonl 

Parquet: https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_policy_tracker/latest/covid_policy_tracker.parquet

Laden Sie die Datasetdatei mithilfe des integrierten Funktionsdownloads in Pandas anhand der HTTP-URL herunter. Pandas verfügt über Reader für verschiedene Dateiformate:

https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_parquet.html

https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html


Laden Sie zunächst die Datasetdatei in einen Pandas-Datenrahmen, und zeigen Sie einige Beispielzeilen an.

```python
import pandas as pd
import numpy as np
%matplotlib inline
import matplotlib.pyplot as plt

df = pd.read_parquet("https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/covid_policy_tracker/latest/covid_policy_tracker.parquet")
df.head(10)
```

Überprüfen Sie die Datentypen der verschiedenen Felder, und ob die aktualisierte Spalte das datetime-Format aufweist.

```python
df.dtypes
```

Dieses Dataset enthält Daten für zahlreiche Länder. Überprüfen Sie, für welche Länder Daten verfügbar sind.

Zunächst sehen Sie sich die neuesten Daten für jedes Land an:

```python
df.groupby('countryname').first().filter(['confirmedcases ', 'confirmeddeaths','h5_investment_in_vaccines',
    'c6_stay_at_home_requirements','h4_emergency_investment_in_healthcare','c4_restrictions_on_gatherings', 'load_date'])
```

Als Nächstes erstellen Sie einige Aggregationen, um sicherzustellen, dass Spalten wie `confirmedcases` und `confirmeddeaths` mit den neuesten Daten übereinstimmen. Sie sollten sehen, dass die Zahlen für positive Testungen und Todesfälle für das letzte Datum in der obigen Tabelle mit der Aggregation von `confirmedcases` und `confirmeddeaths` übereinstimmen.


```python
df.groupby('countryname').agg({'countryname': 'count','confirmedcases': 'sum','confirmeddeaths': 'sum',
                               'h5_investment_in_vaccines': 'count', 'c6_stay_at_home_requirements':'sum'})
```

Einfache Visualisierungen für einige Länder

```python
import plotly.graph_objects as go
import plotly.express as px
import matplotlib.pyplot as plt

df.loc[: , ['countryname', 'confirmedcases', 
'confirmeddeaths']].groupby(['countryname']).max().sort_values(by='confirmedcases', 
                                           ascending=False).reset_index()[:15].style.background_gradient(cmap='rainbow')
```

```python
df_US = df.groupby(df.date).agg({'confirmedcases': 'sum','confirmeddeaths':'sum'}).reset_index()

df_US.plot(kind='line',x='date',y="confirmedcases",grid=True)
df_US.plot(kind='line',x='date',y="confirmeddeaths",grid=True)

```

<!-- nbend -->

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

---

### <a name="azure-databricks"></a>Azure Databricks

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=oxford-covid-19-government-response-tracker -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureDatabricks&package=pyspark&registryId=oxford-covid-19-government-response-tracker)** .

```python
# Azure storage access info
blob_account_name = "pandemicdatalake"
blob_container_name = "public"
blob_relative_path = "curated/covid-19/covid_policy_tracker/latest/covid_policy_tracker.parquet"
blob_sas_token = r""
```

```python
# Allow SPARK to read from Blob remotely
wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
spark.conf.set(
  'fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name),
  blob_sas_token)
print('Remote blob path: ' + wasbs_path)
```

```python
# SPARK read parquet, note that it won't load any data yet by now
df = spark.read.parquet(wasbs_path)
print('Register the DataFrame as a SQL temporary view: source')
df.createOrReplaceTempView('source')
```

```python
# Display top 10 rows
print('Displaying top 10 rows: ')
display(spark.sql('SELECT * FROM source LIMIT 10'))
```

<!-- nbend -->

---

### <a name="azure-synapse"></a>Azure Synapse

# <a name="azure-storage"></a>[azure-storage](#tab/azure-storage)

Für diese Kombination aus Plattform und Paket ist kein Beispiel verfügbar.

# <a name="pyspark"></a>[pyspark](#tab/pyspark)

<!-- nbstart https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=oxford-covid-19-government-response-tracker -->

> [!TIP]
> **[Laden Sie stattdessen das Notebook herunter](https://opendatasets-api.azure.com/discoveryapi/OpenDataset/DownloadNotebook?serviceType=AzureSynapse&package=pyspark&registryId=oxford-covid-19-government-response-tracker)** .

```python
# Azure storage access info
blob_account_name = "pandemicdatalake"
blob_container_name = "public"
blob_relative_path = "curated/covid-19/covid_policy_tracker/latest/covid_policy_tracker.parquet"
blob_sas_token = r""
```

```python
# Allow SPARK to read from Blob remotely
wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
spark.conf.set(
  'fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name),
  blob_sas_token)
print('Remote blob path: ' + wasbs_path)
```

```python
# SPARK read parquet, note that it won't load any data yet by now
df = spark.read.parquet(wasbs_path)
print('Register the DataFrame as a SQL temporary view: source')
df.createOrReplaceTempView('source')
```

```python
# Display top 10 rows
print('Displaying top 10 rows: ')
display(spark.sql('SELECT * FROM source LIMIT 10'))
```

<!-- nbend -->

---

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.
