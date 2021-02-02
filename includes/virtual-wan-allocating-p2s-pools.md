---
title: include file
description: include file
services: virtual-wan
author: wellee
ms.service: virtual-wan
ms.topic: include
ms.date: 01/12/2021
ms.author: wellee
ms.custom: include file
ms.openlocfilehash: 67a996dbe1eb7090ea5f9ee9f0880698f4ba74a3
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98919065"
---
In diesem Abschnitt werden Richtlinien und Anforderungen für die Zuordnung von Clientadressräumen beschrieben, bei denen die ausgewählte Skalierungseinheit des Point-to-Site-VPN-Gateways für den Virtual WAN-Hub größer oder gleich 40 ist.

### <a name="background"></a>Hintergrund

Point-to-Site-VPN-Gateways im Virtual WAN-Hub werden mit mehreren Instanzen bereitgestellt. Jede Instanz eines Point-to-Site-VPN-Gateways kann bis zu 10.000 gleichzeitige Point-to-Site-Benutzerverbindungen unterstützen. Für Skalierungseinheiten über 40 muss daher von Virtual WAN zusätzliche Kapazität bereitgestellt werden. Hierzu muss für verschiedene Skalierungseinheiten eine Mindestanzahl von Adresspools zugewiesen werden.

Wenn für die Skalierungseinheit also beispielsweise der Wert „100“ ausgewählt wird, werden für das Point-to-Site-VPN-Gateway im virtuellen Hub fünf Instanzen bereitgestellt. Von dieser Bereitstellung können 50.000 gleichzeitige Verbindungen und **mindestens** fünf verschiedene Adresspools unterstützt werden.

**Verfügbare Skalierungseinheiten**

| Skalierungseinheit | Maximal unterstützte Clientanzahl | Mindestanzahl von Adresspools |
|--- |--- |--- |
| 40 | 20000 | 2 |
| 60 | 30.000 | 3 |
| 80 | 40.000 | 4 |
| 100 | 50000 | 5 |
| 120 | 60000 | 6 |
| 140 | 70000 | 7 |
| 160 | 80.000 | 8 |
| 180 | 90000 | 9 |
| 200 | 100.000 | 10 |

### <a name="specifying-address-pools"></a>Angeben von Adresspools

Im Anschluss finden Sie einige Richtlinien für die Wahl eines Adresspools. Beachten Sie, dass Zuweisungen von Point-to-Site-VPN-Adresspools automatisch von Virtual WAN vorgenommen werden.

1. Pro Gatewayinstanz sind maximal 10.000 gleichzeitige Verbindungen möglich. Daher muss jeder Adresspool mindestens 10.000 eindeutige RFC1918-IP-Adressen enthalten.
1. Mehrere Adresspoolbereiche werden automatisch kombiniert und einer **einzelnen** Gatewayinstanz zugewiesen. Für Gatewayinstanzen mit weniger als 10.000 IP-Adressen wird das Roundrobin-Verfahren verwendet. So kann beispielsweise ein Pool mit 5.000 Adressen von Virtual WAN automatisch mit einem anderen Pool kombiniert werden, der über 8.000 Adressen verfügt und einer einzelnen Gatewayinstanz zugewiesen ist.
1. Ein einzelner Adresspool wird nur von Virtual WAN einer einzelnen Gatewayinstanz zugewiesen.
1. Adresspools müssen unterschiedlich sein. Zwischen Adresspools darf es keine Überschneidungen geben.

> [!NOTE] 
> Ist ein Adresspool einer Gatewayinstanz zugeordnet, die gerade gewartet wird, kann der Adresspool nicht einer anderen Instanz zugewiesen werden.

### <a name="example"></a>Beispiel 

Im folgenden Beispiel werden durch 60 Skalierungseinheiten zwar bis zu 30.000 Verbindungen unterstützt, der zugeordnete Adresspool führt jedoch zu weniger als 30.000 gleichzeitigen Verbindungen.

In diesem Setup werden insgesamt 28.192 gleichzeitige Verbindungen unterstützt. Die erste Gatewayinstanz unterstützt 10.000 Adressen, die zweite Instanz unterstützt 8.192 Verbindungen, und die dritte Instanz unterstützt wieder 10.000 Adressen.

| Adresspoolnummer | Adresspool | Unterstützte Verbindungen |
|--- |--- |--- |
| 1 | 10.12.0.0/15 | 10000 |
| 2 | 10.13.0.0/19 | 8192 |
| 3 | 10.14.0.0/15 | 10000|

**1. Empfehlung: Stellen Sie sicher, dass der zweite Adresspool mindestens 10.000 unterschiedliche IP-Adressen enthält. (Beispiel: 10.13.0.0/15)**

**2. Empfehlung: Fügen Sie einen weiteren Adresspool hinzu (Beispiel: Adresspool 4 – 10.15.0.0/21 mit 2.048 Adressen). Die Adresspools 2 und 4 werden automatisch kombiniert und ermöglichen es der Gatewayinstanz, 10.000 gleichzeitige Verbindungen zu unterstützen.**
