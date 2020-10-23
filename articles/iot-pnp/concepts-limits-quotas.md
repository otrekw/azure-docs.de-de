---
title: Grenzwerte und Kontingente in IoT Plug & Play | Microsoft-Dokumentation
description: Informieren Sie sich hier über die Grenzwerte, Kontingente und Drosselungen, die in IoT Plug & Play gelten.
author: prashmo
ms.author: prashmo
ms.date: 07/21/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: d965d9cb8b87ce0b67f4fe0c07b660fdfd69cc07
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91577984"
---
# <a name="iot-plug-and-play-limits-quotas-and-throttles"></a>IoT Plug & Play: Grenzwerte, Kontingente und Drosselungen

In diesem Artikel werden die IoT Plug & Play-spezifischen Grenzwerte, Kontingente und Drosselungen erläutert. Einige vorhandene [IoT Hub-Kontingente und Drosselungen](../iot-hub/iot-hub-devguide-quotas-throttling.md) werden ebenfalls angewendet.

## <a name="iot-hub"></a>IoT Hub

Für einen IoT-Hub gelten die folgenden Grenzwerte und Kontingente:

| Grenzwerte, Einschränkungen und Drosselungen | Wert | Notizen |
|-----|-----|-----|
| Anzahl von Schnittstellen, die pro Hub registriert werden können | 1500 ||
| Maximale Größe eines Komponentennamens | 1 bis 64 Zeichen | Zulässige Zeichen: a–z, A–Z, 0–9 (nicht als erstes Zeichen) und Unterstrich (nicht als erstes oder letztes Zeichen) |
| Maximale Größe eines Eigenschaftsnamens | 1 bis 64 Zeichen | Zulässige Zeichen: a–z, A–Z, 0–9 (nicht als erstes Zeichen) und Unterstrich (nicht als erstes oder letztes Zeichen) |
| Maximale Größe eines Eigenschaftswerts | Entspricht DTDL-[Eigenschaft](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md#property) (Digital Twins Definition Language) | 5 Ebenen Tiefe und möglicherweise kein Array oder beliebiges komplexes Schema, das ein Array enthält |
| Maximale Größe eines Befehlsnamens | 1 bis 64 Zeichen | Zulässige Zeichen: a–z, A–Z, 0–9 (nicht als erstes Zeichen) und Unterstrich (nicht als erstes oder letztes Zeichen)|
| Größe des Gerätezwillings | Identisch mit [IoT Hub-Grenzwerten](../iot-hub/iot-hub-devguide-device-twins.md#device-twin-size) ||

## <a name="parser-library"></a>Parserbibliothek

Für die Parserbibliothek gelten die Grenzwerte der [Digital Twins Definition Language](https://github.com/Azure/opendigitaltwins-dtdl).

## <a name="next-steps"></a>Nächste Schritte

Als nächster Schritt wird empfohlen, sich die [IoT Plug & Play-Architektur](concepts-architecture.md) anzusehen.
