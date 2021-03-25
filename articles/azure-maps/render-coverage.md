---
title: Renderabdeckung | Microsoft Azure Maps
description: Erfahren Sie, wie Azure Maps verschiedene Regionen mit detaillierten oder vereinfachten Daten rendert. Lernen Sie, welche Ebene für Raster-Kachel- und Vektor-Kachel-Zuordnungen in diesen Regionen verwendet wird.
author: anastasia-ms
ms.author: v-stharr
ms.date: 03/22/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 69b122196cbeb7508cddf3a890768f83fdcd4727
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88030927"
---
# <a name="azure-maps-render-coverage"></a>Azure Maps-Renderabdeckung

Azure Maps verwendet sowohl Raster- als auch Vektorkacheln zum Erstellen von Karten. Bei der niedrigsten Auflösung passt die ganze Welt auf eine einzige Kachel. Bei der höchsten Auflösung werden mit einer einzelnen Kachel 38 Quadratmeter abgebildet. Wenn Sie die Ansicht vergrößern, werden weitere Details zu Kontinenten, Regionen, Orten und einzelnen Straßen angezeigt. Weitere Informationen zu Kacheln finden Sie unter [Zoomfaktoren und Kachelraster](zoom-levels-and-tile-grid.md).

Maps bietet allerdings nicht für alle Regionen das gleiche Maß an Informationen und Genauigkeit. In den folgenden Tabellen ist der Informationsgrad angegeben, den Sie für die einzelnen Regionen rendern können.

## <a name="legend"></a>Legende

| Symbol | Bedeutung |
|--------|---------|
| ✓ | Region wird mit detaillierten Daten dargestellt.   |
| Ø | Region wird mit vereinfachten Daten dargestellt. |


## <a name="africa"></a>Afrika 


| Land/Region | Rasterkacheln vereinheitlicht | Vektorkacheln vereinheitlicht |
| ------ | :------------------: | :------------------: |
| Algerien                          | ✓ | ✓ |
| Angola                           | ✓ | ✓ |
| Benin                            | ✓ | ✓ |
| Botsuana                         | ✓ | ✓ |
| Burkina Faso                     | ✓ | ✓ |
| Burundi                          | ✓ | ✓ |
| Cabo Verde                       | ✓ | ✓ |
| Kamerun                         | ✓ | ✓ |
| Zentralafrikanische Republik         | ✓ | Ø |
| Tschad                             | ✓ | Ø |
| Komoren                          | ✓ | Ø |
| Demokratische Republik Kongo | ✓ | ✓ |
| Côte d'Ivoire                    | ✓ | Ø |
| Dschibuti                         | ✓ | Ø |
| Ägypten                            | ✓ | ✓ |
| Äquatorialguinea                | ✓ | Ø |
| Eritrea                          | ✓ | Ø |
| Äthiopien                         | ✓ | Ø |
| Gabun                            | ✓ | ✓ |
| Gambia                           | ✓ | Ø |
| Ghana                            | ✓ | ✓ |
| Guinea                           | ✓ | Ø |
| Guinea-Bissau                    | ✓ | Ø |
| Kenia                            | ✓ | ✓ |
| Lesotho                          | ✓ | ✓ |
| Liberia                          | ✓ | Ø |
| Libyen                            | ✓ | Ø |
| Madagaskar                       | ✓ | Ø |
| Malawi                           | ✓ | ✓ |
| Mali                             | ✓ | ✓ |
| Mauretanien                       | ✓ | ✓ |
| Mauritius                        | ✓ | ✓ |
| Mayotte                          | ✓ | ✓ |
| Marokko                          | ✓ | ✓ |
| Mosambik                       | ✓ | ✓ |
| Namibia                          | ✓ | ✓ |
| Niger                            | ✓ | ✓ |
| Nigeria                          | ✓ | ✓ |
| Réunion                          | ✓ | ✓ |
| Ruanda                           | ✓ | ✓ |
| St. Helena, Ascension und Tristan da Cunha | ✓ | Ø |
| São Tomé und Príncipe            | ✓ | Ø |
| Senegal                          | ✓ | ✓ |
| Sierra Leone                     | ✓ | ✓ |
| Somalia                          | ✓ | ✓ |
| Südafrika                     | ✓ | ✓ |
| Südsudan                      | ✓ | ✓ |
| Sudan                            | ✓ | ✓ |
| Swasiland                        | ✓ | ✓ |
| Vereinigte Republik Tansania      | ✓ | ✓ |
| Togo                             | ✓ | ✓ |
| Tunesien                          | ✓ | ✓ |
| Uganda                           | ✓ | ✓ |
| Sambia                           | ✓ | ✓ |
| Simbabwe                         | ✓ | ✓ |

## <a name="americas"></a>Amerika

