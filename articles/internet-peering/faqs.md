---
title: Häufig gestellte Fragen zum Internetpeering
titleSuffix: Azure
description: Häufig gestellte Fragen zum Internetpeering
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: reference
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 9b0b2b08e01c99fc918c4bc5649197c9caa4978a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "75774033"
---
# <a name="internet-peering---faqs"></a>Häufig gestellte Fragen zum Internetpeering

Hier finden Sie Antworten auf allgemeine Fragen.

**Was ist der Unterschied zwischen Internetpeering und Peering Service?**

Peering Service ist ein Dienst zur Bereitstellung professioneller öffentlicher IP-Konnektivität mit Microsoft für Unternehmenskunden. Internet auf Unternehmensniveau umfasst Konnektivität über ISPs, die über Konnektivität mit Microsoft mit hohem Durchsatz sowie über Redundanz für Hochverfügbarkeit verfügen. Darüber hinaus wird die Wartezeit für Benutzerdatenverkehr durch Verwendung des nächstgelegenen Microsoft-Edgestandorts optimiert. Peering Service basiert auf Peeringkonnektivität mit dem Partnernetzbetreiber. Bei der Peeringkonnektivität mit dem Partner muss es sich um direktes Peering (im Gegensatz zu Austauschpeering) handeln. Direktes Peering muss über lokale Redundanz sowie über Georedundanz verfügen.

**Was ist älteres Peering?**

Die Einrichtung einer Peeringverbindung mithilfe von Azure PowerShell wird als Azure-Ressource verwaltet. In der Vergangenheit eingerichtete Peeringverbindungen sind in unserem System als älteres Peering gespeichert und können für die Verwaltung als Azure-Ressource konvertiert werden.

**Welche IP-Adressen werden beim Aufrufen von „New-AzPeeringDirectConnectionObject“ für Microsoft-Geräte und Peergeräte vergeben?**

Beim Aufrufen des Cmdlets „New-AzPeeringDirectConnectionObject“ wird eine /31-Adresse (a.b.c.d/31) oder eine /30-Adresse (a.b.c.d/30) eingegeben. Die erste IP-Adresse (a.b.c.d+0) wird dem Peergerät zugewiesen. Die zweite IP-Adresse (a.b.c.d+1) wird dem Microsoft-Gerät zugewiesen.

**Worum handelt es sich bei den Parametern „MaxPrefixesAdvertisedIPv4“ und „MaxPrefixesAdvertisedIPv6“ im Cmdlet „New-AzPeeringDirectConnectionObject“?**

Die Parameter 2MaxPrefixesAdvertisedIPv4“ und „MaxPrefixesAdvertisedIPv6“ stellen die maximale Anzahl von IPv4- und IPv6-Präfixen dar, die auf Wunsch eines Peers von Microsoft akzeptiert werden sollen. Diese Parameter können jederzeit geändert werden.