---
title: Konvertieren einer älteren Instanz für direktes Peering in eine Azure-Ressource über das Portal
titleSuffix: Azure
description: Konvertieren einer älteren Instanz für direktes Peering in eine Azure-Ressource über das Portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 9900414d38bd597d08a80d15e908228c06ce06ea
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773897"
---
# <a name="convert-a-legacy-direct-peering-to-azure-resource-using-the-portal"></a>Konvertieren einer älteren Instanz für direktes Peering in eine Azure-Ressource über das Portal

In diesem Artikel wird beschrieben, wie Sie mithilfe des Portals ein vorhandenes älteres direktes Peering in eine Azure-Ressource konvertieren.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe von [PowerShell](howto-legacy-direct-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [exemplarische Vorgehensweise für direktes Peering](walkthrough-direct-all.md).


## <a name="convert-legacy-direct-peering-to-azure-resource"></a>Konvertieren eines älteren direkten Peerings in eine Azure-Ressource

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Melden Sie sich beim Portal an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name=create></a>Konvertieren eines älteren direkten Peerings

Sie können ältere Peeringverbindungen mithilfe einer **Peeringressource** konvertieren.

#### <a name="launch-resource-and-configure-basic-settings"></a>Starten der Ressource und Konfigurieren grundlegender Einstellungen
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Konfigurieren von Verbindungen und Übermitteln
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration-legacy.md)]

### <a name=get></a>Überprüfen des direkten Peerings
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern eines direkten Peerings über das Portal](howto-direct-portal.md)
