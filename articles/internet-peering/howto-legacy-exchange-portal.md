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
ms.openlocfilehash: a40c7bbc9f37135814b7bba3396d368faf97a166
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773941"
---
# <a name="convert-a-legacy-exchange-peering-to-azure-resource-using-the-portal"></a>Konvertieren einer älteren Instanz für Austauschpeering in eine Azure-Ressource über das Portal

In diesem Artikel wird beschrieben, wie Sie mithilfe des Portals ein vorhandenes Legacy-Exchange Peering in eine Azure-Ressource konvertieren.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe von [PowerShell](howto-legacy-exchange-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [Exemplarische Vorgehensweise für das Exchange Peering](walkthrough-exchange-all.md).

## <a name="convert-a-legacy-exchange-peering-to-azure-resource"></a>Konvertieren einer älteren Instanz für Austauschpeering in eine Azure-Ressource

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Melden Sie sich beim Portal an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name=create></a>Konvertieren von Legacy-Exchange Peering

Sie können Legacy-Peeringverbindungen mithilfe einer **Peeringressource** konvertieren.

#### <a name="launch-resource-and-configure-basic-settings"></a>Starten der Ressource und Konfigurieren grundlegender Einstellungen
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Konfigurieren von Verbindungen und Übermitteln
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration-legacy.md)]

### <a name=get></a>Überprüfen von Exchange Peering
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Exchange Peering über das Portal](howto-exchange-portal.md)
