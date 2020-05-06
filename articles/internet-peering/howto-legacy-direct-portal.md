---
title: Konvertieren einer Legacyinstanz für direktes Peering in eine Azure-Ressource über das Azure-Portal
titleSuffix: Azure
description: Konvertieren einer Legacyinstanz für direktes Peering in eine Azure-Ressource über das Azure-Portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: e49e4d5debe63b99039bbafbc14f7788367314f3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678864"
---
# <a name="convert-a-legacy-direct-peering-to-an-azure-resource-by-using-the-azure-portal"></a>Konvertieren einer Legacyinstanz für direktes Peering in eine Azure-Ressource über das Azure-Portal

In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure-Portals eine vorhandene Instanz für direktes Peering in eine Azure-Ressource konvertieren.

Sie können diese Anleitung auch mithilfe von [PowerShell](howto-legacy-direct-powershell.md) ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
* Informieren Sie sich vor der Konfiguration zunächst über die [Voraussetzungen](prerequisites.md), und lesen Sie den Artikel [Exemplarische Vorgehensweise für direktes Peering](walkthrough-direct-all.md).


## <a name="convert-a-legacy-direct-peering-to-an-azure-resource"></a>Konvertieren einer Legacyinstanz für direktes Peering in eine Azure-Ressource

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Melden Sie sich beim Portal an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-a-legacy-direct-peering"></a><a name=create></a> Konvertieren einer Legacyinstanz für direktes Peering

Sie können Legacy-Peeringverbindungen mithilfe der **Peeringressource** konvertieren.

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Starten der Ressource und Konfigurieren grundlegender Einstellungen
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Konfigurieren von Verbindungen und Übermitteln
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration-legacy.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Überprüfen des direkten Peerings
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern eines direkten Peerings über das Portal](howto-direct-portal.md)
