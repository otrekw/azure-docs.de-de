---
title: include file
description: include file
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 01/10/2020
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: 5608683578a796ae0fffff7544a749043e8698fd
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656212"
---
| Resource | Begrenzung |
| ---------------------------------------------------------------------- | -------------------------- |
| Maximale Anzahl von Skalierungseinheiten | Zehn pro Region<sup>1</sup> |
| Cachegröße | 5 GiB pro Einheit<sup>2</sup> |
| Gleichzeitige Back-End-Verbindungen<sup>3</sup> pro HTTP-Autorität | 2\.048 pro Einheit<sup>4</sup> |
| Maximale Größe zwischengespeicherter Antworten | 2 MiB |
| Maximale Größe eines Richtliniendokuments | 256 KiB<sup>5</sup> |
| Maximale Anzahl benutzerdefinierter Gatewaydomänen pro Dienstinstanz<sup>6</sup> | 20 |
| Maximale Anzahl von CA-Zertifikaten pro Dienstinstanz | 10 |
| Maximale Anzahl von Instanzen pro Abonnement<sup>7</sup> | 20 |
| Maximale Anzahl von Abonnements pro Dienstinstanz<sup>7</sup> | 500 |
| Maximale Anzahl von Clientzertifikaten pro Dienstinstanz<sup>7</sup> | 50 |
| Maximale Anzahl von APIs pro Dienstinstanz<sup>7</sup> | 50 |
| Maximale Anzahl von API-Vorgängen pro Dienstinstanz<sup>7</sup> | 1\.000 |
| Maximale Gesamtdauer der Anforderungen<sup>7</sup> | 30 Sekunden |
| Maximale gepufferte Nutzlastgröße<sup>7</sup> | 2 MiB |
| Maximale Größe der Anforderungs-URL<sup>8</sup> | 4096 Bytes |

<sup>1</sup>Die Skalierungsgrenzen sind vom Tarif abhängig. Die Tarife und die dazugehörigen Grenzwerte für die Skalierung finden Sie unter [API Management – Preise](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup>Die Cachegröße pro Einheit ist vom Tarif abhängig. Die Tarife und die dazugehörigen Grenzwerte für die Skalierung finden Sie unter [API Management – Preise](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup>Verbindungen werden in Pools zusammengefasst und wiederverwendet, sofern sie nicht explizit durch das Back-End geschlossen wurden.<br/>
<sup>4</sup>Dieser Grenzwert gilt pro Einheit der Tarife „Basic“, „Standard“ und „Premium“. Der Developer-Tarif ist auf 1.024 beschränkt. Dieser Grenzwert betrifft den Tarif „Consumption“ nicht.<br/>
<sup>5</sup>Dieser Grenzwert gilt für die Tarife „Basic“, „Standard“ und „Premium“. Im Tarif „Consumption“ ist die Größe des Richtliniendokuments auf 16 KiB beschränkt.<br/>
<sup>6</sup>Diese Ressource ist nur im Tarif „Premium“ verfügbar.<br/>
<sup>7</sup>Diese Ressource betrifft nur den Tarif „Consumption“.<br/>
<sup>8</sup>Gilt für nur den Tarif „Consumption“. Enthält eine Abfragezeichenfolge der Länge bis zu 2048 Bytes.<br/>
