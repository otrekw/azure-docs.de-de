---
title: Übersicht über Azure Firewall-Diensttags
description: Ein Diensttag steht für eine Gruppe von IP-Adressen und hat die Aufgabe, bei der Erstellung von Sicherheitsregeln die Komplexität zu verringern.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: ec9fb3c9b27d21ac65888379f59d51ecc8042eb0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74168695"
---
# <a name="azure-firewall-service-tags"></a>Azure Firewall-Diensttags

Ein Diensttag steht für eine Gruppe von IP-Adressen und hat die Aufgabe, bei der Erstellung von Sicherheitsregeln die Komplexität zu verringern. Sie können kein eigenes Diensttag erstellen und auch nicht angeben, welche IP-Adressen in einem Tag enthalten sind. Microsoft verwaltet die Adresspräfixe, die mit dem Diensttag abgedeckt werden, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

Azure Firewall-Diensttags können im Zielfeld für Netzwerkregeln verwendet werden. Sie können sie anstelle bestimmter IP-Adressen verwenden.

## <a name="supported-service-tags"></a>Unterstützte Diensttags

Eine Liste mit Diensttags, die für Netzwerkregeln der Azure-Firewall verfügbar sind, finden Sie unter [Diensttags](../virtual-network/security-overview.md#service-tags).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Firewall-Regeln finden Sie unter [Logik für die Azure Firewall-Regelverarbeitung](rule-processing.md).