| Land/Region | Rasterkacheln vereinheitlicht | Vektorkacheln vereinheitlicht |
| ------ | :------------------: | :------------------: |
| Anguilla                  | ✓ | ✓ |
| Antigua und Barbuda       | ✓ | ✓ |
| Argentinien                 | ✓ | ✓ |
| Aruba                     | ✓ | ✓ |
| Bahamas                   | ✓ | ✓ |
| Barbados                  | ✓ | ✓ |
| Belize                    | ✓ | ✓ |
| Bermudas                   | ✓ | ✓ |
| Plurinationaler Staat Bolivien | ✓ | ✓ |
| Bonaire, Sint Eustatius und Saba | ✓ | ✓ |
| Brasilien                    | ✓ | ✓ |
| Canada                    | ✓ | ✓ |
| Kaimaninseln            | ✓ | ✓ |
| Chile                     | ✓ | ✓ |
| Kolumbien                  | ✓ | ✓ |
| Costa Rica                | ✓ | ✓ |
| Kuba                      | ✓ | ✓ |
| Curaçao                   | ✓ | ✓ |
| Dominica                  | ✓ | ✓ |
| Dominikanische Republik        | ✓ | ✓ |
| Ecuador                   | ✓ | ✓ |
| Falklandinseln | ✓ | ✓ |
| Französisch-Guayana             | ✓ | ✓ |
| Grönland                 | ✓ | Ø |
| Grenada                   | ✓ | ✓ |
| Guadeloupe                | ✓ | ✓ |
| Guatemala                 | ✓ | ✓ |
| Guayana                    | ✓ | ✓ |
| Haiti                     | ✓ | ✓ |
| Honduras                  | ✓ | ✓ |
| Jamaika                   | ✓ | ✓ |
| Martinique                | ✓ | ✓ |
| Mexiko                    | ✓ | ✓ |
| Montserrat                | ✓ | ✓ |
| Nicaragua                 | ✓ | ✓ |
| Nördliche Marianen  | ✓ | ✓ |
| Panama                    | ✓ | ✓ | 
| Paraguay                  | ✓ | ✓ |
| Peru                      | ✓ | ✓ |
| Puerto Rico               | ✓ | ✓ |
| Quebec (Kanada)           | ✓ | ✓ |
| St. Barthélemy          | ✓ | ✓ |
| St. Kitts und Nevis     | ✓ | ✓ |
| St. Lucia               | ✓ | ✓ |
| St. Martin (französisch)     | ✓ | ✓ |
| St. Pierre und Miquelon | ✓ | ✓ |
| St. Vincent und die Grenadinen | ✓ | ✓ |
| Sint Maarten (niederländisch)      | ✓ | ✓ |
| Südgeorgien und Südliche Sandwichinseln | ✓ | ✓ |
| Surinam                  | ✓ | ✓ |
| Trinidad und Tobago       | ✓ | ✓ |
| Turks- und Caicosinseln  | ✓ | ✓ |
| USA             | ✓ | ✓ |
| Uruguay                   | ✓ | ✓ |
| Venezuela                 | ✓ | ✓ |
| Britische Jungferninseln   | ✓ | ✓ |
| Amerikanische Jungferninseln      | ✓ | ✓ |

## <a name="asia"></a>Asien 

| Land/Region | Rasterkacheln vereinheitlicht | Vektorkacheln vereinheitlicht |
| ------ | :------------------: | :------------------: |
| Afghanistan               |   | Ø |
| Bahrain                   | ✓ | ✓ |
| Bangladesch                |   | Ø |
| Bhutan                    |   | Ø |
| Britisches Territorium im Indischen Ozean |   | Ø |
| Brunei                    | ✓ | ✓ |
| Kambodscha                  |   | Ø |
| China                     |   | Ø |
| Kokosinseln   |   | Ø |
| Demokratische Volksrepublik Korea |   | Ø |
| Hongkong (SAR)             | ✓ | ✓ |
| Indien                     | Ø | ✓ | 
| Indonesien                 | ✓ | ✓ |
| Iran                      |   | Ø |
| Irak                      | ✓ | ✓ |
| Israel                    |   | ✓ |
| Japan                     |   | Ø |
| Jordan                    | ✓ | ✓ |
| Kasachstan                |   | ✓ |
| Kuwait                    | ✓ | ✓ |
| Kirgisistan                |   | Ø |
| Demokratische Volksrepublik Laos |   | Ø |
| Libanon                   | ✓ | ✓ |
| Macau (SAR)                 | ✓ | ✓ |
| Malaysia                  | ✓ | ✓ |
| Malediven                  |   | Ø |
| Mongolei                  |   | Ø |
| Myanmar                   |   | Ø |
| Nepal                     |   | Ø |
| Oman                      | ✓ | ✓ |
| Pakistan                  |   | Ø |
| Philippinen               | ✓ | ✓ |
| Katar                     | ✓ | ✓ |
| Republik Korea         | ✓ | Ø |
| Saudi-Arabien              | ✓ | ✓ |
| Senkaku-Inseln/Diaoyutai-Inseln           |   | ✓ |
| Singapur                 | ✓ | ✓|
| Sri Lanka                 |   | Ø |
| Arabische Republik Syrien      |   | Ø |
| Taiwan                    | ✓ | ✓ |
| Tadschikistan                |   | Ø |
| Thailand                  | ✓ | ✓ |
| Timor-Leste               |   | Ø |
| Turkmenistan              |   | Ø |
| Vereinigte Arabische Emirate      | ✓ | ✓ |
| Kleinere Amerikanische Überseeinseln |   | Ø |
| Usbekistan                |   | Ø |
| Vietnam                   | ✓ | ✓ |
| Jemen                     | ✓ | ✓ |

