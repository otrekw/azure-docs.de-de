---
title: Grenzwerte und Kontingente in IoT Plug & Play (Vorschau) | Microsoft-Dokumentation
description: Informieren Sie sich über die Grenzwerte, Kontingente und Drosselungen, die in IoT Plug & Play (Vorschau) gelten.
author: prashmo
ms.author: prashmo
ms.date: 07/21/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 5c4377120f61792b580225a22b9f5ff51b5e1b64
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337397"
---
# <a name="iot-plug-and-play-preview-limits-quotas-and-throttles"></a>IoT Plug & Play (Vorschau): Grenzwerte, Kontingente und Drosselungen

In diesem Artikel werden die IoT Plug & Play-spezifischen Grenzwerte, Kontingente und Drosselungen erläutert, die in der öffentlichen Vorschau gelten. Einige vorhandene [IoT Hub-Kontingente und Drosselungen](../iot-hub/iot-hub-devguide-quotas-throttling.md) werden ebenfalls angewendet.

## <a name="iot-hub"></a>IoT Hub

In der öffentlichen Vorschau gelten die folgenden Grenzwerte und Kontingente für einen IoT-Hub:

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
