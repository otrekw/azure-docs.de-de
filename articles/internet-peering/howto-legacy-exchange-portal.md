---
title: Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource über das Azure-Portal
titleSuffix: Azure
description: Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource über das Azure-Portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 87a7a6bca608f1748d3b659eabdc3e941b537377
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678523"
---
# <a name="convert-a-legacy-exchange-peering-to-an-azure-resource-by-using-the-azure-portal"></a>Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource über das Azure-Portal

In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure-Portals ein vorhandenes Legacy-Exchange Peering in eine Azure-Ressource konvertieren.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe von [PowerShell](howto-legacy-exchange-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [Exemplarische Vorgehensweise für das Exchange Peering](walkthrough-exchange-all.md).

## <a name="convert-a-legacy-exchange-peering-to-an-azure-resource"></a>Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Melden Sie sich beim Portal an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-legacy-exchange-peering"></a><a name=create></a>Konvertieren von Legacy-Exchange Peering

Sie können Legacy-Peeringverbindungen mithilfe der **Peeringressource** konvertieren.

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Starten der Ressource und Konfigurieren grundlegender Einstellungen
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Konfigurieren von Verbindungen und Übermitteln
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration-legacy.md)]

### <a name="verify-exchange-peering"></a><a name=get></a>Überprüfen von Exchange Peering
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Exchange Peering über das Portal](howto-exchange-portal.md)