## <a name="oceania"></a>Ozeanien

| Land/Region | Rasterkacheln vereinheitlicht | Vektorkacheln vereinheitlicht |
| ------ | :------------------: | :------------------: |
| Amerikanisch-Samoa            |   | ✓ |
| Australien                 | ✓ | ✓ |
| Cookinseln              |   | Ø |
| Fidschi                      |   | Ø |
| Französisch-Polynesien          |   | Ø |
| Guam                      | ✓ | ✓ |
| Kiribati                  |   | Ø |
| Marshall-Inseln          |   | Ø |
| Mikronesien                |   | Ø |
| Nauru                     |   | Ø |
| Neukaledonien             |   | Ø |
| Neuseeland               | ✓ | ✓ |
| Niue                      |   | Ø |
| Norfolkinsel            |   | Ø |
| Palau                     |   | Ø |
| Papua-Neuguinea          |   | Ø |
| Pitcairn                  |   | Ø |
| Samoa                     |   | Ø |
| Salomonen           |   | Ø|
| Tokelau                   |   | Ø |
| Tonga                     |   | Ø |
| Tuvalu                    |   | Ø |
| Vanuatu                   |   | Ø |
| Wallis und Futuna         |   | Ø |


## <a name="europe"></a>Europa

| Land/Region | Rasterkacheln vereinheitlicht | Vektorkacheln vereinheitlicht |
| ------ | :------------------: | :------------------: |
| Albanien                   | ✓ | ✓ |
| Andorra                   | ✓ | ✓ |
| Armenien                   | ✓ | Ø |
| Österreich                   | ✓ | ✓ |
| Aserbaidschan                | ✓ | Ø |
| Belarus                   | Ø | ✓ |
| Belgien                   | ✓ | ✓ |
| Bosnien und Herzegowina        | ✓ | ✓ |
| Bulgarien                  | ✓ | ✓ |
| Kroatien                   | ✓ | ✓ |
| Zypern                    | ✓ | ✓ |
| Tschechische Republik            | ✓ | ✓ |
| Dänemark                   | ✓ | ✓ |
| Estland                   | ✓ | ✓ |
| Färöer             | ✓ | Ø |
| Finnland                   | ✓ | ✓ |
| Frankreich                    | ✓ | ✓ |
| Georgien                   | ✓ | Ø |
| Deutschland                   | ✓ | ✓ |
| Gibraltar                 | ✓ | ✓ |
| Griechenland                    | ✓ | ✓ |
| Guernsey                  | ✓ | ✓ |
| Ungarn                   | ✓ | ✓ |
| Island                   | ✓ | ✓ |
| Irland                   | ✓ | ✓ |
| Insel Man               | ✓ | ✓ |
| Italien                     | ✓ | ✓ |
| Jan Mayen                 | ✓ | ✓ |
| Jersey                    | ✓ | ✓ |
| Lettland                    | ✓ | ✓ |
| Liechtenstein             | ✓ | ✓ |
| Litauen                 | ✓ | ✓ |
| Luxemburg                | ✓ | ✓ |
| Nordmazedonien           | ✓ | ✓ |
| Malta                     | ✓ | ✓ |
| Moldau                   | ✓ | ✓ |
| Monaco                    | ✓ | ✓ |
| Montenegro                | ✓ | ✓ |
| Niederlande               | ✓ | ✓ |
| Norwegen                    | ✓ | ✓ |
| Polen                    | ✓ | ✓ |
| Portugal                  | ✓ | ✓ |
| Rumänien                   | ✓ | ✓ |
| Russische Föderation        | ✓ | ✓ |
| San Marino                | ✓ | ✓ |
| Serbien                    | ✓ | ✓ |
| Slowakei                  | ✓ | ✓ |
| Slowenien                  | ✓ | ✓ |
| Südliche Kurilen           | ✓ | ✓ |
| Spanien                     | ✓ | ✓ |
| Spitzbergen                  | ✓ | ✓ |
| Schweden                    | ✓ | ✓ |
| Schweiz               | ✓ | ✓ |
| Türkei                    | ✓ | ✓ |
| Ukraine                   | ✓ | ✓ |
| United Kingdom            | ✓ | ✓ |
| Vatikanstadt              | ✓ | ✓ |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Rendern in Azure Maps finden Sie unter [Zoomstufen und Grobraster](zoom-levels-and-tile-grid.md).

Erfahren Sie mehr über die [Abdeckungsbereiche des Routingdiensts von Maps](routing-coverage.md). 
